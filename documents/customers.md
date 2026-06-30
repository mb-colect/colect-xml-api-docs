# Customers

The **Customers** input document carries the master record of every B2B customer that can place orders in Colect — including pricing groups, discount and margin groups, shipping locations, budgets, agreements, and order-level rules.

{% hint style="success" %}
**Example:** [`examples/customers.xml`](../examples/customers.xml) — two B2B customers with the full feature surface: contacts, discount/margin groups, shipping locations, budgets, custom choices, agreements, order rules, and size access codes.
{% endhint %}

***

## Document at a glance

| Property                | Value                                                                                  |
| ----------------------- | -------------------------------------------------------------------------------------- |
| **Direction**           | ERP → Colect                                                                          |
| **Root element**        | `<customers>`                                                                         |
| **File name pattern**   | `customers*.xml`                                                                      |
| **Sync mode**           | Full set — customers not present in the document are removed                          |
| **XSD**                 | [Customer\_Feed\_XSD.xsd](../schema/snapshots/2026-05-06/Customer_Feed_XSD.xsd)       |
| **Identifier**          | `customerNo`                                                                          |

***

## Sub-blocks

A `<customer>` element is composed of one top-level set of fields plus several optional sub-blocks. Each sub-block is documented in its own section below.

| Sub-block                                     | Sent inside `<customer>` as | Purpose                                                  |
| --------------------------------------------- | --------------------------- | -------------------------------------------------------- |
| [Contacts](#contacts)                         | `<contacts>`                | People at the customer who can log in or receive emails  |
| [Discount Groups](#discount-groups)           | `<discountGroups>`          | Per-customer discount rules used by Multi Promotions     |
| [Margin Groups](#margin-groups)               | `<marginGroups>`            | Per-customer margin overrides                            |
| [Shipping Locations](#shipping-locations)     | `<shippingLocations>`       | Addresses goods can be shipped to                        |
| [Budget Components](#budget-components)       | `<budgetComponents>`        | Spending budgets exposed in the app                      |
| [Custom Choices](#custom-choices)             | `<customChoices>`           | Custom dropdowns the user picks from when ordering       |
| [Agreements](#agreements)                     | `<agreements>`              | Multi-channel pricing/discount agreements                |
| [Order Rules](#order-rules)                   | `<orderDiscountRules>` / `<orderDeliveryCostRules>` | Order-level discount and shipping cost rules             |
| [Size Access Codes](#size-access-codes)       | `<sizeAccessCodes>`         | Which restricted sizes the customer can see              |

***

## Top-level customer fields

| Element                       | Type     | Mandatory | Description                                                                                                          |
| ----------------------------- | -------- | --------- | -------------------------------------------------------------------------------------------------------------------- |
| `customerNo`                  | `string` | **YES**   | Unique customer number. Must match the `customerNo` referenced from price elements and orders.                       |
| `name`                        | `string` | **YES**   | Display name of the customer.                                                                                        |
| `email`                       | `string` | **YES**   | Primary email address of the customer.                                                                               |
| `password`                    | `string` | NO        | Initial password (only when the integration auto-provisions B2B logins).                                             |
| `searchName`                  | `string` | NO        | Alternative name used for search.                                                                                    |
| `address`                     | `string` | NO        | Full address — use only when the address can't be split into parts.                                                  |
| `street`                      | `string` | NO        | Street part of the address.                                                                                          |
| `houseNumber`                 | `string` | NO        | House number.                                                                                                        |
| `houseNumberSuffix`           | `string` | NO        | House number suffix.                                                                                                 |
| `state`                       | `string` | NO        | State / province. _New in v1.5._                                                                                     |
| `postalCode`                  | `string` | NO        | Postal code.                                                                                                         |
| `city`                        | `string` | NO        | City.                                                                                                                |
| `country`                     | `string` | NO        | Country.                                                                                                             |
| `gln`                         | `string` | NO        | Global Location Number for the customer. _New in v1.5._                                                              |
| `phone`                       | `string` | NO        | Primary phone number.                                                                                                |
| `language`                    | `string` | NO        | Preferred language code.                                                                                             |
| `channel`                     | `string` | NO        | Sales channel (e.g. `wholesale`, `shop-in-shop`).                                                                    |
| `userDefinedField`            | `string` | NO        | Free-form text field for filtering and integration use.                                                              |
| `primaryUserEmailAddresses`   | `string` | NO        | Comma-separated list of primary user emails.                                                                         |
| `activePriceGroup`            | `string` | NO        | Currently active price group. Resolves which `<price>` block on a product applies to this customer.                  |
| `nextPriceGroup`              | `string` | NO        | Price group that takes effect from `nextPriceGroupDate` onward.                                                      |
| `nextPriceGroupDate`          | `dateTime` | NO      | Date the next price group becomes active. Format `YYYY-MM-DD`.                                                       |
| `currencyCode`                | `string` | **YES**   | Currency the customer transacts in. Must match a `currencyCode` defined in the products file or collection settings. |
| `status`                      | `string` | NO        | Customer status (e.g. `ACTIVE`, `BLOCKED`).                                                                          |
| `statusMessage`               | `string` | NO        | Optional message shown alongside the status.                                                                         |
| `orderFooter`                 | `string` | NO        | Free text appended to every order confirmation for this customer.                                                    |
| `shippingTime`                | `int`    | NO        | Default shipping time (number of days) shown in the app.                                                             |

***

## Contacts

Each `<contact>` represents a person at the customer who can log in to the B2B app or receive order copies.

| Element       | Type     | Mandatory | Description                                                                                                            |
| ------------- | -------- | --------- | ---------------------------------------------------------------------------------------------------------------------- |
| `code`        | `string` | **YES**   | Unique code identifying this contact. New contacts created in the app come back without a code; modifications keep it. |
| `firstName`   | `string` | NO        | First name.                                                                                                            |
| `lastName`    | `string` | NO        | Last name.                                                                                                             |
| `role`        | `string` | NO        | Role of the contact (e.g. `buyer`, `manager`).                                                                         |
| `phone`       | `string` | NO        | Phone number.                                                                                                          |
| `accessType`  | enum     | NO        | `FULL` grants Retailer Web login; `NONE` denies it.                                                                    |
| `email`       | `string` | NO        | Email address. Order confirmations may be cc'd here.                                                                   |

***

## Discount groups

The `<discountGroup>` elements are how Multi Promotions surfaces choosable discounts at the line level. See [Business Logic → Multi Promotions](../business-logic/multi-promotions.md) for the user-facing flow.

| Attribute       | Type     | Mandatory | Description                                                                                            |
| --------------- | -------- | --------- | ------------------------------------------------------------------------------------------------------ |
| `amount`        | `float`  | **YES**   | Discount percentage (e.g. `15.5`).                                                                     |
| `code`          | `string` | NO        | Discount group code matching a product's `discountGroupCode`. Use `*` for a wildcard that applies to all products. |
| `minQuantity`   | `int`    | NO        | Minimum quantity on the line for the discount to be eligible.                                          |
| `identifier`    | `string` | NO        | Code echoed back on the order so the ERP can audit which discount was applied.                         |

```xml
<discountGroups>
  <discountGroup amount="5"   code="*"   identifier="all"/>
  <discountGroup amount="12.5" code="DG1" minQuantity="10" identifier="dg1.min10"/>
  <discountGroup amount="15"   code="DG1" minQuantity="25" identifier="dg1.min25"/>
</discountGroups>
```

***

## Margin groups

Margin groups override the wholesale margin per customer for products with a matching `marginGroupCode`.

| Attribute   | Type    | Mandatory | Description                                                                |
| ----------- | ------- | --------- | -------------------------------------------------------------------------- |
| `amount`    | `float` | **YES**   | Margin amount.                                                             |
| `code`      | `string`| NO        | Code matching a product's `marginGroupCode`. Use `*` for a wildcard.       |

```xml
<marginGroups>
  <marginGroup amount="2.1" code="*" />
  <marginGroup amount="2.3" code="MG1" />
</marginGroups>
```

***

## Shipping locations

A list of addresses goods can be shipped to. Customers can have one default shipping address (top-level) and any number of additional named locations.

| Element              | Type     | Mandatory | Description                                                                                       |
| -------------------- | -------- | --------- | ------------------------------------------------------------------------------------------------- |
| `code`               | `string` | **YES**   | Unique code for this location. New locations created in the app come back without a code.         |
| `name`               | `string` | **YES**   | Display name.                                                                                     |
| `address`            | `string` | NO        | Full address — use only when address can't be split into parts.                                   |
| `street`             | `string` | NO        | Street.                                                                                           |
| `houseNumber`        | `string` | NO        | House number.                                                                                     |
| `houseNumberSuffix`  | `string` | NO        | House number suffix.                                                                              |
| `state`              | `string` | NO        | State / province. _New in v1.5._                                                                  |
| `postalCode`         | `string` | NO        | Postal code.                                                                                      |
| `city`               | `string` | NO        | City.                                                                                             |
| `country`            | `string` | NO        | Country.                                                                                          |
| `gln`                | `string` | NO        | Global Location Number for the shipping location. _New in v1.5._                                  |
| `email`              | `string` | NO        | Location-specific email.                                                                          |

***

## Budget components

Buying budgets exposed in the app — each `<budgetComponent>` represents a slice of the customer's budget the app can deduct against.

| Element  | Type    | Mandatory | Description                                                |
| -------- | ------- | --------- | ---------------------------------------------------------- |
| `key`    | `string`| **YES**   | Unique key identifying this component for the customer.    |
| `amount` | `float` | **YES**   | Amount of the component.                                   |

***

## Custom choices

Dropdowns the user picks from when entering an order (e.g. delivery type, payment method).

| Element     | Type    | Mandatory | Description                                                         |
| ----------- | ------- | --------- | ------------------------------------------------------------------- |
| `group`     | `string`| **YES**   | Group key — exactly one choice per group must be picked on order.   |
| `code`      | `string`| **YES**   | Code reported back on the order.                                    |
| `value`     | `string`| **YES**   | Display text shown to the user.                                     |
| `sortCode`  | `int`   | NO        | Display order.                                                      |

***

## Agreements

Agreements model multi-channel pricing/discount setups — a single customer may have different price groups, discount groups, and margin groups depending on the channel.

| Element              | Type       | Mandatory | Description                                                                                            |
| -------------------- | ---------- | --------- | ------------------------------------------------------------------------------------------------------ |
| `code`               | `string`   | **YES**   | Unique code reported back on the order.                                                                |
| `description`        | `string`   | **YES**   | Display description.                                                                                   |
| `channel`            | `string`   | NO        | Sales channel (e.g. `Wholesale`, `shop in shop`).                                                      |
| `activePriceGroup`   | `string`   | NO        | Active price group for this agreement.                                                                 |
| `nextPriceGroup`     | `string`   | NO        | Price group taking effect on/after `nextPriceGroupDate`.                                               |
| `nextPriceGroupDate` | `dateTime` | NO        | Date the next price group becomes active. Format `YYYY-MM-DD`.                                         |
| `currencyCode`       | `string`   | **YES**   | Currency for this agreement.                                                                           |
| `orderFooter`        | `string`   | NO        | Footer text added to order confirmations under this agreement.                                         |
| `discountGroups`     | object     | NO        | Same shape as the customer-level [Discount Groups](#discount-groups).                                  |
| `marginGroups`       | object     | NO        | Same shape as the customer-level [Margin Groups](#margin-groups).                                      |

***

## Order rules

Two collections of rules live at the customer level: discount rules and delivery cost rules. Both share the same shape — see the [Order Discount & Delivery Cost Rules](order-rules.md) document for the field reference.

```xml
<orderDiscountRules>
  <orderDiscountRule>
    <identification>min500eur</identification>
    <description>5% off orders over EUR 500</description>
    <minimumOrderAmount>500</minimumOrderAmount>
    <evaluationMethod>EVALUATE_BASED_ON_TODAY</evaluationMethod>
    <discountPercentage>5</discountPercentage>
  </orderDiscountRule>
</orderDiscountRules>

<orderDeliveryCostRules>
  <orderDeliveryCostRule>
    <identification>freeshipping1000</identification>
    <description>Free shipping over EUR 1000</description>
    <minimumOrderAmount>1000</minimumOrderAmount>
    <evaluationMethod>EVALUATE_BASED_ON_TODAY</evaluationMethod>
    <amount>0</amount>
  </orderDeliveryCostRule>
</orderDeliveryCostRules>
```

***

## Size access codes

A list of access codes restricting which sizes the customer can see. A size with `<accessCode>X</accessCode>` is visible only to customers whose `<sizeAccessCodes>` contains `X`.

```xml
<sizeAccessCodes>
  <sizeAccessCode>A</sizeAccessCode>
  <sizeAccessCode>B</sizeAccessCode>
</sizeAccessCodes>
```

***

## Basic XML structure

```xml
<?xml version="1.0" encoding="UTF-8"?>
<customers>
  <customer>
    <customerNo>C-00001</customerNo>
    <name>Acme Wholesale BV</name>
    <email>orders@acme.example</email>
    <street>Voorstraat</street>
    <houseNumber>12</houseNumber>
    <postalCode>1011AA</postalCode>
    <city>Amsterdam</city>
    <country>NL</country>
    <gln>8712345000018</gln>
    <currencyCode>EUR</currencyCode>
    <activePriceGroup>WHOLESALE_EU</activePriceGroup>

    <contacts>
      <contact>
        <code>JD-001</code>
        <firstName>Jane</firstName>
        <lastName>Doe</lastName>
        <role>Buyer</role>
        <email>jane@acme.example</email>
        <accessType>FULL</accessType>
      </contact>
    </contacts>

    <discountGroups>
      <discountGroup amount="5"    code="*"   identifier="all"/>
      <discountGroup amount="12.5" code="DG1" minQuantity="10" identifier="dg1.min10"/>
    </discountGroups>

    <marginGroups>
      <marginGroup amount="2.1" code="*"/>
    </marginGroups>

    <shippingLocations>
      <shippingLocation>
        <code>WAREHOUSE</code>
        <name>Acme Distribution Centre</name>
        <street>Industrieweg</street>
        <houseNumber>40</houseNumber>
        <postalCode>1043BB</postalCode>
        <city>Amsterdam</city>
        <country>NL</country>
        <gln>8712345000025</gln>
      </shippingLocation>
    </shippingLocations>

    <agreements>
      <agreement>
        <code>WHOLESALE_2026</code>
        <description>Wholesale 2026 agreement</description>
        <channel>Wholesale</channel>
        <activePriceGroup>WHOLESALE_EU</activePriceGroup>
        <currencyCode>EUR</currencyCode>
      </agreement>
    </agreements>

    <sizeAccessCodes>
      <sizeAccessCode>A</sizeAccessCode>
    </sizeAccessCodes>
  </customer>
</customers>
```

***

## Validation tips

{% hint style="info" %}
**Currency consistency:** A customer's `currencyCode` must match a currency that exists on the products being ordered. If a product has no `<price>` for the customer's currency, ordering that product will fail.
{% endhint %}

{% hint style="warning" %}
**Discount group identifiers should be stable.** If you reuse the same `identifier` across documents, historical orders remain auditable. Changing identifiers retroactively breaks ERP reconciliation reports.
{% endhint %}
