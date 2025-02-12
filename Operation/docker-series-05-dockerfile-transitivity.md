> *`Author: ACatSmiling`*
>
> *`Since: 2025-02-12`*

内网开发时，下载项目依赖的模块，往往会成为一件很复杂的事情。在这种情况下，利用容器化技术，可以很巧妙的解决这个问题。下面，我们以开源框架 dify 来说明操作。

首先，从 Github 下载最新的源码到本地服务器（可以连接外网），dify 源码包含一个 docker-compose.yaml 文件，截取部分内容如下：

```yaml
services:
  # API service
  api:
    image: langgenius/dify-api:0.15.3
    restart: always
    environment:
      # Use the shared environment variables.
      <<: *shared-api-worker-env
      # Startup mode, 'api' starts the API server.
      MODE: api
      SENTRY_DSN: ${API_SENTRY_DSN:-}
      SENTRY_TRACES_SAMPLE_RATE: ${API_SENTRY_TRACES_SAMPLE_RATE:-1.0}
      SENTRY_PROFILES_SAMPLE_RATE: ${API_SENTRY_PROFILES_SAMPLE_RATE:-1.0}
    depends_on:
      - db
      - redis
    volumes:
      # Mount the storage directory to the container, for storing user files.
      - ./volumes/app/storage:/app/api/storage
    networks:
      - ssrf_proxy_network
      - default

  # worker service
  # The Celery worker for processing the queue.
  worker:
    image: langgenius/dify-api:0.15.3
    restart: always
    environment:
      # Use the shared environment variables.
      <<: *shared-api-worker-env
      # Startup mode, 'worker' starts the Celery worker for processing the queue.
      MODE: worker
      SENTRY_DSN: ${API_SENTRY_DSN:-}
      SENTRY_TRACES_SAMPLE_RATE: ${API_SENTRY_TRACES_SAMPLE_RATE:-1.0}
      SENTRY_PROFILES_SAMPLE_RATE: ${API_SENTRY_PROFILES_SAMPLE_RATE:-1.0}
    depends_on:
      - db
      - redis
    volumes:
      # Mount the storage directory to the container, for storing user files.
      - ./volumes/app/storage:/app/api/storage
    networks:
      - ssrf_proxy_network
      - default

  # Frontend web application.
  web:
    image: langgenius/dify-web:0.15.3
    restart: always
    environment:
      CONSOLE_API_URL: ${CONSOLE_API_URL:-}
      APP_API_URL: ${APP_API_URL:-}
      SENTRY_DSN: ${WEB_SENTRY_DSN:-}
      NEXT_TELEMETRY_DISABLED: ${NEXT_TELEMETRY_DISABLED:-0}
      TEXT_GENERATION_TIMEOUT_MS: ${TEXT_GENERATION_TIMEOUT_MS:-60000}
      CSP_WHITELIST: ${CSP_WHITELIST:-}
      TOP_K_MAX_VALUE: ${TOP_K_MAX_VALUE:-}
      INDEXING_MAX_SEGMENTATION_TOKENS_LENGTH: ${INDEXING_MAX_SEGMENTATION_TOKENS_LENGTH:-}

  # The postgres database.
  db:
    image: postgres:15-alpine
    restart: always
    environment:
      PGUSER: ${PGUSER:-postgres}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD:-difyai123456}
      POSTGRES_DB: ${POSTGRES_DB:-dify}
      PGDATA: ${PGDATA:-/var/lib/postgresql/data/pgdata}
    command: >
      postgres -c 'max_connections=${POSTGRES_MAX_CONNECTIONS:-100}'
               -c 'shared_buffers=${POSTGRES_SHARED_BUFFERS:-128MB}'
               -c 'work_mem=${POSTGRES_WORK_MEM:-4MB}'
               -c 'maintenance_work_mem=${POSTGRES_MAINTENANCE_WORK_MEM:-64MB}'
               -c 'effective_cache_size=${POSTGRES_EFFECTIVE_CACHE_SIZE:-4096MB}'
    volumes:
      - ./volumes/db/data:/var/lib/postgresql/data
    healthcheck:
      test: [ 'CMD', 'pg_isready' ]
      interval: 1s
      timeout: 3s
      retries: 30

  # The redis cache.
  redis:
    image: redis:6-alpine
    restart: always
    environment:
      REDISCLI_AUTH: ${REDIS_PASSWORD:-difyai123456}
    volumes:
      # Mount the redis data directory to the container.
      - ./volumes/redis/data:/data
    # Set the redis password when startup redis server.
    command: redis-server --requirepass ${REDIS_PASSWORD:-difyai123456}
    healthcheck:
      test: [ 'CMD', 'redis-cli', 'ping' ]
```

在 docker-compose.yaml 文件中，已经定义好服务相关的镜像，直接通过 docker-compose 命令，下载镜像并启动：

```shell
$ docker-compose up -d
```

待镜像下载完成后，将本地的镜像打包：

```shell
# 打包
$ docker save -o <image-name>.tar <image>
# 打包并压缩
$ docker save <image> | gzip > ./<image-name>.tar.gz
```

再将打包后的文件，迁移到内网，并加载到本地仓库：

```shell
$ docker load -i <image-name>.tar
```

等镜像在内网服务器加载后，再解压 dify 源码，进入 docker-compose.yaml 文件路径，即可启动服务。**此时，加载到内网的镜像中，包含了每个模块运行时需要的所有依赖。**

