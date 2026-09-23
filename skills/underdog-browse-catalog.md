---
name: underdog-browse-catalog
description: Find refurbished appliances in Underdog's catalogue and resolve full product detail over its UCP MCP endpoint.
api: mcp/underdog-mcp.yml
endpoint: https://underdog.shop/api/ucp/mcp
operations: [search_catalog, lookup_catalog, get_product]
generated: '2026-09-16'
method: generated
source: mcp/underdog-ucp-mcp-tools.json, https://underdog.shop/agents.md
---

# Browse the Underdog catalogue

Underdog (underdog.shop) sells professionally refurbished large appliances and TVs in France, priced in EUR.

1. **Discover** — `GET https://underdog.shop/.well-known/ucp` and confirm `dev.ucp.shopping.catalog.search` is listed.
2. **Search** — call `search_catalog` with `meta.ucp-agent.profile` (your reachable UCP agent profile URI) and `catalog.query` (French terms such as `lave-linge`, `réfrigérateur` work best). Pass `context.address_country: FR` and `context.currency: EUR`. Page with `pagination.cursor`.
3. **Resolve** — call `lookup_catalog` for several product/variant ids at once, or `get_product` for one product with its variants.
4. **Quote prices correctly** — amounts are integers in minor units; `{"amount": 49900, "currency": "EUR"}` is €499.00.

Rules: back off on HTTP 429 (the endpoint is rate-limited per IP). A missing or unreachable agent profile returns JSON-RPC `-32001` "UCP discovery failed" (see `errors/underdog-problem-types.yml`). For read-only browsing without MCP, `GET /products.json` and `GET /products/{handle}.json` need no authentication.
