# Data Types Overview

This reference documents all data types used in the Colect SOAP API v3.0.

---

## Type Categories

<table data-view="cards">
<thead>
<tr>
<th></th>
<th></th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>Product Types</strong></td>
<td>XProduct, XSize, XPrice, XMedium, XDeliveryWindow, and related types</td>
</tr>
<tr>
<td><strong>Customer Types</strong></td>
<td>XCustomer, XLocation, XContact, XAgreement, and related types</td>
</tr>
<tr>
<td><strong>Order Types</strong></td>
<td>XOrder, XOrderLine, XHistoricalOrder, XInvoice, and related types</td>
</tr>
<tr>
<td><strong>Enumerations</strong></td>
<td>All enum types for status, types, and options</td>
</tr>
</tbody>
</table>

---

## Naming Conventions

### XML Element Names

The API uses specific naming conventions for XML elements:

| Data Type | XML Element Pattern | Example |
|-----------|---------------------|---------|
| Single object | camelCase | `<product>`, `<customer>` |
| List element | Singular name | `<size>`, `<price>`, `<medium>` |
| Collection | Multiple elements | Multiple `<size>` elements |

{% hint style="info" %}
**WS: Prefix Note:** Fields prefixed with "WS:" indicate a different XML element name. For example, `prices (WS:price)` means the field is named `prices` but the XML element is `<price>`.
{% endhint %}

### Type Suffixes

All types use the `_3_0` suffix indicating API version 3.0:
- `XProduct30` (internal) → `XProduct_3_0` (documentation)
- `XCustomer30` (internal) → `XCustomer_3_0` (documentation)

---

## Common Patterns

### Mandatory vs Optional Fields

Fields are documented with their requirement status:

| Marker | Meaning |
|--------|---------|
| **Required** | Must be provided |
| Optional | Can be omitted |
| Conditional | Required under certain conditions |

### Date/Time Format

All dates use ISO 8601 format:

```xml
<api:timestamp>2025-01-15T14:30:00</api:timestamp>
<api:startDate>2025-02-01T00:00:00</api:startDate>
```

### Boolean Values

```xml
<api:net>true</api:net>
<api:net>false</api:net>
```

### Decimal Values

Use period as decimal separator:

```xml
<api:price>25.50</api:price>
<api:percentage>10.5</api:percentage>
```

---

## Type Hierarchy

```
XObject30 (base)
├── XProduct30
├── XCustomer30
├── XOrder30
├── XOrderLine30
├── XSize30
├── XPrice30
├── XMedium30
├── XLocation30
├── XContact30
├── XAgreement30
├── XDeliveryWindow30
├── XExtraField30
├── XLabel30
├── XTag30
├── XStockLevel30
├── XDiscountGroup30
├── XMarginGroup30
├── XProductAccessRule30
├── XCustomerAccessRecord30
├── XAgentCustomerAccess30
├── XHistoricalOrder30
├── XHistoricalOrderLine30
├── XInvoice30
├── XProductRelation30
├── XBudgetComponent30
├── XCustomChoice30
├── XOrderAmountModificationRule30
├── XMinimumQuantity30
├── XPrepackContentElement30
├── XCustomerSizeNaming30
├── XCustomerApprovalGroup30
├── XPriceInfoElement30
├── XStockInfoElement30
└── ... (translation types)
```

---

## Quick Reference

### Primary Identifiers

| Entity | Identifier Fields |
|--------|-------------------|
| Product | `uniqueId` + `colorCode` |
| Customer | `customerNo` |
| Order | `orderNumber` |
| Invoice | `customerNo` + `invoiceNumber` |
| Size | `name` (within product) |
| Location | `code` (within customer) |
| Contact | `code` (within customer) |

### Commonly Used Types

| Type | Purpose |
|------|---------|
| `XProduct30` | Product catalog item |
| `XCustomer30` | B2B customer account |
| `XOrder30` | Order from Colect |
| `XOrderLine30` | Line item in order |
| `XSize30` | Product size with stock |
| `XPrice30` | Product pricing |
| `XLocation30` | Shipping address |
| `XHistoricalOrder30` | Past order for display |
| `XInvoice30` | Customer invoice |
| `XPriceInfoElement30` | Bulk price update element |
| `XStockInfoElement30` | Bulk stock update element |

---

## Namespace Reference

```xml
xmlns:api="http://api.cc.salesapp.apptitude.nl/"
xmlns:ws="http://ws.cc.salesapp.apptitude.nl/"
```

The `ws` namespace is specifically used for:
- `productRelation` element in product relation operations

---

## Schema Locations

| Schema | URL |
|--------|-----|
| WSDL | `https://connector.colect.services:443/services/api/3.0?wsdl` |
| XSD 1 | `https://connector.colect.services:443/services/api/3.0?xsd=1` |
| XSD 2 | `https://connector.colect.services:443/services/api/3.0?xsd=2` |

---

## Field Length Limits

All string fields have maximum length limits. Exceeding these limits will result in data truncation or API errors.

### Product Fields

