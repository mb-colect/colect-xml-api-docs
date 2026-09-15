# Products

The **Products** input document is the master record of your catalog in Colect. A single Products document can carry every product the customer-facing app and B2B webstore need to display — including pricing, sizes, stock levels, media, delivery windows, and extra fields.

{% hint style="success" %}
**Example:** `examples/products.xml` — comprehensive example with three products, multiple price types, sizes with future stock, a prepack box, delivery windows, and extra fields. See also `examples/products-minimal.xml` for the smallest valid Products document.
{% endhint %}

<details>

<summary>Show full products.xml</summary>

{% code title="products.xml" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
  Products input document — comprehensive example.
  Shows two products, each with multiple sizes, prices (default + price-group + customer-specific),
  stock levels (current + future), delivery windows, and extra fields.
  Validates against https://assets.apptitude.nl/Documentation/Product_Feed_XSD.xsd
-->
<products>

  <!-- ============================================================
       Product 1: Classic T-Shirt in Black
       ============================================================ -->
  <product>
    <uniqueId>STYLE-001</uniqueId>
    <colorCode>BLK</colorCode>
    <colorDesc>Black</colorDesc>
    <styleAttributes>SS25</styleAttributes>
    <status>ACTIVE</status>
    <searchText>tee shirt cotton organic</searchText>
    <name>Classic T-Shirt</name>
    <summary>Premium cotton tee, made in Portugal</summary>
    <description>A wardrobe essential — soft, durable, and ethically made.</description>
    <sortCode>10</sortCode>
    <brandName>Example Brand</brandName>
    <brandId>EB</brandId>
    <material>100% organic cotton</material>
    <seasonCode>SS25</seasonCode>
    <seasonDesc>Spring/Summer 2025</seasonDesc>
    <categoryCode>TSHIRTS</categoryCode>
    <categoryDesc>T-Shirts</categoryDesc>
    <collectionId>MAINLINE</collectionId>
    <collectionDesc>Mainline</collectionDesc>
    <minimalOrderQuantity>0</minimalOrderQuantity>
    <deliveryStartDate>20250115</deliveryStartDate>
    <deliveryEndDate>20250215</deliveryEndDate>
    <firstReceiptDate>20250115</firstReceiptDate>
    <productGroupCode>TOPS</productGroupCode>
    <productGroupDesc>Tops</productGroupDesc>
    <marginGroupCode>MG1</marginGroupCode>
    <discountGroupCode>VOL</discountGroupCode>
    <noos>0</noos>
    <gender>UNISEX</gender>

    <prices>
      <!-- Default price -->
      <price>
        <currencyCode>EUR</currencyCode>
        <retailPrice>49.95</retailPrice>
        <wholesalePrice>25.00</wholesalePrice>
        <originalWholesalePrice>25.00</originalWholesalePrice>
        <net>false</net>
      </price>
      <!-- Price-group price for European wholesale partners -->
      <price>
        <currencyCode>EUR</currencyCode>
        <priceGroup>WHOLESALE_EU</priceGroup>
        <retailPrice>49.95</retailPrice>
        <wholesalePrice>22.50</wholesalePrice>
      </price>
      <!-- Customer-specific price for VIP partner -->
      <price>
        <currencyCode>EUR</currencyCode>
        <customerNo>C-00042</customerNo>
        <wholesalePrice>20.00</wholesalePrice>
      </price>
      <!-- US wholesale price -->
      <price>
        <currencyCode>USD</currencyCode>
        <priceGroup>WHOLESALE_US</priceGroup>
        <retailPrice>54.95</retailPrice>
        <wholesalePrice>27.50</wholesalePrice>
      </price>
    </prices>

    <sizes>
      <size>
        <name>S</name>
        <sortCode>1</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8712345678901</eanCode>
        <stockLevels>
          <stockLevel>
            <startDate>19800101</startDate>
            <quantity>120</quantity>
          </stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>M</name>
        <sortCode>2</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8712345678902</eanCode>
        <stockLevels>
          <stockLevel>
            <startDate>19800101</startDate>
            <quantity>180</quantity>
          </stockLevel>
          <stockLevel>
            <!-- Future restock -->
            <startDate>20260601</startDate>
            <quantity>500</quantity>
          </stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>L</name>
        <sortCode>3</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8712345678903</eanCode>
        <stockLevels>
          <stockLevel>
            <startDate>19800101</startDate>
            <quantity>140</quantity>
          </stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>XL</name>
        <sortCode>4</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8712345678904</eanCode>
        <accessCode>PRO</accessCode>
        <stockLevels>
          <stockLevel>
            <startDate>19800101</startDate>
            <quantity>60</quantity>
          </stockLevel>
        </stockLevels>
      </size>
    </sizes>

    <deliveryWindows>
      <deliveryWindow>
        <code>SS25_DROP1</code>
        <description>Spring 2025 — Drop 1</description>
        <startDate>20250115</startDate>
        <endDate>20250215</endDate>
        <sortCode>1</sortCode>
      </deliveryWindow>
      <deliveryWindow>
        <code>SS25_DROP2</code>
        <description>Spring 2025 — Drop 2</description>
        <startDate>20250301</startDate>
        <endDate>20250401</endDate>
        <sortCode>2</sortCode>
      </deliveryWindow>
    </deliveryWindows>

    <extraFields>
      <extraField>
        <name>fabric</name>
        <description>Fabric composition</description>
        <value>100% organic cotton</value>
        <group>materials</group>
      </extraField>
      <extraField>
        <name>care</name>
        <description>Care instructions</description>
        <value>Machine wash 30°C, do not tumble dry, iron on low</value>
        <group>materials</group>
      </extraField>
      <extraField>
        <name>made-in</name>
        <description>Made in</description>
        <value>Portugal</value>
        <group>provenance</group>
      </extraField>
      <extraField>
        <name>gots</name>
        <description>GOTS certified</description>
        <value>Certified organic by GOTS</value>
        <group>certifications</group>
        <important>true</important>
        <imageUrl>https://images.example.com/icons/gots.svg</imageUrl>
        <linkUrl>https://global-standard.org/</linkUrl>
      </extraField>
    </extraFields>
  </product>

  <!-- ============================================================
       Product 2: Same style in White
       ============================================================ -->
  <product>
    <uniqueId>STYLE-001</uniqueId>
    <colorCode>WHT</colorCode>
    <colorDesc>White</colorDesc>
    <styleAttributes>SS25</styleAttributes>
    <status>ACTIVE</status>
    <name>Classic T-Shirt</name>
    <description>A wardrobe essential — soft, durable, and ethically made.</description>
    <sortCode>11</sortCode>
    <brandName>Example Brand</brandName>
    <brandId>EB</brandId>
    <material>100% organic cotton</material>
    <seasonCode>SS25</seasonCode>
    <seasonDesc>Spring/Summer 2025</seasonDesc>
    <categoryCode>TSHIRTS</categoryCode>
    <productGroupCode>TOPS</productGroupCode>
    <discountGroupCode>VOL</discountGroupCode>
    <gender>UNISEX</gender>

    <prices>
      <price>
        <currencyCode>EUR</currencyCode>
        <retailPrice>49.95</retailPrice>
        <wholesalePrice>25.00</wholesalePrice>
        <net>false</net>
      </price>
    </prices>

    <sizes>
      <size>
        <name>M</name>
        <sortCode>2</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8712345678912</eanCode>
        <stockLevels>
          <stockLevel>
            <startDate>19800101</startDate>
            <quantity>200</quantity>
          </stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>L</name>
        <sortCode>3</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8712345678913</eanCode>
        <stockLevels>
          <stockLevel>
            <startDate>19800101</startDate>
            <quantity>160</quantity>
          </stockLevel>
        </stockLevels>
      </size>
    </sizes>

  </product>

  <!-- ============================================================
       Product 3: Prepack box example (mixed sizes)
       ============================================================ -->
  <product>
    <uniqueId>STYLE-002-PREPACK</uniqueId>
    <colorCode>BLK</colorCode>
    <colorDesc>Black</colorDesc>
    <status>ACTIVE</status>
    <name>Tee Prepack — Sizes M-L-XL</name>
    <brandName>Example Brand</brandName>
    <brandId>EB</brandId>
    <seasonCode>SS25</seasonCode>
    <categoryCode>TSHIRTS</categoryCode>
    <productGroupCode>PREPACK</productGroupCode>

    <prices>
      <price>
        <currencyCode>EUR</currencyCode>
        <wholesalePrice>72.00</wholesalePrice>
      </price>
    </prices>

    <sizes>
      <size>
        <name>BOX-1</name>
        <sortCode>1</sortCode>
        <prePackUnitCount>3</prePackUnitCount>
        <eanCode>8712345678999</eanCode>
        <stockLevels>
          <stockLevel>
            <startDate>19800101</startDate>
            <quantity>40</quantity>
          </stockLevel>
        </stockLevels>
        <prepackDefinition>
          <prepackContentElement>
            <count>1</count>
            <sizeName>M</sizeName>
            <sizeSortCode>2</sizeSortCode>
          </prepackContentElement>
          <prepackContentElement>
            <count>1</count>
            <sizeName>L</sizeName>
            <sizeSortCode>3</sizeSortCode>
          </prepackContentElement>
          <prepackContentElement>
            <count>1</count>
            <sizeName>XL</sizeName>
            <sizeSortCode>4</sizeSortCode>
          </prepackContentElement>
        </prepackDefinition>
      </size>
    </sizes>
  </product>

</products>
```
{% endcode %}

</details>

<details>

<summary>Show full products-minimal.xml</summary>

{% code title="products-minimal.xml" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
  Products input document — minimal example.
  Shows only the mandatory fields needed for ingestion.
  Useful as a smoke test to validate SFTP connectivity and parsing.
-->
<products>
  <product>
    <uniqueId>SMOKETEST-001</uniqueId>
    <colorCode>BLK</colorCode>

    <prices>
      <price>
        <currencyCode>EUR</currencyCode>
        <wholesalePrice>1.00</wholesalePrice>
      </price>
    </prices>

    <sizes>
      <size>
        <name>ONE</name>
        <sortCode>1</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <stockLevels>
          <stockLevel>
            <startDate>19800101</startDate>
            <quantity>1</quantity>
          </stockLevel>
        </stockLevels>
      </size>
    </sizes>
  </product>
</products>
```
{% endcode %}

</details>

***

## Document at a glance

| Property                | Value                                                                                  |
| ----------------------- | -------------------------------------------------------------------------------------- |
| **Direction**           | ERP → Colect                                                                          |
| **Root element**        | `<products>`                                                                          |
| **File name pattern**   | `products*.xml`                                                                       |
| **Sync mode**           | Full set — products not present in the document are removed                            |
| **XSD**                 | [Product\_Feed\_XSD.xsd](https://assets.apptitude.nl/Documentation/Product_Feed_XSD.xsd)         |
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
| `deliveryStartDate`      | `dateTime` | NO        | First date of the delivery block. Use `stock` to flag stock-only delivery; otherwise `yyyyMMdd`.                                                                                                                   |
| `deliveryEndDate`        | `dateTime` | NO        | Last date of the delivery block. Format `yyyyMMdd`.                                                                                                                                                                |
| `firstReceiptDate`       | `dateTime` | NO        | First date this product becomes available. May differ from `deliveryStartDate`. Format `yyyyMMdd`.                                                                                                                 |
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
| `startDate`              | `dateTime` | NO        | First date the product is visible. Empty = no lower bound. Format `yyyyMMdd`.                                                                                                                                      |
| `endDate`                | `dateTime` | NO        | Last date the product is visible. Empty = no upper bound. Format `yyyyMMdd`.                                                                                                                                       |
| `approvalGroupCode`      | `string`   | NO        | Links the product to an approval workflow. Orders containing this product are held for manager sign-off. See [Approvals](../business-logic/approvals.md).                                                             |
| `approvalGroupDesc`      | `string`   | NO        | Human-readable description of the approval group, displayed to the approving manager.                                                                                                                                |
| `labels`                 | object     | NO        | One or more visual badges rendered on the product card. Each `<label>` has `<text>` and `<backgroundColor>` (hex without `#`). See [Labels](../business-logic/labels.md).                                            |

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
| `startDate`              | `dateTime` | NO        | Price start date. Empty = immediately active. Format `yyyyMMdd`.                                                                                                                                                |
| `endDate`                | `dateTime` | NO        | Price end date. Empty = no expiry. Format `yyyyMMdd`.                                                                                                                                                           |

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
| `replenishmentDate`      | `dateTime` | NO        | Date the size is expected back in stock. Format `yyyyMMdd`.                                                            |

### Stock levels

Each `<size>` may carry one or more `<stockLevel>` entries inside `<stockLevels>`:

| Element       | Type       | Mandatory | Description                                                                                                                                                  |
| ------------- | ---------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `startDate`   | `dateTime` | **YES**   | Date from which this stock level applies. If not using future stock, set a fixed past date like `19800101`. Format `yyyyMMdd`.                          |
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

The `customerSizeNaming` block (added in v1.4) lets a single internal size carry per-customer labels — useful when wholesale partners use their own labelling.

| Element        | Type     | Mandatory | Description                                                                                                       |
| -------------- | -------- | --------- | ----------------------------------------------------------------------------------------------------------------- |
| `code`         | `string` | **YES**   | Customer code (or grouping code) this naming applies to.                                                          |
| `sizeName`     | `string` | **YES**   | The customer-facing size name.                                                                                    |
| `subSizeName`  | `string` | NO        | The customer-facing sub-size name.                                                                                |

The combination of `code` + `sizeName` + `subSizeName` must be unique inside a single `<customerSizeNaming>` list.

***

## Media objects

{% hint style="info" %}
Most customers manage product images through backend settings rather than this block — images are dropped into the `images/` folder (see [Document Transfer](../getting-started/document-transfer.md)) and matched up automatically. The `<media>` block below is an alternative for customers who want to control image URLs directly from the XML; ask your Colect contact which setup applies to your collection.
{% endhint %}

The `<media>` block contains one or more `<medium>` entries. Each item requires a `primary` medium; other types add complementary visuals.

| Attribute / Element | Type     | Mandatory | Description                                                                                                                  |
| ------------------- | -------- | --------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `format` (attr)     | `string` | **YES**   | Set to `image` (other formats reserved).                                                                                     |
| `type`              | enum     | **YES**   | `primary`, `back`, `swatch`, `model`, `model_back`, `left`, `right`, `pack`, `top`, `bottom`, `fit`, `stamp_left`, `stamp_right`, `additional_1`–`additional_5`, `video`, `video_model`, `video_model_lead_in`, `video_model_lead_out`, `html`. Full descriptions in [enums](../data-types/enums.md). |
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
        <!-- Regular, single-unit size -->
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
      <size>
        <!-- Prepack SKU: one sealed box containing an assortment of sizes -->
        <name>PPK-ASSORT</name>
        <sortCode>9</sortCode>
        <prePackUnitCount>8</prePackUnitCount>
        <eanCode>8712345678933</eanCode>
        <stockLevels>
          <stockLevel>
            <startDate>19800101</startDate>
            <quantity>20</quantity>
          </stockLevel>
        </stockLevels>
        <prepackDefinition>
          <prepackContentElement>
            <count>2</count>
            <sizeName>S</sizeName>
            <sizeSortCode>1</sizeSortCode>
            <eanCode>8712345678896</eanCode>
          </prepackContentElement>
          <prepackContentElement>
            <count>4</count>
            <sizeName>M</sizeName>
            <sizeSortCode>2</sizeSortCode>
            <eanCode>8712345678902</eanCode>
          </prepackContentElement>
          <prepackContentElement>
            <count>2</count>
            <sizeName>L</sizeName>
            <sizeSortCode>3</sizeSortCode>
            <eanCode>8712345678919</eanCode>
          </prepackContentElement>
        </prepackDefinition>
      </size>
    </sizes>

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

`prePackUnitCount` is the total number of pieces in that SKU. Set it to `1` for a regular, single-unit size, as in `M` above. Set it to the box total for a prepack, `8` in the `PPK-ASSORT` example, where the box holds 2 pieces of S, 4 of M, and 2 of L.

Add a `prepackDefinition` element to a prepack size to describe what's inside the box. It holds one `prepackContentElement` for each size the box contains.

| Element name | Description | Mandatory |
|---|---|---|
| `count` | The number of pieces of this size that are in the prepack | YES |
| `sizeName` | The name of the size | YES |
| `sizeSortCode` | Sort code for this size within the prepack contents | YES |
| `subSizeName` | The sub-size name, for products with two-part sizing (for example, waist/length) | NO |
| `subSizeSortCode` | Sort code for the sub-size | NO |
| `eanCode` | The EAN or SKU-level barcode for this size | NO |
| `customField` | Optional value, included in the order XML as `sizeCustomField` | NO |

***

## Validation tips

{% hint style="info" %}
**Before pushing a large catalog,** validate your file against the [Product\_Feed\_XSD.xsd](https://assets.apptitude.nl/Documentation/Product_Feed_XSD.xsd) using `xmllint` or your editor's XSD support:

```bash
curl -O https://assets.apptitude.nl/Documentation/Product_Feed_XSD.xsd
xmllint --noout --schema Product_Feed_XSD.xsd products.xml
```

Catching schema errors locally is far faster than reading them from the Connector status page in the Colect backend.
{% endhint %}

{% hint style="warning" %}
**Empty elements are not the same as missing elements.** `<price/>` declares an empty price object — which is invalid because `currencyCode` is mandatory. Omit the element entirely if you have no value for it.
{% endhint %}

{% hint style="info" %}
**Connector status page:** Validation errors for processed files are visible in the Connector status page in the Colect backend.

_[Screenshot placeholder — connector status page]_
{% endhint %}
