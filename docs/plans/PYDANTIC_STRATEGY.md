# Pydantic Strategy

## Principle

Datdex should use Pydantic as the canonical metadata-contract and ingestion-validation layer.

> **Pydantic is the metadata contract layer; SQLAlchemy is the persistence layer.**

## Use Pydantic for

- catalog asset records;
- dataset/data-product models;
- dataset locations;
- schema versions;
- field definitions;
- lineage edges;
- ownership/tag/classification records;
- freshness/quality observations;
- normalized ingestion events;
- connector configuration;
- search/filter requests;
- API requests/responses;
- settings;
- JSON Schema/OpenAPI generation.

## Discriminated unions

Use tagged unions for extensible asset, metadata-event, and dataset-location types. Stable discriminator values enable clean adapters and forward-compatible ingestion.

## Validation

Pydantic validators enforce canonical qualified-name structure, field-path grammar, schema-version invariants, URI/location safety, bounded metadata, lineage endpoint consistency, producer/event identity, quality/freshness bounds, and classification/tag namespace rules.

## Schema representation

Datdex defines its normalized schema/field model with Pydantic and provides deterministic conversion adapters from PyArrow schemas, SQLGlot-derived type information, ETLantic/Pydantic models, and external metadata formats.

## JSON Schema

Generated JSON Schema supports OpenAPI, connector/publisher contracts, UI form generation, integration validation, and event version documentation.

## TypeAdapter

Use `TypeAdapter` heavily at ingestion/plugin boundaries to validate discriminated event unions and adapter outputs.

## Settings

Use `pydantic-settings` for database, search, connector, ingestion, and operational-limit configuration.

## Interchange

External formats such as OpenLineage are converted into Datdex-owned Pydantic models before persistence. Third-party SDK models never become Datdex public contracts.

## SQLModel relationship

Use SQLModel when a normalized Pydantic metadata entity also maps naturally to a relational table. Keep ingestion events, adapter payloads, search requests, and interoperability models Pydantic-only when persistence would create unnecessary coupling.
