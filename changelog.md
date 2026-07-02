# Changelog

Schema and feature history for the Colect XML integration.

***

## v1.4 (update)

### Added

* **Product Relations document** — new `productRelations*.xml` input document supporting `MATCHING_SET` and `SUCCESSOR` relation types between products.
* **Customer Size Naming** — per-customer size label overrides via `<customerSizeNaming>` on sizes; customers with a `sizeNamingCode` see custom size names instead of the standard ones.
* **GLN and State** — `gln` (Global Location Number) and `state` (state/province) added to Customers and Locations.
* **Labels** — `<labels>` block on products supports multiple display badges per product, each with `<text>` and `<backgroundColor>`. Replaces the single-badge `<sale>` / `<saleBackgroundColor>` fields.
* **Approval Groups** — optional `<approvalGroupCode>` and `<approvalGroupDesc>` fields on products for routing orders through manager approval workflows configured in the Colect backend.

### Changed

* `<sale>` and `<saleBackgroundColor>` are now **planned for deprecation**. Use `<labels>` instead.

***

## v1.4

### Added

* **Delivery Windows** — `<deliveryWindow>` on products, replacing the deprecated `deliverySubBlocks` field. Supports `code`, `description`, `startDate`, `endDate`, and `sortCode`.
* **Extra Fields sub-feed** — `extraFields*.xml` for updating product extra fields without resending the full product catalog.
* **Historical Orders document** — `historicalOrders*.xml` for pushing past orders into the app for customer reference and reordering.
* **Invoices document** — `invoices*.xml` for surfacing invoice data in the customer portal.
* **Prepack content** — `<prepackContentElement>` on sizes defines the breakdown of sizes inside a prepack/box.

### Deprecated

* `deliverySubBlocks` on products — replaced by `<deliveryWindows>`.

***

## v1.3 and earlier

Contact your Colect contact for change history prior to v1.4.
