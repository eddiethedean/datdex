# Metadata Model

## CatalogAsset

```text
CatalogAsset
- id
- asset_type
- qualified_name
- display_name
- description
- status
- created_at
- updated_at
```

MVP asset types are `DATASET` and `DATA_PRODUCT`. Add pipeline/dashboard/model/API/topic only when a clear metadata contract exists.

## Dataset

A Dataset is a logical data asset with a stable ID, qualified name, type, ownership, tags/classifications, and current schema version.

Examples:

```text
warehouse.analytics.customers
s3://analytics/orders/
api://crm/customers
```

## DatasetLocation

A logical dataset may have multiple physical representations:

```text
DatasetLocation
- id
- dataset_id
- location_type
- uri
- environment
- bounded metadata
```

Datdex records locations but does not copy the data.

## SchemaVersion

Immutable semantic schema version:

```text
SchemaVersion
- id
- dataset_id
- version
- fingerprint
- observed_at
- producer
```

## Field

```text
Field
- id
- schema_version_id
- canonical_path
- name
- data_type
- nullable
- description
- ordinal
- constraints
```

Nested paths support `customer.address.zip` and `orders[].product.id`.

## Ownership

Use provider-neutral `OwnerRef(type, id, display_name)` references. Owners may come from AuthMate, directories, teams, or other systems.

## Tags and classifications

Tags are flexible namespaced metadata such as `domain:finance`. Classifications represent stronger governance metadata and use organization-defined vocabularies.

## External references

Assets may reference warehouses, object URIs, dashboards, source IDs, or artifacts. Credentials never belong in catalog metadata.

## Pydantic normalized model

The normalized Datdex metadata model should be expressed as Pydantic models, including datasets, fields, schema versions, locations, owner refs, tags, classifications, and bounded custom properties.

Use discriminated unions for asset and location types.

## SQLModel persistence

Prefer SQLModel for ordinary persisted catalog entities such as CatalogAsset, Dataset, DatasetLocation, SchemaVersion, Field, Tag, Classification, OwnerBinding, LineageEdge, FreshnessObservation, and QualityObservation.

Keep normalized ingestion/event models Pydantic-only where they do not map 1:1 to rows.

Use direct SQLAlchemy for recursive lineage queries, advanced PostgreSQL search, bulk metadata ingestion, graph-like traversal queries, and backend-specific indexing.
