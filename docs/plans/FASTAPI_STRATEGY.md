# FastAPI Strategy

## Principle

Datdex should use FastAPI for composable catalog APIs, DI, lifecycle, OpenAPI, streaming exports/events, and testing without introducing unnecessary external infrastructure.

## Required FastAPI features

### APIRouter
Separate datasets, schemas, lineage, governance, observations, search, and ingestion routers.

### Dependency injection
Use `Depends`/`Annotated` for sessions, authorization provider, search provider, publisher/ingestion service, connector registry, audit sink, and settings.

FastAPI dependency injection is the preferred runtime composition mechanism.

### Security()
Use FastAPI security/scopes where they map cleanly to `datdex.*` permissions, while keeping resource-level authorization provider-neutral.

### Router-level dependencies
Use broad dependencies for authenticated governance/admin publishers while allowing separately configurable discovery/read surfaces.

### yield dependencies
Use for request-scoped sessions and short-lived connector/provider resources.

### Lifespan
Use lifespan for long-lived catalog/search/connector resources that need explicit startup/shutdown.

### JSON Lines streaming
Provide JSONL streaming for large metadata exports such as dataset inventories, lineage edges, schema snapshots, and metadata history. Yield Pydantic models rather than buffering huge JSON arrays.

### SSE
Optionally expose server-sent catalog events such as `dataset.registered`, `schema.changed`, `lineage.updated`, `freshness.changed`, and `quality.changed`. SSE is optional for MVP but is the preferred one-way live-update primitive.

### OpenAPI webhooks
Document outbound metadata-change subscriptions such as schema changes, stale freshness, and quality failures.

### Exception handling
Use a stable Datdex error envelope and custom validation/domain exception handlers.

### OpenAPI
Treat OpenAPI as a product surface for publisher contracts, normalized metadata models, error responses, security, and SDK generation.

### Testing
Use dependency overrides for search providers, identity providers, publishers, connectors, audit sinks, and sessions.

## Do not misuse

- Do not require WebSockets for one-way catalog events.
- Do not buffer massive export arrays when JSONL streaming is appropriate.
- Do not use middleware for resource authorization that belongs in DI/service checks.
