# Approval Groups

The **Approval Groups** feature lets you tag products as belonging to a named approval group. When a customer whose account is configured for approvals places an order containing tagged products, the order is held for manager sign-off before it is finalised.

{% hint style="info" %}
Approval Groups is a **business logic feature**, not an API document. The approval workflow itself — who must approve, notification routing, time limits — is configured in the Colect backend by your Colect contact. The XML feed only assigns products to groups.
{% endhint %}

***

## What it does

Products tagged with an `approvalGroupCode` are routed through a configurable approval step before their order is confirmed. The group code links a product to a backend workflow — Colect matches incoming orders against the configured rules and holds any order line that contains a product from an approval-required group.

```
   Order submitted
        │
        ▼
   Contains approval-group product?
        │
   ┌────┴─────────────────────────────────────┐
   │ No                                       │ Yes
   ▼                                          ▼
   Order confirmed                     Order held for approval
   immediately                         → manager notified
                                       → confirmed or rejected
```

***

## Where the data comes from

The two approval fields are optional attributes on `<product>` in the Products document:

| Field | XML element | Description |
| ----- | ----------- | ----------- |
| `approvalGroupCode` | `<approvalGroupCode>` | Code identifying the approval group. Must match the group configured in the Colect backend. |
| `approvalGroupDesc` | `<approvalGroupDesc>` | Human-readable description of the group — displayed to the manager reviewing the order. |

***

## Example

```xml
<product>
  <uniqueId>STYLE-002</uniqueId>
  <colorCode>NVY</colorCode>
  <name>Premium Outerwear</name>

  <approvalGroupCode>HIGH-VALUE</approvalGroupCode>
  <approvalGroupDesc>High-value items requiring manager approval</approvalGroupDesc>
</product>
```

{% hint style="info" %}
Both fields are optional — products without an `approvalGroupCode` pass through the normal order flow without any approval step.
{% endhint %}

***

## Configuration

The approval workflow is configured on the Colect backend side. Your integration only needs to populate the fields:

1. Agree on approval group codes with your Colect contact (e.g., `HIGH-VALUE`, `RESTRICTED`, `SEASONAL`).
2. Include `<approvalGroupCode>` and optionally `<approvalGroupDesc>` on all products that belong to each group.
3. Your Colect contact configures the workflow rules (approvers, notification channels, expiry windows) per group code.

{% hint style="warning" %}
Group codes are matched exactly and are case-sensitive. A product with `approvalGroupCode` set to `High-Value` will **not** match a workflow configured for `HIGH-VALUE`.
{% endhint %}

***

## Related

* [Products document](../documents/products.md) — where `<approvalGroupCode>` is pushed alongside all other product data.
* [Product Types](../data-types/products.md#xproduct) — field-level reference showing `approvalGroupCode` and `approvalGroupDesc` in context.
