# Datdex Vision

## Purpose

Datdex adds a lightweight data catalog to any FastAPI application. It provides APIs for datasets, data products, physical locations, schemas, fields, ownership, tags, classifications, lineage, freshness, quality summaries, metadata history, and discovery.

Datdex stores **metadata about data**, not underlying datasets.

## Positioning

Datdex should not reproduce enterprise metadata platforms feature-for-feature. Its differentiation is embeddability, Python/FastAPI-native composition, small operational footprint, standalone usefulness, and protocol-driven integration.

## Example

```python
from fastapi import FastAPI
from datdex import Datdex

app = FastAPI()
datdex = Datdex(database_url="postgresql://...")
app.include_router(datdex.router, prefix="/catalog")
```

## Non-goals

- storing arbitrary datasets;
- becoming a query engine;
- becoming a workflow orchestrator;
- implementing general authentication;
- requiring ShuETL or ETLantic;
- matching enterprise catalogs on connector count or governance breadth.
