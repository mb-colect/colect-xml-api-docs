# Overview

[Your First Document](../getting-started/first-document.md) got one product into a collection to prove the pipeline works. This section goes further: build a small but real demo shop, one document type at a time, and see each push reflected in the app.

***

## The brand: Atoll

**Atoll** is a fictional swimwear brand used throughout this section. It's a small catalog on purpose — enough to show how documents connect to each other (a matching top and bottom, customers who can log in with different currencies) without the noise of a full production catalog.

{% hint style="info" %}
Everything here is trimmed down for readability. A real Atoll catalog has 8 products across 5 styles and 6 customers across 5 countries — this walkthrough uses 2 products and 2 customers so each step stays short enough to copy-paste in full.
{% endhint %}

***

## What you'll build

| Step                                          | Document pushed                        | What appears in the app                                          |
| ---------------------------------------------- | --------------------------------------- | ------------------------------------------------------------------ |
| [The Catalog](catalog.md)                      | Products                                | A bikini top and matching bottom, each with sizes, prices, media   |
| [Stock](stock.md)                              | Sizes & Stock (sub-feed)                | Preorder stock quantities on the order grid                        |
| [Matching Sets](relations.md)                  | Product Relations                       | The top and bottom surfaced as a matching set                      |
| [Customers & Access](customers.md)             | Customers, Customer Access              | Two boutique customers, each visible to their assigned sales rep   |

By the end you'll have a working shop: a rep can log in, see the customers they're allowed to sell to, browse a catalog with a styled matching set, and check stock — all from documents you wrote yourself.

***

## Before you start

You'll need the same SFTP credentials and connector setup from [Document Transfer](../getting-started/document-transfer.md). If you haven't pushed a document before, do [Your First Document](../getting-started/first-document.md) first — this section assumes you know how to write and upload an XML file.

Each step in this section follows the same shape: write the document, push it, verify it in the app. Start with [The Catalog](catalog.md).
