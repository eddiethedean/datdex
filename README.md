# Datdex

**Datdex** is an embeddable, FastAPI-native data catalog and metadata registry.

> **Datdex — Index, discover, and understand your data.**

Datdex catalogs metadata about data; it is not a storage engine.

## Architecture principles

- **Independent by default, composable by contract.**
- **Own the contracts; reuse the mechanics.**
- **Pydantic-first metadata contracts.**
- **SQLModel where it cleanly fits; SQLAlchemy for advanced query and lineage mechanics.**
- **FastAPI-native routing, DI, streaming, lifecycle, and OpenAPI.**
- **SQL-only infrastructure baseline** — core production functionality requires only the FastAPI application process and a relational SQL database.

A reference ecosystem composition is:

```text
FastAPI
├── Hedron
├── AuthMate
├── ShuETL
├── Datdex
└── custom APIs
```

ETLantic and ShuETL may publish metadata into Datdex through optional adapters, while AuthMate may provide authorization and Hedron may provide a catalog UI. None are required by Datdex core.

## Status

Datdex is currently in the architecture and planning phase.

The complete design pack is in [`docs/plans/`](docs/plans/README.md).

## Planned capabilities

- datasets and data products;
- physical locations and stable qualified names;
- immutable schema versions and fields;
- ownership, tags, and classifications;
- dataset and field-level lineage;
- freshness and quality observations;
- metadata ingestion/publishing APIs;
- SQL-backed search and discovery;
- JSONL metadata exports;
- optional SSE catalog events and OpenAPI webhooks;
- optional SQLGlot, PyArrow, OpenLineage, and quality-framework adapters.

## Default deployment goal

```text
FastAPI application
+
PostgreSQL
```

SQLite should remain sufficient for local development.

No Elasticsearch/OpenSearch, graph database, Kafka, object store, or separate metadata service is required for core functionality.
