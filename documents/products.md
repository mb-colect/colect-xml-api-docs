# Products

The **Products** input document is the master record of your catalog in Colect. A single Products document can carry every product the customer-facing app and B2B webstore need to display — including pricing, sizes, stock levels, media, delivery windows, and extra fields.

{% hint style="success" %}
**Example:** [`examples/products.xml`](../examples/products.xml) — comprehensive example with three products, multiple price types, sizes with future stock, a prepack box, media, delivery windows, and extra fields. See also [`examples/products-minimal.xml`](../examples/products-minimal.xml) for the smallest valid Products document.
{% endhint %}

***

## Document at a glance

| Property                | Value                                                                                  |
| ----------------------- | -------------------------------------------------------------------------------------- |
| **Direction**           | ERP → Colect                                                                          |
| **Root element**        | `<products>`                                                                          |
| **File name pattern**   | `products*.xml`                                                                       |
| **Sync mode**           | Full set — products not present in the document are removed                            |
| **XSD**                 | [Product\_Feed\_XSD.xsd](../schema/snapshots/2026-05-06/Product_Feed_XSD.xsd)         |
| **Identifier**          | `uniqueId` + `colorCode` together identify a product uniquely                          |

{% hint style="info" %}
The combination of `uniqueId` and `colorCode` may appear **only once** in a Products document. To represent a style with multiple colors, send one `<product>` element per color.
{% endhint %}

***

## Sub-feeds

Products is the most layered document in the API. It contains five sub-feeds that you can also send independently as their own documents:

