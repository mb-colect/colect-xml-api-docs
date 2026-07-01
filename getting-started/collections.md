# Collections

Colect's data model is organized around **collections**. A collection is a self-contained set of products, customers, access rules, orders, and configuration — and the unit of separation between brands, seasons, or business lines using the same Colect platform.

{% hint style="info" %}
The word "collection" in Colect doesn't necessarily mean a "fashion collection". It just means "a set of products that are provided in one set". A single Colect collection might contain items from multiple brands, seasons, genders, or even legal entities — or it might map exactly to one fashion drop. The contents are defined by your ERP, not by Colect.
{% endhint %}

***

## What lives at the collection level

<table><thead><tr><th width="235.0546875">Domain</th><th>Scope</th></tr></thead><tbody><tr><td><strong>Products</strong></td><td>Each Colect collection has its own product catalog. The same SKU can exist in multiple collections independently.</td></tr><tr><td><strong>Customers</strong></td><td>Customers are scoped to a collection. A customer's currency, price groups, discount groups, and access rules live here.</td></tr><tr><td><strong>Currency</strong></td><td>Default currency is configured per collection.</td></tr><tr><td><strong>Orders</strong></td><td>Orders are placed against a specific collection — <code>&#x3C;collectionId></code> on the <a href="../documents/orders.md">Orders output document</a> records which one.</td></tr><tr><td><strong>Access rules</strong></td><td>Customer Access and Product Access are evaluated within the collection.</td></tr><tr><td><strong>Business-logic toggles</strong></td><td>Settings like <code>discountGroupFilter</code> (see <a href="../business-logic/multi-promotions.md">Multi Promotions</a>) live here.</td></tr></tbody></table>

***

## One ERP, multiple collections

It's common for a single ERP to drive several Colect collections — for example one per brand, or one per market. Each gets its own set of XML documents.

```
                    ┌─────────────────────────────────┐
                    │           Your ERP              │
                    └────────────┬────────────────────┘
                                 │
                ┌────────────────┼────────────────┐
                │                │                │
                ▼                ▼                ▼
       ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
       │ Collection: │    │ Collection: │    │ Collection: │
       │   Brand A   │    │   Brand B   │    │   Outlet    │
       │  (SS25)     │    │  (SS25)     │    │  (FW24)     │
       └─────────────┘    └─────────────┘    └─────────────┘
```

Each collection has its own document set (one Products doc per collection, one Customers doc per collection, etc.).

There's no "cross-collection" document — products in Brand A's collection are completely independent of products in Brand B's collection, even if they share a SKU number.

***

## Identifying the collection on documents

The XML documents themselves don't carry an explicit `collectionId` (except on the [Orders output document](../documents/orders.md), which echoes it back so your ERP knows where the order came from). Routing is implicit:

<table><thead><tr><th width="168.953125">Channel</th><th>How the collection is identified</th></tr></thead><tbody><tr><td><strong>SFTP</strong></td><td>The collection is determined by the file path configured in your connector settings. A single SFTP account can serve multiple collections, each with its own configured file paths.</td></tr></tbody></table>

Your Colect contact configures the file paths for each collection during onboarding. See [Document Transfer](document-transfer.md) for details.

***

## When to use multiple collections vs. one larger collection

| Situation                                                  | Recommended structure                                                           |
| ---------------------------------------------------------- | ------------------------------------------------------------------------------- |
| Two distinct brands with separate sales teams and websites | **Two collections**, one per brand                                              |
| Same brand, multiple markets with different currency mixes | **One collection** (use price groups + currency)                                |
| Pre-order season vs. immediate stock                       | **Two collections** — pre-order and in-stock are separate buying contexts       |
| Mainline vs. outlet                                        | **Two collections** if pricing/customers differ significantly                   |
| Different business units sharing the same ERP              | **Two collections** — keeps customer lists clean                                |

When in doubt, talk to your Colect contact. Splitting a collection later requires re-importing master data; merging two collections is harder still.

***

## Where collections appear in this documentation

* [Document Transfer](document-transfer.md) — how file paths route documents to a collection.
* [Orders output document](../documents/orders.md) — `<collectionId>` field that identifies the collection on every order.
* [Customers](../documents/customers.md) — `currencyCode` and `activePriceGroup` are scoped to the collection's configured currencies and price groups.
