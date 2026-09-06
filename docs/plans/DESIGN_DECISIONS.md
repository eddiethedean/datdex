# Initial Design Decisions

1. **Separate package** — Datdex is not a ShuETL subsystem.
2. **Independent by default, composable by contract** — no required Hedron/AuthMate/ShuETL/ETLantic dependency.
3. **Metadata, not data** — catalog assets without becoming their storage/query engine.
4. **FastAPI-native** — routers, dependencies, Pydantic, and OpenAPI are primary surfaces.
5. **Relational DB first** — SQLModel/SQLAlchemy; SQLite dev, PostgreSQL production reference.
6. **Immutable schema history** — semantic schema changes create versions.
7. **Lineage provenance** — every assertion records producer; unknown remains explicit.
8. **Push and pull ingestion** — publisher events and connectors normalize into one model.
9. **Search without infrastructure** — relational DB provides useful baseline discovery.
10. **External identity** — Datdex owns permission/resource names, not authentication.
11. **Bounded observations** — quality/freshness store summaries/references.
12. **Full-stack compatibility target** — test optional Hedron + AuthMate + ShuETL + Datdex + ETLantic composition.

## Open ADRs

- qualified-name/canonical URI grammar;
- Dataset vs DataProduct relationship;
- cross-engine schema type normalization;
- recursive lineage query strategy/limits;
- PostgreSQL search implementation;
- metadata event schema/versioning;
- conflict resolution between manual/automated producers;
- ownership semantics;
- field classification inheritance;
- soft-delete/restoration;
- OpenLineage mapping;
- adapter packaging conventions.

## D13 — Reuse mature metadata mechanics

Datdex owns catalog semantics but delegates extraction/interchange mechanics to maintained libraries. Initial choices: optional SQLGlot, PyArrow, fsspec + universal-pathlib, OpenLineage, external GX/Soda-style quality producers, and PostgreSQL/relational baseline search.

## D14 — Pydantic is the Datdex metadata contract layer

Datdex uses Pydantic for normalized metadata, ingestion events, adapter boundaries, configuration, validation, and JSON Schema generation so third-party models never become the public contract.

## D15 — Prefer SQLModel for catalog persistence

Datdex uses SQLModel as the default persisted-entity modeling layer and direct SQLAlchemy for advanced query/index/lineage operations.

## D16 — Use FastAPI streaming, DI, and OpenAPI directly

Datdex uses FastAPI DI for provider composition, JSONL streaming for large metadata exports, optional SSE for live catalog events, OpenAPI webhooks for callback contracts, and dependency overrides for testing.

## D17 — SQL-only infrastructure baseline

Datdex's default production deployment requires only the FastAPI application process and a relational SQL database. A search cluster, graph database, broker, or object store cannot be required for core catalog, lineage, and discovery capabilities.
