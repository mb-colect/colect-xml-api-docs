# Historical Orders XSD

Schema for the [Historical Orders](../documents/historical-orders.md) input document.

**Live schema:** [https://assets.apptitude.nl/Documentation/HistoricalOrders\_Feed\_XSD.xsd](https://assets.apptitude.nl/Documentation/HistoricalOrders_Feed_XSD.xsd)

{% hint style="warning" %}
This XSD's `shippingLocation` has a single `address` field, but the document page and examples currently use `street` + `houseNumber`. Not yet live-tested which one the connector actually accepts — see [#40](https://github.com/mb-colect/colect-xml-api-docs/issues/40).
{% endhint %}

### Fragment

```xml
<?xml version="1.0" encoding="utf-8"?>
<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="orders">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="order">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="orderNumber" />
              <xs:element name="erpOrderReference" />
              <xs:element name="customerOrderReference" />
              <xs:element name="internalOrderType" />
              <xs:element name="collection" />
              <xs:element name="season" />
              <xs:element name="timestamp" />
              <xs:element name="salesPerson" />
              <xs:element name="customerNo" />
              <xs:element name="customerPriceGroup" />
              <xs:element name="comment" />
              <xs:element name="shippingLocation">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element name="name" />
                    <xs:element name="address" />
                    <xs:element name="postalCode" />
                    <xs:element name="city" />
                    <xs:element name="country" />
                    <xs:element name="contact" />
                  </xs:sequence>
                </xs:complexType>
                <!-- …continues with product lines — see live schema for the rest -->
```

For the full schema, see the live link above.
