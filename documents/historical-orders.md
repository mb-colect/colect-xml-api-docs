# Historical Orders

The **Historical Orders** input document pushes past orders from your ERP into Colect for display in the Sales App and B2B Webstore. Customers and sales reps can view their order history, reorder previous lines, and see ship/delivery status — but the ledger of record stays in your ERP.

{% hint style="success" %}
**Example:** [`examples/historical-orders.xml`](../examples/historical-orders.xml) — a past order with shipped lines, track-and-trace URL, an attached shipping label, and a returned line with `returnReason`.
{% endhint %}

{% hint style="info" %}
Historical Orders is **read-only inside Colect**. Customers can view and use these records as templates for new orders, but they can't edit them. Lifecycle changes (status updates, shipment tracking) are reflected by re-sending the order with updated fields.
{% endhint %}

{% hint style="warning" %}
**Work in progress.** Some fields on this page — particularly `timestamp` format and the `shippingLocation` structure — are not yet fully verified against the live connector. Details will be confirmed and updated.
{% endhint %}

***

## Document at a glance

| Property                | Value                                                                                  |
| ----------------------- | -------------------------------------------------------------------------------------- |
| **Direction**           | ERP → Colect                                                                          |
| **Root element**        | `<orders>`                                                                            |
| **File name pattern**   | `historicalOrders*.xml`                                                               |
| **Sync mode**           | Full set — orders not in the document are removed from display                         |
| **XSD**                 | [HistoricalOrders\_Feed\_XSD.xsd](../schema/snapshots/2026-05-06/HistoricalOrders_Feed_XSD.xsd) |
| **Identifier**          | `orderNumber`                                                                         |

***

## Top-level order fields

| Element                       | Type       | Mandatory | Description                                                                                                            |
| ----------------------------- | ---------- | --------- | ---------------------------------------------------------------------------------------------------------------------- |
| `orderNumber`                 | `string`   | **YES**   | The ERP's order number — identifies the order across re-pushes.                                                        |
| `erpOrderReference`           | `string`   | NO        | Internal ERP reference (e.g. linked PO, contract number).                                                              |
| `customerOrderReference`      | `string`   | NO        | Customer's own reference (PO number) for the order.                                                                    |
| `internalOrderType`           | `string`   | NO        | ERP-side order type code (e.g. `STANDARD`, `RETURN`, `PRE_ORDER`).                                                     |
| `collection`                  | `string`   | NO        | Collection identifier the order belongs to.                                                                            |
| `season`                      | `string`   | NO        | Season the order targets (e.g. `SS25`).                                                                                |
| `timestamp`                   | `dateTime` | NO        | Order creation date/time. Format `yyyyMMdd HH:mm`.                                                                     |
| `salesPerson`                 | `string`   | NO        | Email of the salesperson who entered the order.                                                                        |
| `customerNo`                  | `string`   | **YES**   | Customer number — must match a `customerNo` in the [Customers](customers.md) document.                                 |
| `customerPriceGroup`          | `string`   | NO        | Price group used for this order.                                                                                       |
| `comment`                     | `string`   | NO        | Free-text comment captured at order entry.                                                                             |

***

## Shipping location

A `<shippingLocation>` block on the order captures the address goods were shipped to. Format matches the shipping locations on the [Customers](customers.md) document.

| Element              | Type     | Mandatory | Description                          |
| -------------------- | -------- | --------- | ------------------------------------ |
| `name`               | `string` | NO        | Display name.                        |
| `address`            | `string` | NO        | Full address.                        |
| `street`             | `string` | NO        | Street.                              |
| `houseNumber`        | `string` | NO        | House number.                        |
| `houseNumberSuffix`  | `string` | NO        | House number suffix.                 |
| `state`              | `string` | NO        | State / province. _New in v1.4._     |
| `postalCode`         | `string` | NO        | Postal code.                         |
| `city`               | `string` | NO        | City.                                |
| `country`            | `string` | NO        | Country.                             |
| `gln`                | `string` | NO        | Global Location Number. _New in v1.4._ |
| `contact`            | `string` | NO        | Contact name at the location.        |
| `email`              | `string` | NO        | Location-specific email.             |

***

## Order lines