| Sub-feed                                            | Sent inside `<product>` as | Standalone document                          |
| --------------------------------------------------- | -------------------------- | -------------------------------------------- |
| [Prices](#prices)                                   | `<prices>`                 | [Prices document](prices.md)                |
| [Sizes & Stock](#sizes)                             | `<sizes>`                  | [Sizes & Stock document](sizes-stock.md)    |
| [Media objects](#media-objects)                     | `<media>`                  | _(included in Products only)_               |
| [Delivery Windows](#delivery-windows)               | `<deliveryWindows>`        | _(included in Products only)_               |
| [Extra Fields](#extra-fields)                       | `<extraFields>`            | [Extra Fields document](extra-fields.md)    |

The standalone documents are useful when you only want to refresh a single dimension (e.g. push new prices nightly) without re-sending the entire catalog.

***

## Top-level product fields

| Element                  | Type       | Mandatory | Description                                                                                                                                                                                                          |
| ------------------------ | ---------- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `uniqueId`               | `string`   | **YES**   | The number identifying the product. Unique only when combined with `colorCode`.                                                                                                                                      |
| `colorCode`              | `string`   | **YES**   | The color code identifying the product. Combined with `uniqueId`, this uniquely identifies the product. The combination must appear only once per document.                                                          |
| `colorDesc`              | `string`   | NO        | Color description (e.g. `black`, `red`, `green`).                                                                                                                                                                    |
| `styleAttributes`        | `string`   | NO        | Additional display-only attributes for the salesperson (e.g. a season prefix on the style name).                                                                                                                     |
| `status`                 | enum       | NO        | One of `ACTIVE` (default), `CANCELED`, `SOLDOUT`, `INVISIBLE`, `BLOCKED`, `BLOCKED_1`, `BLOCKED_2`, `BLOCKED_3`. Non-`ACTIVE` statuses prevent ordering. `INVISIBLE` is useful for re-rendering historical orders. |
| `searchText`             | `string`   | NO        | Extra text included in product search queries.                                                                                                                                                                       |
| `name`                   | `string`   | NO        | Product name displayed in the app.                                                                                                                                                                                   |
| `summary`                | `string`   | NO        | Free-text summary displayed on the product overview.                                                                                                                                                                 |
| `description`            | `string`   | NO        | Description displayed under the product name.                                                                                                                                                                        |
| `sortCode`               | `int`      | NO        | Numeric sort code for the product list and order confirmations.                                                                                                                                                      |
| `brandName`              | `string`   | NO        | Brand name. Usable for categorizing and filtering.                                                                                                                                                                   |
| `brandId`                | `string`   | NO        | Brand ID, paired with `brandName`.                                                                                                                                                                                   |
| `material`               | `string`   | NO        | Material description for display and filtering.                                                                                                                                                                      |
| `seasonCode`             | `string`   | NO        | Season code for categorizing and filtering.                                                                                                                                                                          |
| `seasonDesc`             | `string`   | NO        | Season description, paired with `seasonCode`.                                                                                                                                                                        |
| `categoryCode`           | `string`   | NO        | Category code.                                                                                                                                                                                                       |
| `categoryDesc`           | `string`   | NO        | Category description, paired with `categoryCode`.                                                                                                                                                                    |
| `collectionId`           | `string`   | NO        | Collection identifier.                                                                                                                                                                                               |
| `collectionDesc`         | `string`   | NO        | Collection description, paired with `collectionId`.                                                                                                                                                                  |
| `minimalOrderQuantity`   | `int`      | NO        | Minimum quantity required for this item or item/color combination. Default `0`.                                                                                                                                      |
| `deliveryStartDate`      | `dateTime` | NO        | First date of the delivery block. Use `stock` to flag stock-only delivery; otherwise format `yyyyMMdd`.                                                                                                              |
| `deliveryEndDate`        | `dateTime` | NO        | Last date of the delivery block. Format `yyyyMMdd`.                                                                                                                                                                  |
| `firstReceiptDate`       | `dateTime` | NO        | First date this product becomes available. May differ from `deliveryStartDate`. Format `yyyyMMdd`.                                                                                                                   |
| `productGroupCode`       | `string`   | NO        | Product group code.                                                                                                                                                                                                  |
| `productGroupDesc`       | `string`   | NO        | Product group description, paired with `productGroupCode`.                                                                                                                                                           |
| `marginGroupCode`        | `string`   | NO        | Margin group identifier — links to margin groups defined per customer.                                                                                                                                               |
| `discountGroupCode`      | `string`   | NO        | Discount group identifier — links to discount groups defined per customer.                                                                                                                                           |
| `sale`                   | `string`   | NO        | Plain text displayed on the product to identify it as on sale.                                                                                                                                                       |
| `saleBackgroundColor`    | `string`   | NO        | Hex color for the sale banner.                                                                                                                                                                                       |
| `noos`                   | `boolean`  | NO        | "Never out of stock" flag. `1` allows the item to be ordered with no available stock; `0` blocks it.                                                                                                                 |
| `crossReference`         | `string`   | NO        | Item/color-level barcode for searching and scanning when no EAN/SKU barcodes exist.                                                                                                                                  |
| `fit`                    | `string`   | NO        | Fit attribute (e.g. `slim`, `regular`).                                                                                                                                                                              |
| `gender`                 | `string`   | NO        | Gender for categorizing and filtering.                                                                                                                                                                               |
| `userDefinedField1`      | `string`   | NO        | Free-form text field 1 for filtering.                                                                                                                                                                                |
| `userDefinedField2`      | `string`   | NO        | Free-form text field 2 for filtering.                                                                                                                                                                                |
| `startDate`              | `dateTime` | NO        | First date the product is visible. Empty = no lower bound. Format `yyyyMMdd`.                                                                                                                                        |
| `endDate`                | `dateTime` | NO        | Last date the product is visible. Empty = no upper bound. Format `yyyyMMdd`.                                                                                                                                         |

***

## Prices

The `<prices>` block contains one or more `<price>` elements. Each price is one of three kinds:

* **Default price** — no `priceGroup` and no `customerNo` set
* **Price group price** — `priceGroup` set; matches customers in that group
* **Customer-specific price** — `customerNo` set; matches that single customer

{% hint style="warning" %}
A `<price>` element may have **either** `priceGroup` **or** `customerNo`, never both.
{% endhint %}

| Element                  | Type       | Mandatory | Description                                                                                                                                                                                                       |
| ------------------------ | ---------- | --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `currencyCode`           | `string`   | **YES**   | Currency code for this price object.                                                                                                                                                                              |
| `priceGroup`             | `string`   | NO        | Price group code, matched against the customer file. Mutually exclusive with `customerNo`.                                                                                                                        |
| `customerNo`             | `string`   | NO        | Customer number, matched against the customer file. Mutually exclusive with `priceGroup`.                                                                                                                         |
| `sizeName`               | `string`   | NO        | When set, the price applies only to that size (size-based pricing).                                                                                                                                               |
| `retailPrice`            | `float`    | NO        | Retail price for a single size.                                                                                                                                                                                   |
| `wholesalePrice`         | `float`    | NO        | Wholesale price for a single size.                                                                                                                                                                                |
| `originalWholesalePrice` | `float`    | NO        | Original wholesale price (when this is a discounted item).                                                                                                                                                        |
| `purchasePrice`          | `float`    | NO        | Purchase price (used when the app supports buying).                                                                                                                                                               |
| `net`                    | `boolean`  | NO        | `true` if this is a net price (customer discount is leading, not the discount group).                                                                                                                             |
| `startDate`              | `dateTime` | NO        | Price start date. Empty = immediately active. Format `yyyyMMdd`.                                                                                                                                                  |
| `endDate`                | `dateTime` | NO        | Price end date. Empty = no expiry. Format `yyyyMMdd`.                                                                                                                                                             |

***

## Sizes

The `<sizes>` block contains one `<size>` per available size. Each size carries its own EAN, prepack definition (if any), and stock levels.

### Size element

| Element                  | Type       | Mandatory | Description                                                                                                              |
| ------------------------ | ---------- | --------- | ------------------------------------------------------------------------------------------------------------------------ |
| `name`                   | `string`   | **YES**   | The size name (e.g. `M`, `42`).                                                                                          |
| `sortCode`               | `int`      | **YES**   | Primary numeric sort code. Sizes are sorted by `sortCode` first, then `subSizeSortCode`.                                  |
| `orderRoundingQuantity`  | `int`      | NO        | If set, ordering pieces for this size is only allowed in multiples of this value.                                        |
| `subSizeName`            | `string`   | NO        | Sub-size name (e.g. `34` for jeans width 32 / length 34).                                                                |
| `accessCode`             | `string`   | NO        | If set, only customers whose `sizeAccessCodes` contain this value can see the size.                                      |
| `customerSizeNaming`     | object     | NO        | Per-customer size labels — see [Customer size naming](#customer-size-naming).                                            |
| `subSizeSortCode`        | `int`      | NO        | Secondary sort code for sub-sizes.                                                                                       |
| `prePackUnitCount`       | `int`      | **YES**   | Total pieces in this SKU. `1` for a single item; higher when the SKU is a prepack.                                       |
| `eanCode`                | `string`   | NO        | EAN or SKU-level barcode for this item/size.                                                                             |
| `replenishmentDate`      | `dateTime` | NO        | Date the size is expected back in stock. Format `yyyyMMdd`.                                                              |

### Stock levels

Each `<size>` may carry one or more `<stockLevel>` entries inside `<stockLevels>`:

| Element       | Type       | Mandatory | Description                                                                                                                                                  |
| ------------- | ---------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `startDate`   | `dateTime` | **YES**   | Date from which this stock level applies. If not using future stock, set a fixed past date like `19800101`. Format `yyyyMMdd`.                              |
| `quantity`    | `int`      | **YES**   | Number of pieces available for this SKU at the given date.                                                                                                   |

### Prepack definition

If the size is a prepack box, define its contents in `<prepackDefinition>`:

| Element              | Type       | Mandatory | Description                                                                |
| -------------------- | ---------- | --------- | -------------------------------------------------------------------------- |
| `count`              | `int`      | **YES**   | Number of pieces of this size/sub-size in the prepack.                     |
| `sizeName`           | `string`   | **YES**   | The size name.                                                              |
| `sortCode`           | `int`      | **YES**   | Numeric sort code.                                                          |
| `subSizeName`        | `string`   | NO        | Sub-size name.                                                              |
| `subSizeSortCode`    | `int`      | NO        | Sub-size sort code.                                                         |
| `eanCode`            | `string`   | NO        | EAN code at sub-element level.                                              |
| `prePackUnitCount`   | `int`      | **YES**   | Total pieces in this SKU.                                                   |
| `customField`        | `string`   | NO        | Optional field surfaced on the order XML as `sizeCustomField`.             |

### Customer size naming

The `customerSizeNaming` block (added in v1.5) lets a single internal size carry per-customer labels — useful when wholesale partners use their own labelling.

| Element        | Type     | Mandatory | Description                                                                                                       |
| -------------- | -------- | --------- | ----------------------------------------------------------------------------------------------------------------- |
| `code`         | `string` | **YES**   | Customer code (or grouping code) this naming applies to.                                                          |
| `sizeName`     | `string` | **YES**   | The customer-facing size name.                                                                                    |
| `subSizeName`  | `string` | NO        | The customer-facing sub-size name.                                                                                |

The combination of `code` + `sizeName` + `subSizeName` must be unique inside a single `<customerSizeNaming>` list.

***

## Media objects

The `<media>` block contains one or more `<medium>` entries. Each item requires a `primary` image; other types add complementary visuals.

| Attribute / Element | Type     | Mandatory | Description                                                                                                                  |
| ------------------- | -------- | --------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `format` (attr)     | `string` | **YES**   | Set to `image` (other formats reserved).                                                                                     |
| `type`              | enum     | **YES**   | `primary`, `swatch`, `back`, `model`, `model_back`, `left`, `right`, `stamp_left`, `stamp_right`.                            |
| `url`               | `string` | **YES**   | URL to the high-resolution image.                                                                                            |
| `thumbUrl`          | `string` | NO        | URL to a thumbnail (no effect for `swatch`). If omitted, the device generates one — slower.                                  |
| `sortCode`          | `int`    | NO        | Sort order. No effect for `primary`, `swatch`, `stamp_left`, `stamp_right`.                                                  |

***

## Delivery windows

A list of windows the customer can choose when adding the product to an order.

| Element       | Type       | Mandatory | Description                                              |
| ------------- | ---------- | --------- | -------------------------------------------------------- |
| `code`        | `string`   | **YES**   | Code of the delivery window — reported back on the order. |
| `description` | `string`   | **YES**   | Display description of the delivery window.              |
| `startDate`   | `dateTime` | NO        | Window start date.                                       |
| `endDate`     | `dateTime` | NO        | Window end date.                                         |
| `sortCode`    | `int`      | NO        | Display order.                                           |

***

## Extra fields

Free-form, optionally grouped attributes that surface in the app for things the standard fields don't cover.

| Element        | Type       | Mandatory | Description                                                                                                          |
| -------------- | ---------- | --------- | -------------------------------------------------------------------------------------------------------------------- |
| `name`         | `string`   | **YES**   | Internal name of the extra field.                                                                                    |
| `description`  | `string`   | **YES**   | Title displayed when rendering the field.                                                                            |
| `value`        | `string`   | **YES**   | The value.                                                                                                           |
| `group`        | `string`   | NO        | Group name — extra fields with the same `group` value are rendered together.                                         |
| `important`    | `boolean`  | NO        | `true` to display this field more prominently.                                                                       |
| `imageUrl`     | `string`   | NO        | URL to an icon displayed at the top of the group.                                                                    |
| `linkUrl`      | `string`   | NO        | URL opened when the icon is tapped (rendering not yet implemented in all surfaces).                                  |
| `visible`      | `boolean`  | NO        | Whether the field is visible.                                                                                        |

***

## Basic XML structure

```xml
<?xml version="1.0" encoding="UTF-8"?>
<products>
  <product>
    <uniqueId>STYLE-001</uniqueId>
    <colorCode>BLK</colorCode>
    <colorDesc>Black</colorDesc>
    <status>ACTIVE</status>
    <name>Classic T-Shirt</name>
    <description>Premium cotton t-shirt</description>
    <brandName>Your Brand</brandName>
    <brandId>BRAND01</brandId>
    <categoryCode>TSHIRTS</categoryCode>
    <categoryDesc>T-Shirts</categoryDesc>
    <seasonCode>SS25</seasonCode>
    <seasonDesc>Spring/Summer 2025</seasonDesc>

    <prices>
      <price>
        <!-- Default price -->
        <currencyCode>EUR</currencyCode>
        <retailPrice>49.95</retailPrice>
        <wholesalePrice>25.00</wholesalePrice>
        <net>false</net>
      </price>
      <price>
        <!-- Price-group price -->
        <currencyCode>EUR</currencyCode>
        <priceGroup>WHOLESALE_EU</priceGroup>
        <wholesalePrice>22.50</wholesalePrice>
      </price>
    </prices>

    <sizes>
      <size>
        <name>M</name>
        <sortCode>2</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8712345678902</eanCode>
        <stockLevels>
          <stockLevel>
            <startDate>19800101</startDate>
            <quantity>150</quantity>
          </stockLevel>
        </stockLevels>
      </size>
    </sizes>

    <media>
      <medium format="image">
        <type>primary</type>
        <url>https://images.example.com/style-001-blk.jpg</url>
        <thumbUrl>https://images.example.com/style-001-blk_thumb.jpg</thumbUrl>
      </medium>
    </media>

    <deliveryWindows>
      <deliveryWindow>
        <code>SS25_DROP1</code>
        <description>Spring 2025 — Drop 1</description>
        <startDate>20250115</startDate>
        <endDate>20250215</endDate>
      </deliveryWindow>
    </deliveryWindows>

    <extraFields>
      <extraField>
        <name>fabric</name>
        <description>Fabric composition</description>
        <value>100% organic cotton</value>
        <group>materials</group>
      </extraField>
    </extraFields>
  </product>
</products>
```

***

## Validation tips

{% hint style="info" %}
**Before pushing a large catalog,** validate your file against the [Product\_Feed\_XSD.xsd](../schema/snapshots/2026-05-06/Product_Feed_XSD.xsd) using `xmllint` or your editor's XSD support:

```bash
xmllint --noout --schema schema/snapshots/2026-05-06/Product_Feed_XSD.xsd products.xml
```

Catching schema errors locally is far faster than parsing rejection messages from the SFTP `error/` folder.
{% endhint %}

{% hint style="warning" %}
**Empty elements are not the same as missing elements.** `<price/>` declares an empty price object — which is invalid because `currencyCode` is mandatory. Omit the element entirely if you have no value for it.
{% endhint %}
