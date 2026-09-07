# Datdex Extensibility

## Principle

> **Useful defaults, extensible by contract.**

Every major Datdex catalog capability should expose a stable, typed extension surface where practical. Applications may extend supported metadata models, asset/location types, ingestion events, connectors, lineage relations, search, quality/freshness observations, governance metadata, publishers/exporters, and lifecycle behavior without forking Datdex.

> **Extensions must not weaken catalog truth/provenance invariants implicitly.**

Datdex remains authoritative for stable asset identity, metadata provenance, schema history, lineage evidence, bounded metadata, authorization boundaries, and explicit unknown/unsupported states.

## Extensible SQLModel metadata

Expose non-table SQLModel bases for selected persisted catalog entities where organizations commonly need additional metadata.

Potential supported bases:

```text
Dataset
DataProduct
DatasetLocation
selected governance metadata
```

Conceptually:

```python
class Dataset(DatdexDatasetBase, table=True):
    __tablename__ = "datdex_datasets"

    business_domain: str | None = None
    cost_center: str | None = None
```

Core identity/provenance/history fields remain required and protected.

Do not make every internal table extensible. Schema-version internals, event-idempotency records, and other correctness-critical persistence may remain Datdex-owned.

## Managed Alembic migrations

Developers should not need the Alembic CLI for normal supported Datdex model extensions.

Expose:

```python
datdex.schema.status()
datdex.schema.plan()
datdex.schema.check()
datdex.schema.upgrade()
```

with optional:

```python
Datdex(
    dataset_model=Dataset,
    auto_migrate="safe",
)
```

Safe automatic changes are additive and validated. Destructive or ambiguous schema changes are blocked for explicit action.

Datdex owns its migration namespace even when sharing a database with AuthMate/ShuETL.

## Extensible asset types

Datdex MVP ships Dataset and DataProduct. New asset types should be registered through typed Pydantic/SQLModel contracts rather than arbitrary JSON records.

Potential future types include:

```text
Pipeline
Dashboard
Model
API
Topic
Report
```

A registered asset type must define stable identity, serialization, search/index behavior, authorization resource naming, and relationship semantics.

## Extensible locations

Dataset locations use discriminated Pydantic models and provider adapters. Applications can add new location types without changing the core dataset model.

Location extensions must never imply that Datdex owns or fetches the underlying data unless an explicit connector operation is invoked and authorized.

## Ingestion event extensions

Datdex owns a stable event envelope containing producer, external event ID, event/schema version, observed time, ingestion time, provenance, and bounded payload.

Applications/adapters may register typed Pydantic event payloads beyond core DatasetObserved, SchemaObserved, LineageObserved, FreshnessObserved, and QualityObserved events.

Idempotency and provenance remain mandatory.

## Connector extensions

Define stable connector/discovery protocols for relational databases, warehouses, object stores, files, APIs, and organization-specific systems.

Connectors normalize discovered evidence into Datdex-owned Pydantic metadata/events before persistence.

Third-party SDK models never become Datdex public contracts.

## Lineage extensions

Allow registration of additional typed lineage relation kinds and extraction providers while preserving Datdex's core lineage edge envelope.

Extensions must record producer/evidence/confidence where applicable. They may not fabricate field-level lineage from weaker dataset-level evidence.

## Search providers

Datdex ships SQL/PostgreSQL search. A `SearchProvider` protocol permits optional OpenSearch, semantic/vector, or organization-specific search backends.

All providers return Datdex-owned result models and obey the same authorization/filter semantics.

## Quality and freshness providers

Datdex does not become a general quality engine. Applications can register adapters/normalizers for Great Expectations, Soda, ETLantic, ShuETL, or custom producers.

Custom observation types use bounded Pydantic payloads and preserve producer/time/schema-version provenance.

## Governance extensions

Allow organization-defined classification vocabularies, tag namespaces, stewardship/owner roles, lifecycle states, and typed bounded custom properties.

Extensions cannot silently change authorization semantics; governance metadata and access control remain separate contracts unless an explicit policy provider links them.

## Export/interoperability providers

Provide typed exporters/adapters for OpenLineage and future metadata standards without making those external schemas Datdex's internal model.

## Lifecycle hooks

Potential typed hooks/events include:

```text
before_asset_register
after_asset_register
before_schema_publish
after_schema_publish
before_lineage_publish
after_lineage_publish
before_observation_publish
after_observation_publish
before_classification_change
after_classification_change
```

Hook ordering, transaction semantics, timeouts, and failure behavior must be documented. Security/provenance-sensitive pre-hooks should fail closed where appropriate.

## Registration surface

Prefer explicit registration:

```python
datdex.register_asset_type(...)
datdex.register_location_type(...)
datdex.register_event_type(...)
datdex.register_connector(...)
datdex.register_lineage_provider(...)
datdex.register_search_provider(...)
datdex.register_observation_adapter(...)
datdex.register_exporter(...)
datdex.register_hook(...)
```

## Extension conformance

Ship conformance tests covering Pydantic validation, persistence normalization, provenance, idempotency, authorization filtering, bounded metadata, async cleanup, unsupported/unknown behavior, OpenAPI compatibility, and FastAPI dependency overrides.
