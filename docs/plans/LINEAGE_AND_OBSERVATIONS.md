# Lineage and Observations

## Lineage

Support dataset and field-level lineage.

```text
raw.customers -> clean.customers -> analytics.customer_360
```

```text
raw.email_address -> Customer.email -> warehouse.customers.email
```

A `LineageEdge` records source/target assets, optional field paths, relation type, producer, confidence, and observed time.

Relations may include derived, copied, renamed, cast, aggregated, joined, filter dependency, validation dependency, and unknown transform.

Every edge records provenance. Datdex never invents precise lineage when the producer supplies weaker evidence.

## Freshness

Store expectations and observations with `FRESH`, `STALE`, or `UNKNOWN` status.

## Quality

Store bounded summaries:

```text
QualityObservation
- dataset_id
- schema_version_id
- status
- check_count
- passed_count
- failed_count
- producer
- report_ref
- observed_at
```

Do not store unbounded rejected rows.

Observations may come from ShuETL, ETLantic, dbt, Great Expectations, Soda, OpenLineage-compatible systems, or custom applications.
