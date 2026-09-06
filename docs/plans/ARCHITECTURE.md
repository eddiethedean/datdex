# Datdex Architecture

```text
FastAPI
   |
Datdex Router
   |
Datdex Services
 |      |       |
Catalog Lineage Discovery
   \    |    /
   Metadata Store
       |
PostgreSQL / SQLite
```

## Core services

Catalog owns datasets, data products, owners, tags, and locations. Schema owns immutable schema versions and fields. Lineage owns dataset/field relationships. Discovery owns search/filtering. Observation owns freshness and quality metadata. Ingestion normalizes producer events.

## Composition rule

**Independent by default, composable by contract.**

Datdex core must not import Hedron, AuthMate, ShuETL, or ETLantic. Integration occurs through FastAPI, stable Python protocols, generic references, metadata publishing contracts, optional adapters, and compatibility tests.

A requirement for Datdex core to understand a ShuETL ORM object, ETLantic internal node, Hedron component, or AuthMate ORM model is an architectural smell.

## Persistence

Use SQLAlchemy 2.x and Alembic. SQLite is the development backend; PostgreSQL is the production reference. Datdex owns its tables/migrations even in a shared database.

## Dependency boundary

```text
Datdex domain/services
   ├── SQLGlot adapter
   ├── PyArrow metadata adapter
   ├── fsspec/UPath filesystem adapter
   ├── OpenLineage adapter
   └── quality adapters
       ├── Great Expectations
       └── Soda
```

These libraries provide mechanics/evidence; Datdex normalizes and persists metadata with provenance.

## Pydantic contract layer

Datdex normalizes external metadata into Datdex-owned Pydantic models before persistence, preventing SQLGlot, PyArrow, OpenLineage, or connector-specific models from leaking into the catalog domain.

## SQLModel-first persistence strategy

Use **SQLModel** by default where it cleanly unifies Pydantic domain models with relational persistence.

> **Prefer SQLModel for ordinary persisted domain entities; use SQLAlchemy directly for advanced persistence mechanics.**

SQLAlchemy remains available for complex joins/window queries, explicit transaction/control flow, advisory locks, bulk operations, engine/session configuration, backend-specific features, migration internals, and performance-critical paths.

Alembic remains the migration tool. Do not force SQLModel where a plain Pydantic or direct SQLAlchemy model is clearer.

## FastAPI runtime composition

DI composes sessions, authorization, search, publisher, connector, and audit providers. FastAPI streaming/OpenAPI features are used directly rather than introducing separate service infrastructure for common catalog API needs.

## Infrastructure baseline

Default deployment:

```text
FastAPI application
        +
relational SQL database
```

No other service is required for core functionality. Redis/RabbitMQ, Kafka, OpenSearch/Elasticsearch, object stores, Vault/cloud secret managers, external schedulers, and separate worker fleets are optional extensions only.
