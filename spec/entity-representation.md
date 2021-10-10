# Entity Representation For Claims And Attestation

## Terminology

### Registry

Any data store that stores and provides API endpoints to create, access, modify
and delete data in accordance with this specification.

### Entity

Any record stored in the [registry](#registry).

### Schema

A minimum representation of all [entities](#entity) of the same type.

### Claim

An assertion made about an [entity](#entity), which must be
[verified](#attestation) to be considered true.

### Attestation

The process of evaluating a [claim](#claim) made about an [entity](#entity),
checking if it is indeed true, and then generating a cryptographically secure
[proof](#proof) to declare it so.

### Attestor

A role an [entity](#entity) might perform by receiving a representation of an
[entity](#entity) and putting its [claims](#claim) through the process of
[attestation](#attestation), if it is designated as attestor for a certain
[claim](#claim) made by another [entity](#entity).

## Representation

A student [entity](#entity) might be represented as follows:

```json
{
	"context": {
		"baseUrl": "https://example.com/registry/",
		"schemas": ["https://registries/schemas/entity.json"]
	},
	"id": "https://example.com/registry/api/student/GcivYPrwdeIOREu4UqCpO854",
	"kind": "https://example.com/registry/api/student",
	"claims": [
		{
			"id": "https://example.com/registry/api/student/GcivYPrwdeIOREu4UqCpO854/name",
			"value": "Pranav Agate"
		},
		{
			"id": "https://example.com/registry/api/student/GcivYPrwdeIOREu4UqCpO854/phoneNumber",
			"value": {
				"country": "IN",
				"number": 9988776655
			}
		},
		{
			"id": "https://example.com/registry/api/student/GcivYPrwdeIOREu4UqCpO854/class",
			"value": 10
		},
		{
			"id": "https://example.com/registry/api/student/GcivYPrwdeIOREu4UqCpO854/school",
			"value": "UP Public School"
		}
	],
	"attestations": [
		{
			"attestor": "https://example.com/registry/api/teacher/RBFrXEOG890jZBUf1Vpuy",
			"date": "2021-10-08T19:37:03+1200",
			"claims": [
				"https://example.com/registry/api/student/GcivYPrwdeIOREu4UqCpO854/school"
			],
			"signature": {
				"type": "https://registries/signature-types/jws-rsa256",
				"keyId": "https://example.com/registry/meta/signing-keys/IL5JLA2MinN9vVLFUxiLR",
				"value": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJjb250ZXh0Ijp7ImJhc...mZG8GbRlzpUkAEPI1vkhGc"
			}
		}
	],
	"metadata": {
		"created": "2021-10-07T16:52:32+0530",
		"lastUpdated": "2021-10-08T19:37:03+1200"
	}
}
```

Following is a line-by-line breakdown of the [entity](#entity) representation
above:

```json
"context": {
	"baseUrl": "https://example.com/registry/",
	"schemas": [
		"https://registries/schemas/entity.json"
	]
}
```

The `baseUrl` property tells the client parsing this JSON representation that
the API path actually starts after this point in the URL. The `schemas` field
can be used to extend the representation by adding more fields to it.

```json
"id": "https://example.com/registry/api/student/GcivYPrwdeIOREu4UqCpO854",
"kind": "https://example.com/registry/api/student"
```

The `id` field is a direct URL to the [entity](#entity). If you make a GET call
to the URL, it should return exactly this representation of the
[entity](#entity). The `kind` field gives us a URL to the schema of the
[entity](#entity).

```json
"claims": [...]
```

Each [claim](#claim) is represented by a JSON object as follows:

```json
{
	"id": "https://example.com/registry/api/student/GcivYPrwdeIOREu4UqCpO854/class",
	"value": 10
},
```

In this case, the [claim](#claim) is `class`, the value is `10`.

The attestationPolicy for a [claim](#claim) can be mentioned in the schema as
follows:

```json
"attestationPolicy": {
	"attestorKind": "https://example.com/registry/api/teacher",
	"condition": "{ATTESTOR}.school == {ENTITY}.school"
}
```

In this case, the [claim](#claim) can be attested only by a `teacher`
[entity](#entity) that is in the same school as the student. When evaluating the
`condition`, the field's value must be attested, else it will be considered
null.

```json
"attestations": [...]
```

Each attestation is represented as follows:

```json
{
	"attestor": "https://example.com/registry/api/teacher/RBFrXEOG890jZBUf1Vpuy",
	"date": "2021-10-08T19:37:03+1200",
	"claims": [
		"https://example.com/registry/api/student/GcivYPrwdeIOREu4UqCpO854/school"
	],
	"signature": {
		"type": "https://registries/signature-types/jws-rsa256",
		"keyId": "https://example.com/registry/meta/signing-keys/IL5JLA2MinN9vVLFUxiLR",
		"value": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJjb250ZXh0Ijp7ImJhc...mZG8GbRlzpUkAEPI1vkhGc"
	}
}
```

In this case, the attestor is a `teacher` [entity](#entity). The attestation of
the `school` [claim](#claim) took place on `2021-10-08T19:37:03+1200` and the
cryptographic proof is attached as the `signature` object.

```json
"signature": {
	"type": "https://registries/signature-types/jws-rsa256",
	"keyId": "https://example.com/registry/meta/signing-keys/IL5JLA2MinN9vVLFUxiLR",
	"value": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJjb250ZXh0Ijp7ImJhc...mZG8GbRlzpUkAEPI1vkhGc"
}
```

In this case, the [claims](#claim) have been signed using an RSA private key
whose ID is `IL5JLA2MinN9vVLFUxiLR`. The `claims` object of the
[entity](#entity) representation at the time of attesting is taken as the
payload of the JWT, signed by a private key by the registry, and appened to the
`attestations` array.
