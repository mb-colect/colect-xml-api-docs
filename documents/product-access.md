# Product Access

The **Product Access** input document declares which products each customer can see. It's the inverse of [Customer Access](customer-access.md): where Customer Access controls which customers a sales rep can sell to, Product Access controls which products a given customer is allowed to view and order.

***

## Document at a glance

| Property                | Value                                                                                  |
| ----------------------- | -------------------------------------------------------------------------------------- |
| **Direction**           | ERP → Colect                                                                          |
| **Root element**        | `<productAccess>`                                                                     |
| **File name pattern**   | `productAccess*.xml`                                                                  |
| **Sync mode**           | Full set — access rules not in the document are removed                                |
| **XSD**                 | [ProductAccess\_Feed\_XSD.xsd](../schema/snapshots/2026-05-06/ProductAccess_Feed_XSD.xsd) |
| **Identifier**          | `customer/customerNo` (attribute)                                                     |

***

## Customer element

The root carries one `<customer>` element per customer that has product-level restrictions. The customer is identified by a `customerNo` attribute, and the body lists access rules.

| Attribute     | Type     | Mandatory | Description                                                                |
| ------------- | -------- | --------- | -------------------------------------------------------------------------- |
| `customerNo`  | `string` | **YES**   | Customer number — must match a `customerNo` in the [Customers](customers.md) document. |

***

## Product access rules

Each `<productAccessRule>` is a single rule. Rules apply in the order they appear: a `REMOVE` rule with no filter wipes the customer's product visibility, and subsequent `ADD` rules narrow the visible set back down to specific groups.

| Attribute            | Type     | Mandatory | Description                                                                                                                          |
| -------------------- | -------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| `type`               | enum     | **YES**   | `ADD` to grant visibility, `REMOVE` to revoke. Use a leading `REMOVE` (no filters) to start from a blank slate before adding back.    |
| `productGroupCode`   | `string` | NO        | Filter on a product's `productGroupCode`. Combined with other filters using AND.                                                     |
| `categoryCode`       | `string` | NO        | Filter on a product's `categoryCode`.                                                                                                |
| `seasonCode`         | `string` | NO        | Filter on a product's `seasonCode`.                                                                                                  |
| `brandId`            | `string` | NO        | Filter on a product's `brandId`.                                                                                                     |
| `gender`             | `string` | NO        | Filter on a product's `gender`.                                                                                                      |

{% hint style="info" %}
**Common pattern:** lead with one `<productAccessRule type="REMOVE"/>` (no filters → wipe all visibility), then add back specific groups/categories with `type="ADD"` rules.
{% endhint %}

***

## Basic XML structure

```xml
<?xml version="1.0" encoding="UTF-8"?>
<productAccess>
  <customer customerNo="C-00001">
    <!-- Wipe all product visibility -->
    <productAccessRule type="REMOVE"/>

    <!-- Add back two product groups -->
    <productAccessRule type="ADD" productGroupCode="GR-01" categoryCode="CAT-01"/>
    <productAccessRule type="ADD" productGroupCode="GR-02" categoryCode="CAT-02"/>
  </customer>

  <customer customerNo="C-00042">
    <!-- This customer sees everything except SS24 -->
    <productAccessRule type="REMOVE" seasonCode="SS24"/>
  </customer>
</productAccess>
```

***

## Validation tips

{% hint style="warning" %}
**A customer absent from this document has unrestricted product access.** Product Access is opt-in: only customers explicitly listed have rules applied. To restrict a new customer, add a `<customer>` block for them.
{% endhint %}

{% hint style="info" %}
**Rule evaluation is sequential.** A rule that matches a product is the rule that wins; subsequent rules don't override it. Order your rules from broadest to narrowest.
{% endhint %}
