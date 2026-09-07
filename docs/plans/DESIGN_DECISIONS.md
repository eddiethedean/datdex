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
13. **Useful defaults, extensible by contract** — major catalog capabilities expose typed extension surfaces without requiring forks.
14. **Truth/provenance invariants remain authoritative** — extensions cannot silently fabricate evidence or bypass provenance/identity/history requirements.

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
- adapter packaging conventions;
- exact supported extensible SQLModel entities;
- managed migration revision/version strategy;
- safe-vs-unsafe DDL classification;
- custom asset type persistence/search semantics;
- hook ordering/transaction/failure semantics;
- lineage relation extension registry/versioning.

## D13 — Reuse mature metadata mechanics
Datdex owns catalog semantics but delegates extraction/interchange mechanics to maintained libraries including fastapi-pagination and optional SQLGlot, PyArrow, fsspec/UPath, OpenLineage, and external quality producers.

## D14 — Pydantic is the Datdex metadata contract layer
Datdex uses Pydantic for normalized metadata, extension payloads, ingestion events, adapter boundaries, configuration, validation, and JSON Schema generation.

## D15 — Prefer SQLModel for catalog persistence
Datdex uses SQLModel as the default persisted-entity modeling layer and direct SQLAlchemy for advanced query/index/lineage operations.

Supported developer-extensible persisted entities should inherit from non-table Datdex SQLModel bases rather than relying on accidental mapped-table inheritance behavior.

## D16 — Use FastAPI streaming, DI, and OpenAPI directly
Datdex uses FastAPI DI, JSONL streaming, optional SSE, OpenAPI webhooks, and dependency overrides.

## D17 — SQL-only infrastructure baseline
Default production requires only FastAPI + relational SQL. Search clusters, graph databases, brokers, and object stores remain optional.

## D18 — Useful defaults, extensible by contract

**Decision:** major Datdex capabilities expose typed extension surfaces where practical, including selected persisted metadata, asset/location/event types, connectors, lineage providers/relations, search providers, quality/freshness adapters, governance vocabularies, exporters, and lifecycle hooks.

**Constraint:** extensions must preserve stable identity, provenance, schema history, bounded metadata, authorization filtering, and explicit unknown/unsupported states.

## D19 — Managed Alembic migrations for supported model extensions

**Decision:** supported SQLModel extensions use Datdex-managed programmatic Alembic migrations. Safe additive changes may auto-apply under `auto_migrate="safe"`; destructive/ambiguous changes require explicit action.

Datdex retains a separate migration namespace when sharing a database with sibling packages.

## D20 — External formats normalize into Datdex-owned contracts

**Decision:** connector, OpenLineage, SQLGlot, PyArrow, search-provider, quality-tool, and custom extension objects must normalize into Datdex-owned Pydantic models before persistence/public exposure.

**Reason:** extensibility must not allow third-party implementation schemas to become the catalog's stable domain model.
