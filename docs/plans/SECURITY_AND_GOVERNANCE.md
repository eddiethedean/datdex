# Security and Governance

Datdex does not implement general authentication. It consumes provider-neutral authorization hooks; AuthMate is the reference provider.

Generic resources include:

```text
datdex.dataset
datdex.schema
datdex.lineage
datdex.tag
datdex.classification
datdex.data_product
```

Potential actions include `datdex.dataset.read`, `datdex.dataset.update`, `datdex.schema.publish`, `datdex.lineage.publish`, and `datdex.governance.manage`.

Catalog metadata can itself be sensitive: names, existence, locations, lineage, ownership, and classifications. Authorization must apply to reads and mutations.

Datdex never stores source-system credentials. Validate registered URIs and never automatically fetch arbitrary URLs merely because they are cataloged.

Support ownership, tags, classifications, lifecycle/status, documentation, bounded custom metadata, and provider-neutral audit hooks.

Multi-tenancy is deferred, but resource IDs/scopes must permit future workspace isolation.
