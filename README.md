# Overview

## Colect XML 1.4

Welcome to the Colect XML integration documentation. This document describes the structure of the XML-based interface offered by Colect — a document-exchange integration that lets your ERP system synchronize products, customers, orders, and related data with the Colect platform.

{% hint style="info" %}
**Choosing an integration:** Colect offers two integrations for ERP systems — the **XML integration** (this documentation) and the [**SOAP API**](https://docs.colect.io/admin/soap-api). The XML integration is asynchronous and file-based, ideal for ERP systems that batch data exports. The SOAP API is synchronous and call-based, ideal for systems that integrate via live web service calls. The two interfaces share the same underlying schema lineage and either can drive the same Colect collection.
{% endhint %}

***

## What the XML integration is

The XML integration is a **document-exchange interface**. Your ERP produces or consumes XML documents — Products, Customers, Orders, and so on — that move between your systems and Colect over a file-transfer channel.

| Domain                | Description                                                                                |
| --------------------- | ------------------------------------------------------------------------------------------ |
| **Products**          | Sync your complete product catalog — sizes, colors, prices, stock, media, delivery windows |
| **Customers**         | Manage customer accounts with pricing groups, discounts, agreements, and locations         |
| **Orders**            | Pull orders placed through the Colect Sales App and B2B Webstore back into your ERP        |
| **Access**            | Control which products are visible to which customers, and which agents can sell to whom   |
| **Invoices**          | Push invoice data for customer visibility in the app                                       |
| **Historical Orders** | Make past orders visible inside the app for reordering and reference                       |

***

## Document transfer

Documents move between your ERP and Colect over a file-transfer channel. The default and most common option is **Colect Secure FTP (SFTP)**. The XML document structure is identical regardless of transport.

```
                    ┌─────────────────────────────────┐
                    │         Colect Platform         │
                    │ (Sales App, B2B Webstore, Cache)│
                    └─────────────────────────────────┘
                                    ▲
                                    │
                    ┌───────────────┴───────────────┐
                    │      Cloud Connector          │
                    │   (interface-specific glue)   │
                    └───────────────┬───────────────┘
                                    │
                    ┌───────────────┴───────────────┐
                    │     SFTP / HTTP / etc.        │
                    └───────────────┬───────────────┘
                                    │
            ┌───────────────────────┼───────────────────────┐
            │                       │                       │
            │                       │                       ▼
    ┌───────────────┐               │               ┌───────────────┐
    │   PUSH DATA   │               │               │   PULL DATA   │
    │───────────────│               │               │───────────────│
    │ • Products    │               │               │ • Orders      │
    │ • Customers   │               │               │               │
    │ • Prices      │               │               │               │
    │ • Sizes/Stock │               │               │               │
    │ • Extra Flds  │               │               │               │
    │ • Access      │               │               │               │
    │ • Invoices    │               │               │               │
    │ • Hist Orders │               │               │               │
    │ • Relations   │               │               │               │
    └───────────────┘               │               └───────────────┘
            │                       │                       ▲
            │                       │                       │
            └───────────────────────┼───────────────────────┘
                                    │
                    ┌───────────────┴───────────────┐
                    │        Your ERP System        │
                    └───────────────────────────────┘
```

| Direction        | Documents                                                                                                                                            | Description                                                      |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- |
| **ERP → Colect** | Products, Customers, Customer Access, Product Access, Product Relations, Prices, Sizes/Stock, Extra Fields, Invoices, Historical Orders, Order Rules | You push master data, access rules, and reference data to Colect |
| **Colect → ERP** | Orders                                                                                                                                               | You pull orders placed by customers and sales reps               |

***

## Document inventory

### Input documents (ERP → Colect)

| Document                                                         | Replaces / Updates                          | Sub-feeds                                                                                |
| ---------------------------------------------------------------- | ------------------------------------------- | ---------------------------------------------------------------------------------------- |
| [Products](documents/products.md)                                | Full product set                            | Prices, Sizes, Media, Delivery Windows, Extra Fields                                     |
| [Customers](documents/customers.md)                              | Full customer set                           | Contacts, Discount Groups, Margin Groups, Locations, Budgets, Custom Choices, Agreements |
| [Customer Access](documents/customer-access.md)                  | Per-user customer visibility                | —                                                                                        |
| [Product Access](documents/product-access.md)                    | Per-customer product visibility             | Add/Remove rules                                                                         |
| [Product Relations](documents/product-relations.md)              | Product relations (Successor, Matching Set) | _new in v1.5_                                                                            |
| [Prices](documents/prices.md)                                    | Prices only (sub-feed)                      | Replaces all prices in product feed                                                      |
| [Sizes & Stock](documents/sizes-stock.md)                        | Sizes & stock levels (sub-feed)             | Stock levels, prepacks                                                                   |
| [Extra Fields](documents/extra-fields.md)                        | Product extra fields (sub-feed)             | —                                                                                        |
| [Invoices](documents/invoices.md)                                | Invoice list                                | —                                                                                        |
| [Historical Orders](documents/historical-orders.md)              | Historical orders                           | Order Lines, Shipping Locations                                                          |
| [Order Discount & Delivery Cost Rules](documents/order-rules.md) | Order-level rules                           | —                                                                                        |

### Output documents (Colect → ERP)

| Document                      | Description                                                              |
| ----------------------------- | ------------------------------------------------------------------------ |
| [Orders](documents/orders.md) | Orders placed in the Sales App or B2B Webstore, ready for ERP processing |

***

## Sync patterns

The XML integration uses two basic sync patterns. Each input document declares which one applies.

{% tabs %}
{% tab title="Full set" %}
**Documents:** Products, Customers, Customer Access, Prices, Sizes/Stock, Extra Fields

The document is treated as the complete set. Items not included in the document are removed from Colect.

```
Before: [A, B, C, D]
Document: [A, B, E]
After:  [A, B, E]   ← C and D removed
```

**Use case:** Daily full export from ERP master data.
{% endtab %}

{% tab title="Sub-feed replace" %}
**Documents:** Prices, Sizes/Stock, Extra Fields

These documents only contain `uniqueId` + `colorCode` plus the relevant sub-feed (e.g. `<prices>`). The sub-feed is replaced for each product included; products not in the document are unchanged.

```
Before: Product A has prices [P1, P2, P3]
Document: Product A with prices [P4, P5]
After:  Product A has prices [P4, P5]
```

**Use case:** Frequent price updates without re-sending the full product catalog.
{% endtab %}
{% endtabs %}

***

## Next steps

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><strong>Document Transfer</strong></td><td>Set up SFTP / HTTP delivery and file naming</td><td><a href="getting-started/document-transfer.md">document-transfer.md</a></td></tr><tr><td><strong>Collections</strong></td><td>Understand how data is grouped at the collection level</td><td><a href="getting-started/collections.md">collections.md</a></td></tr><tr><td><strong>Your First Document</strong></td><td>Walk through a minimal Products push end-to-end</td><td><a href="getting-started/first-document.md">first-document.md</a></td></tr><tr><td><strong>Document Reference</strong></td><td>Field-by-field documentation for all 12 documents</td><td><a href="documents/products.md">products.md</a></td></tr></tbody></table>

***

## Changelog

See the [full changelog](changelog.md) for the complete schema and feature history. Highlights since v1.4:

* **Product Relations input document** — new in v1.5, supports Successor and Matching Set relation types
* **Customer Size Naming** — added per-customer size labels on the Sizes sub-feed
* **GLN and State** — added on Customers and Locations for Global Location Number and state/province
