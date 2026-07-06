# Extra Fields XSD

Schema for the [Extra Fields](../documents/extra-fields.md) sub-feed document.

**Live schema:** [https://assets.apptitude.nl/Documentation/ProductExtraField\_Feed\_XSD.xsd](https://assets.apptitude.nl/Documentation/ProductExtraField_Feed_XSD.xsd)

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
              <xs:element name="extraFields">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element name="extraField">
                      <xs:complexType>
                        <xs:sequence>
                          <xs:element name="name" />
                          <xs:element name="description" />
                          <xs:element name="value" />
                          <xs:element name="important" />
                          <xs:element name="group" />
                          <xs:element name="imageUrl" />
                          <xs:element name="linkUrl" />
                        </xs:sequence>
                      </xs:complexType>
                    </xs:element>
                  </xs:sequence>
                </xs:complexType>
              </xs:element>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
      </xs:sequence>
    </xs:complexType>
  </xs:element>
</xs:schema>
```

This document's schema is short enough to show in full — no truncation above.
