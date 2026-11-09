# Service Level Agreement

The service levels an API provider publishes — the uptime it promises, the credit it owes when it
misses, the response times it commits to, and the uptime its own status page reports — as a
**profile of [SLA4OAI](https://github.com/isa-group/SLA4OAI-Specification)**, not a new format.

This is the schema behind the [Service Level Agreement property](https://apicommons.org/common/service-level-agreement/)
in [API Commons](https://apicommons.org).

## Why it matters

API Commons was started to make the business side of API onboarding machine-readable. Whether an API
will be there when you need it — and what you are owed when it is not — decides whether it can sit in
a production path. Today that answer lives in a PDF or a legal page. Expressed against SLA4OAI, it can
be compared across providers and read by an agent choosing between them.

## What the profile adds

SLA4OAI 1.0.0 Draft describes plans, pricing, quotas and rates against an OpenAPI. It has no field for
an uptime figure, a service credit or a response time, and account-wide limits have no path to sit
under. Those travel on each plan as `x-apis-io-*` extensions, which SLA4OAI allows:

| Extension | Meaning |
|---|---|
| `x-apis-io-guarantees[]` | What the provider **promises**, quoted from its SLA page |
| `x-apis-io-observed[]` | What the provider **reports** it delivered, quoted from its own status page — never a promise, never measured by APIs.io |
| `x-apis-io-limits[]` | Limits as documented, verbatim, that name no method + path and so cannot go in `rates` |
| `context.x-apis-io-derivation` | How the document was built: `synthetic: false`, `method: searched`, `read`, `tool`, `coverage` (promised / reported uptime: `found` / `not found` / `not checked`), `statement`, `sources[]` |

Each guarantee / observed row is `{text, percent, window, component, credits_text, response_time_text, source, read}`;
`text`, `source` and `read` are required. A limit enters SLA4OAI `rates` only when the docs name both
a method and a path; a price enters `pricing` only when it is a bare amount. Nothing is inferred to fill a field.

## Two document forms

- **`<slug>-sla.yml`** — a conformant SLA4OAI document (`sla4oas: 1.0.0`), pointing at the provider's OpenAPI.
- **`<slug>-service-levels.yml`** — written when figures were found but the provider has no OpenAPI,
  which SLA4OAI requires. It carries `conformant: false` and `not_sla4oai_because`, and says so in its
  header. It is not an SLA4OAI document.

## Provenance: verbatim text, never synthetic

Every figure is quoted from the provider's own page, with the page and the date on the row. A document
built only from plans and rate limits, with no service level found, is never published — a file labelled
as a provider's SLA reads as one the provider published, whatever it says inside. The schema rejects
`synthetic: true`.

## Artifacts

- **[service-level-agreement-json-schema.yml](service-level-agreement-json-schema.yml)** — the profile (JSON Schema 2020-12). Upstream SLA4OAI keys pass through and are validated by the upstream schema.
- **[service-level-agreement-example-1.yml](service-level-agreement-example-1.yml)** — a conformant SLA4OAI document for the fictional **Example Weather API** (also valid against the upstream SLA4OAI 1.0.0 Draft schema).
- **[service-level-agreement-example-2.yml](service-level-agreement-example-2.yml)** — the same document inside the `ServiceLevelAgreement` apis.yml property envelope.
- **[validate.py](validate.py)** — validates any document against the profile.

The figures in the examples are illustrative and are not the service levels of any real company.

```
pip install jsonschema pyyaml
python3 validate.py service-level-agreement-example-1.yml service-level-agreement-example-2.yml
```

## Upstream

- SLA4OAI specification — https://github.com/isa-group/SLA4OAI-Specification
- Produced by `engine/economics/build-sla4oai.py` in the API Evangelist pipeline.

## License

- **Artifacts** — the schemas and examples — are **[CC BY-NC-SA 4.0](LICENSE)**.
- **Code** — the validator — is **[Apache-2.0](LICENSE-CODE)**.

## Part of API Commons

Related business-layer blocks:
[terms-of-service](https://github.com/api-commons/terms-of-service) ·
[privacy-policy](https://github.com/api-commons/privacy-policy) ·
[deprecation-policy](https://github.com/api-commons/deprecation-policy) ·
[compliance](https://github.com/api-commons/compliance) ·
[eligibility](https://github.com/api-commons/eligibility) ·
[ai-preferences](https://github.com/api-commons/ai-preferences) ·
[data-license](https://github.com/api-commons/data-license) ·
[plans](https://github.com/api-commons/plans) ·
[rate-limits](https://github.com/api-commons/rate-limits)
