# API Design

Recommended prefix: `/api/catalog`.

## Datasets

```http
POST   /datasets
GET    /datasets
GET    /datasets/{id}
PATCH  /datasets/{id}
DELETE /datasets/{id}
```

Deletion normally archives/tombstones.

## Schemas

```http
POST /datasets/{id}/schemas
GET  /datasets/{id}/schemas
GET  /datasets/{id}/schemas/{version}
GET  /datasets/{id}/fields
```

## Lineage

```http
POST /lineage
GET  /datasets/{id}/lineage
GET  /fields/{field_id}/lineage
```

## Governance

```http
POST   /datasets/{id}/tags
DELETE /datasets/{id}/tags/{tag}
POST   /datasets/{id}/classifications
POST   /datasets/{id}/owners
```

## Observations and discovery

```http
GET  /datasets/{id}/freshness
POST /datasets/{id}/freshness-observations
GET  /datasets/{id}/quality
POST /datasets/{id}/quality-observations
GET  /search?q=customer
GET  /datasets/{id}/history
```

Use Pydantic schemas, OpenAPI, stable IDs/qualified names, bounded pagination, idempotent publisher endpoints, structured errors, and never return underlying dataset contents.

## Streaming metadata exports

Use FastAPI JSON Lines streaming for large inventories, schema histories, and lineage exports rather than buffering large JSON arrays.

## Catalog event streams

Optional SSE endpoints may stream typed catalog events for live consumers.

## Outbound webhooks

Use OpenAPI webhook declarations for optional metadata-change subscriptions/callbacks.

## Error contracts

All routers use the Datdex Pydantic error envelope through custom FastAPI exception handlers.
