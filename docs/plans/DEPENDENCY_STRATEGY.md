# Dependency Strategy

## Principle

Datdex should own metadata/catalog semantics while delegating pagination, SQL parsing, file metadata inspection, lineage interchange, filesystem access, and quality-engine logic to mature libraries.

> **Own the contracts; reuse the mechanics.**

## Minimal core

```text
fastapi
fastapi-pagination
pydantic
pydantic-settings
sqlmodel
sqlalchemy>=2
alembic
```

Datdex remains useful as a pure metadata registry without SQL parsing, Arrow, cloud, OpenLineage, GraphQL, or quality-framework dependencies.

## fastapi-pagination

Use `fastapi-pagination` for dataset inventories, schemas, lineage/history records, observations, governance collections, and search results rather than building a custom pagination framework.

Datdex owns search/filter semantics and metadata contracts. `fastapi-pagination` owns pagination mechanics and SQLModel/SQLAlchemy integration.

Use bounded defaults/maxima and adopt cursor/keyset pagination for high-volume metadata histories where appropriate.

## Filtering

`fastapi-filter` is an approved candidate for Datdex filtering because its current releases support modern FastAPI, Pydantic v2, and SQLAlchemy. Do **not** make it foundational until an implementation spike confirms that its query semantics fit Datdex's owner/tag/classification/freshness/quality filters without leaking its model types into public contracts.

If adopted, keep it behind Datdex-owned filter Pydantic models.

## SQL extra

```text
datdex[sql]
  sqlglot
```

Use SQLGlot for SQL parsing, dialect normalization, AST inspection, table/column extraction, and supported lineage. Never build a custom SQL parser.

## Parquet extra

```text
datdex[parquet]
  pyarrow
  fsspec
  universal-pathlib
```

Use PyArrow for schema/Parquet metadata inspection without loading full datasets, and fsspec/UPath for provider-neutral filesystem access.

## OpenLineage extra

```text
datdex[openlineage]
  openlineage-python
```

Use OpenLineage as an interoperability adapter, not Datdex's internal domain schema.

## GraphQL extra

Strawberry GraphQL is the approved implementation if Datdex later adds GraphQL:

```text
datdex[graphql]
  strawberry-graphql[fastapi]
```

GraphQL is not an MVP requirement. Datdex service contracts must remain transport-neutral enough to support it later without duplicating catalog logic.

## Quality adapters

Do not build a general quality engine. Ingest normalized results from Great Expectations, Soda, and other producers into bounded `QualityObservation` records.

## Search

Use relational/PostgreSQL search first. External search backends remain optional behind a `SearchProvider`.

## Admin interfaces

Starlette-Admin and SQLAdmin are mature enough to consider for optional developer/operator tooling, but neither is part of Datdex's core product architecture. Hedron remains the preferred polished ecosystem UI.

## Rules

- Public Datdex models expose no fastapi-pagination/fastapi-filter/SQLGlot/PyArrow/OpenLineage/Strawberry/GX/Soda implementation types.
- Heavy integrations stay optional and lazy.
- Every imported fact records producer/provenance.
- Unsupported extraction yields explicit unknown/unsupported states, never fabricated metadata.

## SQLModel

Prefer `sqlmodel` for normal persisted catalog metadata entities. Keep direct SQLAlchemy for recursive lineage queries, bulk ingestion, advanced PostgreSQL search/indexing, and other lower-level database features.

## Infrastructure dependency rule

Python package dependencies are allowed when they run in-process. The restriction applies to external infrastructure/services, not libraries.

The default may use FastAPI, `fastapi-pagination`, Pydantic, SQLModel, SQLAlchemy, SQLGlot, PyArrow, and other in-process libraries. Redis, RabbitMQ, Kafka, OpenSearch/Elasticsearch, object storage, Vault/cloud secret managers, external schedulers, and separate workers cannot be required baseline infrastructure.
