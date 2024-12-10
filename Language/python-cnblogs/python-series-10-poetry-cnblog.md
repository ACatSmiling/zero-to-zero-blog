> `Author: ACatSmiling`
>
> `Since: 2024-12-07`

官网：https://python-poetry.org/

## Installation

### Window（Powershell）

安装命令：`(Invoke-WebRequest -Uri https://install.python-poetry.org -UseBasicParsing).Content | python -`。

````shell
(base) PS C:\Users\XiSun> python --version
Python 3.10.9
(base) PS C:\Users\XiSun> (Invoke-WebRequest -Uri https://install.python-poetry.org -UseBasicParsing).Content | python -

Retrieving Poetry metadata

# Welcome to Poetry!

This will download and install the latest version of Poetry,
a dependency and package manager for Python.

It will add the `poetry` command to Poetry's bin directory, located at:

C:\Users\XiSun\AppData\Roaming\Python\Scripts

You can uninstall at any time by executing this script with the --uninstall option,
and these changes will be reverted.

Installing Poetry (1.8.5)
Installing Poetry (1.8.5): Creating environment
Installing Poetry (1.8.5): Installing Poetry
Installing Poetry (1.8.5): Creating script
Installing Poetry (1.8.5): Done

Poetry (1.8.5) is installed now. Great!

To get started you need Poetry's bin directory (C:\Users\XiSun\AppData\Roaming\Python\Scripts) in your `PATH`
environment variable.

You can choose and execute one of the following commands in PowerShell:

A. Append the bin directory to your user environment variable `PATH`:

```
[Environment]::SetEnvironmentVariable("Path", [Environment]::GetEnvironmentVariable("Path", "User") + ";C:\Users\XiSun\AppData\Roaming\Python\Scripts", "User")
```

B. Try to append the bin directory to PATH every when you run PowerShell (>=6 recommended):

```
echo 'if (-not (Get-Command poetry -ErrorAction Ignore)) { $env:Path += ";C:\Users\XiSun\AppData\Roaming\Python\Scripts" }' | Out-File -Append $PROFILE
```

Alternatively, you can call Poetry explicitly with `C:\Users\XiSun\AppData\Roaming\Python\Scripts\poetry`.

You can test that everything is set up by executing:

`poetry --version`
````

设置环境变量：

![image-20241207191209698](https://img2023.cnblogs.com/blog/3488201/202412/3488201-20241210002612087-757856907.png)

验证：`poetry --version`。

```cmd
C:\Users\XiSun>poetry --version
Poetry (version 1.8.5)
```

## Quick Start

初始化：`poetry init`。
```cmd
C:\Users\XiSun\NewVolume-D\Codes\python>cd poetry-demo

C:\Users\XiSun\NewVolume-D\Codes\python\poetry-demo>poetry init

This command will guide you through creating your pyproject.toml config.

Package name [poetry-demo]:
Version [0.1.0]:
Description []:
Author [ACatSmiling <1172042509@qq.com>, n to skip]:
License []:
Compatible Python versions [^3.10]:

Would you like to define your main dependencies interactively? (yes/no) [yes]
You can specify a package in the following forms:
  - A single name (requests): this will search for matches on PyPI
  - A name and a constraint (requests@^2.23.0)
  - A git url (git+https://github.com/python-poetry/poetry.git)
  - A git url with a revision (git+https://github.com/python-poetry/poetry.git#develop)
  - A file path (../my-package/my-package.whl)
  - A directory (../my-package/)
  - A url (https://example.com/packages/my-package-0.1.0.tar.gz)

Package to add or search for (leave blank to skip):

Would you like to define your development dependencies interactively? (yes/no) [yes]
Package to add or search for (leave blank to skip):

Generated file

[tool.poetry]
name = "poetry-demo"
version = "0.1.0"
description = ""
authors = ["ACatSmiling <xxx@qq.com>"]
readme = "README.md"

[tool.poetry.dependencies]
python = "^3.10"


[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"


Do you confirm generation? (yes/no) [yes]
```

执行`poetry init`命令后，在 poetry-demo 路径下，会生成一个 pyproject.toml：

```toml
[tool.poetry]
name = "poetry-demo"
version = "0.1.0"
description = ""
authors = ["ACatSmiling <xxx@qq.com>"]
readme = "README.md"

[tool.poetry.dependencies]
python = "^3.10"


[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"
```

**创建虚拟环境：**`poetry env use python`。

```cmd
C:\Users\XiSun\NewVolume-D\Codes\python\poetry-demo>poetry env use python
Creating virtualenv poetry-demo--agK16Zd-py3.10 in C:\Users\XiSun\AppData\Local\pypoetry\Cache\virtualenvs
Using virtualenv: C:\Users\XiSun\AppData\Local\pypoetry\Cache\virtualenvs\poetry-demo--agK16Zd-py3.10
```

- poetry 默认会将虚拟环境统一放在指定目录，例如 "C:\Users\XiSun\AppData\Local\pypoetry\Cache\virtualenvs"。
- 虚拟环境的命名模式为：`项目名--随机数--Python版本`。

**查看 Poetry 配置：**`poetry config --list`。

```cmd
C:\Users\XiSun\NewVolume-D\Codes\python\poetry-demo>poetry config --list
cache-dir = "C:\\Users\\XiSun\\AppData\\Local\\pypoetry\\Cache"
experimental.system-git-client = false
installer.max-workers = null
installer.modern-installation = true
installer.no-binary = null
installer.parallel = true
keyring.enabled = true
solver.lazy-wheel = true
virtualenvs.create = true
virtualenvs.in-project = null
virtualenvs.options.always-copy = false
virtualenvs.options.no-pip = false
virtualenvs.options.no-setuptools = false
virtualenvs.options.system-site-packages = false
virtualenvs.path = "{cache-dir}\\virtualenvs"  # C:\Users\XiSun\AppData\Local\pypoetry\Cache\virtualenvs
virtualenvs.prefer-active-python = false
virtualenvs.prompt = "{project_name}-py{python_version}"
warnings.export = true
```

如果需要将虚拟环境设置在当前项目路径下，则修改`virtualenvs.in-project`配置为 true：`poetry config virtualenvs.in-project true`。

```cmd
C:\Users\XiSun\NewVolume-D\Codes\python\poetry-demo>poetry config virtualenvs.in-project true
```

再删除全局的虚拟环境，并重新**初始化：**`poetry env remove python`。
```cmd
C:\Users\XiSun\NewVolume-D\Codes\python\poetry-demo>poetry env remove python
Deleted virtualenv: C:\Users\XiSun\AppData\Local\pypoetry\Cache\virtualenvs\poetry-demo--agK16Zd-py3.10

C:\Users\XiSun\NewVolume-D\Codes\python\poetry-demo>poetry env use python
Creating virtualenv poetry-demo in C:\Users\XiSun\NewVolume-D\Codes\python\poetry-demo\.venv
Using virtualenv: C:\Users\XiSun\NewVolume-D\Codes\python\poetry-demo\.venv

 C:\Users\XiSun\NewVolume-D\Codes\python\poetry-demo 的目录

2024/12/07  20:16    <DIR>          .
2024/12/07  19:19    <DIR>          ..
2024/12/07  20:16    <DIR>          .venv    # 当前项目的虚拟环境目录
2024/12/07  19:18               282 pyproject.toml
               1 个文件            282 字节
               3 个目录 33,321,934,848 可用字节
```

- 虚拟环境的目录，固定在当前项目的根目录下，名称为`.venv`。

**启动虚拟环境：**`poetry shell`。

```cmd
# 在项目的根目录下，使用命令 poetry shell 进入虚拟环境
C:\Users\XiSun\NewVolume-D\Codes\python\poetry-demo>poetry shell

# 进入虚拟环境后
(poetry-demo-py3.10) C:\Users\XiSun\NewVolume-D\Codes\python\poetry-demo>
```

- poetry shell 会检查当前目录或上层目录是否存在`pyproject.toml`，以此确定需要启动的虚拟环境，如果没有该文件，则会报错。

**退出虚拟环境：**`exit`。

```cmd
# 在虚拟环境中，执行 exit 退出虚拟环境
(poetry-demo-py3.10) C:\Users\XiSun\NewVolume-D\Codes\python\poetry-demo>exit

C:\Users\XiSun\NewVolume-D\Codes\python\poetry-demo>
```

## Command

**安装模块：**`poetry add <module name>`。

```cmd
# 可以在项目的根目录下直接使用 poetry add 命令，也可以进入虚拟环境后再使用 poetry add 命令
C:\Users\XiSun\NewVolume-D\Codes\python\poetry-demo>poetry add flask
Using version ^3.1.0 for flask

Updating dependencies
Resolving dependencies... (2.9s)

Package operations: 8 installs, 0 updates, 0 removals

  - Installing colorama (0.4.6)
  - Installing markupsafe (3.0.2)
  - Installing blinker (1.9.0)
  - Installing click (8.1.7)
  - Installing itsdangerous (2.2.0)
  - Installing jinja2 (3.1.4)
  - Installing werkzeug (3.1.3)
  - Installing flask (3.1.0)

Writing lock file
```

>当安装 flask 后，项目结构也发生了变化：
>
>![image-20241207232346977](https://img2023.cnblogs.com/blog/3488201/202412/3488201-20241210002611430-315920474.png)
>
>pyproject.toml 文件的变化：
>
>```toml
>[tool.poetry]
>name = "poetry-demo"
>version = "0.1.0"
>description = ""
>authors = ["ACatSmiling <xxx@qq.com>"]
>readme = "README.md"
>
>[tool.poetry.dependencies]
>python = "^3.10"
>flask = "^3.1.0" # 新增 flask 模块
>
>
>[build-system]
>requires = ["poetry-core"]
>build-backend = "poetry.core.masonry.api"
>```
>
>- 虽然在安装 flask 的时候，也安装了其他的依赖，但在 pyproject.toml 文件中，只会增加 "flask = "^3.1.0"" 这个字段的第三方模块，其余依赖不会出现在 pyproject.toml 文件中。这可以方便用户区分所安装的第三方模块，以及第三方模块安装时附属的依赖。

**新增模块至 dev-dependencies：**`poetry add <module name> --group dev`。

```cmd
C:\Users\XiSun\NewVolume-D\Codes\python\poetry-demo>poetry add black --group dev
Using version ^24.10.0 for black

Updating dependencies
Resolving dependencies... (0.1s)

Package operations: 7 installs, 0 updates, 0 removals

  - Installing mypy-extensions (1.0.0)
  - Installing packaging (24.2)
  - Installing pathspec (0.12.1)
  - Installing platformdirs (4.3.6)
  - Installing tomli (2.2.1)
  - Installing typing-extensions (4.12.2)
  - Installing black (24.10.0)

Writing lock file
```

- 以 Black 为例，将其添加到 dev-dependencies，对于一些只需要在开发环境使用的依赖，将其添加到 dev 分组，是非常有必要的。

>执行命令后，pyproject.toml 文件的变化：
>
>```toml
>[tool.poetry]
>name = "poetry-demo"
>version = "0.1.0"
>description = ""
>authors = ["ACatSmiling <1172042509@qq.com>"]
>readme = "README.md"
>
>[tool.poetry.dependencies]
>python = "^3.10"
>flask = "^3.1.0"
>
>
># black 模块添加到 dev 分组
>[tool.poetry.group.dev.dependencies]
>black = "^24.10.0"
>
>[build-system]
>requires = ["poetry-core"]
>build-backend = "poetry.core.masonry.api"
>```

**展示模块：**`poetry show`。

```cmd
C:\Users\XiSun\NewVolume-D\Codes\python\poetry-demo>poetry show
blinker      1.9.0 Fast, simple object-to-object and broadcast signaling
click        8.1.7 Composable command line interface toolkit
colorama     0.4.6 Cross-platform colored terminal text.
flask        3.1.0 A simple framework for building complex web applications.
itsdangerous 2.2.0 Safely pass data to untrusted environments and back.
jinja2       3.1.4 A very fast and expressive template engine.
markupsafe   3.0.2 Safely add untrusted strings to HTML/XML markup.
werkzeug     3.1.3 The comprehensive WSGI web application library.

# 也可以进入虚拟环境，通过 pip list 查看已安装的模块
C:\Users\XiSun\NewVolume-D\Codes\python\poetry-demo>poetry shell
(poetry-demo-py3.10) C:\Users\XiSun\NewVolume-D\Codes\python\poetry-demo>pip list
Package      Version
------------ -------
blinker      1.9.0
click        8.1.7
colorama     0.4.6
Flask        3.1.0
itsdangerous 2.2.0
Jinja2       3.1.4
MarkupSafe   3.0.2
pip          24.3.1
setuptools   75.6.0
Werkzeug     3.1.3

# 以树状形式展示模块
C:\Users\XiSun\NewVolume-D\Codes\python\poetry-demo>poetry show --tree
black 24.10.0 The uncompromising code formatter.
├── click >=8.0.0
│   └── colorama *
├── mypy-extensions >=0.4.3
├── packaging >=22.0
├── pathspec >=0.9.0
├── platformdirs >=2
├── tomli >=1.1.0
└── typing-extensions >=4.0.1
flask 3.1.0 A simple framework for building complex web applications.
├── blinker >=1.9
├── click >=8.1.3
│   └── colorama *
├── itsdangerous >=2.2
├── jinja2 >=3.1.2
│   └── markupsafe >=2.0
└── werkzeug >=3.1
    └── markupsafe >=2.1.1
    
# 只展示特定模块的依赖层级
C:\Users\XiSun\NewVolume-D\Codes\python\poetry-demo>poetry show click --tree
click 8.1.7 Composable command line interface toolkit
└── colorama *
```

**卸载模块：**`poetry remove <module name>`。

```cmd
C:\Users\XiSun\NewVolume-D\Codes\python\poetry-demo>poetry remove flask
Updating dependencies
Resolving dependencies... (0.1s)

Package operations: 0 installs, 0 updates, 8 removals

  - Removing blinker (1.9.0)
  - Removing click (8.1.7)
  - Removing colorama (0.4.6)
  - Removing flask (3.1.0)
  - Removing itsdangerous (2.2.0)
  - Removing jinja2 (3.1.4)
  - Removing markupsafe (3.0.2)
  - Removing werkzeug (3.1.3)

Writing lock file

# 可以看到，poetry remove 会把安装 flask 时涉及到的模块都卸载，这也是 pip 无法做到的
C:\Users\XiSun\NewVolume-D\Codes\python\poetry-demo>poetry show

# 进入虚拟环境后，pip 列表也没有了依赖
C:\Users\XiSun\NewVolume-D\Codes\python\poetry-demo>poetry shell
(poetry-demo-py3.10) C:\Users\XiSun\NewVolume-D\Codes\python\poetry-demo>pip list
Package    Version
---------- -------
pip        24.3.1
setuptools 75.6.0
```

**更新模块：**`poetry update`。

```cmd
# 更新全部模块
C:\Users\XiSun\NewVolume-D\Codes\python\poetry-demo>poetry update
Updating dependencies
Resolving dependencies... (0.8s)

No dependencies to install or update

# 更新特定模块
C:\Users\XiSun\NewVolume-D\Codes\python\poetry-demo>poetry update click toml
Updating dependencies
Resolving dependencies... (0.2s)

No dependencies to install or update
```

## poetry.lock

`poetry.lock`：相当于 pip 中的 requirements.txt，详细记录了所有安装的模块与版本。

当使用 poetry add 指令时，poetry 会自动依序完成三件事：

1. 更新 pyproject.toml 文件。
2. 根据 pyproject.toml 文件的内容，更新 poetry.lock 文件。
3. 根据 poetry.lock 文件的那日，更新虚拟环境。

注意：虽然 poetry.lock 的内容取决于 pyproject.toml，但二者不会自动关联，是基于特定的指令才会进行同步与更新，例如 poetry add。

如果自行修改了 pyproject.toml 文件的内容，例如变更特定模块的版本，此时，为了让 poetry.lock 文件的内容与 pyproject.toml 文件的内容同步，需要手动执行`poetry lock`指令，重新更新 poetry.lock 文件的内容。但是，更新 poetry.lock 后，不会在虚拟环境中自动安装模块，需要执行`poetry install`安装模块，以此保证 poetry.lock 与虚拟环境一致。

> 总之：
>
> - `poetry lock`：使 poetry.lock 与 pyproject.toml 保持一致。
>   - `--no-update`：主要用于根据项目中已有的 pyproject.toml 文件里定义的依赖及其版本范围等信息，重新生成或更新 poetry.lock 文件，但会跳过更新依赖的版本检查这一环节，也就是不会去尝试获取最新的符合版本范围的依赖版本来更新到 poetry.lock 里。它旨在基于当前已确定的依赖状态 "原样" 锁定依赖版本，确保在后续安装等操作时使用的是当前已指定好的版本组合，维持项目依赖环境的稳定性和一致性。（**poetry lock 命令默认会去检查更新依赖版本**）
> - `poetry install`：使虚拟环境与 poetry.lock 保持一致。

## requirements.txt

使用`poetry export`命令，可以将安装的模块，生成 requirements.txt 文件：

```cmd
C:\Users\XiSun\NewVolume-D\Codes\python\poetry-demo>poetry export -f requirements.txt -o requirements.txt --without-hash
es
Warning: poetry-plugin-export will not be installed by default in a future version of Poetry.
In order to avoid a breaking change and make your automation forward-compatible, please install poetry-plugin-export explicitly. See https://python-poetry.org/docs/plugins/#using-plugins for details on how to install a plugin.
To disable this warning run 'poetry config warnings.export false'.
```

生成的 requirements.txt 文件内容：

```tex
blinker==1.9.0 ; python_version >= "3.10" and python_version < "4.0"
click==8.1.7 ; python_version >= "3.10" and python_version < "4.0"
colorama==0.4.6 ; python_version >= "3.10" and python_version < "4.0" and platform_system == "Windows"
flask==3.1.0 ; python_version >= "3.10" and python_version < "4.0"
itsdangerous==2.2.0 ; python_version >= "3.10" and python_version < "4.0"
jinja2==3.1.4 ; python_version >= "3.10" and python_version < "4.0"
markupsafe==3.0.2 ; python_version >= "3.10" and python_version < "4.0"
werkzeug==3.1.3 ; python_version >= "3.10" and python_version < "4.0"
```

注意：poetry export 只会将 pyproject.toml 文件中的`[tool.poetry.dependencies]`区块的模块输出，如果需要将`[tool.poetry.group.dev.dependencies]`区块的模块输出，使用以下命令。

```cmd
C:\Users\XiSun\NewVolume-D\Codes\python\poetry-demo>poetry export -f requirements.txt -o requirements.txt --without-hashes --with dev
Warning: poetry-plugin-export will not be installed by default in a future version of Poetry.
In order to avoid a breaking change and make your automation forward-compatible, please install poetry-plugin-export explicitly. See https://python-poetry.org/docs/plugins/#using-plugins for details on how to install a plugin.
To disable this warning run 'poetry config warnings.export false'.
```

生成的 requirements.txt 文件内容：

```tex
black==24.10.0 ; python_version >= "3.10" and python_version < "4.0" # 输出的模块，包含了 black
blinker==1.9.0 ; python_version >= "3.10" and python_version < "4.0"
click==8.1.7 ; python_version >= "3.10" and python_version < "4.0"
colorama==0.4.6 ; python_version >= "3.10" and python_version < "4.0" and platform_system == "Windows"
flask==3.1.0 ; python_version >= "3.10" and python_version < "4.0"
itsdangerous==2.2.0 ; python_version >= "3.10" and python_version < "4.0"
jinja2==3.1.4 ; python_version >= "3.10" and python_version < "4.0"
markupsafe==3.0.2 ; python_version >= "3.10" and python_version < "4.0"
mypy-extensions==1.0.0 ; python_version >= "3.10" and python_version < "4.0"
packaging==24.2 ; python_version >= "3.10" and python_version < "4.0"
pathspec==0.12.1 ; python_version >= "3.10" and python_version < "4.0"
platformdirs==4.3.6 ; python_version >= "3.10" and python_version < "4.0"
tomli==2.2.1 ; python_version >= "3.10" and python_version < "3.11"
typing-extensions==4.12.2 ; python_version >= "3.10" and python_version < "3.11"
werkzeug==3.1.3 ; python_version >= "3.10" and python_version < "4.0"
```

## Reference

https://blog.kyomind.tw/python-poetry/
