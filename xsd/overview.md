# XSD Reference

Every input and output document in the XML integration has a corresponding XSD. This section links to the live schema for each document, and shows a fragment so you can see the shape without leaving these docs.

{% hint style="danger" %}
**Use the right host.** The XML integration's schemas are hosted at `assets.apptitude.nl`. Colect's connector also exposes `connector.colect.services/services/api/3.0?xsd=1` and `?xsd=2` — that is the auto-generated schema for the **SOAP API**, a completely different integration (synchronous, call-based) with different field names (`productColorCode` instead of `colorCode`, for example). It describes the same underlying concepts but not the XML file format. Don't use it to validate or generate XML documents.
{% endhint %}

{% hint style="warning" %}
**XSDs are informational, not authoritative.** Even the correct XSDs below reflect Colect's internal/export model rather than the exact import format. Field names, attributes, and enum values occasionally differ from what the connector actually accepts on import — the document pages under **Documents** are live-tested and take precedence over the XSD. See [Fix XSDs to reflect import format](https://github.com/mb-colect/colect-xml-api-docs/issues/13).
{% endhint %}

## Documents and their schemas

| Document | XSD | Live schema |
| --- | --- | --- |
| [Products](products.md) | `Product_Feed_XSD.xsd` | [assets.apptitude.nl](https://assets.apptitude.nl/Documentation/Product_Feed_XSD.xsd) |
| [Customers](customers.md) | `Customer_Feed_XSD.xsd` | [assets.apptitude.nl](https://assets.apptitude.nl/Documentation/Customer_Feed_XSD.xsd) |
| [Customer Access](customer-access.md) | `Customer_Access_XSD.xsd` | [assets.apptitude.nl](https://assets.apptitude.nl/Documentation/Customer_Access_XSD.xsd) |
| [Product Access](product-access.md) | `ProductAccess_Feed_XSD.xsd` | [assets.apptitude.nl](https://assets.apptitude.nl/Documentation/ProductAccess_Feed_XSD.xsd) |
| [Product Relations](product-relations.md) | `ProductRelations_Feed_XSD.xsd` | not yet published ([#12](https://github.com/mb-colect/colect-xml-api-docs/issues/12)) |
| [Prices](prices.md) | `ProductPrices_Feed_XSD.xsd` | [assets.apptitude.nl](https://assets.apptitude.nl/Documentation/ProductPrices_Feed_XSD.xsd) |
| [Sizes & Stock](sizes-stock.md) | `ProductSizes_Feed_XSD.xsd` | [assets.apptitude.nl](https://assets.apptitude.nl/Documentation/ProductSizes_Feed_XSD.xsd) |
| [Extra Fields](extra-fields.md) | `ProductExtraField_Feed_XSD.xsd` | [assets.apptitude.nl](https://assets.apptitude.nl/Documentation/ProductExtraField_Feed_XSD.xsd) |
| [Invoices](invoices.md) | `Invoices_Feed_XSD.xsd` | [assets.apptitude.nl](https://assets.apptitude.nl/Documentation/Invoices_Feed_XSD.xsd) |
| [Historical Orders](historical-orders.md) | `HistoricalOrders_Feed_XSD.xsd` | [assets.apptitude.nl](https://assets.apptitude.nl/Documentation/HistoricalOrders_Feed_XSD.xsd) |
| [Orders (output)](orders.md) | `OrderOutput_Feed_XSD.xsd` | [assets.apptitude.nl](https://assets.apptitude.nl/Documentation/OrderOutput_Feed_XSD.xsd) |
