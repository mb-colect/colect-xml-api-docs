# Labels

The **Labels** feature lets you attach visual badges to products — each with display text and a background colour. Labels appear overlaid on the product card in the Sales App and B2B Webstore, and are the preferred replacement for the legacy `<sale>` field.

{% hint style="info" %}
Labels is a **business logic feature**, not an API document. It changes how products are presented inside Colect — but the data that drives it comes through the standard [Products](../documents/products.md) document, in the `<labels>` block.
{% endhint %}

***

## What it does

Each `<label>` element provides two pieces of information: the text to display and the colour of the badge behind it. Multiple labels per product are supported — they are shown in the order supplied.

```
   Product card
   ┌─────────────────────────────┐
   │  [NEW ARRIVAL]  [SALE]      │  ← labels
   │                             │
   │  [product image]            │
   │                             │
   │  Style-001 / Black          │
   └─────────────────────────────┘
```

Labels replace the legacy `<sale>` + `<saleBackgroundColor>` fields. Those two fields allowed only a single badge; `<labels>` supports any number.

{% hint style="warning" %}
**`<sale>` is planned for deprecation.** New integrations should use `<labels>` instead. The `<sale>` field still works but will be removed in a future version.
{% endhint %}

***

## Where the data comes from

Labels are nested inside `<product>` in the Products document:

| Element | Description |
| ------- | ----------- |
| `<labels>` | Container — wraps one or more `<label>` elements |
| `<label>` | A single badge |
| `<text>` | The text displayed on the badge |
| `<backgroundColor>` | Hex colour code for the badge background (e.g., `ff0000` for red) |

***

## Example

A product with two labels:

```xml
<product>
  <uniqueId>STYLE-001</uniqueId>
  <colorCode>BLK</colorCode>
  <name>Essential Tee</name>

  <labels>
    <label>
      <text>NEW ARRIVAL</text>
      <backgroundColor>1a1a2e</backgroundColor>
    </label>
    <label>
      <text>LIMITED</text>
      <backgroundColor>e94560</backgroundColor>
    </label>
  </labels>
</product>
```

{% hint style="info" %}
Hex colours should be supplied without the leading `#`. Use `ff0000`, not `#ff0000`.
{% endhint %}

***

## Related

* [Products document](../documents/products.md) — where `<labels>` are pushed alongside all other product data.
* [Product Types — XLabel](../data-types/products.md#xlabel) — field-level reference for `<text>` and `<backgroundColor>`.
