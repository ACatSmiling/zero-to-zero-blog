> *`Author: ACatSmiling`*
>
> *`Since: 2024-12-14`*

## 概述

**`OpenAPI 规范`**：OpenAPI Specification，简称 OAS，原称 **Swagger 规范**，是种机器可读的接口描述语言规范，作用是描述、生成、使用和可视化 Web 服务。其前身是 Swagger 框架的一部分，2015年成为独立项目，由 Linux 基金会的开源合作项目 OpenAPI Initiative 监督。目前最新的 OpenAPI 规范版本是 3.1.0，该版本于 2021 年 2 月 15 日发布。

3.1.0 版本官网：https://swagger.io/specification/

3.0.0 版本中文翻译参考：https://openapi.apifox.cn/

在线编辑：https://editor.swagger.io/

**OpenAPI 文档支持 JSON 和 YAML 两种格式，推荐将 OpenAPI 文档命名为 openapi.json 或 openapi.yaml。**

## 框架

### OpenAPI Object

`OpenAPI Object`：是 [OpenAPI document](https://openapi.apifox.cn/#oasDocument) 的**根文档对象**。包含以下字段：

| Field Name        |                             Type                             | Description                                                  |
| ----------------- | :----------------------------------------------------------: | ------------------------------------------------------------ |
| openapi           |                           `string`                           | **`REQUIRED`**. This string MUST be the [version number](https://swagger.io/specification/#versions) of the OpenAPI Specification that the OpenAPI document uses. The `openapi` field SHOULD be used by tooling to interpret the OpenAPI document. This is *not* related to the API [`info.version`](https://swagger.io/specification/#info-version) string. |
| info              | [Info Object](https://swagger.io/specification/#info-object) | **`REQUIRED`**. Provides metadata about the API. The metadata MAY be used by tooling as required. |
| jsonSchemaDialect |                           `string`                           | The default value for the `$schema` keyword within [Schema Objects](https://swagger.io/specification/#schema-object) contained within this OAS document. This MUST be in the form of a URI. |
| servers           | [Server Object](https://swagger.io/specification/#server-object) | An array of Server Objects, which provide connectivity information to a target server. If the `servers` property is not provided, or is an empty array, the default value would be a [Server Object](https://swagger.io/specification/#server-object) with a [url](https://swagger.io/specification/#server-url) value of `/`. |
| paths             | [Paths Object](https://swagger.io/specification/#paths-object) | The available paths and operations for the API.              |
| webhooks          | Map[`string`, [Path Item Object](https://swagger.io/specification/#path-item-object) \| [Reference Object](https://swagger.io/specification/#reference-object)] | The incoming webhooks that MAY be received as part of this API and that the API consumer MAY choose to implement. Closely related to the `callbacks` feature, this section describes requests initiated other than by an API call, for example by an out of band registration. The key name is a unique string to refer to each webhook, while the (optionally referenced) Path Item Object describes a request that may be initiated by the API provider and the expected responses. An [example](https://github.com/-o-a-i/-open-a-p-i--specification/blob/main/examples/v3.1/webhook-example.yaml) is available. |
| components        | [Components Object](https://swagger.io/specification/#components-object) | An element to hold various schemas for the document.         |
| security          | [Security Requirement Object](https://swagger.io/specification/#security-requirement-object) | A declaration of which security mechanisms can be used across the API. The list of values includes alternative security requirement objects that can be used. Only one of the security requirement objects need to be satisfied to authorize a request. Individual operations can override this definition. To make security optional, an empty security requirement (`{}`) can be included in the array. |
| tags              |  [Tag Object](https://swagger.io/specification/#tag-object)  | A list of tags used by the document with additional metadata. The order of the tags can be used to reflect on their order by the parsing tools. Not all tags that are used by the [Operation Object](https://swagger.io/specification/#operation-object) must be declared. The tags that are not declared MAY be organized randomly or based on the tools' logic. Each tag name in the list MUST be unique. |
| externalDocs      | [External Documentation Object](https://swagger.io/specification/#external-documentation-object) | Additional external documentation.                           |

## 脉络

### Info Object

`Info Object`：提供了 OpenAPI 的**元数据信息**。包含以下字段：

| Field Name     |                             Type                             | Description                                                  |
| -------------- | :----------------------------------------------------------: | ------------------------------------------------------------ |
| title          |                           `string`                           | **`REQUIRED`**. The title of the API.                        |
| summary        |                           `string`                           | A short summary of the API.                                  |
| description    |                           `string`                           | A description of the API. [CommonMark syntax](https://spec.commonmark.org/) MAY be used for rich text representation. |
| termsOfService |                           `string`                           | A URL to the Terms of Service for the API. This MUST be in the form of a URL. |
| contact        | [Contact Object](https://swagger.io/specification/#contact-object) | The contact information for the exposed API.                 |
| license        | [License Object](https://swagger.io/specification/#license-object) | The license information for the exposed API.                 |
| version        |                           `string`                           | **`REQUIRED`**. The version of the OpenAPI document (which is distinct from the [OpenAPI Specification version](https://swagger.io/specification/#oas-version) or the API implementation version). |

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
  

### Server Object

`Server Object`：表示一个**服务器的对象**。包含以下字段：

| Field Name  |                             Type                             | Description                                                  |
| ----------- | :----------------------------------------------------------: | ------------------------------------------------------------ |
| url         |                           `string`                           | **`REQUIRED`**. A URL to the target host. This URL supports Server Variables and MAY be relative, to indicate that the host location is relative to the location where the OpenAPI document is being served. Variable substitutions will be made when a variable is named in `{`brackets`}`. |
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

### Paths Object

`Paths Object`：定义**各个端点及其操作的相对路径**。此处指定的路径会和 [Server Object](https://swagger.io/specification/#server-object) 内指定的 URL 地址组成完整的URL地址，路径可以为空，这依赖于 [Access Control List (ACL) constraints](https://swagger.io/specification/#security-filtering) 的设置。包含以下字段：

| Field Pattern |                             Type                             | Description                                                  |
| ------------- | :----------------------------------------------------------: | ------------------------------------------------------------ |
| /{path}       | [Path Item Object](https://swagger.io/specification/#path-item-object) | A relative path to an individual endpoint. The field name MUST begin with a forward slash (`/`). The path is **appended** (no relative URL resolution) to the expanded URL from the [Server Object](https://swagger.io/specification/#server-object)'s `url` field in order to construct the full URL. [Path templating](https://swagger.io/specification/#path-templating) is allowed. When matching URLs, concrete (non-templated) paths would be matched before their templated counterparts. Templated paths with the same hierarchy but different templated names MUST NOT exist as they are identical. In case of ambiguous matching, it's up to the tooling to decide which one to use. |

- 这个对象可能会被 [Specification Extensions](https://swagger.io/specification/#specification-extensions) 扩展。

示例：

- json 格式：

  ```json
  {
    "/pets": {
      "get": {
        "description": "Returns all pets from the system that the user has access to",
        "responses": {
          "200": {
            "description": "A list of pets.",
            "content": {
              "application/json": {
                "schema": {
                  "type": "array",
                  "items": {
                    "$ref": "#/components/schemas/pet"
                  }
                }
              }
            }
          }
        }
      }
    }
  }
  ```

- yaml 格式：

  ```yaml
  /pets:
    get:
      description: Returns all pets from the system that the user has access to
      responses:
        '200':
          description: A list of pets.
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/pet'
  ```

#### Path Templating Matching

假设有以下路径，明确定义的路径 "/pets/mine" 会被优先匹配：

```tex
/pets/{petId}
/pets/mine
```

以下路径被认为是等价且无效的：

```tex
/pets/{petId}
/pets/{name}
```

以下路径会产生歧义：

```tex
/{entity}/me
/books/{id}
```

### Path Item Object

`Path Item Object`：描述**对一个路径可执行的有效操作**。依赖于 [ACL constraints](https://swagger.io/specification/#security-filtering) 的设置，一个 Path Item 可能是一个空对象，文档的读者仍然可以看到这个路径，但是他们无法了解到对这个路径可用的任何操作和参数。包含以下字段：

| Field Name  |                             Type                             | Description                                                  |
| ----------- | :----------------------------------------------------------: | ------------------------------------------------------------ |
| $ref        |                           `string`                           | Allows for a referenced definition of this path item. The value MUST be in the form of a URI, and the referenced structure MUST be in the form of a [Path Item Object](https://swagger.io/specification/#path-item-object). In case a Path Item Object field appears both in the defined object and the referenced object, the behavior is undefined. See the rules for resolving [Relative References](https://swagger.io/specification/#relative-references-in-api-description-uris).  ***Note:** The behavior of `$ref` with adjacent properties is likely to change in future versions of this specification to bring it into closer alignment with the behavior of the [Reference Object](https://swagger.io/specification/#reference-object).* |
| summary     |                           `string`                           | An optional string summary, intended to apply to all operations in this path. |
| description |                           `string`                           | An optional string description, intended to apply to all operations in this path. [CommonMark syntax](https://spec.commonmark.org/) MAY be used for rich text representation. |
| get         | [Operation Object](https://swagger.io/specification/#operation-object) | A definition of a GET operation on this path.                |
| put         | [Operation Object](https://swagger.io/specification/#operation-object) | A definition of a PUT operation on this path.                |
| post        | [Operation Object](https://swagger.io/specification/#operation-object) | A definition of a POST operation on this path.               |
| delete      | [Operation Object](https://swagger.io/specification/#operation-object) | A definition of a DELETE operation on this path.             |
| options     | [Operation Object](https://swagger.io/specification/#operation-object) | A definition of a OPTIONS operation on this path.            |
| head        | [Operation Object](https://swagger.io/specification/#operation-object) | A definition of a HEAD operation on this path.               |
| patch       | [Operation Object](https://swagger.io/specification/#operation-object) | A definition of a PATCH operation on this path.              |
| trace       | [Operation Object](https://swagger.io/specification/#operation-object) | A definition of a TRACE operation on this path.              |
| servers     | [[Server Object](https://swagger.io/specification/#server-object)] | An alternative `servers` array to service all operations in this path. If a `servers` array is specified at the [OpenAPI Object](https://swagger.io/specification/#oas-servers) level, it will be overridden by this value. |
| parameters  | [[Parameter Object](https://swagger.io/specification/#parameter-object) \| [Reference Object](https://swagger.io/specification/#reference-object)] | A list of parameters that are applicable for all the operations described under this path. These parameters can be overridden at the operation level, but cannot be removed there. The list MUST NOT include duplicated parameters. A unique parameter is defined by a combination of a [name](https://swagger.io/specification/#parameter-name) and [location](https://swagger.io/specification/#parameter-in). The list can use the [Reference Object](https://swagger.io/specification/#reference-object) to link to parameters that are defined in the [OpenAPI Object's `components.parameters`](https://swagger.io/specification/#components-parameters). |

- 这个对象可能会被 [Specification Extensions](https://swagger.io/specification/#specification-extensions) 扩展。

示例：

- json 格式：

  ```json
  {
    "get": {
      "description": "Returns pets based on ID",
      "summary": "Find pets by ID",
      "operationId": "getPetsById",
      "responses": {
        "200": {
          "description": "pet response",
          "content": {
            "*/*": {
              "schema": {
                "type": "array",
                "items": {
                  "$ref": "#/components/schemas/Pet"
                }
              }
            }
          }
        },
        "default": {
          "description": "error payload",
          "content": {
            "text/html": {
              "schema": {
                "$ref": "#/components/schemas/ErrorModel"
              }
            }
          }
        }
      }
    },
    "parameters": [
      {
        "name": "id",
        "in": "path",
        "description": "ID of pet to use",
        "required": true,
        "schema": {
          "type": "array",
          "items": {
            "type": "string"
          }
        },
        "style": "simple"
      }
    ]
  }
  ```

- yaml 格式：

  ```yaml
  get:
    description: Returns pets based on ID
    summary: Find pets by ID
    operationId: getPetsById
    responses:
      '200':
        description: pet response
        content:
          '*/*':
            schema:
              type: array
              items:
                $ref: '#/components/schemas/Pet'
      default:
        description: error payload
        content:
          text/html:
            schema:
              $ref: '#/components/schemas/ErrorModel'
  parameters:
    - name: id
      in: path
      description: ID of pet to use
      required: true
      schema:
        type: array
        items:
          type: string
      style: simple
  ```

### Components Object

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

### Security Requirement Object

`略，详见官网`。

### Tag Object

`略，详见官网`。

### Reference Object

`略，详见官网`。

## 组成

### Contact Object

`Contact Object`：OpenAPI 的**联系人信息**。包含以下字段：

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

`License Object`：OpenAPI 的**许可证信息**。包含以下字段：

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

### Server Variable Object

`Server Variable Object`：代表服务器变量的对象，**用于服务器 URL 模板替换**。包含以下字段：

| Field Name  |    Type    | Description                                                  |
| ----------- | :--------: | ------------------------------------------------------------ |
| enum        | [`string`] | An enumeration of string values to be used if the substitution options are from a limited set. The array MUST NOT be empty. |
| default     |  `string`  | **REQUIRED**. The default value to use for substitution, which SHALL be sent if an alternate value is *not* supplied. Note this behavior is different than the [Schema Object's](https://swagger.io/specification/#schema-object) treatment of default values, because in those cases parameter values are optional. If the [`enum`](https://swagger.io/specification/#server-variable-enum) is defined, the value MUST exist in the enum's values. |
| description |  `string`  | An optional description for the server variable. [CommonMark syntax](https://spec.commonmark.org/) MAY be used for rich text representation. |

- 这个对象可能会被 [Specification Extensions](https://swagger.io/specification/#specification-extensions) 扩展。

### Operation Object

`Operation Object`：描述对路径的某个操作。包含以下字段：

| Field Name   |                             Type                             | Description                                                  |
| ------------ | :----------------------------------------------------------: | ------------------------------------------------------------ |
| tags         |                          [`string`]                          | A list of tags for API documentation control. Tags can be used for logical grouping of operations by resources or any other qualifier. |
| summary      |                           `string`                           | A short summary of what the operation does.                  |
| description  |                           `string`                           | A verbose explanation of the operation behavior. [CommonMark syntax](https://spec.commonmark.org/) MAY be used for rich text representation. |
| externalDocs | [External Documentation Object](https://swagger.io/specification/#external-documentation-object) | Additional external documentation for this operation.        |
| operationId  |                           `string`                           | Unique string used to identify the operation. The id MUST be unique among all operations described in the API. The operationId value is **case-sensitive**. Tools and libraries MAY use the operationId to uniquely identify an operation, therefore, it is RECOMMENDED to follow common programming naming conventions. |
| parameters   | [[Parameter Object](https://swagger.io/specification/#parameter-object) \| [Reference Object](https://swagger.io/specification/#reference-object)] | A list of parameters that are applicable for this operation. If a parameter is already defined at the [Path Item](https://swagger.io/specification/#path-item-parameters), the new definition will override it but can never remove it. The list MUST NOT include duplicated parameters. A unique parameter is defined by a combination of a [name](https://swagger.io/specification/#parameter-name) and [location](https://swagger.io/specification/#parameter-in). The list can use the [Reference Object](https://swagger.io/specification/#reference-object) to link to parameters that are defined in the [OpenAPI Object's `components.parameters`](https://swagger.io/specification/#components-parameters). |
| requestBody  | [Request Body Object](https://swagger.io/specification/#request-body-object) \| [Reference Object](https://swagger.io/specification/#reference-object) | The request body applicable for this operation. The `requestBody` is fully supported in HTTP methods where the HTTP 1.1 specification [RFC7231](https://tools.ietf.org/html/rfc7231#section-4.3.1) has explicitly defined semantics for request bodies. In other cases where the HTTP spec is vague (such as [GET](https://tools.ietf.org/html/rfc7231#section-4.3.1), [HEAD](https://tools.ietf.org/html/rfc7231#section-4.3.2) and [DELETE](https://tools.ietf.org/html/rfc7231#section-4.3.5)), `requestBody` is permitted but does not have well-defined semantics and SHOULD be avoided if possible. |
| responses    | [Responses Object](https://swagger.io/specification/#responses-object) | The list of possible responses as they are returned from executing this operation. |
| callbacks    | Map[`string`, [Callback Object](https://swagger.io/specification/#callback-object) \| [Reference Object](https://swagger.io/specification/#reference-object)] | A map of possible out-of band callbacks related to the parent operation. The key is a unique identifier for the Callback Object. Each value in the map is a [Callback Object](https://swagger.io/specification/#callback-object) that describes a request that may be initiated by the API provider and the expected responses. |
| deprecated   |                          `boolean`                           | Declares this operation to be deprecated. Consumers SHOULD refrain from usage of the declared operation. Default value is `false`. |
| security     | [[Security Requirement Object](https://swagger.io/specification/#security-requirement-object)] | A declaration of which security mechanisms can be used for this operation. The list of values includes alternative Security Requirement Objects that can be used. Only one of the Security Requirement Objects need to be satisfied to authorize a request. To make security optional, an empty security requirement (`{}`) can be included in the array. This definition overrides any declared top-level [`security`](https://swagger.io/specification/#oas-security). To remove a top-level security declaration, an empty array can be used. |
| servers      | [[Server Object](https://swagger.io/specification/#server-object)] | An alternative `servers` array to service this operation. If a `servers` array is specified at the [Path Item Object](https://swagger.io/specification/#path-item-servers) or [OpenAPI Object](https://swagger.io/specification/#oas-servers) level, it will be overridden by this value. |

- 这个对象可能会被 [Specification Extensions](https://swagger.io/specification/#specification-extensions) 扩展。


示例：

- json 格式：

  ```json
  {
    "tags": ["pet"],
    "summary": "Updates a pet in the store with form data",
    "operationId": "updatePetWithForm",
    "parameters": [
      {
        "name": "petId",
        "in": "path",
        "description": "ID of pet that needs to be updated",
        "required": true,
        "schema": {
          "type": "string"
        }
      }
    ],
    "requestBody": {
      "content": {
        "application/x-www-form-urlencoded": {
          "schema": {
            "type": "object",
            "properties": {
              "name": {
                "description": "Updated name of the pet",
                "type": "string"
              },
              "status": {
                "description": "Updated status of the pet",
                "type": "string"
              }
            },
            "required": ["status"]
          }
        }
      }
    },
    "responses": {
      "200": {
        "description": "Pet updated.",
        "content": {
          "application/json": {},
          "application/xml": {}
        }
      },
      "405": {
        "description": "Method Not Allowed",
        "content": {
          "application/json": {},
          "application/xml": {}
        }
      }
    },
    "security": [
      {
        "petstore_auth": ["write:pets", "read:pets"]
      }
    ]
  }
  ```

- yaml 格式：

  ```yaml
  tags:
    - pet
  summary: Updates a pet in the store with form data
  operationId: updatePetWithForm
  parameters:
    - name: petId
      in: path
      description: ID of pet that needs to be updated
      required: true
      schema:
        type: string
  requestBody:
    content:
      application/x-www-form-urlencoded:
        schema:
          type: object
          properties:
            name:
              description: Updated name of the pet
              type: string
            status:
              description: Updated status of the pet
              type: string
          required:
            - status
  responses:
    '200':
      description: Pet updated.
      content:
        application/json: {}
        application/xml: {}
    '405':
      description: Method Not Allowed
      content:
        application/json: {}
        application/xml: {}
  security:
    - petstore_auth:
        - write:pets
        - read:pets
  ```

### Parameter Object

`Parameter Object`：描述单个操作参数。通过[name](https://swagger.io/specification/#parameter-name) 和 [location](https://swagger.io/specification/#parameter-in) 的组合，唯一定义了一个 parameter。

>有关百分比编码问题的详细研究，包括与 application/x-www-form-urlencoded 查询字符串格式的交互，请参见 [Appendix E](https://swagger.io/specification/#appendix-e-percent-encoding-and-form-media-types)。

#### Parameter Locations

`in`字段指定了四个可能的参数位置：

- `path`：与 [Path Templating](https://swagger.io/specification/#path-templating) 一起使用时，**参数值实际上是操作 URL 的一部分**。 这不包括 API 的主机或基本路径。 例如，在 "/items/{itemId}" 中，path 参数对应的是 itemId。
- `query`：**参数值会追加到 URL 中**。例如，在 "/items?id=####" 中，query 参数对应的是 id。
- `header`：header 参数会作为**请求的自定义标头**。注意，[RFC7230](https://tools.ietf.org/html/rfc7230#section-3.2) 规定在，header 参数不区分大小写。
- `cookie`：用于向 API 传递**特定的 cookie 值**。

#### Fixed Fields

parameter 参数的序列化规则有两种指定方式。Parameter Objects 必须包括`content`字段或`schema`字段，但不能同时包括，有关将各种类型的值转换为字符串的讨论，请参见 [Appendix B](https://swagger.io/specification/#appendix-b-data-type-conversion)。

##### Common Fixed Fields

包含以下字段，这些字段可用于`content`或`schema`：

| Field Name      |   Type    | Description                                                  |
| --------------- | :-------: | ------------------------------------------------------------ |
| name            | `string`  | **`REQUIRED`**. The name of the parameter. Parameter names are *case sensitive*.If [`in`](https://swagger.io/specification/#parameter-in) is `"path"`, the `name` field MUST correspond to a template expression occurring within the [path](https://swagger.io/specification/#paths-path) field in the [Paths Object](https://swagger.io/specification/#paths-object). See [Path Templating](https://swagger.io/specification/#path-templating) for further information.If [`in`](https://swagger.io/specification/#parameter-in) is `"header"` and the `name` field is `"Accept"`, `"Content-Type"` or `"Authorization"`, the parameter definition SHALL be ignored.For all other cases, the `name` corresponds to the parameter name used by the [`in`](https://swagger.io/specification/#parameter-in) field. |
| in              | `string`  | **`REQUIRED`**. The location of the parameter. Possible values are `"query"`, `"header"`, `"path"` or `"cookie"`. |
| description     | `string`  | A brief description of the parameter. This could contain examples of use. [CommonMark syntax](https://spec.commonmark.org/) MAY be used for rich text representation. |
| required        | `boolean` | Determines whether this parameter is mandatory. If the [parameter location](https://swagger.io/specification/#parameter-in) is `"path"`, this field is **REQUIRED** and its value MUST be `true`. Otherwise, the field MAY be included and its default value is `false`. |
| deprecated      | `boolean` | Specifies that a parameter is deprecated and SHOULD be transitioned out of usage. Default value is `false`. |
| allowEmptyValue | `boolean` | If `true`, clients MAY pass a zero-length string value in place of parameters that would otherwise be omitted entirely, which the server SHOULD interpret as the parameter being unused. Default value is `false`. If [`style`](https://swagger.io/specification/#parameter-style) is used, and if [behavior is *n/a* (cannot be serialized)](https://swagger.io/specification/#style-examples), the value of `allowEmptyValue` SHALL be ignored. Interactions between this field and the parameter's [Schema Object](https://swagger.io/specification/#schema-object) are implementation-defined. This field is valid only for `query` parameters. Use of this field is NOT RECOMMENDED, and it is likely to be removed in a later revision. |

- 这个对象可能会被 [Specification Extensions](https://swagger.io/specification/#specification-extensions) 扩展。
- 注意，如果 in 是 header，Cookie 作为 name 并没有被禁止，但这样定义 cookie 参数的效果是未定义的，请使用 in: "cookie" 代替。

##### Fixed Fields for use with schema

对于较简单的情况，[schema](https://swagger.io/specification/#parameter-schema) 和 [style](https://swagger.io/specification/#parameter-style) 可以描述 parameter 参数的结构和语法。当 example 或 examples 与 schema 字段一起提供时，example 应与指定的模式相匹配，并遵循规定的 parameter 参数序列化策略。example 字段和 examples 字段是互斥的，如果其中任何一个字段出现，都应覆盖 schema 中的任何 example。

包含以下字段：

| Field Name    |                             Type                             | Description                                                  |
| ------------- | :----------------------------------------------------------: | ------------------------------------------------------------ |
| style         |                           `string`                           | Describes how the parameter value will be serialized depending on the type of the parameter value. Default values (based on value of `in`): for `"query"` - `"form"`; for `"path"` - `"simple"`; for `"header"` - `"simple"`; for `"cookie"` - `"form"`. |
| explode       |                          `boolean`                           | When this is true, parameter values of type `array` or `object` generate separate parameters for each value of the array or key-value pair of the map. For other types of parameters this field has no effect. When [`style`](https://swagger.io/specification/#parameter-style) is `"form"`, the default value is `true`. For all other styles, the default value is `false`. Note that despite `false` being the default for `deepObject`, the combination of `false` with `deepObject` is undefined. |
| allowReserved |                          `boolean`                           | When this is true, parameter values are serialized using reserved expansion, as defined by [RFC6570](https://datatracker.ietf.org/doc/html/rfc6570#section-3.2.3), which allows [RFC3986's reserved character set](https://datatracker.ietf.org/doc/html/rfc3986#section-2.2), as well as percent-encoded triples, to pass through unchanged, while still percent-encoding all other disallowed characters (including `%` outside of percent-encoded triples). Applications are still responsible for percent-encoding reserved characters that are [not allowed in the query string](https://datatracker.ietf.org/doc/html/rfc3986#section-3.4) (`[`, `]`, `#`), or have a special meaning in `application/x-www-form-urlencoded` (`-`, `&`, `+`); see Appendices [C](https://swagger.io/specification/#appendix-c-using-rfc6570-based-serialization) and [E](https://swagger.io/specification/#appendix-e-percent-encoding-and-form-media-types) for details. This field only applies to parameters with an `in` value of `query`. The default value is `false`. |
| schema        | [Schema Object](https://swagger.io/specification/#schema-object) | The schema defining the type used for the parameter.         |
| example       |                             Any                              | Example of the parameter's potential value; see [Working With Examples](https://swagger.io/specification/#working-with-examples). |
| examples      | Map[ `string`, [Example Object](https://swagger.io/specification/#example-object) \| [Reference Object](https://swagger.io/specification/#reference-object)] | Examples of the parameter's potentia                         |

##### Fixed Fields for use with content

对于更复杂的情况，content 字段可以定义 parameter 参数的 media type 以及 schema。在 in: "header" 和 in: "cookie" 两种模式下，content 推荐使用 text/plain，但 schema 字段不适用。

包含以下字段：

| Field Name |                             Type                             | Description                                                  |
| ---------- | :----------------------------------------------------------: | ------------------------------------------------------------ |
| content    | Map[`string`, [Media Type Object](https://swagger.io/specification/#media-type-object)] | A map containing the representations for the parameter. The key is the media type and the value describes it. The map MUST only contain one entry. |

#### Style Values

为了支持简单参数序列化的常用方法，定义了一组 style 值：

| `style`        | [`type`](https://swagger.io/specification/#data-types) | `in`              | Comments                                                     |
| -------------- | ------------------------------------------------------ | ----------------- | ------------------------------------------------------------ |
| matrix         | `primitive`, `array`, `object`                         | `path`            | Path-style parameters defined by [RFC6570](https://tools.ietf.org/html/rfc6570#section-3.2.7) |
| label          | `primitive`, `array`, `object`                         | `path`            | Label style parameters defined by [RFC6570](https://tools.ietf.org/html/rfc6570#section-3.2.5) |
| simple         | `primitive`, `array`, `object`                         | `path`, `header`  | Simple style parameters defined by [RFC6570](https://tools.ietf.org/html/rfc6570#section-3.2.2). This option replaces `collectionFormat` with a `csv` value from OpenAPI 2.0. |
| form           | `primitive`, `array`, `object`                         | `query`, `cookie` | Form style parameters defined by [RFC6570](https://tools.ietf.org/html/rfc6570#section-3.2.8). This option replaces `collectionFormat` with a `csv` (when `explode` is false) or `multi` (when `explode` is true) value from OpenAPI 2.0. |
| spaceDelimited | `array`, `object`                                      | `query`           | Space separated array values or object properties and values. This option replaces `collectionFormat` equal to `ssv` from OpenAPI 2.0. |
| pipeDelimited  | `array`, `object`                                      | `query`           | Pipe separated array values or object properties and values. This option replaces `collectionFormat` equal to `pipes` from OpenAPI 2.0. |
| deepObject     | `object`                                               | `query`           | Allows objects with scalar properties to be represented using form parameters. The representation of array or object properties is not defined. |

示例，假设名为 color 的参数具有以下值之一：

```tex
string -> "blue"
array -> ["blue", "black", "brown"]
object -> { "R": 100, "G": 200, "B": 150 }
```

下表列出了每个值的不同序列化示例，与 example 或 examples 关键字一样。

`略，详见官网`。

#### Examples

header 参数，包含一个 64 位整数数组：

- json 格式：

  ```json
  {
    "name": "token",
    "in": "header",
    "description": "token to be passed as a header",
    "required": true,
    "schema": {
      "type": "array",
      "items": {
        "type": "integer",
        "format": "int64"
      }
    },
    "style": "simple"
  }
  ```

- yaml 格式：

  ```yaml
  name: token
  in: header
  description: token to be passed as a header
  required: true
  schema:
    type: array
    items:
      type: integer
      format: int64
  style: simple
  ```

path 参数，使用 string 值：

- json 格式：

  ```json
  {
    "name": "username",
    "in": "path",
    "description": "username to fetch",
    "required": true,
    "schema": {
      "type": "string"
    }
  }
  ```

- yaml 格式：

  ```yaml
  name: username
  in: path
  description: username to fetch
  required: true
  schema:
    type: string
  ```

可选的 query 参数，包含一个 string 值，通过重复 query 参数可获得多个值：

- json 格式：

  ```json
  {
    "name": "id",
    "in": "query",
    "description": "ID of the object to fetch",
    "required": false,
    "schema": {
      "type": "array",
      "items": {
        "type": "string"
      }
    },
    "style": "form",
    "explode": true
  }
  ```

- yaml 格式：

  ```yaml
  name: id
  in: query
  description: ID of the object to fetch
  required: false
  schema:
    type: array
    items:
      type: string
  style: form
  explode: true
  ```

自由格式的 query 参数，允许使用未定义的特定类型参数：

- json 格式：

  ```json
  {
    "in": "query",
    "name": "freeForm",
    "schema": {
      "type": "object",
      "additionalProperties": {
        "type": "integer"
      }
    },
    "style": "form"
  }
  ```

- yaml 格式：

  ```yaml
  in: query
  name: freeForm
  schema:
    type: object
    additionalProperties:
      type: integer
  style: form
  ```

使用 content 字段定义序列化的复杂参数（上面的示例，都是使用的 schema 字段定义序列化的参数）：

- json 格式：

  ```json
  {
    "in": "query",
    "name": "coordinates",
    "content": {
      "application/json": {
        "schema": {
          "type": "object",
          "required": ["lat", "long"],
          "properties": {
            "lat": {
              "type": "number"
            },
            "long": {
              "type": "number"
            }
          }
        }
      }
    }
  }
  ```

- yaml 格式：

  ```yaml
  in: query
  name: coordinates
  content:
    application/json:
      schema:
        type: object
        required:
          - lat
          - long
        properties:
          lat:
            type: number
          long:
            type: number
  ```

### Schema Object

`Schema Object`：用于**定义输入和输出的数据类型**，这些类型可以是对象，也可以是原始值和数组。这个对象是 [JSON Schema Specification Draft 2020-12](https://tools.ietf.org/html/draft-bhutton-json-schema-00) 扩展后的子集。

> 有关属性的更多信息，查看 [JSON Schema Core](https://tools.ietf.org/html/draft-bhutton-json-schema-00) 和 [JSON Schema Validation](https://tools.ietf.org/html/draft-bhutton-json-schema-validation-00)。除非另有说明，否则属性定义均遵循 JSON Schema 的定义，不添加任何其他语义。



### Request Body Object



### Response Object





### Example Object







### Header Object



### Security Scheme Object



### Link Object



### Callback Object



## External Documentation Object