| Field | Max Length | Description |
|-------|------------|-------------|
| `uniqueId` | 64 | Product style identifier |
| `colorCode` | 64 | Color code |
| `name` | 128 | Product name |
| `colorDesc` | 128 | Color description |
| `description` | 512 | Product description |
| `summary` | 2048 | Product summary |
| `brandName` | 128 | Brand name |
| `brandId` | 128 | Brand identifier |
| `seasonCode` | 64 | Season code |
| `seasonDesc` | 128 | Season description |
| `categoryCode` | 64 | Category code |
| `categoryDesc` | 128 | Category description |
| `collectionId` | 64 | Collection identifier |
| `collectionDesc` | 128 | Collection description |
| `productGroupCode` | 64 | Product group code |
| `productGroupDesc` | 128 | Product group description |
| `material` | 128 | Material description |
| `fit` | 512 | Fit description |
| `gender` | 256 | Gender |
| `searchText` | 1024 | Search keywords |
| `crossReference` | 128 | Cross reference |
| `styleAttributes` | 256 | Style attributes |
| `status` | 128 | Product status |
| `sortCode` | 128 | Sort code |
| `userDefinedField1` | 512 | Custom field 1 |
| `userDefinedField2` | 512 | Custom field 2 |

### Customer Fields

| Field | Max Length | Description |
|-------|------------|-------------|
| `customerNo` | 32 | Customer number (unique identifier) |
| `name` | 128 | Company/customer name |
| `email` | 128 | Email address |
| `phone` | 64 | Phone number |
| `street` | 128 | Street name |
| `houseNumber` | 32 | House number |
| `houseNumberSuffix` | 32 | House number suffix |
| `state` | 128 | State/province |
| `address` | 128 | Full address (single field) |
| `postalCode` | 32 | Postal/ZIP code |
| `city` | 128 | City |
| `country` | 128 | Country |
| `currencyCode` | 16 | Currency code (EUR, USD, etc.) |
| `language` | 32 | Language code |
| `channel` | 512 | Sales channel |
| `activePriceGroup` | 128 | Active price group |
| `nextPriceGroup` | 128 | Next price group |
| `status` | 512 | Status message |
| `externalReference` | 128 | External reference |
| `userDefinedField` | 2048 | Custom field |
| `paymentStatus` | 64 | Payment status |
| `gln` | 64 | Global Location Number |

### Order Fields (Output)

| Field | Max Length | Description |
|-------|------------|-------------|
| `orderNumber` | 64 | Order number |
| `customerNo` | 32 | Customer number |
| `salesPerson` | 128 | Sales person |
| `customerPriceGroup` | 128 | Price group |
| `currency` | 64 | Currency code |
| `comment1` | 512 | Comment field 1 |
| `comment2` | 512 | Comment field 2 |
| `customerOrderReference` | 256 | Customer's order reference |
| `freeTextField1` | 1024 | Free text field 1 |
| `freeTextField2` | 1024 | Free text field 2 |
| `internalComment` | 1024 | Internal comment |
| `signee` | 512 | Signee name |
| `trackingNumber` | 64 | Tracking number |
| `orderTypeCode` | 256 | Order type code |

### Historical Order Fields

| Field | Max Length | Description |
|-------|------------|-------------|
| `orderNumber` | 128 | Order number |
| `erpOrderReference` | 128 | ERP order reference |
| `customerOrderReference` | 128 | Customer order reference |
| `comment` | 2048 | Order comment |
| `collection` | 128 | Collection |
| `season` | 128 | Season |
| `salesPerson` | 128 | Sales person |
| `customerNo` | 128 | Customer number |
| `channel` | 512 | Sales channel |
| `externalUrl` | 512 | External URL (link to order in ERP) |
| `agreementCode` | 256 | Agreement code |
| `agreementDescription` | 1024 | Agreement description |
| `shippingLocationName` | 256 | Shipping location name |
| `shippingLocationAddress` | 256 | Shipping address |

### Historical Order Line Fields

| Field | Max Length | Description |
|-------|------------|-------------|
| `productUniqueId` | 256 | Product unique ID |
| `productColorCode` | 128 | Product color code |
| `productDescription` | 512 | Product description |
| `productSize` | 128 | Size name |
| `eanCode` | 256 | EAN/barcode |
| `currency` | 32 | Currency |
| `remark` | 256 | Line remark |
| `packingSlipNumber` | 128 | Packing slip number |
| `trackTraceUrl` | 512 | Track & trace URL |
| `productImageUrl` | 512 | Product image URL |

### Invoice Fields

| Field | Max Length | Description |
|-------|------------|-------------|
| `invoiceNumber` | 128 | Invoice number |
| `customerNo` | 128 | Customer number |
| `status` | 128 | Invoice status |
| `currency` | 32 | Currency code |
| `externalUrl` | 512 | External URL (link to invoice) |

### Access Rule Fields

| Field | Max Length | Description |
|-------|------------|-------------|
| `customerNo` | 128 | Customer number |
| `uniqueId` | 64 | Product unique ID |
| `colorCode` | 64 | Product color code |
| `categoryCode` | 64 | Category code |
| `productGroupCode` | 64 | Product group code |
| `seasonCode` | 64 | Season code |
| `brandId` | 128 | Brand identifier |

---

## Next Steps

- [Product Types](products.md) - Detailed product data structure
- [Customer Types](customers.md) - Customer and related types
- [Order Types](orders.md) - Order and invoice types
- [Enumerations](enums.md) - All enum values
