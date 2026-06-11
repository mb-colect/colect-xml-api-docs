# Multi Promotions

The **Multi Promotions** feature lets users select which discount to apply at the product level in the cart. Available discounts are determined by the discount group assigned to the selected customer — giving the user the flexibility to choose the most beneficial discount per product, and giving you control over which promotions stack and which exclude each other.

{% hint style="info" %}
Multi Promotions is a **business logic feature**, not an API document. It changes how discounts are presented and applied inside the Colect Sales App and B2B Webstore — but the data that drives it comes through the standard [Customers](../documents/customers.md) document, in the `<discountGroups>` and `<orderDiscountRules>` blocks.
{% endhint %}

***

## What it does

When a user adds a product to the cart and the assigned customer has multiple eligible discount groups, the user sees a list of discounts to choose from. Picking one applies that discount to that line; other lines on the same order can use different discounts independently.

```
   Cart line: T-Shirt × 12
   ┌─────────────────────────────────────┐
   │ Available discounts for this line:  │
   │  ○ Volume — 10 + pcs   ( 12.5% off )│
   │  ● Volume — 25 + pcs   ( 15.0% off )│  ← user picks this
   │  ○ Loyalty — Tier B    (  8.0% off )│
   └─────────────────────────────────────┘
```

This contrasts with the legacy single-promotion model, where a customer's discount group was applied automatically and silently — leaving no opportunity for the user to substitute a more advantageous offer.

***

## Where the data comes from

| Source                                       | Feeds…                                              |
| -------------------------------------------- | --------------------------------------------------- |
| `<discountGroups>` on the customer           | The list of percentages and group codes available to that customer. |
| `<orderDiscountRules>` on the customer       | Order-level discounts that may surface alongside line-level options. |
| Product `discountGroupCode`                  | Which discount group(s) on the customer are eligible for that product. |

A discount becomes "eligible" for a line when:

1. The customer carries a discount group whose `code` matches the product's `discountGroupCode`, **or** the discount group's `code` is `*` (wildcard, applies to all products), **and**
2. The line's `quantity` meets the discount's `minQuantity` (if specified), **and**
3. Any time-bound or exclusivity rules permit it.

***

## Requirements

The only requirement for enabling Multi Promotions is the **availability of relevant data** — a customer with two or more discount groups eligible on the same line. Once you supply that data, the feature is available automatically across both the App and B2B platform.

There is no separate XSD or document to enable Multi Promotions. The behavior is gated on the data shape, not a flag.

***

## Optional configurable settings

| Collection setting    | Setting              | Description                                                              |
| --------------------- | -------------------- | ------------------------------------------------------------------------ |
| **Filters**           | `discountGroupFilter` | Adds a filter on the discount group description, allowing users to filter the shop or app on what is given as customer `discountGroupDescription`. |

To enable, ask your Colect contact to set `discountGroupFilter = true` on the collection.

***

## Example: discount data on a customer

Below is the relevant slice of a Customers document for a customer eligible for two volume tiers and a loyalty discount:

```xml
<customer>
  <customerNo>C-00042</customerNo>
  <name>Acme Wholesale</name>

  <discountGroups>
    <!-- Wildcard 5% on everything -->
    <discountGroup amount="5" code="*" identifier="all"/>

    <!-- Volume tiers on category VOL -->
    <discountGroup amount="12.5" code="VOL" minQuantity="10" identifier="vol.10"/>
    <discountGroup amount="15"   code="VOL" minQuantity="25" identifier="vol.25"/>

    <!-- Loyalty group B (no minimum) -->
    <discountGroup amount="8" code="LOY-B" identifier="loyalty.b"/>
  </discountGroups>

  <!-- ... other customer fields ... -->
</customer>
```

When this customer adds 25 pieces of a product whose `discountGroupCode` is `VOL`, the user is offered: Wildcard (5%), Volume 10+ (12.5%), Volume 25+ (15%), Loyalty B (8%). Picking one writes the chosen `identifier` back on the order so your ERP can audit which discount was actually applied.

***

## How the choice is reported back

The discount the user picks is reported on the **Orders output document** at the line level:

```xml
<orderLine>
  <productUniqueId>STYLE-001</productUniqueId>
  <productColorCode>BLK</productColorCode>
  <productSize>M</productSize>
  <quantity>25</quantity>

  <discountPercentage>15</discountPercentage>
  <discountGroupCode>VOL</discountGroupCode>
  <discountFromDiscountGroup>3.38</discountFromDiscountGroup>
  <discountGroupIdentifier>vol.25</discountGroupIdentifier>
  <!-- ... -->
</orderLine>
```

The `discountGroupIdentifier` is the most useful field for ERP reconciliation — it tells you exactly which entry from the customer's `discountGroups` list was chosen.

***

## Related

* [Customers document](../documents/customers.md) — where `discountGroups` and `orderDiscountRules` are pushed.
* [Order Discount & Delivery Cost Rules](../documents/order-rules.md) — order-level rules that work alongside Multi Promotions.
* [Orders output document](../documents/orders.md) — where the user's chosen discount is reported back to the ERP.

***

{% hint style="success" %}
**For end-users:** the user-facing flow is documented in the [Admin Documentation → Orders → Discounts](https://docs.colect.io/admin/orders/discounts) section. This page is for ERP integrators who need to populate the data correctly.
{% endhint %}
