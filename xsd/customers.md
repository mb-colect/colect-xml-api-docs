# Customers XSD

Schema for the [Customers](../documents/customers.md) input document.

**Live schema:** [https://assets.apptitude.nl/Documentation/Customer\_Feed\_XSD.xsd](https://assets.apptitude.nl/Documentation/Customer_Feed_XSD.xsd)

### Fragment

```xml
<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="customers">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="customer">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="customerNo" />
              <xs:element name="name" />
              <xs:element name="email" />
              <xs:element name="password" />
              <xs:element name="searchName" />
              <xs:element name="address" />
              <xs:element name="street" />
              <xs:element name="houseNumber" />
              <xs:element name="houseNumberSuffix" />
              <xs:element name="postalCode" />
              <xs:element name="primaryUserEmailAddresses" />
              <xs:element name="city" />
              <xs:element name="country" />
              <xs:element name="phone" />
              <xs:element name="language" />
              <xs:element name="channel" />
              <xs:element name="userDefinedField" />
              <xs:element name="activePriceGroup" />
              <xs:element name="nextPriceGroup" />
              <xs:element name="nextPriceGroupDate" />
              <xs:element name="currencyCode" />
              <!-- …continues with contacts, discountGroups, marginGroups, locations, budgets — see live schema for the rest -->
```

For the full schema, see the live link above.
