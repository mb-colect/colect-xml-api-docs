# Orders

The **Orders** output document is the single document Colect produces *for* your ERP. Every order placed through the Sales App, B2B Webstore, or any other Colect surface is delivered as an Orders document for your ERP to ingest.

{% hint style="success" %}
**Example:** [`examples/orders-output.xml`](../examples/orders-output.xml) — a new order with multiple lines including a Multi-Promotions discount selection, plus a cancellation showing the `trackingNumber` convention.
{% endhint %}

{% hint style="info" %}
This is the only **output** document in the API. The XML structure mirrors the input documents, but the direction is **Colect → ERP** — files appear in your `outbox/` folder for your ERP to pick up.
{% endhint %}

***

## Document at a glance

| Property                | Value                                                                                  |
| ----------------------- | -------------------------------------------------------------------------------------- |
| **Direction**           | Colect → ERP                                                                          |
| **Root element**        | `<orders>`                                                                            |
| **File name pattern**   | `orders*.xml`                                                                         |
| **Sync mode**           | Append — each file contains new orders ready for processing                            |
| **XSD**                 | [OrderOutput\_Feed\_XSD.xsd](../schema/snapshots/2026-05-06/OrderOutput_Feed_XSD.xsd) |
| **Identifier**          | `orderNumber`                                                                         |

{% hint style="info" %}
**Polling cadence:** every 5–15 minutes is typical. Orders placed in the app appear in the outbox within seconds.
{% endhint %}

***

## Top-level order fields

| Element                       | Type       | Description                                                                                                            |
| ----------------------------- | ---------- | ---------------------------------------------------------------------------------------------------------------------- |
| `collectionId`                | `string`   | The Colect collection ID the order was placed under.                                                                    |
| `orderNumber`                 | `string`   | The Colect-generated order number — 12 characters alphanumeric. Globally unique.                                        |
| `trackingNumber`              | `string`   | A Colect order number that stays the same across all operations of an order transaction (first order, cancel, edited). |
| `canceledOrderNumber`         | `string`   | If this is a cancellation, the original `orderNumber`. Empty otherwise.                                                |
| `customerOrderReference`      | `string`   | Reference number/text manually entered in the app.                                                                     |
| `orderTypeCode`               | `string`   | Order type selected from the app's dropdown (only when configured in backend).                                         |
| `signed`                      | `boolean`  | `true` if the order is signed.                                                                                         |
| `timestamp`                   | `dateTime` | Creation date/time. Format `yyyyMMdd HH:mm`.                                                                           |
| `salesPerson`                 | `string`   | Email of the salesperson who entered the order.                                                                        |
| `customerNo`                  | `string`   | The selected customer's number.                                                                                        |
| `customerPriceGroup`          | `string`   | Active price group at order time.                                                                                      |
| `comment`                     | `string`   | Comment captured at order entry.                                                                                       |
| `requestedDeliveryDate`       | `dateTime` | Selected requested delivery start date. Format `yyyyMMdd`.                                                             |
| `requestedDeliveryEndDate`    | `dateTime` | Selected requested delivery end date. Format `yyyyMMdd`.                                                               |
| `numberOfBoxes`               | `int`      | Total number of boxes (only relevant for the returns shop).                                                            |
| `shipToCode`                  | `string`   | Code of the selected shipping location.                                                                                |

***

## Contact

The `<contact>` block captures the contact selected on the order.

| Element       | Type     | Description                                |
| ------------- | -------- | ------------------------------------------ |
| `code`        | `string` | Contact code from the customer's contact list. |
| `firstName`   | `string` | First name.                                |
| `lastName`    | `string` | Last name.                                 |
| `role`        | `string` | Role.                                      |
| `phone`       | `string` | Phone number.                              |
| `email`       | `string` | Email address.                             |

***

## Ship-to details / Alt shipping location

If the customer picked an existing shipping location, `<shipToDetails>` captures it; if they entered a new address, `<altShippingLocation>` carries it. Both share the same shape.

