# Order Discount & Delivery Cost Rules

The **Order Rules** documents push order-level discount and delivery cost rules — modifications applied to an order *as a whole* based on its total amount, total quantity, or other criteria. They live alongside line-level discount groups (see [Multi Promotions](../business-logic/multi-promotions.md)) and apply on top of them.

{% hint style="success" %}
**Example:** [`examples/order-rules.xml`](../examples/order-rules.xml) — a standalone collection-wide order-rules document covering time-bound, exclusive, and delivery-date-based rules. For per-customer rules, see the same blocks inside [`examples/customers.xml`](../examples/customers.xml).
{% endhint %}

{% hint style="info" %}
Order rules can be carried two ways:

* **Inside the [Customers](customers.md) document** as `<orderDiscountRules>` and `<orderDeliveryCostRules>` blocks. Use this when rules are customer-specific.
* **As a standalone document** for collection-wide rules. Use this when the same rules apply to every customer.

Both shapes share the same `<orderAmountModificationRule>` field set documented below.
{% endhint %}

***

## Document at a glance

| Property                | Value                                                                                  |
| ----------------------- | -------------------------------------------------------------------------------------- |
| **Direction**           | ERP → Colect                                                                          |
| **Root element**        | `<orderRules>` (when standalone) or nested in `<customer>` (when per-customer)         |
| **File name pattern**   | `orderRules*.xml`                                                                     |
| **Sync mode**           | Full set — rules not in the document are removed                                       |
| **Identifier**          | `identification`                                                                      |

***

## Rule fields

The same field set applies to both `<orderDiscountRule>` and `<orderDeliveryCostRule>`. Discount rules use `discountPercentage`; delivery cost rules use `amount`.

| Element              | Type       | Mandatory   | Description                                                                                                                |
| -------------------- | ---------- | ----------- | -------------------------------------------------------------------------------------------------------------------------- |
| `identification`     | `string`   | **YES**     | Unique identifier of the rule. Reported back on the order so the ERP can audit which rule fired.                           |
| `description`        | `string`   | **YES**     | Display description shown to the user when the rule fires.                                                                 |
| `warning`            | `string`   | NO          | Optional warning message shown alongside the rule when it applies (e.g. "Discount applied — final price may differ").      |
| `group`              | `string`   | NO          | Group label. Multiple rules in the same group can stack unless `exclusive` is set on one of them.                          |
| `exclusive`          | `boolean`  | NO          | If `true`, this rule cannot be combined with any other rule, even outside its group.                                       |
| `minimumQuantity`    | `int`      | NO          | Minimum number of pieces in the order for the rule to fire.                                                                |
| `minimumOrderAmount` | `float`    | NO          | Minimum order amount for the rule to fire.                                                                                 |
| `startDate`          | `dateTime` | NO          | First date the rule is valid. Format `yyyyMMdd`.                                                                         |
| `endDate`            | `dateTime` | NO          | Last date the rule is valid. Format `yyyyMMdd`.                                                                          |
| `evaluationMethod`   | enum       | **YES**     | `EVALUATE_BASED_ON_TODAY` or `EVALUATE_BASED_ON_DELIVERY_DATE` — controls which date is checked against `startDate`/`endDate`. |
| `amount`             | `float`    | conditional | Amount added to (or subtracted from) the order. Used by **delivery cost** rules. Use `0` to indicate free shipping.        |
| `discountPercentage` | `float`    | conditional | Discount percentage applied to the order. Used by **discount** rules.                                                      |

{% hint style="info" %}
**Discount rule vs. delivery cost rule:** a single rule object uses either `discountPercentage` (then it's a discount) or `amount` (then it's a delivery cost). Don't set both on the same rule.
{% endhint %}

***

## Basic XML structure

### Per-customer (inside Customers document)

```xml
<customer>
  <customerNo>C-00042</customerNo>
  <!-- ... other customer fields ... -->

  <orderDiscountRules>
    <orderDiscountRule>
      <identification>min500eur</identification>
      <description>5% off orders over EUR 500</description>
      <minimumOrderAmount>500</minimumOrderAmount>
      <evaluationMethod>EVALUATE_BASED_ON_TODAY</evaluationMethod>
      <discountPercentage>5</discountPercentage>
    </orderDiscountRule>

    <orderDiscountRule>
      <identification>min1000eur</identification>
      <description>10% off orders over EUR 1000</description>
      <minimumOrderAmount>1000</minimumOrderAmount>
      <evaluationMethod>EVALUATE_BASED_ON_TODAY</evaluationMethod>
      <discountPercentage>10</discountPercentage>
      <exclusive>true</exclusive>
    </orderDiscountRule>
  </orderDiscountRules>

  <orderDeliveryCostRules>
    <orderDeliveryCostRule>
      <identification>freeshipping1000</identification>
      <description>Free shipping on orders over EUR 1000</description>
      <minimumOrderAmount>1000</minimumOrderAmount>
      <evaluationMethod>EVALUATE_BASED_ON_TODAY</evaluationMethod>
      <amount>0</amount>
    </orderDeliveryCostRule>
  </orderDeliveryCostRules>
</customer>
```

### Standalone (collection-wide rules)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<orderRules>
  <orderDiscountRules>
    <orderDiscountRule>
      <identification>spring2026</identification>
      <description>Spring 2026 promo — 8% off all orders</description>
      <startDate>20260301</startDate>
      <endDate>20260331</endDate>
      <evaluationMethod>EVALUATE_BASED_ON_TODAY</evaluationMethod>
      <discountPercentage>8</discountPercentage>
    </orderDiscountRule>
  </orderDiscountRules>
</orderRules>
```

***

## How rules report back on the order

The rule's `identification` is echoed on the order line or order header so your ERP can see which rule actually fired:

```xml
<order>
  <orderNumber>ORD-2026-00342</orderNumber>
  <appliedDiscountRule>min500eur</appliedDiscountRule>
  <appliedDeliveryCostRule>freeshipping1000</appliedDeliveryCostRule>
  <!-- ... -->
</order>
```

***

## Validation tips

{% hint style="warning" %}
**Stable identifiers matter.** Reusing the same `identification` across documents preserves auditability of historical orders. Renaming a rule retroactively breaks ERP reconciliation reports.
{% endhint %}

{% hint style="info" %}
**Date evaluation:** `EVALUATE_BASED_ON_TODAY` checks the rule's date range against the order date — useful for seasonal promotions. `EVALUATE_BASED_ON_DELIVERY_DATE` checks against the requested delivery date — useful for next-season promotions ordered today for future delivery.
{% endhint %}
