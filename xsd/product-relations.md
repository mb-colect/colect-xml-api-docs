# Product Relations XSD

Schema for the [Product Relations](../documents/product-relations.md) input document.

{% hint style="danger" %}
**Not published.** This schema isn't live at `assets.apptitude.nl` yet — see [#12](https://github.com/mb-colect/colect-xml-api-docs/issues/12). The only copy we had was a 2026-05-06 local snapshot declaring `<productRelation>`'s fields with `<xs:all>` (any order allowed); it's been removed from this repo since nothing linked to it directly anymore (#58).

That snapshot's `<xs:all>` claim is unverified: the SOAP API's equivalent type, `xProductRelation30`, has the same field set but declares them with `<xs:sequence>`, implying order-sensitivity. Don't rely on either schema for the XML import format — see the [XSD Reference](overview.md) for why neither host is authoritative without live testing.
{% endhint %}

Until this schema is published and field order is live-tested, follow the order shown in the [Product Relations](../documents/product-relations.md) examples.
