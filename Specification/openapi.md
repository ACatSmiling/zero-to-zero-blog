> *`Author: ACatSmiling`*
>
> *`Since: 2024-12-14`*

**`OpenAPI 规范`**：OpenAPI Specification，简称 OAS，原称 **Swagger 规范**，是种机器可读的接口描述语言规范，作用是描述、生成、使用和可视化 Web 服务。其前身是 Swagger 框架的一部分，2015年成为独立项目，由 Linux 基金会的开源合作项目 OpenAPI Initiative 监督。目前最新的 OpenAPI 规范版本是 3.1.0，该版本于 2021 年 2 月 15 日发布。

3.1.0 版本官网：https://swagger.io/specification/

3.0.0 版本中文翻译参考：https://openapi.apifox.cn/

**OpenAPI 文档支持 JSON 和 YAML 两种格式，推荐将 OpenAPI 文档命名为 openapi.json 或 openapi.yaml。**

## OpenAPI Object

`OpenAPI Object`：是 [OpenAPI document](https://openapi.apifox.cn/#oasDocument) 的**根文档对象**。包含以下字段：

| Field Name        |                             Type                             | Description                                                  |
| ----------------- | :----------------------------------------------------------: | ------------------------------------------------------------ |
| openapi           |                           `string`                           | **REQUIRED**. This string MUST be the [version number](https://swagger.io/specification/#versions) of the OpenAPI Specification that the OpenAPI document uses. The `openapi` field SHOULD be used by tooling to interpret the OpenAPI document. This is *not* related to the API [`info.version`](https://swagger.io/specification/#info-version) string. |
| info              | [Info Object](https://swagger.io/specification/#info-object) | **REQUIRED**. Provides metadata about the API. The metadata MAY be used by tooling as required. |
| jsonSchemaDialect |                           `string`                           | The default value for the `$schema` keyword within [Schema Objects](https://swagger.io/specification/#schema-object) contained within this OAS document. This MUST be in the form of a URI. |
| servers           | [Server Object](https://swagger.io/specification/#server-object) | An array of Server Objects, which provide connectivity information to a target server. If the `servers` property is not provided, or is an empty array, the default value would be a [Server Object](https://swagger.io/specification/#server-object) with a [url](https://swagger.io/specification/#server-url) value of `/`. |
| paths             | [Paths Object](https://swagger.io/specification/#paths-object) | The available paths and operations for the API.              |
| webhooks          | Map[`string`, [Path Item Object](https://swagger.io/specification/#path-item-object) \| [Reference Object](https://swagger.io/specification/#reference-object)] | The incoming webhooks that MAY be received as part of this API and that the API consumer MAY choose to implement. Closely related to the `callbacks` feature, this section describes requests initiated other than by an API call, for example by an out of band registration. The key name is a unique string to refer to each webhook, while the (optionally referenced) Path Item Object describes a request that may be initiated by the API provider and the expected responses. An [example](https://github.com/-o-a-i/-open-a-p-i--specification/blob/main/examples/v3.1/webhook-example.yaml) is available. |
| components        | [Components Object](https://swagger.io/specification/#components-object) | An element to hold various schemas for the document.         |
| security          | [Security Requirement Object](https://swagger.io/specification/#security-requirement-object) | A declaration of which security mechanisms can be used across the API. The list of values includes alternative security requirement objects that can be used. Only one of the security requirement objects need to be satisfied to authorize a request. Individual operations can override this definition. To make security optional, an empty security requirement (`{}`) can be included in the array. |
| tags              |  [Tag Object](https://swagger.io/specification/#tag-object)  | A list of tags used by the document with additional metadata. The order of the tags can be used to reflect on their order by the parsing tools. Not all tags that are used by the [Operation Object](https://swagger.io/specification/#operation-object) must be declared. The tags that are not declared MAY be organized randomly or based on the tools' logic. Each tag name in the list MUST be unique. |
| externalDocs      | [External Documentation Object](https://swagger.io/specification/#external-documentation-object) | Additional external documentation.                           |

## Info Object

`Info Object`：提供了 API 的**元数据信息**。包含以下字段：

| Field Name     |                             Type                             | Description                                                  |
| -------------- | :----------------------------------------------------------: | ------------------------------------------------------------ |
| title          |                           `string`                           | **REQUIRED**. The title of the API.                          |
| summary        |                           `string`                           | A short summary of the API.                                  |
| description    |                           `string`                           | A description of the API. [CommonMark syntax](https://spec.commonmark.org/) MAY be used for rich text representation. |
| termsOfService |                           `string`                           | A URL to the Terms of Service for the API. This MUST be in the form of a URL. |
| contact        | [Contact Object](https://swagger.io/specification/#contact-object) | The contact information for the exposed API.                 |
| license        | [License Object](https://swagger.io/specification/#license-object) | The license information for the exposed API.                 |
| version        |                           `string`                           | **REQUIRED**. The version of the OpenAPI document (which is distinct from the [OpenAPI Specification version](https://swagger.io/specification/#oas-version) or the API implementation version). |

示例：

- json 格式：

  ```json
  {
    "title": "Sample Pet Store App",
    "summary": "A pet store manager.",
    "description": "This is a sample server for a pet store.",
    "termsOfService": "https://example.com/terms/",
    "contact": {
      "name": "API Support",
      "url": "https://www.example.com/support",
      "email": "support@example.com"
    },
    "license": {
      "name": "Apache 2.0",
      "url": "https://www.apache.org/licenses/LICENSE-2.0.html"
    },
    "version": "1.0.1"
  }
  ```

- yaml 格式：

  ```yaml
  title: Sample Pet Store App
  summary: A pet store manager.
  description: This is a sample server for a pet store.
  termsOfService: https://example.com/terms/
  contact:
    name: API Support
    url: https://www.example.com/support
    email: support@example.com
  license:
    name: Apache 2.0
    url: https://www.apache.org/licenses/LICENSE-2.0.html
  version: 1.0.1
  ```
  

### Contact Object

`Contact Object`：所公开的 API 的**联系人信息**。包含以下字段：

| Field Name |   Type   | Description                                                  |
| ---------- | :------: | ------------------------------------------------------------ |
| name       | `string` | The identifying name of the contact person/organization.     |
| url        | `string` | The URL pointing to the contact information. This MUST be in the form of a URL. |
| email      | `string` | The email address of the contact person/organization. This MUST be in the form of an email address. |

- 这个对象可能会被 [Specification Extensions](https://swagger.io/specification/#specification-extensions) 扩展。

示例：

- json 格式：

  ```json
  {
    "name": "API Support",
    "url": "https://www.example.com/support",
    "email": "support@example.com"
  }
  ```

- yaml 格式：

  ```yaml
  name: API Support
  url: https://www.example.com/support
  email: support@example.com
  ```

### License Object

`License Object`：所公开 API 的**许可证信息**。包含以下字段：

| Field Name |   Type   | Description                                                  |
| ---------- | :------: | ------------------------------------------------------------ |
| name       | `string` | **REQUIRED**. The license name used for the API.             |
| identifier | `string` | An [SPDX](https://spdx.org/licenses/) license expression for the API. The `identifier` field is mutually exclusive of the `url` field. |
| url        | `string` | A URL to the license used for the API. This MUST be in the form of a URL. The `url` field is mutually exclusive of the `identifier` field. |

- 这个对象可能会被 [Specification Extensions](https://swagger.io/specification/#specification-extensions) 扩展。

示例：

- json 格式：

  ```json
  {
    "name": "Apache 2.0",
    "identifier": "Apache-2.0"
  }
  ```

- yaml 格式：

  ```yaml
  name: Apache 2.0
  identifier: Apache-2.0
  ```

## Server Object

`Server Object`：表示一个**服务器的对象**。包含以下字段：

| Field Name  |                             Type                             | Description                                                  |
| ----------- | :----------------------------------------------------------: | ------------------------------------------------------------ |
| url         |                           `string`                           | **REQUIRED**. A URL to the target host. This URL supports Server Variables and MAY be relative, to indicate that the host location is relative to the location where the OpenAPI document is being served. Variable substitutions will be made when a variable is named in `{`brackets`}`. |
| description |                           `string`                           | An optional string describing the host designated by the URL. [CommonMark syntax](https://spec.commonmark.org/) MAY be used for rich text representation. |
| variables   | Map[`string`, [Server Variable Object](https://swagger.io/specification/#server-variable-object)] | A map between a variable name and its value. The value is used for substitution in the server's URL template. |

- 这个对象可能会被 [Specification Extensions](https://swagger.io/specification/#specification-extensions) 扩展。

示例：

- json 格式：

  ```json
  {
    "url": "https://development.gigantic-server.com/v1",
    "description": "Development server"
  }
  ```

- yaml 格式：

  ```yaml
  url: https://development.gigantic-server.com/v1
  description: Development server
  ```

以下示例，表示的是有**多个服务器**时应该如何描述，比如 OpenAPI 对象的 [servers](https://swagger.io/specification/#oas-servers)：

- json 格式：

  ```json
  {
    "servers": [
      {
        "url": "https://development.gigantic-server.com/v1",
        "description": "Development server"
      },
      {
        "url": "https://staging.gigantic-server.com/v1",
        "description": "Staging server"
      },
      {
        "url": "https://api.gigantic-server.com/v1",
        "description": "Production server"
      }
    ]
  }
  ```

- yaml 格式：

  ```yaml
  servers:
  - url: https://development.gigantic-server.com/v1
    description: Development server
  - url: https://staging.gigantic-server.com/v1
    description: Staging server
  - url: https://api.gigantic-server.com/v1
    description: Production server
  ```

以下示例，展示了如何**使用变量配置服务器**：

- json 格式：

  ```json
  {
    "servers": [
      {
        "url": "https://{username}.gigantic-server.com:{port}/{basePath}",
        "description": "The production API server",
        "variables": {
          "username": {
            "default": "demo",
            "description": "this value is assigned by the service provider, in this example `gigantic-server.com`"
          },
          "port": {
            "enum": [
              "8443",
              "443"
            ],
            "default": "8443"
          },
          "basePath": {
            "default": "v2"
          }
        }
      }
    ]
  }
  ```

- yaml 格式：

  ```yaml
  servers:
  - url: https://{username}.gigantic-server.com:{port}/{basePath}
    description: The production API server
    variables:
      username:
        # note! no enum here means it is an open value
        default: demo
        description: this value is assigned by the service provider, in this example `gigantic-server.com`
      port:
        enum:
          - '8443'
          - '443'
        default: '8443'
      basePath:
        # open meaning there is the opportunity to use special base paths as assigned by the provider, default is `v2`
        default: v2
  ```

### Server Variable Object

`Server Variable Object`：代表服务器变量的对象，用于服务器 URL 模板替换。包含以下字段：

| Field Name  |    Type    | Description                                                  |
| ----------- | :--------: | ------------------------------------------------------------ |
| enum        | [`string`] | An enumeration of string values to be used if the substitution options are from a limited set. The array MUST NOT be empty. |
| default     |  `string`  | **REQUIRED**. The default value to use for substitution, which SHALL be sent if an alternate value is *not* supplied. Note this behavior is different than the [Schema Object's](https://swagger.io/specification/#schema-object) treatment of default values, because in those cases parameter values are optional. If the [`enum`](https://swagger.io/specification/#server-variable-enum) is defined, the value MUST exist in the enum's values. |
| description |  `string`  | An optional description for the server variable. [CommonMark syntax](https://spec.commonmark.org/) MAY be used for rich text representation. |

- 这个对象可能会被 [Specification Extensions](https://swagger.io/specification/#specification-extensions) 扩展。

## Paths Object



## Path Item Object



## Reference Object



## Components Object

`Components Object`：包含 OpenAPI 规范固定的各种可重用组件，当没有被其他对象引用时，在这里定义定义的组件不会产生任何效果。包含以下字段：

| Field Name      | Type                                                         | Description                                                  |
| --------------- | :----------------------------------------------------------- | ------------------------------------------------------------ |
| schemas         | Map[`string`, [Schema Object](https://swagger.io/specification/#schema-object)] | An object to hold reusable [Schema Objects](https://swagger.io/specification/#schema-object). |
| responses       | Map[`string`, [Response Object](https://swagger.io/specification/#response-object) \| [Reference Object](https://swagger.io/specification/#reference-object)] | An object to hold reusable [Response Objects](https://swagger.io/specification/#response-object). |
| parameters      | Map[`string`, [Parameter Object](https://swagger.io/specification/#parameter-object) \| [Reference Object](https://swagger.io/specification/#reference-object)] | An object to hold reusable [Parameter Objects](https://swagger.io/specification/#parameter-object). |
| examples        | Map[`string`, [Example Object](https://swagger.io/specification/#example-object) \| [Reference Object](https://swagger.io/specification/#reference-object)] | An object to hold reusable [Example Objects](https://swagger.io/specification/#example-object). |
| requestBodies   | Map[`string`, [Request Body Object](https://swagger.io/specification/#request-body-object) \| [Reference Object](https://swagger.io/specification/#reference-object)] | An object to hold reusable [Request Body Objects](https://swagger.io/specification/#request-body-object). |
| headers         | Map[`string`, [Header Object](https://swagger.io/specification/#header-object) \| [Reference Object](https://swagger.io/specification/#reference-object)] | An object to hold reusable [Header Objects](https://swagger.io/specification/#header-object). |
| securitySchemes | Map[`string`, [Security Scheme Object](https://swagger.io/specification/#security-scheme-object) \| [Reference Object](https://swagger.io/specification/#reference-object)] | An object to hold reusable [Security Scheme Objects](https://swagger.io/specification/#security-scheme-object). |
| links           | Map[`string`, [Link Object](https://swagger.io/specification/#link-object) \| [Reference Object](https://swagger.io/specification/#reference-object)] | An object to hold reusable [Link Objects](https://swagger.io/specification/#link-object). |
| callbacks       | Map[`string`, [Callback Object](https://swagger.io/specification/#callback-object) \| [Reference Object](https://swagger.io/specification/#reference-object)] | An object to hold reusable [Callback Objects](https://swagger.io/specification/#callback-object). |
| pathItems       | Map[`string`, [Path Item Object](https://swagger.io/specification/#path-item-object) \| [Reference Object](https://swagger.io/specification/#reference-object)] | An object to hold reusable [Path Item Object](https://swagger.io/specification/#path-item-object). |

- 这个对象可能会被 [Specification Extensions](https://swagger.io/specification/#specification-extensions) 扩展。

- 上面定义的所有字段的值都是对象，对象包含的 key 的命名必须满足正则表达式： `^[a-zA-Z0-9\.\-_]+$`。示例如下：

  ```tex
  User
  User_1
  User_Name
  user-name
  my.org.User
  ```

示例：

- json 格式：

  ```json
  "components": {
    "schemas": {
      "GeneralError": {
        "type": "object",
        "properties": {
          "code": {
            "type": "integer",
            "format": "int32"
          },
          "message": {
            "type": "string"
          }
        }
      },
      "Category": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int64"
          },
          "name": {
            "type": "string"
          }
        }
      },
      "Tag": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer",
            "format": "int64"
          },
          "name": {
            "type": "string"
          }
        }
      }
    },
    "parameters": {
      "skipParam": {
        "name": "skip",
        "in": "query",
        "description": "number of items to skip",
        "required": true,
        "schema": {
          "type": "integer",
          "format": "int32"
        }
      },
      "limitParam": {
        "name": "limit",
        "in": "query",
        "description": "max records to return",
        "required": true,
        "schema" : {
          "type": "integer",
          "format": "int32"
        }
      }
    },
    "responses": {
      "NotFound": {
        "description": "Entity not found."
      },
      "IllegalInput": {
        "description": "Illegal input for operation."
      },
      "GeneralError": {
        "description": "General Error",
        "content": {
          "application/json": {
            "schema": {
              "$ref": "#/components/schemas/GeneralError"
            }
          }
        }
      }
    },
    "securitySchemes": {
      "api_key": {
        "type": "apiKey",
        "name": "api_key",
        "in": "header"
      },
      "petstore_auth": {
        "type": "oauth2",
        "flows": {
          "implicit": {
            "authorizationUrl": "https://example.org/api/oauth/dialog",
            "scopes": {
              "write:pets": "modify pets in your account",
              "read:pets": "read your pets"
            }
          }
        }
      }
    }
  }
  ```

- yaml 格式：

  ```yaml
  components:
    schemas:
      GeneralError:
        type: object
        properties:
          code:
            type: integer
            format: int32
          message:
            type: string
      Category:
        type: object
        properties:
          id:
            type: integer
            format: int64
          name:
            type: string
      Tag:
        type: object
        properties:
          id:
            type: integer
            format: int64
          name:
            type: string
    parameters:
      skipParam:
        name: skip
        in: query
        description: number of items to skip
        required: true
        schema:
          type: integer
          format: int32
      limitParam:
        name: limit
        in: query
        description: max records to return
        required: true
        schema:
          type: integer
          format: int32
    responses:
      NotFound:
        description: Entity not found.
      IllegalInput:
        description: Illegal input for operation.
      GeneralError:
        description: General Error
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/GeneralError'
    securitySchemes:
      api_key:
        type: apiKey
        name: api_key
        in: header
      petstore_auth:
        type: oauth2
        flows:
          implicit:
            authorizationUrl: https://example.org/api/oauth/dialog
            scopes:
              write:pets: modify pets in your account
              read:pets: read your pets
  ```

### Schema Object



## Security Requirement Object



## Tag Object



## External Documentation Object