| Element              | Type     | Description                                        |
| -------------------- | -------- | -------------------------------------------------- |
| `code`               | `string` | Shipping location code (only on `<shipToDetails>`). |
| `name`               | `string` | Display name.                                       |
| `address`            | `string` | Full address.                                       |
| `street`             | `string` | Street.                                             |
| `houseNumber`        | `string` | House number.                                       |
| `houseNumberSuffix`  | `string` | House number suffix.                                |
| `state`              | `string` | State / province. _New in v1.5._                    |
| `postalCode`         | `string` | Postal code.                                        |
| `city`               | `string` | City.                                               |
| `country`            | `string` | Country.                                            |
| `gln`                | `string` | Global Location Number. _New in v1.5._              |
| `email`              | `string` | Email.                                              |
| `telephone`          | `string` | Phone.                                              |
| `contactFirstName`   | `string` | Contact first name (on `<shipToDetails>` only).     |
| `contactLastName`    | `string` | Contact last name (on `<shipToDetails>` only).      |
| `contact`            | `string` | Contact name (on `<altShippingLocation>` only).     |

***

## Consumer location

For drop-ship scenarios where the order ships directly to an end consumer:

| Element              | Type     | Description                |
| -------------------- | -------- | -------------------------- |
| `firstName`          | `string` | First name.                |
| `lastName`           | `string` | Last name.                 |
| `street`             | `string` | Street.                    |
| `houseNumber`        | `string` | House number.              |
| `houseNumberSuffix`  | `string` | House number suffix.       |
| `postalCode`         | `string` | Postal code.               |
| `city`               | `string` | City.                      |
| `country`            | `string` | Country.                   |
| `telephone`          | `string` | Phone.                     |
| `email`              | `string` | Email.                     |

***

## Custom choices

Any custom choices the user picked at order entry. The `code` echoes back what was defined on the customer's `<customChoices>` block.

| Element     | Type     | Description                |
| ----------- | -------- | -------------------------- |
| `group`     | `string` | Group key.                 |
| `code`      | `string` | Code of the chosen option. |

***

## Order lines

