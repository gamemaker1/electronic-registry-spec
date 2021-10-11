# RFC - Registry Server APIs

> Proposal for improvements submitted to the
> [Sunbird RC project](https://github.com/sunbird-rc/sunbird-rc-core).

See the [document on entity representations](/spec/entity-representation.md) for
a detailed explanation on all the JSON objects returned from API requests.

## API Endpoints

### Create An Entity

**Request**

`POST /api/{entity-kind}`

**Parameters**

| Name           | Type   | In     | Description                                                                             |
| -------------- | ------ | ------ | --------------------------------------------------------------------------------------- |
| `content-type` | string | header | Recommended to set to `application/json`                                                |
| `accept`       | string | header | Recommended to set to `application/vnd.registry.v1+json`                                |
| `entity-kind`  | string | path   | The kind of [entity](/spec/terms.md#entity) to create                                   |
| `claims`       | array  | body   | An array of [claims](/spec/terms.md#claims) made by the [entity](/spec/terms.md#entity) |

**Usage**

```bash
curl \
  --request POST \
  --header 'content-type: application/json' \
  --header 'accept: application/vnd.registry.v1+json' \
  --data-raw '{
    "claims": [
      {
        "id": "{registry-url}/api/{entity-kind}/{entity-id}/{entity-claim}",
        "value": "..."
      }
    ]
  }' \
  '{registry-url}/api/{entity-kind}/'
```

**Response**

```http
HTTP/1.1 201 CREATED
server: {registry-url}
content-type: application/json
x-registry-media-type: registry.v1; format=json
```

```json
{
  "context": {
    "baseUrl": "{registry-url}",
    "schemas": ["https://registries/schemas/entity.json"]
  },
  "id": "{registry-url}/api/{entity-kind}/{entity-id}",
  "kind": "{registry-url}/api/{entity-kind}",
  "claims": [
    {
      "id": "{registry-url}/api/{entity-kind}/{entity-id}/{entity-claim}",
      "value": "..."
    }
  ],
  "attestations": [],
  "metadata": {
    "created": "...",
    "lastUpdated": "..."
  }
}
```

### Retrieve An Entity

**Request**

`GET /api/{entity-kind}/{entity-id}`

**Parameters**

| Name           | Type   | In     | Description                                              |
| -------------- | ------ | ------ | -------------------------------------------------------- |
| `content-type` | string | header | Recommended to set to `application/json`                 |
| `accept`       | string | header | Recommended to set to `application/vnd.registry.v1+json` |
| `entity-kind`  | string | path   | The kind of [entity](/spec/terms.md#entity) to retrieve  |
| `entity-id`    | array  | path   | ID of the [entity](/spec/terms.md#entity) to retrieve    |

**Usage**

```bash
curl \
  --request GET \
  --header 'content-type: application/json' \
  --header 'accept: application/vnd.registry.v1+json' \
  '{registry-url}/api/{entity-kind}/{entity-id}'
```

**Response**

```http
HTTP/1.1 200 OK
server: {registry-url}
content-type: application/json
x-registry-media-type: registry.v1; format=json
```

```json
{
  "context": {
    "baseUrl": "{registry-url}",
    "schemas": ["https://registries/schemas/entity.json"]
  },
  "id": "{registry-url}/api/{entity-kind}/{entity-id}",
  "kind": "{registry-url}/api/{entity-kind}",
  "claims": ["..."],
  "attestations": ["..."],
  "metadata": {
    "created": "...",
    "lastUpdated": "..."
  }
}
```

### Update An Entity

**Request**

`PATCH /api/{entity-kind}/{entity-id}`

**Parameters**

| Name           | Type   | In     | Description                                                                                                |
| -------------- | ------ | ------ | ---------------------------------------------------------------------------------------------------------- |
| `content-type` | string | header | Recommended to set to `application/json`                                                                   |
| `accept`       | string | header | Recommended to set to `application/vnd.registry.v1+json`                                                   |
| `entity-kind`  | string | path   | The kind of [entity](/spec/terms.md#entity) to create                                                      |
| `entity-id`    | string | path   | ID of the [entity](/spec/terms.md#entity) to update                                                        |
| `claims`       | array  | body   | An array of [claims](/spec/terms.md#claims) that will replace the existing [claims](/spec/terms.md#claims) |

**Usage**

```bash
curl \
  --request PATCH \
  --header 'content-type: application/json' \
  --header 'accept: application/vnd.registry.v1+json' \
  --data-raw '{
    "claims": [
      {
        "id": "{registry-url}/api/{entity-kind}/{entity-id}/{entity-claim}",
        "value": "..."
      }
    ]
  }' \
  '{registry-url}/api/{entity-kind}/{entity-id}'
```

**Response**

```http
HTTP/1.1 200 OK
server: {registry-url}
content-type: application/json
x-registry-media-type: registry.v1; format=json
```

```json
{
  "context": {
    "baseUrl": "{registry-url}",
    "schemas": ["https://registries/schemas/entity.json"]
  },
  "id": "{registry-url}/api/{entity-kind}/{entity-id}",
  "kind": "{registry-url}/api/{entity-kind}",
  "claims": [
    {
      "id": "{registry-url}/api/{entity-kind}/{entity-id}/{entity-claim}",
      "value": "..."
    }
  ],
  "attestations": ["..."],
  "metadata": {
    "created": "...",
    "lastUpdated": "..."
  }
}
```

### Delete An Entity

**Request**

`DELETE /api/{entity-kind}/{entity-id}`

**Parameters**

| Name           | Type   | In     | Description                                              |
| -------------- | ------ | ------ | -------------------------------------------------------- |
| `content-type` | string | header | Recommended to set to `application/json`                 |
| `accept`       | string | header | Recommended to set to `application/vnd.registry.v1+json` |
| `entity-kind`  | string | path   | The kind of [entity](/spec/terms.md#entity) to retrieve  |
| `entity-id`    | array  | path   | ID of the [entity](/spec/terms.md#entity) to delete      |

**Usage**

```bash
curl \
  --request DELETE \
  --header 'content-type: application/json' \
  --header 'accept: application/vnd.registry.v1+json' \
  '{registry-url}/api/{entity-kind}/{entity-id}'
```

**Response**

```http
HTTP/1.1 204 NO CONTENT
server: {registry-url}
content-type: application/json
x-registry-media-type: registry.v1; format=json
```

```json
{}
```

### Attest A Claim

**Request**

`POST /api/{entity-kind}/{entity-id}/{entity-claim}/attest`

**Parameters**

| Name           | Type   | In     | Description                                                                                   |
| -------------- | ------ | ------ | --------------------------------------------------------------------------------------------- |
| `content-type` | string | header | Recommended to set to `application/json`                                                      |
| `accept`       | string | header | Recommended to set to `application/vnd.registry.v1+json`                                      |
| `entity-kind`  | string | path   | The kind of [entity](/spec/terms.md#entity) whose [claims](/spec/terms.md#claims) to attest   |
| `entity-id`    | string | path   | The ID of the [entity](/spec/terms.md#entity) whose [claims](/spec/terms.md#claims) to attest |
| `entity-claim` | string | path   | The ID of the [claim](/spec/terms.md#claim) to attest                                         |
| `attestor`     | string | body   | The ID of the [entity](/spec/terms.md#entity) attesting the [claim](/spec/terms.md#claim)     |
| `value`        | string | body   | The attested value of the [claim](/spec/terms.md#claim)                                       |

**Usage**

```bash
curl \
  --request POST \
  --header 'content-type: application/json' \
  --header 'accept: application/vnd.registry.v1+json' \
  --data-raw '{
    "attestor": "{registry-url}/api/{entity-kind}/{entity-id}",
    "value": "..."
  }' \
  '{registry-url}/api/{entity-kind}/{entity-id}/{entity-claim}/attest'
```

**Response**

```http
HTTP/1.1 200 OK
server: {registry-url}
content-type: application/json
x-registry-media-type: registry.v1; format=json
```

```json
{
  "context": {
    "baseUrl": "{registry-url}",
    "schemas": ["https://registries/schemas/entity.json"]
  },
  "id": "{registry-url}/api/{entity-kind}/{entity-id}",
  "kind": "{registry-url}/api/{entity-kind}",
  "claims": [
    {
      "id": "{registry-url}/api/{entity-kind}/{entity-id}/{entity-claim}",
      "value": "..."
    }
  ],
  "attestations": [
    {
      "attestor": "{registry-url}/api/{entity-kind}/{entity-id}",
      "date": "...",
      "claims": ["{registry-url}/api/{entity-kind}/{entity-id}/{entity-claim}"],
      "signature": {
        "type": "https://registries/signature-types/jws-rsa256",
        "keyId": "{registry-url}/meta/signing-keys/{key-id}",
        "value": "..."
      }
    }
  ],
  "metadata": {
    "created": "...",
    "lastUpdated": "..."
  }
}
```

### Retrieve An Entity's Schema

**Request**

`GET /api/{entity-kind}`

**Parameters**

| Name           | Type   | In     | Description                                                                                   |
| -------------- | ------ | ------ | --------------------------------------------------------------------------------------------- |
| `content-type` | string | header | Recommended to set to `application/json`                                                      |
| `accept`       | string | header | Recommended to set to `application/vnd.registry.v1+json`                                      |
| `entity-kind`  | string | path   | The kind of [entity](/spec/terms.md#entity) whose [schema](/spec/terms.md#schema) to retrieve |

**Usage**

```bash
curl \
  --request GET \
  --header 'content-type: application/json' \
  --header 'accept: application/vnd.registry.v1+json' \
  '{registry-url}/api/{entity-kind}'
```

**Response**

```http
HTTP/1.1 200 OK
server: {registry-url}
content-type: application/json
x-registry-media-type: registry.v1; format=json
```

```json
{
  "context": {
    "baseUrl": "{registry-url}",
    "schemas": ["https://registries/schemas/schema.json"]
  },
  "kind": "{registry-url}/api/{entity-kind}",
  "claims": [
    {
      "id": "{registry-url}/api/{entity-kind}/{entity-id}/{entity-claim}",
      "value": "...",
      "attestationPolicy": {
        "attestorKind": "{registry-url}/api/{attestor-kind}",
        "condition": "{ATTESTOR}.{entity-claim} == {ENTITY}.{entity-claim}"
      }
    }
  ]
}
```

### Search For An Entity

**Request**

`GET /api/{entity-kind}/search`

**Parameters**

| Name           | Type   | In     | Description                                                   |
| -------------- | ------ | ------ | ------------------------------------------------------------- |
| `content-type` | string | header | Recommended to set to `application/json`                      |
| `accept`       | string | header | Recommended to set to `application/vnd.registry.v1+json`      |
| `entity-kind`  | string | path   | The kind of [entities](/spec/terms.md#entity) to search       |
| `query`        | string | body   | The query to search through [entities](/spec/terms.md#entity) |

**Usage**

```bash
curl \
  --request GET \
  --header 'content-type: application/json' \
  --header 'accept: application/vnd.registry.v1+json' \
  --data-raw '{
    "query": {
      "claims": [
        {
          "id": "{registry-url}/api/{entity-kind}/{entity-id}/{entity-claim}",
          "value": "..."
        }
      ],
      "attestations": [
        {
          "attestor": "{registry-url}/api/{entity-kind}/{entity-id}"
        }
      ]
    }
  }' \
  '{registry-url}/api/{entity-kind}/search'
```

**Response**

```http
HTTP/1.1 200 OK
server: {registry-url}
content-type: application/json
x-registry-media-type: registry.v1; format=json
```

```json
{
  "context": {
    "baseUrl": "{registry-url}",
    "schemas": ["https://registries/schemas/schema.json"]
  },
  "kind": "{registry-url}/api/{entity-kind}",
  "claims": [
    {
      "id": "{registry-url}/api/{entity-kind}/{entity-id}/{entity-claim}",
      "value": "..."
    }
  ]
}
```

### Retrieve Signing Keys

**Request**

`GET /meta/signing-keys/{key-id}`

**Parameters**

| Name           | Type   | In     | Description                                              |
| -------------- | ------ | ------ | -------------------------------------------------------- |
| `content-type` | string | header | Recommended to set to `application/json`                 |
| `accept`       | string | header | Recommended to set to `application/vnd.registry.v1+json` |
| `key-id`       | array  | path   | ID of the key to retrieve                                |

**Usage**

```bash
curl \
  --request GET \
  --header 'content-type: application/json' \
  --header 'accept: application/vnd.registry.v1+json' \
  '{registry-url}/meta/signing-keys/{key-id}'
```

**Response**

```http
HTTP/1.1 200 OK
server: {registry-url}
content-type: application/json
x-registry-media-type: registry.v1; format=json
```

```json
{
  "context": {
    "baseUrl": "{registry-url}",
    "schemas": ["https://registries/schemas/signing-key.json"]
  },
  "id": "{registry-url}/meta/signing-keys/{key-id}",
  "kind": "https://registries/key-types/{key-type}",
  "publicKey": "..."
}
```
