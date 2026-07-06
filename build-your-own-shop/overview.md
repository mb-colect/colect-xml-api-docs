# Overview

[Your First Document](../getting-started/first-document.md) got one product into a collection to prove the pipeline works. This section goes further: build a small but real demo shop, one document type at a time, and see each push reflected in the app.

***

## The brand: Atoll

**Atoll** is a fictional swimwear brand used throughout this section — a real (if small) catalog rather than a single placeholder product, so you can see how documents connect to each other: stock tied to a catalog, relations tied to two products, access tied to customers who must already exist.

***

## What you'll build

| Step                                          | Document pushed                        | What appears in the app                                                     |
| ---------------------------------------------- | --------------------------------------- | ------------------------------------------------------------------------------ |
| [The Catalog](catalog.md)                      | Products                                | 8 Atoll products across 5 styles, sold out (no stock yet)                      |
| [Stock](stock.md)                              | Sizes & Stock (sub-feed)                | Every size flips to Available, with a Feb 2028 restock on some                 |
| [Matching Sets](relations.md)                  | Product Relations                       | The Coral (and Black) bikini top and bottom surfaced as a matching set         |
| [Customers & Access](customers.md)             | Customers, Customer Access              | 6 boutique customers across 5 countries, each visible only to their sales rep  |

By the end you'll have a working shop: two sales reps who can log in and each see only their own territory, a catalog with real stock and a styled matching set — all from documents you wrote yourself. See [What's Next](next-steps.md) once you're done.

***

## Before you start

You'll need the same SFTP credentials and connector setup from [Document Transfer](../getting-started/document-transfer.md). If you haven't pushed a document before, do [Your First Document](../getting-started/first-document.md) first — this section assumes you know how to write and upload an XML file.

Each step in this section follows the same shape: write the document, push it, verify it in the app. Start with [The Catalog](catalog.md).
