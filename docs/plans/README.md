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

## Ecosystem principle

> **Independent by default, composable by contract.**

Datdex must remain independently useful and must not require Hedron, AuthMate, ShuETL, or ETLantic. Integrations use public FastAPI interfaces, Python protocols, metadata events, and optional adapters.

## Documents

VISION, ARCHITECTURE, METADATA_MODEL, LINEAGE_AND_OBSERVATIONS, API_DESIGN, INGESTION_AND_INTEGRATIONS, SEARCH_AND_DISCOVERY, SECURITY_AND_GOVERNANCE, STORAGE_AND_VERSIONING, MVP, ROADMAP, and DESIGN_DECISIONS.

## Dependency philosophy

> **Own the contracts; reuse the mechanics.**

Datdex should use mature libraries for SQL parsing, Parquet/Arrow inspection, filesystem access, lineage interchange, and external quality integration while keeping Datdex-owned metadata contracts.

See `DEPENDENCY_STRATEGY.md`.

## Pydantic-first contracts

Pydantic is a first-class architectural dependency, not merely FastAPI request validation.

Public domain models, configuration, discriminated unions, validation, serialization boundaries, and generated JSON Schema should use Pydantic wherever appropriate.

See `PYDANTIC_STRATEGY.md`.

## FastAPI-native architecture

FastAPI is a first-class integration substrate, not merely the HTTP server.

The package should fully use FastAPI routing, dependency injection, security primitives, lifespan, OpenAPI, exception handling, and testing overrides while preserving clear domain boundaries.

See `FASTAPI_STRATEGY.md`.

## SQL-only infrastructure baseline

> **SQL-only infrastructure baseline.**

Core production capability must require only the FastAPI application process and a relational SQL database.

No Redis, RabbitMQ, Kafka, Elasticsearch/OpenSearch, object store, Vault, external scheduler, separate worker service, or other infrastructure may be required for the default production deployment.

Additional services may only extend scale, interoperability, or specialized functionality.

For local development, SQLite should remain sufficient wherever practical.
