# Collections

Colect's data model is organized around **collections**. A collection is a self-contained set of products, customers, access rules, orders, and configuration — and the unit of separation between brands, seasons, or business lines using the same Colect platform.

{% hint style="info" %}
The word "collection" in Colect doesn't necessarily mean a "fashion collection". It just means "a set of products that are provided in one set". A single Colect collection might contain items from multiple brands, seasons, genders, or even legal entities — or it might map exactly to one fashion drop. The contents are defined by your ERP, not by Colect.
{% endhint %}

***

## What lives at the collection level

| Domain                     | Scope                                                                                                              |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| **Products**               | Each Colect collection has its own product catalog. The same SKU can exist in multiple collections independently.   |
| **Customers**              | Customers are scoped to a collection. A customer's price groups, discount groups, and access rules live here.       |
| **Pricing & currencies**   | Default currencies and price-group definitions are configured per collection.                                       |
| **Orders**                 | Orders are placed against a specific collection — `<collectionId>` on the [Orders output document](../documents/orders.md) records which one. |
| **Access rules**           | Customer Access and Product Access are evaluated within the collection.                                              |
| **Business-logic toggles** | Settings like `discountGroupFilter` (see [Multi Promotions](../business-logic/multi-promotions.md)) live here.       |
| **Integration credentials**| SFTP users, HTTP credentials, and webhook endpoints are issued per collection.                                       |

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

Each collection has:

* Its own SFTP user and inbox/outbox.
* Its own document set (one Products doc per collection, one Customers doc per collection, etc.).
* Its own settings configured by your Colect Support contact.

There's no "cross-collection" document — products in Brand A's collection are completely independent of products in Brand B's collection, even if they share a SKU number.

***

## Identifying the collection on documents

The XML documents themselves don't carry an explicit `collectionId` (except on the [Orders output document](../documents/orders.md), which echoes it back so your ERP knows where the order came from). Routing is implicit:

| Channel        | How the collection is identified                                                                  |
| -------------- | ------------------------------------------------------------------------------------------------- |
| **SFTP**       | The collection is determined by which SFTP user uploaded the file.                                |
| **HTTP POST**  | The collection is determined by the credentials presented (Basic Auth, API key, or signed URL).   |

When you're configuring the integration, your Colect Support contact issues credentials per collection. The credentials are the routing key.

***

## When to use multiple collections vs. one larger collection

| Situation                                                              | Recommended structure                              |
| ---------------------------------------------------------------------- | -------------------------------------------------- |
| Two distinct brands with separate sales teams and websites              | **Two collections**, one per brand                 |
| Same brand, multiple markets with different currency mixes              | **One collection** (use price groups + currency)   |
| Pre-order season vs. immediate stock                                    | Typically **one collection**, separated by `seasonCode` and `deliveryStartDate` |
| Mainline vs. outlet                                                     | **Two collections** if pricing/customers differ significantly |
| Different business units sharing the same ERP                          | **Two collections** — keeps customer lists clean   |

When in doubt, talk to your Colect Support contact. Splitting a collection later requires re-importing master data; merging two collections is harder still.

***

## Where collections appear in this documentation

* [Document Transfer](document-transfer.md) — how SFTP / HTTP credentials route to a collection.
* [Orders output document](../documents/orders.md) — `<collectionId>` field that identifies the collection on every order.
* [Customers](../documents/customers.md) — `currencyCode` and `activePriceGroup` are scoped to the collection's configured currencies and price groups.
