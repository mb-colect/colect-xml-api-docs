# Sizes & Stock

The **Sizes & Stock** input document is a sub-feed of [Products](products.md) that contains only `uniqueId`, `colorCode`, and `<sizes>` (with stock levels and prepack definitions). Use it to refresh sizes and stock for products already in Colect — typically faster than re-sending the full catalog.

{% hint style="success" %}
**Example:** `examples/sizes-stock.xml` — current and future stock levels, an access-restricted size, and the v1.4 `customerSizeNaming` pattern.
{% endhint %}

<details>

<summary>Show full sizes-stock.xml</summary>

{% code title="sizes-stock.xml" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
  Sizes & Stock sub-feed input document — example.
  Hourly stock refresh for one style/color, including current stock + future restock dates.
  This document REPLACES the entire <sizes> block for each included product.

  Validates against https://assets.apptitude.nl/Documentation/ProductSizes_Feed_XSD.xsd
-->
<products>

  <product>
    <uniqueId>STYLE-001</uniqueId>
    <colorCode>BLK</colorCode>
    <sizes>
      <size>
        <name>S</name>
        <sortCode>1</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8712345678901</eanCode>
        <stockLevels>
          <stockLevel>
            <startDate>19800101</startDate>
            <quantity>110</quantity>
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
            <quantity>165</quantity>
          </stockLevel>
          <!-- Future restock arriving June 1 -->
          <stockLevel>
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
            <quantity>132</quantity>
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
            <quantity>0</quantity>
          </stockLevel>
        </stockLevels>
        <replenishmentDate>20260615</replenishmentDate>
      </size>
    </sizes>
  </product>

  <!-- Example with customerSizeNaming (v1.4) — internal "M" labelled "MEDIUM" for one customer group -->
  <product>
    <uniqueId>STYLE-001</uniqueId>
    <colorCode>WHT</colorCode>
    <sizes>
      <size>
        <name>M</name>
        <sortCode>2</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8712345678912</eanCode>
        <customerSizeNaming>
          <code>WHOLESALE_EU</code>
          <sizeName>MEDIUM</sizeName>
        </customerSizeNaming>
        <customerSizeNaming>
          <code>RETAIL</code>
          <sizeName>M</sizeName>
        </customerSizeNaming>
        <stockLevels>
          <stockLevel>
            <startDate>19800101</startDate>
            <quantity>200</quantity>
          </stockLevel>
        </stockLevels>
      </size>
    </sizes>
  </product>

</products>
```
{% endcode %}

</details>

{% hint style="warning" %}
**This document REPLACES the entire `<sizes>` block** for each included product. If a product previously had sizes S/M/L and your Sizes & Stock document only declares M/L, S is removed.
{% endhint %}

***

## Document at a glance

| Property              | Value                                                                                   |
| --------------------- | --------------------------------------------------------------------------------------- |
| **Direction**         | ERP → Colect                                                                            |
| **Root element**      | `<products>`                                                                            |
| **File name pattern** | `sizes*.xml`                                                                            |
| **Sync mode**         | Sub-feed replace — products in document have sizes/stock replaced; others unchanged     |
| **XSD**               | [ProductSizes\_Feed\_XSD.xsd](https://assets.apptitude.nl/Documentation/ProductSizes_Feed_XSD.xsd) |
| **Identifier**        | `uniqueId` + `colorCode`                                                                |

***

## Size element

| Element                 | Type       | Mandatory | Description                                                                                               |
| ----------------------- | ---------- | --------- | --------------------------------------------------------------------------------------------------------- |
| `name`                  | `string`   | **YES**   | The size name (e.g. `M`, `42`).                                                                           |
| `sortCode`              | `int`      | **YES**   | Primary numeric sort code. Sizes are sorted by `sortCode` first, then `subSizeSortCode`.                  |
| `orderRoundingQuantity` | `int`      | NO        | If set, ordering pieces for this size is only allowed in multiples of this value.                         |
| `subSizeName`           | `string`   | NO        | Sub-size name (e.g. `34` for jeans width 32 / length 34).                                                 |
| `subSizeSortCode`       | `int`      | NO        | Secondary sort code for sub-sizes.                                                                        |
| `accessCode`            | `string`   | NO        | If set, only customers whose `sizeAccessCodes` contain this value can see the size.                       |
| `customerSizeNaming`    | object     | NO        | Per-customer size labels. _Added in v1.4._ See [customerSizeNaming](sizes-stock.md#customer-size-naming). |
| `prePackUnitCount`      | `int`      | **YES**   | Total pieces in this SKU. `1` for a single item; higher when the SKU is a prepack.                        |
| `eanCode`               | `string`   | NO        | EAN or SKU-level barcode for this item/size.                                                              |
| `replenishmentDate`     | `dateTime` | NO        | Date the size is expected back in stock. Format `yyyyMMdd`.                                               |
| `customField`           | `string`   | NO        | Optional field surfaced on the order XML as `sizeCustomField`.                                            |

### Stock levels

Each `<size>` carries one or more `<stockLevel>` entries inside `<stockLevels>`:

| Element     | Type       | Mandatory | Description                                                                                                                    |
| ----------- | ---------- | --------- | ------------------------------------------------------------------------------------------------------------------------------ |
| `startDate` | `dateTime` | **YES**   | Date from which this stock level applies. If not using future stock, set a fixed past date like `19800101`. Format `yyyyMMdd`. |
| `quantity`  | `int`      | **YES**   | Number of pieces available for this SKU at the given date.                                                                     |

### Prepack definition

If the size represents a prepack box, define its contents in `<prepackDefinition>`:

| Element            | Type     | Mandatory | Description                                                    |
| ------------------ | -------- | --------- | -------------------------------------------------------------- |
| `count`            | `int`    | **YES**   | Number of pieces of this size/sub-size in the prepack.         |
| `sizeName`         | `string` | **YES**   | The size name.                                                 |
| `sizeSortCode`     | `int`    | **YES**   | Sort code for this size within the prepack contents.           |
| `subSizeName`      | `string` | NO        | Sub-size name.                                                 |
| `subSizeSortCode`  | `int`    | NO        | Sub-size sort code.                                            |
| `eanCode`          | `string` | NO        | EAN code at sub-element level.                                 |
| `customField`      | `string` | NO        | Optional field surfaced on the order XML as `sizeCustomField`. |

### Customer size naming

Added in v1.4: lets a single internal size carry per-customer labels for wholesale partners that use their own labelling.

| Element       | Type     | Mandatory | Description                                              |
| ------------- | -------- | --------- | -------------------------------------------------------- |
| `code`        | `string` | **YES**   | Customer code (or grouping code) this naming applies to. |
| `sizeName`    | `string` | **YES**   | The customer-facing size name.                           |
| `subSizeName` | `string` | NO        | The customer-facing sub-size name.                       |

The combination of `code` + `sizeName` + `subSizeName` must be unique inside a single `<customerSizeNaming>` list.

***

## Basic XML structure

```xml
<?xml version="1.0" encoding="UTF-8"?>
<products>
  <product>
    <uniqueId>STYLE-001</uniqueId>
    <colorCode>BLK</colorCode>
    <sizes>
      <size>
        <name>S</name>
        <sortCode>1</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8712345678901</eanCode>
        <stockLevels>
          <stockLevel>
            <startDate>1980-01-01</startDate>
            <quantity>100</quantity>
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
            <startDate>1980-01-01</startDate>
            <quantity>150</quantity>
          </stockLevel>
          <stockLevel>
            <!-- Future restock -->
            <startDate>2026-06-01</startDate>
            <quantity>500</quantity>
          </stockLevel>
        </stockLevels>
      </size>
    </sizes>
  </product>
</products>
```

***

## When to use this vs. the full Products document

| Scenario                                  | Use                                                        |
| ----------------------------------------- | ---------------------------------------------------------- |
| Hourly stock refresh from WMS             | **Sizes & Stock document** — fastest                       |
| New size or new EAN                       | **Sizes & Stock document** — sub-feed handles it           |
| New product or any non-size field changed | **Products document** — full set required                  |
| Prepack composition changed               | **Sizes & Stock document** — replaces the size block fully |

{% hint style="info" %}
**Stock-only refreshes** are the most common usage of this document. Many integrations push Sizes & Stock every 15 minutes and Products only once per day.
{% endhint %}