| Element                       | Type       | Mandatory | Description                                                                                                            |
| ----------------------------- | ---------- | --------- | ---------------------------------------------------------------------------------------------------------------------- |
| `status`                      | `string`   | NO        | Line status (e.g. `OPEN`, `SHIPPED`, `DELIVERED`, `CANCELLED`).                                                        |
| `type`                        | `string`   | NO        | Line type — typically `STANDARD` or `RETURN`.                                                                          |
| `productDescription`          | `string`   | NO        | Snapshot of the product description at order time (in case the catalog has since changed).                             |
| `productUniqueId`             | `string`   | **YES**   | The product's `uniqueId` from the [Products](products.md) document.                                                    |
| `productColorCode`            | `string`   | **YES**   | The product's `colorCode`.                                                                                             |
| `productCategory`             | `string`   | NO        | Snapshot of the product's `categoryCode`.                                                                              |
| `productGroup`                | `string`   | NO        | Snapshot of the product's `productGroupCode`.                                                                          |
| `productGender`               | `string`   | NO        | Snapshot of the product's `gender`.                                                                                    |
| `productCustomField1`         | `string`   | NO        | Snapshot of `userDefinedField1`.                                                                                       |
| `productCustomField2`         | `string`   | NO        | Snapshot of `userDefinedField2`.                                                                                       |
| `productCustomField3`         | `string`   | NO        | Snapshot of an additional custom field (collection-specific).                                                          |
| `productSize`                 | `string`   | **YES**   | Size name.                                                                                                             |
| `productSizeSortCode`         | `int`      | NO        | Sort code of the size at order time.                                                                                   |
| `productSubSize`              | `string`   | NO        | Sub-size name.                                                                                                         |
| `productSubSizeSortCode`      | `int`      | NO        | Sub-size sort code.                                                                                                    |
| `deliverySubBlock`            | `string`   | NO        | (Legacy) Delivery sub-block code. Replaced by `deliveryWindow` in v3.2 of the schema.                                  |
| `eanCode`                     | `string`   | NO        | EAN code at order time.                                                                                                |
| `quantity`                    | `int`      | **YES**   | Number of pieces ordered.                                                                                              |
| `currency`                    | `string`   | NO        | Currency for the line.                                                                                                 |
| `netWholesalePrice`           | `float`    | NO        | Net wholesale price per piece.                                                                                         |
| `grossWholesalePrice`         | `float`    | NO        | Gross wholesale price per piece.                                                                                       |
| `netLineAmount`               | `float`    | NO        | Net total for the line.                                                                                                |
| `grossLineAmount`             | `float`    | NO        | Gross total for the line.                                                                                              |
| `remark`                      | `string`   | NO        | Free-text remark captured at order entry.                                                                              |
| `shipmentDate`                | `dateTime` | NO        | Date the line shipped. Format `yyyyMMdd`.                                                                            |
| `packingSlipNumber`           | `string`   | NO        | Packing slip / shipment reference.                                                                                     |
| `quantityDelivered`           | `int`      | NO        | Pieces actually delivered (may differ from `quantity` for partial shipments).                                          |
| `trackTraceUrl`               | `string`   | NO        | URL to a track-and-trace page for the shipment.                                                                        |
| `returnReason`                | `string`   | NO        | Reason for return (when `type` is `RETURN`).                                                                           |
| `returnAllowed`               | `boolean`  | NO        | `true` if the customer can initiate a return for this line.                                                            |

### Attached files

Lines may carry attachments (e.g. shipping labels, return forms):

```xml
<files>
  <remoteFile>
    <name>shipping_label.pdf</name>
    <size>5000</size>
    <type>application/pdf</type>
    <url>https://files.example.com/labels/ORD-2026-00342.pdf</url>
  </remoteFile>
</files>
```

***

## Basic XML structure

```xml
<?xml version="1.0" encoding="UTF-8"?>
<orders>
  <order>
    <orderNumber>ORD-2026-00342</orderNumber>
    <erpOrderReference>SAP-7821</erpOrderReference>
    <customerOrderReference>PO-12345</customerOrderReference>
    <internalOrderType>STANDARD</internalOrderType>
    <collection>SS25</collection>
    <season>SS25</season>
    <timestamp>20260415 14:32</timestamp>
    <salesPerson>jane.rep@example.com</salesPerson>
    <customerNo>C-00042</customerNo>
    <customerPriceGroup>WHOLESALE_EU</customerPriceGroup>
    <comment>Rush delivery for store opening</comment>

    <shippingLocation>
      <name>Acme Distribution Centre</name>
      <street>Industrieweg</street>
      <houseNumber>40</houseNumber>
      <postalCode>1043BB</postalCode>
      <city>Amsterdam</city>
      <country>NL</country>
      <contact>Jane Doe</contact>
    </shippingLocation>

    <orderLines>
      <orderLine>
        <status>SHIPPED</status>
        <type>STANDARD</type>
        <productDescription>Classic T-Shirt</productDescription>
        <productUniqueId>STYLE-001</productUniqueId>
        <productColorCode>BLK</productColorCode>
        <productSize>M</productSize>
        <productSizeSortCode>2</productSizeSortCode>
        <eanCode>8712345678902</eanCode>
        <quantity>50</quantity>
        <currency>EUR</currency>
        <netWholesalePrice>22.50</netWholesalePrice>
        <grossWholesalePrice>27.23</grossWholesalePrice>
        <netLineAmount>1125.00</netLineAmount>
        <grossLineAmount>1361.25</grossLineAmount>
        <shipmentDate>20260420</shipmentDate>
        <packingSlipNumber>PS-77123</packingSlipNumber>
        <quantityDelivered>50</quantityDelivered>
        <trackTraceUrl>https://tracking.example.com/77123</trackTraceUrl>
      </orderLine>
    </orderLines>
  </order>
</orders>
```

***

## Validation tips

{% hint style="warning" %}
**Customer numbers and product identifiers must already exist** in the most recent Customers and Products documents. Historical orders referencing unknown customers or products are silently dropped from display.
{% endhint %}

{% hint style="info" %}
**To remove an order** from the display (e.g. it was created in error), simply omit it from the next document — the full-set sync model removes it. The ERP remains the source of truth.
{% endhint %}
