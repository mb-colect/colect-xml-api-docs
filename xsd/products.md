# Products XSD

Schema for the [Products](../documents/products.md) input document.

**Live schema:** [https://assets.apptitude.nl/Documentation/Product\_Feed\_XSD.xsd](https://assets.apptitude.nl/Documentation/Product_Feed_XSD.xsd)

### Fragment

```xml
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" attributeFormDefault="unqualified" elementFormDefault="qualified">
    <xs:element name="products">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="product">
                    <xs:complexType>
                        <xs:sequence>
                            <xs:element name="uniqueId" minOccurs="1">
                                <xs:simpleType>
                                    <xs:restriction base="xs:string">
                                        <xs:minLength value="1" />
                                        <xs:maxLength value="64" />
                                    </xs:restriction>
                                </xs:simpleType>
                            </xs:element>
                            <xs:element name="colorCode" minOccurs="1">
                                <xs:simpleType>
                                    <xs:restriction base="xs:string">
                                        <xs:minLength value="1" />
                                        <xs:maxLength value="64" />
                                    </xs:restriction>
                                </xs:simpleType>
                            </xs:element>
                            <xs:element name="colorDesc" minOccurs="1">
                                <xs:simpleType>
                                    <xs:restriction base="xs:string">
                                        <xs:minLength value="1" />
                                        <xs:maxLength value="128" />
                                    </xs:restriction>
                                </xs:simpleType>
                            </xs:element>
                            <!-- …continues with simpleColor, media, sizes, prices, deliveryWindows, extraFields — see live schema for the rest -->
```

For the full schema, see the live link above.
