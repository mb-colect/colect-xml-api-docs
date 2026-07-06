# Prices

The **Prices** input document is a sub-feed of [Products](products.md) that contains only `uniqueId`, `colorCode`, and `<prices>`. Use it to refresh prices for products already in Colect — typically much faster than re-sending the full catalog.

{% hint style="success" %}
**Example:** `examples/prices.xml` — multi-currency prices with default, price-group, customer-specific, and time-bound promotional entries.
{% endhint %}

{% code title="prices.xml" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
  Prices sub-feed input document — example.
  Refreshes prices for two products without re-sending the full catalog.
  This document REPLACES all prices for each included product.

  Validates against https://assets.apptitude.nl/Documentation/ProductPrices_Feed_XSD.xsd
-->
<products>

  <product>
    <uniqueId>STYLE-001</uniqueId>
    <colorCode>BLK</colorCode>
    <prices>
      <!-- Default price — across the board increase from 25.00 to 27.50 -->
      <price>
        <currencyCode>EUR</currencyCode>
        <retailPrice>54.95</retailPrice>
        <wholesalePrice>27.50</wholesalePrice>
        <originalWholesalePrice>25.00</originalWholesalePrice>
        <net>false</net>
      </price>
      <!-- EU wholesale price-group -->
      <price>
        <currencyCode>EUR</currencyCode>
        <priceGroup>WHOLESALE_EU</priceGroup>
        <retailPrice>54.95</retailPrice>
        <wholesalePrice>24.95</wholesalePrice>
      </price>
      <!-- US wholesale price-group -->
      <price>
        <currencyCode>USD</currencyCode>
        <priceGroup>WHOLESALE_US</priceGroup>
        <retailPrice>59.95</retailPrice>
        <wholesalePrice>30.00</wholesalePrice>
      </price>
      <!-- Time-bound promotional price -->
      <price>
        <currencyCode>EUR</currencyCode>
        <priceGroup>WHOLESALE_EU</priceGroup>
        <wholesalePrice>22.50</wholesalePrice>
        <startDate>20260601</startDate>
        <endDate>20260630</endDate>
      </price>
    </prices>
  </product>

  <product>
    <uniqueId>STYLE-001</uniqueId>
    <colorCode>WHT</colorCode>
    <prices>
      <price>
        <currencyCode>EUR</currencyCode>
        <retailPrice>54.95</retailPrice>
        <wholesalePrice>27.50</wholesalePrice>
        <net>false</net>
      </price>
    </prices>
  </product>

</products>
```
{% endcode %}

{% hint style="warning" %}
**This document REPLACES all prices** for each included product. Sending a Prices document with two `<price>` elements for product `STYLE-001/BLK` removes any other price entries that product previously had.
{% endhint %}

***

## Document at a glance

| Property                | Value                                                                                  |
| ----------------------- | -------------------------------------------------------------------------------------- |
| **Direction**           | ERP → Colect                                                                          |
| **Root element**        | `<products>`                                                                          |
| **File name pattern**   | `prices*.xml`                                                                         |
| **Sync mode**           | Sub-feed replace — products in document have prices replaced; others unchanged         |
| **XSD**                 | [ProductPrices\_Feed\_XSD.xsd](https://assets.apptitude.nl/Documentation/ProductPrices_Feed_XSD.xsd) |
| **Identifier**          | `uniqueId` + `colorCode`                                                              |

{% hint style="info" %}
The root element is `<products>` (same as the full Products document) — but only `uniqueId`, `colorCode`, and `<prices>` are processed. Other product-level fields, if present, are ignored.
{% endhint %}

***

## Price element

The `<prices>` block contains one or more `<price>` elements. Each is one of three kinds:

* **Default price** — no `priceGroup` and no `customerNo` set
* **Price group price** — `priceGroup` set; matches customers in that group
* **Customer-specific price** — `customerNo` set; matches that single customer

{% hint style="warning" %}
A `<price>` may have **either** `priceGroup` **or** `customerNo`, never both.
{% endhint %}

| Element                  | Type       | Mandatory | Description                                                                                                |
| ------------------------ | ---------- | --------- | ---------------------------------------------------------------------------------------------------------- |
| `currencyCode`           | `string`   | **YES**   | Currency code for this price object.                                                                       |
| `priceGroup`             | `string`   | NO        | Price group code, matched against the customer file. Mutually exclusive with `customerNo`.                 |
| `customerNo`             | `string`   | NO        | Customer number, matched against the customer file. Mutually exclusive with `priceGroup`.                  |
| `sizeName`               | `string`   | NO        | When set, the price applies only to that size (size-based pricing).                                        |
| `retailPrice`            | `float`    | NO        | Retail price for a single size.                                                                            |
| `wholesalePrice`         | `float`    | NO        | Wholesale price for a single size.                                                                         |
| `originalWholesalePrice` | `float`    | NO        | Original wholesale price (when this is a discounted item).                                                 |
| `purchasePrice`          | `float`    | NO        | Purchase price (used when the app supports buying).                                                        |
| `net`                    | `boolean`  | NO        | `true` if this is a net price (customer discount is leading, not the discount group).                      |
| `startDate`              | `dateTime` | NO        | Price start date. Empty = immediately active. Format `yyyyMMdd`.                                         |
| `endDate`                | `dateTime` | NO        | Price end date. Empty = no expiry. Format `yyyyMMdd`.                                                    |

***

## Basic XML structure

```xml
<?xml version="1.0" encoding="UTF-8"?>
<products>
  <product>
    <uniqueId>STYLE-001</uniqueId>
    <colorCode>BLK</colorCode>
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
      <price>
        <!-- Customer-specific price -->
        <currencyCode>EUR</currencyCode>
        <customerNo>C-00042</customerNo>
        <wholesalePrice>20.00</wholesalePrice>
      </price>
    </prices>
  </product>
</products>
```

***

## When to use this vs. the full Products document

| Scenario                                                | Use                                          |
| ------------------------------------------------------- | -------------------------------------------- |
| Daily price refresh from ERP, catalog unchanged          | **Prices document** — faster                 |
| New product or any non-price field changed              | **Products document** — full set required    |
| Customer-specific price added or removed                | **Prices document** — narrower change        |
| Multiple sub-feeds change at once (prices + stock)      | **Products document** — single source of truth |

{% hint style="info" %}
**Sub-feed timing matters.** A Prices document delivered before the corresponding Products document references unknown products and is rejected. Run the full catalog sync before the first price refresh.
{% endhint %}
