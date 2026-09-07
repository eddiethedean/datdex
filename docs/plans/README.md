# Datdex Planning Pack

**Datdex** is an embeddable, FastAPI-native data catalog and metadata registry.

> **Datdex — Index, discover, and understand your data.**

Datdex catalogs metadata about data; it is not a storage engine.

## Ecosystem

```text
Hedron    -> presentation / catalog UI
AuthMate  -> identity / authorization
ShuETL    -> pipeline operations / metadata publisher
Datdex    -> datasets / schemas / lineage / metadata
ETLantic  -> pipeline semantics / lineage producer
```

## Core principles

> **Independent by default, composable by contract.**

> **Own the contracts; reuse the mechanics.**

> **Useful defaults, extensible by contract.**

> **Extensions must not weaken catalog truth/provenance invariants implicitly.**

Datdex must remain independently useful and extensible without requiring forks. Integrations and extensions use public FastAPI interfaces, Pydantic models, Python protocols, metadata events, and optional adapters.

## Documents

VISION, ARCHITECTURE, METADATA_MODEL, EXTENSIBILITY, LINEAGE_AND_OBSERVATIONS, API_DESIGN, INGESTION_AND_INTEGRATIONS, SEARCH_AND_DISCOVERY, SECURITY_AND_GOVERNANCE, STORAGE_AND_VERSIONING, MVP, ROADMAP, DESIGN_DECISIONS, DEPENDENCY_STRATEGY, PYDANTIC_STRATEGY, and FASTAPI_STRATEGY.

## Dependency philosophy

Datdex uses mature libraries for pagination, SQL parsing, Parquet/Arrow inspection, filesystem access, lineage interchange, and external quality integration while keeping Datdex-owned metadata contracts.

See `DEPENDENCY_STRATEGY.md`.

## Pydantic-first contracts

Pydantic is a first-class architectural dependency for domain models, extension payloads, ingestion events, discriminated unions, validation, serialization boundaries, and JSON Schema.

See `PYDANTIC_STRATEGY.md`.

## FastAPI-native architecture

FastAPI is the integration substrate for routing, DI, security, lifespan, OpenAPI, streaming, exception handling, and testing overrides.

See `FASTAPI_STRATEGY.md`.

## SQL-only infrastructure baseline

Core production capability requires only the FastAPI application process and a relational SQL database. Redis, brokers, Elasticsearch/OpenSearch, graph databases, object stores, external schedulers, and separate workers cannot be baseline requirements.

SQLite should remain sufficient for local development wherever practical.

## Extensibility

Datdex supports controlled extension of selected SQLModel metadata, asset/location/event types, connectors, lineage relations/providers, search providers, observation adapters, governance vocabularies, exporters, and lifecycle hooks.

Supported persistence extensions pair with Datdex-managed Alembic migrations so safe additive schema evolution does not require normal Alembic CLI use.

See `EXTENSIBILITY.md`.
