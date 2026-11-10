# Order Discount & Delivery Cost Rules

In Colect you can apply discounts and delivery costs at **order level** — modifications applied to an order *as a whole* based on its total amount, total quantity, or delivery date. You do this by providing a set of rules **on a customer**: one set of discount rules and a separate set of delivery cost rules. They work alongside line-level discount groups (see [Multi Promotions](../business-logic/multi-promotions.md)) and apply on top of them.

{% hint style="warning" %}
**Order rules are not a separate document.** There is no standalone order-rules file and no backend location setting for one. The `<orderDiscountRules>` and `<orderDeliveryCostRules>` blocks are always sent **inside each `<customer>`** in the [Customers](customers.md#order-rules) document.

If the same rules should apply to every customer, repeat the blocks on each customer.
{% endhint %}

***

## At a glance

| Property         | Value                                                                              |
| ---------------- | ---------------------------------------------------------------------------------- |
| **Direction**    | ERP → Colect                                                                       |
| **Carried in**   | [Customers](customers.md) document, nested in each `<customer>`                    |
| **Containers**   | `<orderDiscountRules>` (discount rules) and `<orderDeliveryCostRules>` (delivery cost rules) |
| **Identifier**   | `identification`                                                                   |

***

## Rule fields

The same field set applies to both `<orderDiscountRule>` and `<orderDeliveryCostRule>`. Discount rules use `discountPercentage`; delivery cost rules use `amount`.

| Element              | Type       | Mandatory   | Description                                                                                                                |
| -------------------- | ---------- | ----------- | -------------------------------------------------------------------------------------------------------------------------- |
| `identification`     | `string`   | **YES**     | Unique identifier of the rule. Reported back on the order so the ERP can audit which rule fired.                           |
| `description`        | `string`   | **YES**     | Display description shown to the user and printed on the order confirmation when the rule applies.                         |
| `warning`            | `string`   | NO          | Optional warning message shown when the rule applies.                                                                      |
| `group`              | `string`   | NO          | Group the rule belongs to. Rules in the same group are never combined — see [How rules are applied](#how-rules-are-applied). |
| `exclusive`          | `boolean`  | NO          | If `true`, this rule is never combined or stacked with other rules, even outside its group.                                |
| `minimumQuantity`    | `int`      | NO          | Minimum number of pieces in the order for the rule to apply.                                                               |
| `minimumOrderAmount` | `float`    | NO          | Minimum order amount for the rule to apply.                                                                                |
| `startDate`          | `dateTime` | NO          | First date the rule is valid. Format `yyyyMMdd`.                                                                           |
| `endDate`            | `dateTime` | NO          | Last date the rule is valid. Format `yyyyMMdd`.                                                                            |
| `evaluationMethod`   | enum       | **YES**     | `EVALUATE_BASED_ON_TODAY` or `EVALUATE_BASED_ON_DELIVERY_DATE` — controls which date is checked against `startDate`/`endDate`. |
| `amount`             | `float`    | conditional | Amount added to (or subtracted from) the order. Used by **delivery cost** rules. Use `0` to indicate free shipping.        |
| `discountPercentage` | `float`    | conditional | Discount percentage applied to the order. Used by **discount** rules.                                                      |

{% hint style="info" %}
* **Discount rule vs. delivery cost rule:** a rule uses either `discountPercentage` (discount) or `amount` (delivery cost). Don't set both on the same rule.
* **Quantity vs. amount threshold:** set either `minimumQuantity` or `minimumOrderAmount` on a rule, not both.
{% endhint %}

***

## Basic XML structure

Both blocks sit inside a `<customer>` element in the Customers document:

```xml
<customers>
  <customer>
    <customerNo>C-00042</customerNo>
    <!-- ... other customer fields ... -->

    <orderDiscountRules>
      <orderDiscountRule>
        <identification>min500eur</identification>
        <description>5% off orders over EUR 500</description>
        <group>VOLUME</group>
        <minimumOrderAmount>500</minimumOrderAmount>
        <evaluationMethod>EVALUATE_BASED_ON_TODAY</evaluationMethod>
        <discountPercentage>5</discountPercentage>
      </orderDiscountRule>

      <orderDiscountRule>
        <identification>min1000eur</identification>
        <description>10% off orders over EUR 1000</description>
        <group>VOLUME</group>
        <minimumOrderAmount>1000</minimumOrderAmount>
        <evaluationMethod>EVALUATE_BASED_ON_TODAY</evaluationMethod>
        <discountPercentage>10</discountPercentage>
      </orderDiscountRule>

      <orderDiscountRule>
        <identification>preorderss26</identification>
        <description>15% off pre-orders for SS26 delivery</description>
        <startDate>20260101</startDate>
        <endDate>20261231</endDate>
        <evaluationMethod>EVALUATE_BASED_ON_DELIVERY_DATE</evaluationMethod>
        <discountPercentage>15</discountPercentage>
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
</customers>
```

***

## How rules are applied

Each order is matched against the customer's rules:

1. **Non-matching rules are discarded** (threshold not met, or outside the date range).
2. **One rule per group.** Rules with the same `group` are never combined — only the best matching rule in each group is kept. In the example above, an order of EUR 1200 matches both `min500eur` and `min1000eur`, but because they share the `VOLUME` group only the 10% applies.
3. **Non-exclusive rules stack.** The remaining non-exclusive rules (one per group) are combined and their discounts compounded.
4. **Best outcome wins.** Each exclusive rule is compared with the total of the stacked rules, and whichever gives the best price/most discount is applied.

The result is either no rule, one exclusive rule, or a set of stacked rules. All applied rules are returned in the order data and printed on the order confirmation.

***

## How rules report back on the order

Applied delivery cost rules are echoed in full on the exported order inside `<deliveryCostRules>`, including the rule's `identification` and the `calculatedAmount`. See [Orders → Delivery cost rules](orders.md#delivery-cost-rules) for the field reference.

***

## Validation tips

{% hint style="warning" %}
**Stable identifiers matter.** Keep the same `identification` for a rule across customer exports. Renaming a rule retroactively breaks ERP reconciliation of historical orders.
{% endhint %}

{% hint style="info" %}
**Date evaluation:** `EVALUATE_BASED_ON_TODAY` checks the rule's date range against the order entry date — useful for seasonal promotions. `EVALUATE_BASED_ON_DELIVERY_DATE` checks against the requested delivery date — useful for next-season promotions ordered today for future delivery.
{% endhint %}
