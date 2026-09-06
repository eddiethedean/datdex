# Ingestion and Integrations

## Publisher contract

Producers publish normalized metadata without Datdex ORM coupling:

```python
await catalog.publish(DatasetObserved(...))
await catalog.publish(SchemaObserved(...))
await catalog.publish(LineageObserved(...))
await catalog.publish(FreshnessObserved(...))
await catalog.publish(QualityObserved(...))
```

Events carry producer, external event ID, observed time, and bounded payload. Producer/event IDs provide idempotency. Observed and ingestion time remain distinct.

## ShuETL

May publish output datasets, schemas, run references, freshness, quality summaries, artifact/location references, and lineage. Catalog publication policy must be explicit and not silently redefine pipeline execution success.

## ETLantic

May publish contract metadata, schema observations, field lineage, validation summaries, and drift metadata using public ETLantic records.

## AuthMate

May provide current principal, `datdex.*` authorization, owner refs, and audit. Datdex remains provider-neutral.

## Hedron

Optional adapter may provide catalog browsing, dataset detail, schema explorer, lineage visualization, freshness/quality status, and governance screens.

## OpenLineage and connectors

Future optional adapters may ingest OpenLineage and introspect databases, warehouses, object stores, Parquet/Arrow, and APIs. Support both push publishing and pull discovery.

## Recommended integration libraries

- **SQLGlot** for SQL parsing and supported table/column lineage.
- **PyArrow** for Arrow/Parquet schema and file metadata inspection.
- **fsspec + universal-pathlib** for normalized filesystem/object-store access.
- **OpenLineage** for optional lineage/run metadata interoperability.
- **Great Expectations / Soda** as external quality producers rather than a Datdex-native quality engine.

## Pydantic ingestion boundary

Every ingestion adapter must convert third-party structures into Datdex-owned Pydantic events before catalog services process or persist them.

Use discriminated event unions and `TypeAdapter` for efficient validation of heterogeneous publisher payloads.
