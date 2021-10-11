# RFC - Schema Declarations For Entities

> Proposal for improvements submitted to the
> [Sunbird RC project](https://github.com/sunbird-rc/sunbird-rc-core).

A student [entity's](/spec/terms.md#entity) schema might be represented as
follows:

```json
{
  "context": {
    "baseUrl": "https://example.com/registry/",
    "schemas": ["https://registries/schemas/schema.json"]
  },
  "claims": [
    {
      "subject": "name",
      "value": "string"
    },
    {
      "subject": "phoneNumber",
      "value": {
        "country": "string",
        "number": "number"
      }
    },
    {
      "subject": "class",
      "value": "number",
      "attestationPolicy": {
        "attestorKind": "https://example.com/registry/api/teacher",
        "condition": "{ATTESTOR}.school == {ENTITY}.school"
      }
    },
    {
      "subject": "school",
      "value": "string",
      "attestationPolicy": {
        "attestorKind": "https://example.com/registry/api/teacher",
        "condition": "{ATTESTOR}.school == {ENTITY}.school"
      }
    }
  ]
}
```

Following is a line-by-line breakdown of the [entity](/spec/terms.md#entity)
schema declaration above:

```json
"context": {
  "baseUrl": "https://example.com/registry/",
  "schemas": [
    "https://registries/schemas/schema.json"
  ]
}
```

The `baseUrl` property tells the client parsing this JSON schema declaration
that the API path actually starts after this point in the URL. The `schemas`
field can be used to extend the schema declaration by adding more fields to it.

```json
"claims": [...]
```

Each [claim](/spec/terms.md#claim) is represented by a JSON object as follows:

```json
{
  "subject": "class",
  "value": "number",
  "attestationPolicy": {
    "attestorKind": "https://example.com/registry/api/teacher",
    "condition": "{ATTESTOR}.school == {ENTITY}.school"
  }
},
```

In this case, the [claim](/spec/terms.md#claim) `class` must have a value that
is a number. This value can be [attested](/spec/terms.md#attestation) only by a
`teacher` [entity](/spec/terms.md#entity) that is in the same school as the
student. When evaluating the `condition`, the field's value must be
[attested](/spec/terms.md#attestation), else it will be considered null.
