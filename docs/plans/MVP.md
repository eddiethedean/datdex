# Datdex MVP

## Objective

Ship the smallest useful embeddable FastAPI data catalog.

## Required capabilities

- FastAPI integration;
- Dataset and DataProduct assets;
- physical locations;
- immutable schema versions/fields;
- owner refs, tags, classifications;
- dataset lineage and supplied field lineage;
- freshness observations;
- bounded quality summaries;
- relational search/filtering;
- normalized metadata publishing;
- idempotent producer events;
- SQLModel/SQLAlchemy/Alembic;
- SQLite/PostgreSQL;
- provider-neutral authorization hooks;
- OpenAPI.

## Acceptance criteria

- [ ] Mounts into an existing FastAPI app.
- [ ] Registers datasets without storing underlying data.
- [ ] Schema changes create immutable versions.
- [ ] Nested fields use deterministic canonical paths.
- [ ] Dataset and supplied field lineage are queryable.
- [ ] Opaque lineage remains explicit rather than invented.
- [ ] Ownership/tags/classifications are manageable.
- [ ] Freshness and bounded quality observations publish successfully.
- [ ] Events are idempotent by producer/event ID.
- [ ] Search works without external search infrastructure.
- [ ] Authorization can be supplied externally.
- [ ] SQLite works locally; PostgreSQL is integration-tested.
- [ ] Catalog records require no source credentials or dataset contents.
- [ ] Compatibility tests cover standalone Datdex and the optional Hedron + AuthMate + ShuETL + Datdex + ETLantic composition.

## Dependency requirements

- [ ] Core Datdex works with FastAPI/Pydantic/SQLModel/SQLAlchemy/Alembic only.
- [ ] SQL parsing/lineage is optional through SQLGlot.
- [ ] Parquet/Arrow inspection is optional through PyArrow.
- [ ] File/object-store access uses optional fsspec/UPath.
- [ ] OpenLineage remains an optional interoperability adapter.
- [ ] Great Expectations/Soda remain external quality producers.
- [ ] Basic search works without Elasticsearch/OpenSearch.

## Pydantic requirements

- [ ] Core metadata entities have explicit Pydantic models.
- [ ] Metadata events use discriminated Pydantic unions.
- [ ] Adapter outputs are normalized through Pydantic before persistence.
- [ ] Qualified names, field paths, URIs, and bounded metadata use declarative Pydantic validation.
- [ ] `pydantic-settings` manages Datdex configuration.
- [ ] JSON Schema/OpenAPI derives from the same ingestion/domain models.

## SQLModel requirements

- [ ] Core persisted catalog entities use SQLModel where the domain and persistence models align.
- [ ] Ingestion/event contracts remain Pydantic-only where appropriate.
- [ ] Direct SQLAlchemy is allowed for recursive lineage/search/bulk operations.

## FastAPI requirements

- [ ] Routers compose into an existing FastAPI app.
- [ ] DI composes sessions, authorization, search, publisher, connector, and audit services.
- [ ] Large catalog exports support JSONL streaming rather than large buffered arrays.
- [ ] Stable custom exception handlers/error envelopes exist.
- [ ] OpenAPI is treated as the publisher/client contract.
- [ ] Dependency overrides support provider/connector integration tests.
- [ ] SSE/webhook event surfaces remain compatible with future operational releases.

## SQL-only infrastructure acceptance

- [ ] Datdex core production functionality requires only FastAPI + relational SQL.
- [ ] SQLite supports local development.
- [ ] PostgreSQL is the production reference backend.
- [ ] Catalog search works without Elasticsearch/OpenSearch.
- [ ] Lineage works without a graph database.
- [ ] Metadata ingestion works without Kafka/message brokers.
- [ ] Catalog storage works without object storage.
- [ ] External search, brokers, and lineage services remain optional adapters.
