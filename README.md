# Failure As A Service (FaaS)

**Enterprise-grade disappointment, delivered on demand.**

FaaS is a tiny HTTP service that returns a random “failure reason” from a curated library — ideal for demos, hackathons, status pages, placeholder error handling, and general workplace catharsis.

If the failure itself fails, the service responds with:

> **I… don’t know what to say.**

---

## Base URL

Production (planned): `https://faas.dansec.red`

All examples below assume this base URL.

---

## Endpoints

### Get one failure
`GET /v1/failure`

Returns a single failure message (random by default).

**Examples**
```bash
curl -s https://faas.dansec.red/v1/failure
curl -s https://faas.dansec.red/v1/failure?format=text
curl -s "https://faas.dansec.red/v1/failure?category=business&format=text"
curl -s "https://faas.dansec.red/v1/failure?seed=1234"
````

---

### Get a batch of failures

`GET /v1/failure/batch?n=5`

Returns multiple failure messages (useful for “bundle of excuses” output).

**Examples**

```bash
curl -s "https://faas.dansec.red/v1/failure/batch?n=5"
curl -s "https://faas.dansec.red/v1/failure/batch?n=5&format=text"
curl -s "https://faas.dansec.red/v1/failure/batch?n=10&category=devops"
```

---

### List categories

`GET /v1/categories`

Lists available message categories.

```bash
curl -s https://faas.dansec.red/v1/categories
```

---

### Health check

`GET /healthz`

Simple health endpoint.

```bash
curl -s https://faas.dansec.red/healthz
```

---

## Query parameters

Available on `/v1/failure` and `/v1/failure/batch`:

* `category` — message category (e.g. `general`, `business`, `devops`, `security`, `product`)
* `format` — `json` (default) or `text`
* `seed` — deterministic output (great for tests/demos)
* `tone` — `safe` (default) or `spicy` (still workplace-safe)
* `n` — batch size for `/v1/failure/batch` (default `5`, max `50`)

---

## Response formats

### JSON (default)

Single:

```json
{
  "id": "faas_00000000",
  "message": "Task failed successfully.",
  "category": "general",
  "tone": "safe",
  "httpStatusHint": null,
  "time": "2026-01-10T00:00:00.000Z",
  "requestId": "..."
}
```

Batch:

```json
{
  "items": [
    { "id": "...", "message": "...", "category": "...", "tone": "safe", "httpStatusHint": null }
  ],
  "count": 5,
  "category": "general",
  "tone": "safe",
  "time": "2026-01-10T00:00:00.000Z",
  "requestId": "..."
}
```

### Text

Single:

```text
Task failed successfully.
```

Batch:

```text
• It worked in staging. Production felt differently.
• DNS is propagating. Somewhere. Probably.
• Deploy succeeded. Reality did not.
```

---

## Response headers

FaaS adds a few helpful/funny headers for logging and tracing:

* `X-FAAS-Reason` — the selected message (or batch summary)
* `X-FAAS-Category` — the category served
* `X-FAAS-Tone` — `safe` / `spicy`
* `X-FAAS-Request-Id` — per-request identifier
* `X-FAAS-Failure-Failed: true` — only when *the failure failed*

---

## Content guidelines

Messages are intended to be:

* short (log-line friendly)
* SFW / workplace-safe
* funny without being mean

---

## Spec

An OpenAPI definition is provided in `openapi.yaml`.

---

## Licence

MIT — see `LICENSE`.
