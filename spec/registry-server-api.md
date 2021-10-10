# Registry Server APIs

See the
[RFC for entity representations](https://github.com/sunbird-rc/community/discussions/98)
for a detailed explanation on all the JSON objects returned from API requests.

## Registry URIs

- Reference to the schema of a particular entity kind: `/api/{entity-kind}`
- Reference to a particular entity: `/api/{entity-kind}/{entity-id}`
- Reference to a particular claim an entity makes:
  `/api/{entity-kind}/{entity-id}/{field-name}`

## API Endpoints

### Create an Entity

```http
POST /api/{entity-kind} HTTP/1.1
Host: {registry-url}
Content-Type: application/json
Accept: application/vnd.registry.v1+json

{
	"claims": [
		...
	]
}

---

HTTP/1.1 201 Created
Server: {registry-url}
Content-Type: application/vnd.registry.v1+json

{
	"context": {
		"baseUrl": "{registry-url}",
		"schemas": ["https://registries/schemas/entity.json"]
	},
	"id": "{registry-url}/api/{entity-kind}/{entity-id}",
	"kind": "{registry-url}/api/{entity-kind}",
	"claims": [
		...
	],
	"attestations": [
		...
	],
	"metadata": {
		"created": "...",
		"lastUpdated": "..."
	}
}
```

### Retrieve an Entity

```http
GET /api/{entity-kind}/{entity-id} HTTP/1.1
Host: {registry-url}
Content-Type: application/json
Accept: application/vnd.registry.v1+json



---

HTTP/1.1 200 Ok
Server: {registry-url}
Content-Type: application/vnd.registry.v1+json

{
	"context": {
		"baseUrl": "{registry-url}",
		"schemas": ["https://registries/schemas/entity.json"]
	},
	"id": "{registry-url}/api/{entity-kind}/{entity-id}",
	"kind": "{registry-url}/api/{entity-kind}",
	"claims": [
		...
	],
	"attestations": [
		...
	],
	"metadata": {
		"created": "...",
		"lastUpdated": "..."
	}
}
```

### Update an Entity

```http
PUT /api/{entity-kind} HTTP/1.1
Host: {registry-url}
Content-Type: application/json
Accept: application/vnd.registry.v1+json

{
	"claims": [
		...
	]
}

---

HTTP/1.1 200 Ok
Server: {registry-url}
Content-Type: application/vnd.registry.v1+json

{
	"context": {
		"baseUrl": "{registry-url}",
		"schemas": ["https://registries/schemas/entity.json"]
	},
	"id": "{registry-url}/api/{entity-kind}/{entity-id}",
	"kind": "{registry-url}/api/student",
	"claims": [
		...
	],
	"attestations": [
		...
	],
	"metadata": {
		"created": "...",
		"lastUpdated": "..."
	}
}
```

### Delete an Entity

```http
DELETE /api/{entity-kind}/{entity-id} HTTP/1.1
Host: {registry-url}
Content-Type: application/json
Accept: application/vnd.registry.v1+json



---

HTTP/1.1 204 No Content
Server: {registry-url}
Content-Type: application/vnd.registry.v1+json



```

### Retrieve an Entity's Schema

```http
GET /api/{entity-kind} HTTP/1.1
Host: {registry-url}
Content-Type: application/json
Accept: application/vnd.registry.v1+json



---

HTTP/1.1 200 Ok
Server: {registry-url}
Content-Type: application/vnd.registry.v1+json

{
	"kind": "{registry-url}/api/{entity-kind}",
	"claims": [
		...
	],
}
```

### Signing Keys

```http
GET /meta/signing-keys/{key-id} HTTP/1.1
Host: {registry-url}
Content-Type: application/json
Accept: application/vnd.registry.v1+json



---

HTTP/1.1 200 Ok
Server: {registry-url}
Content-Type: application/vnd.registry.v1+json

{
	"id": "{key-id}"
	"kind": "{key-type}",
	"publicKey": "..."
}
```
