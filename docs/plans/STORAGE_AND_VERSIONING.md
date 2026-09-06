# Storage and Versioning

Use SQLAlchemy 2.x + Alembic, SQLite for development, PostgreSQL for production reference.

Schema versions and lineage/quality/freshness observations should be append-oriented or immutable where practical. Current state can be projected from history.

Datasets have stable Datdex IDs plus qualified names. Renaming is explicit and preserves identity.

Schema fingerprints derive deterministically from normalized semantic schema content and exclude timestamps/database IDs.

Prefer archive/tombstone behavior over destructive deletion for assets with history.

Descriptions, custom properties, events, quality summaries, and imported metadata are bounded. Datdex is not an arbitrary JSON store.

Use optimistic concurrency/version fields for mutable descriptive/governance metadata where appropriate.

## SQL-only persistence baseline

All core catalog state—assets, schemas, fields, ownership, tags, classifications, lineage, quality, freshness, and history—must be representable in the relational SQL backend.

No graph database, document store, search cluster, object store, or event broker is required for core Datdex behavior.
