---
name: underdog-cart-and-checkout
description: Build a cart on underdog.shop and take it through a buyer-approved UCP checkout.
api: mcp/underdog-mcp.yml
endpoint: https://underdog.shop/api/ucp/mcp
operations: [create_cart, get_cart, update_cart, cancel_cart, create_checkout, get_checkout, update_checkout, complete_checkout, cancel_checkout, get_order]
generated: '2026-09-16'
method: generated
source: mcp/underdog-ucp-mcp-tools.json, https://underdog.shop/agents.md, https://underdog.shop/policies/terms-of-sale
---

# Cart and checkout at Underdog

1. **Cart** — `create_cart` with the chosen variant ids; inspect with `get_cart`; change lines with `update_cart`; abandon with `cancel_cart`.
2. **Checkout** — `create_checkout`, then `update_checkout` to set the delivery address and shipping method (delivery is within metropolitan France). Read state with `get_checkout`.
3. **Buyer approval** — Underdog's agents.md is explicit: *checkout requires human approval*. Do not call `complete_checkout` without contemporaneous, explicit buyer consent. If you cannot get it, route the purchase through Shop Pay via https://shop.app/SKILL.md instead.
4. **Complete** — `complete_checkout` requires `meta.idempotency-key`. Generate one per purchase attempt and reuse it on retries; it is the only write with replay protection, so do not blindly retry the other tools.
5. **Order** — `get_order` requires a Shopify agent JWT (JSON-RPC `-32000` AuthenticationRequired otherwise).
6. **Abort** — `cancel_checkout` before completion.

Reversibility: once an order is placed there is no API refund. The buyer has a 14-day right of withdrawal from receipt (CGV section 11) and a 24-month commercial warranty (section 10.2) — tell the buyer before completing.
