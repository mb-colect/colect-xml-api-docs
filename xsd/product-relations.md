# Product Relations XSD

Schema for the [Product Relations](../documents/product-relations.md) input document.

{% hint style="warning" %}
Not yet published at `assets.apptitude.nl` — this is the local snapshot copy. See [#12](https://github.com/mb-colect/colect-xml-api-docs/issues/12).
{% endhint %}

**Snapshot:** [ProductRelations\_Feed\_XSD.xsd](../.gitbook/assets/ProductRelations_Feed_XSD.xsd)

### Fragment

```xml
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified">

    <!-- Root element -->
    <xs:element name="productRelations">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="productRelation" type="productRelationType" minOccurs="0" maxOccurs="unbounded"/>
            </xs:sequence>
        </xs:complexType>
    </xs:element>

    <!-- productRelation complex type -->
    <xs:complexType name="productRelationType">
        <xs:all>
            <xs:element name="groupCode" type="xs:string" minOccurs="0"/>
            <xs:element name="sourceProductUniqueId" type="xs:string" minOccurs="0"/>
            <xs:element name="sourceProductColorCode" type="xs:string" minOccurs="0"/>
            <xs:element name="targetProductUniqueId" type="xs:string" minOccurs="0"/>
            <xs:element name="targetProductColorCode" type="xs:string" minOccurs="0"/>
            <xs:element name="type" type="relationTypeEnum" minOccurs="0"/>
            <xs:element name="groupDescription" type="xs:string" minOccurs="0"/>
        </xs:all>
    </xs:complexType>

    <!-- Enumeration of relation types -->
    <xs:simpleType name="relationTypeEnum">
        <xs:restriction base="xs:string">
            <xs:enumeration value="SUCCESSOR"/>
            <xs:enumeration value="MATCHING_SET"/>
        </xs:restriction>
    </xs:simpleType>

</xs:schema>
```

This document's schema is short enough to show in full — no truncation above. `SUCCESSOR` / `MATCHING_SET` match the `type` values used in the document's live-tested examples.