| Element                       | Type       | Description                                                                                                            |
| ----------------------------- | ---------- | ---------------------------------------------------------------------------------------------------------------------- |
| `type`                        | `string`   | Line type — `STOCK_ORDER`, `PRE_ORDER`, or `RETURN_ORDER`. See [XOrderLineType](../data-types/enums.md#xorderlinetype). |
| `productUniqueId`             | `string`   | Product `uniqueId`.                                                                                                    |
| `productColorCode`            | `string`   | Product `colorCode`.                                                                                                   |
| `sizeName`                    | `string`   | Size name.                                                                                                             |
| `subSize`                     | `string`   | Sub-size name.                                                                                                         |
| `sizeCustomField`             | `string`   | Custom field defined on the size (passed through from products).                                                       |
| `productSeason`               | `string`   | Season at order time.                                                                                                  |
| `productUserDefinedField1`    | `string`   | `userDefinedField1` snapshot.                                                                                          |
| `productUserDefinedField2`    | `string`   | `userDefinedField2` snapshot.                                                                                          |
| `deliverySubBlock`            | `string`   | (Legacy) Delivery sub-block code. Use `deliveryDate` going forward.                                                    |
| `eanCode`                     | `string`   | EAN at order time.                                                                                                     |
| `numberOfPieces`              | `int`      | Pieces ordered.                                                                                                        |
| `wholesalePrice`              | `float`    | Per-piece wholesale price applied.                                                                                     |
| `margin`                      | `float`    | Effective margin applied to the line.                                                                                  |
| `marginGroupCode`             | `string`   | Margin group whose margin applied.                                                                                     |
| `marginFromMarginGroup`       | `float`    | Margin value from a margin group (when margin was sourced from a group rather than a manual override).                 |
| `discountPercentage`          | `float`    | Discount percentage applied.                                                                                           |
| `discountGroupCode`           | `string`   | Discount group code applied.                                                                                           |
| `discountFromDiscountGroup`   | `float`    | Discount value from a discount group (when discount was sourced from a group rather than a manual override).           |
| `discountGroupIdentifier`     | `string`   | The `identifier` of the discount group entry the user picked (Multi Promotions audit trail).                          |
| `netWholesalePrice`           | `float`    | Net wholesale price per piece.                                                                                         |
| `grossWholesalePrice`         | `float`    | Gross wholesale price per piece.                                                                                       |
| `lineAmount`                  | `float`    | Total line amount.                                                                                                     |
| `remark`                      | `string`   | Free-text remark captured at order entry.                                                                              |
| `deliveryDate`                | `dateTime` | Requested delivery date for the line. Format `yyyyMMdd`.                                                              |
| `returnReason`                | `string`   | Reason for return (when `type` is `RETURN`).                                                                           |

***

## Basic XML structure

```xml
<?xml version="1.0" encoding="UTF-8"?>
<orders>
  <order>
    <collectionId>SS25</collectionId>
    <orderNumber>4XBQK7P2N1ZA</orderNumber>
    <trackingNumber>4XBQK7P2N1ZA</trackingNumber>
    <customerOrderReference>PO-12345</customerOrderReference>
    <orderTypeCode>STANDARD</orderTypeCode>
    <signed>true</signed>
    <timestamp>20260506 14:32</timestamp>
    <salesPerson>jane.rep@example.com</salesPerson>
    <customerNo>C-00042</customerNo>
    <customerPriceGroup>WHOLESALE_EU</customerPriceGroup>
    <comment>Rush delivery for store opening</comment>
    <requestedDeliveryDate>20260601</requestedDeliveryDate>
    <requestedDeliveryEndDate>20260615</requestedDeliveryEndDate>
    <shipToCode>WAREHOUSE</shipToCode>

    <contact>
      <code>JD-001</code>
      <firstName>Jane</firstName>
      <lastName>Doe</lastName>
      <role>Buyer</role>
      <email>jane@acme.example</email>
    </contact>

    <shipToDetails>
      <code>WAREHOUSE</code>
      <name>Acme Distribution Centre</name>
      <street>Industrieweg</street>
      <houseNumber>40</houseNumber>
      <postalCode>1043BB</postalCode>
      <city>Amsterdam</city>
      <country>NL</country>
      <gln>8712345000025</gln>
    </shipToDetails>

    <orderLines>
      <orderLine>
        <type>STOCK_ORDER</type>
        <productUniqueId>STYLE-001</productUniqueId>
        <productColorCode>BLK</productColorCode>
        <sizeName>M</sizeName>
        <eanCode>8712345678902</eanCode>
        <numberOfPieces>25</numberOfPieces>
        <wholesalePrice>22.50</wholesalePrice>
        <discountPercentage>15</discountPercentage>
        <discountGroupCode>VOL</discountGroupCode>
        <discountFromDiscountGroup>3.38</discountFromDiscountGroup>
        <discountGroupIdentifier>vol.25</discountGroupIdentifier>
        <netWholesalePrice>19.13</netWholesalePrice>
        <lineAmount>478.13</lineAmount>
        <deliveryDate>20260601</deliveryDate>
      </orderLine>
    </orderLines>
  </order>
</orders>
```

***

## Order lifecycle and cancellations

When an order is cancelled or edited, you receive a new Orders document with the modification. Three signals tell you what kind of operation it is:

| `canceledOrderNumber` set? | `trackingNumber` matches an earlier order's? | Meaning                                                            |
| -------------------------- | -------------------------------------------- | ------------------------------------------------------------------ |
| Empty                      | No                                            | **New order**                                                      |
| Set                        | Yes                                            | **Cancellation** of the order whose number is in `canceledOrderNumber` |
| Empty                      | Yes                                            | **Edit** — replaces the previous order with the same `trackingNumber` |

{% hint style="info" %}
**Use `trackingNumber` to group operations** belonging to the same order transaction. The `orderNumber` changes between the original, the cancellation, and the edit; `trackingNumber` stays the same.
{% endhint %}

***

## After processing

Once your ERP has ingested an Orders file, move it from `outbox/` to `outbox/archive/` (or delete it, depending on your setup). Colect doesn't manage the archive — that's your ERP's responsibility.
