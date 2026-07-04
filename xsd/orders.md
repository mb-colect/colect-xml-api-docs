# Orders (output) XSD

Schema for the [Orders (output)](../documents/orders.md) document.

**Live schema:** [https://assets.apptitude.nl/Documentation/OrderOutput\_Feed\_XSD.xsd](https://assets.apptitude.nl/Documentation/OrderOutput_Feed_XSD.xsd)

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
              <xs:element name="collectionId" />
              <xs:element name="orderNumber" />
              <xs:element name="trackingNumber" />
              <xs:element name="canceledOrderNumber" />
              <xs:element name="customerOrderReference" />
              <xs:element name="orderTypeCode" />
              <xs:element name="signed" />
              <xs:element name="timestamp" />
              <xs:element name="salesPerson" />
              <xs:element name="customerNo" />
              <xs:element name="customerPriceGroup" />
              <xs:element name="comment" />
              <xs:element name="contact">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element name="code" />
                    <xs:element name="firstName" />
                    <xs:element name="lastName" />
                    <xs:element name="role" />
                    <xs:element name="phone" />
                    <xs:element name="email" />
                  </xs:sequence>
                  <!-- …continues with shipping address, order lines — see live schema for the rest -->
```

For the full schema, see the live link above.
