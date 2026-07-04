# Sizes & Stock XSD

Schema for the [Sizes & Stock](../documents/sizes-stock.md) sub-feed document.

**Live schema:** [https://assets.apptitude.nl/Documentation/ProductSizes\_Feed\_XSD.xsd](https://assets.apptitude.nl/Documentation/ProductSizes_Feed_XSD.xsd)

### Fragment

```xml
<?xml version="1.0" encoding="utf-8"?>
<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="products">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="product">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="uniqueId" />
              <xs:element name="colorCode" />
              <xs:element name="sizes">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element name="size">
                      <xs:complexType>
                        <xs:sequence>
                          <xs:element name="name" />
                          <xs:element name="sortCode" />
                          <xs:element name="subSizeName" />
                          <xs:element name="subSizeSortCode" />
                          <xs:element name="prePackUnitCount" />
                          <xs:element name="customField" />
                          <xs:element name="stockLevels">
                            <xs:complexType>
                              <xs:sequence>
                                <xs:element name="stockLevel">
                                  <xs:complexType>
                                    <xs:sequence>
                                      <xs:element name="startDate" />
                                      <!-- …continues with quantity, endDate — see live schema for the rest -->
```

For the full schema, see the live link above.
