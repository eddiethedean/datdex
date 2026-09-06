# Search and Discovery

MVP must not require Elasticsearch/OpenSearch.

Search/filter by qualified/display name, description, tags, classification, owner, dataset type, location/environment, freshness, and quality.

Ranking remains deterministic and explainable: exact qualified-name, prefix/name token, description token, tag, and owner matches.

PostgreSQL FTS/trigram can enhance the reference implementation. External search or semantic/vector search remains optional later.

## Search dependency strategy

The default deployment must not require Elasticsearch/OpenSearch.

Use PostgreSQL full-text/trigram/index capabilities as the first production search path. Future external or semantic search providers remain optional behind a Datdex-owned `SearchProvider`.

## Streaming discovery/export

Normal search remains paginated.

Bulk export endpoints should use JSON Lines streaming with typed Pydantic records, preserving bounded memory usage.

## SQL-only search baseline

Datdex search must remain fully functional without Elasticsearch/OpenSearch.

The production reference path should use PostgreSQL-native capabilities such as:

- full-text search;
- trigram indexes;
- normal B-tree/GIN/GiST indexes where appropriate;
- deterministic filtering/faceting queries.

External search engines are optional `SearchProvider` implementations only.