然后，dify 的各个模块，也有独立的 Dockefile 文件，以 api 模块为例：

```dockerfile
# base image
FROM python:3.12-slim-bookworm AS base

WORKDIR /app/api

# Install Poetry
ENV POETRY_VERSION=2.0.1

# if you located in China, you can use aliyun mirror to speed up
# RUN pip install --no-cache-dir poetry==${POETRY_VERSION} -i https://mirrors.aliyun.com/pypi/simple/

RUN pip install --no-cache-dir poetry==${POETRY_VERSION}

# Configure Poetry
ENV POETRY_CACHE_DIR=/tmp/poetry_cache
ENV POETRY_NO_INTERACTION=1
ENV POETRY_VIRTUALENVS_IN_PROJECT=true
ENV POETRY_VIRTUALENVS_CREATE=true
ENV POETRY_REQUESTS_TIMEOUT=15

FROM base AS packages

# if you located in China, you can use aliyun mirror to speed up
# RUN sed -i 's@deb.debian.org@mirrors.aliyun.com@g' /etc/apt/sources.list.d/debian.sources

RUN apt-get update \
    && apt-get install -y --no-install-recommends gcc g++ libc-dev libffi-dev libgmp-dev libmpfr-dev libmpc-dev

# Install Python dependencies
COPY pyproject.toml poetry.lock ./
RUN poetry install --sync --no-cache --no-root

# production stage
FROM base AS production

ENV FLASK_APP=app.py
ENV EDITION=SELF_HOSTED
ENV DEPLOY_ENV=PRODUCTION
ENV CONSOLE_API_URL=http://127.0.0.1:5001
ENV CONSOLE_WEB_URL=http://127.0.0.1:3000
ENV SERVICE_API_URL=http://127.0.0.1:5001
ENV APP_WEB_URL=http://127.0.0.1:3000

EXPOSE 5001

# set timezone
ENV TZ=UTC

WORKDIR /app/api

RUN \
    apt-get update \
    # Install dependencies
    && apt-get install -y --no-install-recommends \
        # basic environment
        curl nodejs libgmp-dev libmpfr-dev libmpc-dev \
        # For Security
        expat libldap-2.5-0 perl libsqlite3-0 zlib1g \
        # install a chinese font to support the use of tools like matplotlib
        fonts-noto-cjk \
        # install libmagic to support the use of python-magic guess MIMETYPE
        libmagic1 \
    && apt-get autoremove -y \
    && rm -rf /var/lib/apt/lists/*

# Copy Python environment and packages
ENV VIRTUAL_ENV=/app/api/.venv
COPY --from=packages ${VIRTUAL_ENV} ${VIRTUAL_ENV}
ENV PATH="${VIRTUAL_ENV}/bin:${PATH}"

# Download nltk data
RUN python -c "import nltk; nltk.download('punkt'); nltk.download('averaged_perceptron_tagger')"

# Copy source code
COPY . /app/api/

# Copy entrypoint
COPY docker/entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

ARG COMMIT_SHA
ENV COMMIT_SHA=${COMMIT_SHA}

ENTRYPOINT ["/bin/bash", "/entrypoint.sh"]
```

根据业务需求，如果需要修改相应的源码，可以**修改 docker-compose.yaml 文件和 Dockerfile 文件内容（重点即为此步骤）**，基于已经加载到内网的镜像，替换修改后的代码文件：

```yaml
services:
  # API service
  api:
    # image: langgenius/dify-api:0.14.0
    build: ../api # 以 api 模块为例，取消拉取镜像，而是从 api 模块的 Dockerfile 进行 build
    restart: always
    privileged: true
    environment:
      # Use the shared environment variables.
      <<: *shared-api-worker-env
      # Startup mode, 'api' starts the API server.
      MODE: api
      SENTRY_DSN: ${API_SENTRY_DSN:-}
      SENTRY_TRACES_SAMPLE_RATE: ${API_SENTRY_TRACES_SAMPLE_RATE:-1.0}
      SENTRY_PROFILES_SAMPLE_RATE: ${API_SENTRY_PROFILES_SAMPLE_RATE:-1.0}
    depends_on:
      - db
      - redis
    volumes:
      # Mount the storage directory to the container, for storing user files.
      - ./volumes/app/storage:/app/api/storage
      #- ./volumes/app/auth:/app/api/controllers/console/auth
      - ./volumes/app/logs:/app/logs
    networks:
      - ssrf_proxy_network
      - default
```

```shell
# base image
FROM langgenius/dify-api:0.14.0 # 基于原镜像

# test
RUN ls -al ../ # 测试代码

# Copy source code
COPY . /app/api/ # 替换修改后的代码文件
```

再重新构建：

```shell
$ docker-compose build
```

构建完成后启动服务：

```shell
$ docker-compose up -d
```

至此，我们既可以使用容器中的依赖，也可以根据需求自行修改代码，不需要在内网环境中加载相应的依赖。

**如果我们的项目需要加载较多的依赖，可以在外网环境下，通过 Dockerfile 构建好镜像，然后导入内网，再修改 docker-compose.yaml 文件和 Dockerfile 文件，以此镜像作为基础，按业务需求，修改源码后，以新的 docker-compose.yaml 文件和 Dockerfile 文件，拷贝源码替换，重新 build 新的镜像，然后测试验证。**