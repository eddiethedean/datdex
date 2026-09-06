# Dependency Strategy

## Principle

Datdex should own metadata/catalog semantics while delegating SQL parsing, file metadata inspection, lineage interchange, filesystem access, and quality-engine logic to mature libraries.

> **Own the contracts; reuse the mechanics.**

## Minimal core

```text
fastapi
pydantic
pydantic-settings
sqlmodel
sqlalchemy>=2
alembic
```

Datdex remains useful as a pure metadata registry without SQL, Arrow, cloud, OpenLineage, or quality-framework dependencies.

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

## Quality adapters

Do not build a general quality engine. Ingest normalized results from Great Expectations, Soda, and other producers into bounded `QualityObservation` records.

## Search

Use relational/PostgreSQL search first. External search backends remain optional behind a `SearchProvider`.

## Rules

- Public Datdex models expose no SQLGlot/PyArrow/OpenLineage/GX/Soda types.
- Heavy integrations stay optional and lazy.
- Every imported fact records producer/provenance.
- Unsupported extraction yields explicit unknown/unsupported states, never fabricated metadata.

## SQLModel

Prefer `sqlmodel` for normal persisted catalog metadata entities. Keep direct SQLAlchemy for recursive lineage queries, bulk ingestion, advanced PostgreSQL search/indexing, and other lower-level database features.

## Infrastructure dependency rule

Python package dependencies are allowed when they run in-process. The restriction applies to external infrastructure/services, not libraries.

The default may use FastAPI, Pydantic, SQLModel, SQLAlchemy, SQLGlot, PyArrow, and other in-process libraries. Redis, RabbitMQ, Kafka, OpenSearch/Elasticsearch, object storage, Vault/cloud secret managers, external schedulers, and separate workers cannot be required baseline infrastructure.
