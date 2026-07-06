# The Catalog

First up: get the Atoll catalog into the collection — 8 products across 5 styles. This is the same `<products>` document from [Your First Document](../getting-started/first-document.md), just with a real (if fictional) catalog instead of one placeholder.

{% hint style="info" %}
**No stock yet, on purpose.** Sizes are defined here, but quantities come from a separate sub-feed in the [next step](stock.md) — a deliberate, commonly-used setup (see [Stock sub-feed vs inline stock](../documents/sizes-stock.md)). Every size below will show as sold out until you push stock.
{% endhint %}

***

## The catalog at a glance

| uniqueId  | Color        | Name               | Category | Gender |
| --------- | ------------ | ------------------ | -------- | ------ |
| `ATL-BKT` | Coral (`CRL`) | Atoll Bikini Top    | Women's  | Female |
| `ATL-BKT` | Black (`BLK`) | Atoll Bikini Top    | Women's  | Female |
| `ATL-BKB` | Coral (`CRL`) | Atoll Bikini Bottom | Women's  | Female |
| `ATL-BKB` | Black (`BLK`) | Atoll Bikini Bottom | Women's  | Female |
| `ATL-OPS` | Sand (`SND`)  | Atoll One-Piece     | Women's  | Female |
| `ATL-SHS` | Navy (`NVY`)  | Atoll Swim Shorts   | Men's    | Male   |
| `ATL-SHS` | Olive (`OLV`) | Atoll Swim Shorts   | Men's    | Male   |
| `ATL-RSG` | White (`WHT`) | Atoll Rash Guard    | Men's    | Male   |

{% hint style="success" %}
Keep an eye on **`ATL-BKT` / `CRL`** and **`ATL-BKB` / `CRL`** — the Coral bikini top and bottom are a matching pair, and they come back in [Matching Sets](relations.md).
{% endhint %}

***

## Step — Write the document

Create `atoll_products.xml`. This is the full file — sub-sizes on the bikini tops, translations, delivery windows, extra fields, and labels included, so it's ready to push as-is.

<details>

<summary>Show full atoll_products.xml (8 products)</summary>

{% code title="atoll_products.xml" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
  Atoll — walkthrough products document.
  A fictional swimwear brand used to demonstrate the Colect XML integration end-to-end.

  8 products (5 women's, 3 men's), across 3 categories:
    ATL-BKT  Bikini Top       — Coral, Black   (EU bra sizing: band 75–90 × cup C–F)
    ATL-BKB  Bikini Bottom    — Coral, Black   (XS–XL)
    ATL-OPS  One-Piece        — Sand           (XS–XL)
    ATL-SHS  Swim Shorts      — Navy, Olive    (S–XL)
    ATL-RSG  Rash Guard       — White          (S–XL)

  Features demonstrated:
    - Sub-sizes (cup sizes on bikini tops)
    - Matching sets (BKT-CRL ↔ BKB-CRL, BKT-BLK ↔ BKB-BLK) — see atoll_product_relations.xml
    - Delivery windows (two SS27 drops)
    - Default + price-group prices
    - Preorder stock (20270201) + SS28 restock (20280201)
    - Extra fields with grouping
    - Labels
    - Dutch translations
    - Discount groups (SWIM)
    - Gender filtering (WOMENS / MENS)
-->
<products>

  <!-- ============================================================
       ATL-BKT / CRL  —  Atoll Bikini Top, Coral
       Demonstrates: sub-sizes (band + cup), full price set,
       delivery windows, extra fields, labels, translations
       ============================================================ -->
  <product>
    <uniqueId>ATL-BKT</uniqueId>
    <colorCode>CRL</colorCode>
    <colorDesc>Coral</colorDesc>
    <styleAttributes>SS27</styleAttributes>
    <status>ACTIVE</status>
    <searchText>bikini top swimwear coral women swimsuit</searchText>
    <name>Atoll Bikini Top</name>
    <summary>Adjustable triangle top in signature Coral — mix and match with the Atoll Bikini Bottom.</summary>
    <description>Soft-touch recycled polyamide with UPF 50+ protection. Fully lined. Adjustable tie straps for a custom fit.</description>
    <sortCode>10</sortCode>
    <brandName>Atoll</brandName>
    <brandId>ATL</brandId>
    <material>80% Recycled Polyamide, 20% Elastane</material>
    <seasonCode>SS27</seasonCode>
    <seasonDesc>Spring/Summer 2027</seasonDesc>
    <categoryCode>WOMENS</categoryCode>
    <categoryDesc>Women's</categoryDesc>
    <collectionId>ATOLL-SS27</collectionId>
    <collectionDesc>Atoll SS27</collectionDesc>
    <minimalOrderQuantity>0</minimalOrderQuantity>
    <deliveryStartDate>20270201</deliveryStartDate>
    <deliveryEndDate>20270630</deliveryEndDate>
    <firstReceiptDate>20270201</firstReceiptDate>
    <productGroupCode>BIKINI-TOPS</productGroupCode>
    <productGroupDesc>Bikini Tops</productGroupDesc>
    <marginGroupCode>STD</marginGroupCode>
    <discountGroupCode>SWIM</discountGroupCode>
    <noos>0</noos>
    <fit>Triangle, adjustable tie straps</fit>
    <gender>FEMALE</gender>

    <translations>
      <translation language="nl">
        <name>Atoll Bikinitop</name>
        <summary>Verstelbare driehoekige top in kenmerkend Coral — te combineren met de Atoll Bikinibroek.</summary>
        <description>Zacht gerecycled polyamide met UPF 50+ bescherming. Volledig gevoerd. Verstelbare strikbandjes voor een perfecte pasvorm.</description>
        <categoryDesc>Dames</categoryDesc>
        <seasonDesc>Lente/Zomer 2027</seasonDesc>
        <gender>Vrouw</gender>
        <fit>Driehoek, verstelbare strikbandjes</fit>
        <material>80% Gerecycled Polyamide, 20% Elastaan</material>
      </translation>
      <translation language="fr">
        <name>Haut de bikini Atoll</name>
        <summary>Haut triangle réglable en Corail signature — à associer au bas de bikini Atoll.</summary>
        <description>Polyamide recyclé doux avec protection UPF 50+. Entièrement doublé. Bretelles à nouer ajustables pour un ajustement parfait.</description>
        <categoryDesc>Femmes</categoryDesc>
        <seasonDesc>Printemps/Été 2027</seasonDesc>
        <gender>Femme</gender>
        <fit>Triangle, bretelles à nouer ajustables</fit>
        <material>80% Polyamide recyclé, 20% Élasthanne</material>
      </translation>
    </translations>

    <prices>
      <!-- Default wholesale price -->
      <price>
        <currencyCode>EUR</currencyCode>
        <retailPrice>89.95</retailPrice>
        <wholesalePrice>45.00</wholesalePrice>
        <net>false</net>
      </price>
      <!-- European wholesale partners -->
      <price>
        <currencyCode>EUR</currencyCode>
        <priceGroup>WHOLESALE-EU</priceGroup>
        <retailPrice>89.95</retailPrice>
        <wholesalePrice>40.50</wholesalePrice>
        <net>false</net>
      </price>
    </prices>

    <sizes>
      <!-- Band 75 — cups C D E F -->
      <size>
        <name>75</name>
        <subSizeName>C</subSizeName>
        <sortCode>10</sortCode>
        <subSizeSortCode>1</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001001101</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>6</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>75</name>
        <subSizeName>D</subSizeName>
        <sortCode>10</sortCode>
        <subSizeSortCode>2</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001001102</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>10</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>75</name>
        <subSizeName>E</subSizeName>
        <sortCode>10</sortCode>
        <subSizeSortCode>3</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001001103</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>6</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>75</name>
        <subSizeName>F</subSizeName>
        <sortCode>10</sortCode>
        <subSizeSortCode>4</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001001104</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>4</quantity></stockLevel>
        </stockLevels>
      </size>
      <!-- Band 80 — cups C D E F -->
      <size>
        <name>80</name>
        <subSizeName>C</subSizeName>
        <sortCode>20</sortCode>
        <subSizeSortCode>1</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001001201</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>12</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>80</name>
        <subSizeName>D</subSizeName>
        <sortCode>20</sortCode>
        <subSizeSortCode>2</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001001202</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>20</quantity></stockLevel>
          <stockLevel><startDate>20280201</startDate><quantity>40</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>80</name>
        <subSizeName>E</subSizeName>
        <sortCode>20</sortCode>
        <subSizeSortCode>3</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001001203</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>14</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>80</name>
        <subSizeName>F</subSizeName>
        <sortCode>20</sortCode>
        <subSizeSortCode>4</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001001204</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>8</quantity></stockLevel>
        </stockLevels>
      </size>
      <!-- Band 85 — cups C D E F -->
      <size>
        <name>85</name>
        <subSizeName>C</subSizeName>
        <sortCode>30</sortCode>
        <subSizeSortCode>1</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001001301</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>10</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>85</name>
        <subSizeName>D</subSizeName>
        <sortCode>30</sortCode>
        <subSizeSortCode>2</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001001302</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>18</quantity></stockLevel>
          <stockLevel><startDate>20280201</startDate><quantity>36</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>85</name>
        <subSizeName>E</subSizeName>
        <sortCode>30</sortCode>
        <subSizeSortCode>3</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001001303</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>14</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>85</name>
        <subSizeName>F</subSizeName>
        <sortCode>30</sortCode>
        <subSizeSortCode>4</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001001304</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>10</quantity></stockLevel>
        </stockLevels>
      </size>
      <!-- Band 90 — cups C D E F -->
      <size>
        <name>90</name>
        <subSizeName>C</subSizeName>
        <sortCode>40</sortCode>
        <subSizeSortCode>1</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001001401</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>6</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>90</name>
        <subSizeName>D</subSizeName>
        <sortCode>40</sortCode>
        <subSizeSortCode>2</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001001402</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>10</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>90</name>
        <subSizeName>E</subSizeName>
        <sortCode>40</sortCode>
        <subSizeSortCode>3</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001001403</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>8</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>90</name>
        <subSizeName>F</subSizeName>
        <sortCode>40</sortCode>
        <subSizeSortCode>4</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001001404</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>6</quantity></stockLevel>
        </stockLevels>
      </size>
    </sizes>

    <deliveryWindows>
      <deliveryWindow>
        <code>SS27-D1</code>
        <description>Spring 2027 — Drop 1</description>
        <startDate>20270201</startDate>
        <endDate>20270331</endDate>
        <sortCode>1</sortCode>
      </deliveryWindow>
      <deliveryWindow>
        <code>SS27-D2</code>
        <description>Summer 2027 — Drop 2</description>
        <startDate>20270501</startDate>
        <endDate>20270630</endDate>
        <sortCode>2</sortCode>
      </deliveryWindow>
    </deliveryWindows>

    <extraFields>
      <extraField>
        <name>fabric</name>
        <description>Fabric composition</description>
        <value>80% Recycled Polyamide, 20% Elastane</value>
        <group>materials</group>
        <important>false</important>
      </extraField>
      <extraField>
        <name>upf</name>
        <description>UV protection</description>
        <value>UPF 50+</value>
        <group>materials</group>
        <important>true</important>
      </extraField>
      <extraField>
        <name>care</name>
        <description>Care instructions</description>
        <value>Hand wash cold. Do not tumble dry. Do not bleach.</value>
        <group>materials</group>
        <important>false</important>
      </extraField>
      <extraField>
        <name>made-in</name>
        <description>Made in</description>
        <value>Portugal</value>
        <group>provenance</group>
        <important>false</important>
      </extraField>
    </extraFields>

    <labels>
      <label>
        <text>New</text>
        <backgroundColor>FF7F50</backgroundColor>
      </label>
    </labels>

  </product>

  <!-- ============================================================
       ATL-BKT / BLK  —  Atoll Bikini Top, Black
       Same EU numeric size grid as Coral colorway (75–90 × C–F)
       ============================================================ -->
  <product>
    <uniqueId>ATL-BKT</uniqueId>
    <colorCode>BLK</colorCode>
    <colorDesc>Black</colorDesc>
    <styleAttributes>SS27</styleAttributes>
    <status>ACTIVE</status>
    <searchText>bikini top swimwear black women swimsuit</searchText>
    <name>Atoll Bikini Top</name>
    <summary>Adjustable triangle top in classic Black — mix and match with the Atoll Bikini Bottom.</summary>
    <description>Soft-touch recycled polyamide with UPF 50+ protection. Fully lined. Adjustable tie straps for a custom fit.</description>
    <sortCode>11</sortCode>
    <brandName>Atoll</brandName>
    <brandId>ATL</brandId>
    <material>80% Recycled Polyamide, 20% Elastane</material>
    <seasonCode>SS27</seasonCode>
    <seasonDesc>Spring/Summer 2027</seasonDesc>
    <categoryCode>WOMENS</categoryCode>
    <categoryDesc>Women's</categoryDesc>
    <collectionId>ATOLL-SS27</collectionId>
    <collectionDesc>Atoll SS27</collectionDesc>
    <minimalOrderQuantity>0</minimalOrderQuantity>
    <deliveryStartDate>20270201</deliveryStartDate>
    <deliveryEndDate>20270630</deliveryEndDate>
    <firstReceiptDate>20270201</firstReceiptDate>
    <productGroupCode>BIKINI-TOPS</productGroupCode>
    <productGroupDesc>Bikini Tops</productGroupDesc>
    <marginGroupCode>STD</marginGroupCode>
    <discountGroupCode>SWIM</discountGroupCode>
    <noos>0</noos>
    <fit>Triangle, adjustable tie straps</fit>
    <gender>FEMALE</gender>

    <translations>
      <translation language="nl">
        <name>Atoll Bikinitop</name>
        <summary>Verstelbare driehoekige top in klassiek Zwart — te combineren met de Atoll Bikinibroek.</summary>
        <description>Zacht gerecycled polyamide met UPF 50+ bescherming. Volledig gevoerd. Verstelbare strikbandjes voor een perfecte pasvorm.</description>
        <categoryDesc>Dames</categoryDesc>
        <seasonDesc>Lente/Zomer 2027</seasonDesc>
        <gender>Vrouw</gender>
        <fit>Driehoek, verstelbare strikbandjes</fit>
        <material>80% Gerecycled Polyamide, 20% Elastaan</material>
      </translation>
      <translation language="fr">
        <name>Haut de bikini Atoll</name>
        <summary>Haut triangle réglable en Noir classique — à associer au bas de bikini Atoll.</summary>
        <description>Polyamide recyclé doux avec protection UPF 50+. Entièrement doublé. Bretelles à nouer ajustables pour un ajustement parfait.</description>
        <categoryDesc>Femmes</categoryDesc>
        <seasonDesc>Printemps/Été 2027</seasonDesc>
        <gender>Femme</gender>
        <fit>Triangle, bretelles à nouer ajustables</fit>
        <material>80% Polyamide recyclé, 20% Élasthanne</material>
      </translation>
    </translations>

    <prices>
      <price>
        <currencyCode>EUR</currencyCode>
        <retailPrice>89.95</retailPrice>
        <wholesalePrice>45.00</wholesalePrice>
        <net>false</net>
      </price>
      <price>
        <currencyCode>EUR</currencyCode>
        <priceGroup>WHOLESALE-EU</priceGroup>
        <retailPrice>89.95</retailPrice>
        <wholesalePrice>40.50</wholesalePrice>
        <net>false</net>
      </price>
    </prices>

    <sizes>
      <!-- Band 75 — cups C D E F -->
      <size>
        <name>75</name>
        <subSizeName>C</subSizeName>
        <sortCode>10</sortCode>
        <subSizeSortCode>1</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001002101</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>8</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>75</name>
        <subSizeName>D</subSizeName>
        <sortCode>10</sortCode>
        <subSizeSortCode>2</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001002102</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>12</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>75</name>
        <subSizeName>E</subSizeName>
        <sortCode>10</sortCode>
        <subSizeSortCode>3</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001002103</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>8</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>75</name>
        <subSizeName>F</subSizeName>
        <sortCode>10</sortCode>
        <subSizeSortCode>4</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001002104</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>5</quantity></stockLevel>
        </stockLevels>
      </size>
      <!-- Band 80 — cups C D E F -->
      <size>
        <name>80</name>
        <subSizeName>C</subSizeName>
        <sortCode>20</sortCode>
        <subSizeSortCode>1</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001002201</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>15</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>80</name>
        <subSizeName>D</subSizeName>
        <sortCode>20</sortCode>
        <subSizeSortCode>2</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001002202</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>25</quantity></stockLevel>
          <stockLevel><startDate>20280201</startDate><quantity>50</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>80</name>
        <subSizeName>E</subSizeName>
        <sortCode>20</sortCode>
        <subSizeSortCode>3</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001002203</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>18</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>80</name>
        <subSizeName>F</subSizeName>
        <sortCode>20</sortCode>
        <subSizeSortCode>4</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001002204</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>10</quantity></stockLevel>
        </stockLevels>
      </size>
      <!-- Band 85 — cups C D E F -->
      <size>
        <name>85</name>
        <subSizeName>C</subSizeName>
        <sortCode>30</sortCode>
        <subSizeSortCode>1</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001002301</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>12</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>85</name>
        <subSizeName>D</subSizeName>
        <sortCode>30</sortCode>
        <subSizeSortCode>2</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001002302</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>22</quantity></stockLevel>
          <stockLevel><startDate>20280201</startDate><quantity>44</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>85</name>
        <subSizeName>E</subSizeName>
        <sortCode>30</sortCode>
        <subSizeSortCode>3</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001002303</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>16</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>85</name>
        <subSizeName>F</subSizeName>
        <sortCode>30</sortCode>
        <subSizeSortCode>4</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001002304</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>12</quantity></stockLevel>
        </stockLevels>
      </size>
      <!-- Band 90 — cups C D E F -->
      <size>
        <name>90</name>
        <subSizeName>C</subSizeName>
        <sortCode>40</sortCode>
        <subSizeSortCode>1</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001002401</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>8</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>90</name>
        <subSizeName>D</subSizeName>
        <sortCode>40</sortCode>
        <subSizeSortCode>2</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001002402</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>12</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>90</name>
        <subSizeName>E</subSizeName>
        <sortCode>40</sortCode>
        <subSizeSortCode>3</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001002403</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>10</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>90</name>
        <subSizeName>F</subSizeName>
        <sortCode>40</sortCode>
        <subSizeSortCode>4</subSizeSortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001002404</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>8</quantity></stockLevel>
        </stockLevels>
      </size>
    </sizes>

    <deliveryWindows>
      <deliveryWindow>
        <code>SS27-D1</code>
        <description>Spring 2027 — Drop 1</description>
        <startDate>20270201</startDate>
        <endDate>20270331</endDate>
        <sortCode>1</sortCode>
      </deliveryWindow>
      <deliveryWindow>
        <code>SS27-D2</code>
        <description>Summer 2027 — Drop 2</description>
        <startDate>20270501</startDate>
        <endDate>20270630</endDate>
        <sortCode>2</sortCode>
      </deliveryWindow>
    </deliveryWindows>

    <labels>
      <label>
        <text>New</text>
        <backgroundColor>FF7F50</backgroundColor>
      </label>
    </labels>
  </product>

  <!-- ============================================================
       ATL-BKB / CRL  —  Atoll Bikini Bottom, Coral
       Matches ATL-BKT/CRL — see atoll_product_relations.xml
       ============================================================ -->
  <product>
    <uniqueId>ATL-BKB</uniqueId>
    <colorCode>CRL</colorCode>
    <colorDesc>Coral</colorDesc>
    <styleAttributes>SS27</styleAttributes>
    <status>ACTIVE</status>
    <searchText>bikini bottom swimwear coral women swimsuit</searchText>
    <name>Atoll Bikini Bottom</name>
    <summary>Mid-rise bikini bottom in Coral — the matching set partner for the Atoll Bikini Top.</summary>
    <description>Soft-touch recycled polyamide with UPF 50+ protection. Fully lined. Mid-rise cut.</description>
    <sortCode>20</sortCode>
    <brandName>Atoll</brandName>
    <brandId>ATL</brandId>
    <material>80% Recycled Polyamide, 20% Elastane</material>
    <seasonCode>SS27</seasonCode>
    <seasonDesc>Spring/Summer 2027</seasonDesc>
    <categoryCode>WOMENS</categoryCode>
    <categoryDesc>Women's</categoryDesc>
    <collectionId>ATOLL-SS27</collectionId>
    <collectionDesc>Atoll SS27</collectionDesc>
    <minimalOrderQuantity>0</minimalOrderQuantity>
    <deliveryStartDate>20270201</deliveryStartDate>
    <deliveryEndDate>20270630</deliveryEndDate>
    <firstReceiptDate>20270201</firstReceiptDate>
    <productGroupCode>BIKINI-BOTTOMS</productGroupCode>
    <productGroupDesc>Bikini Bottoms</productGroupDesc>
    <marginGroupCode>STD</marginGroupCode>
    <discountGroupCode>SWIM</discountGroupCode>
    <noos>0</noos>
    <gender>FEMALE</gender>

    <translations>
      <translation language="nl">
        <name>Atoll Bikinibroek</name>
        <summary>Hoge bikinibroek in kenmerkend Coral — het bijpassende stuk voor de Atoll Bikinitop.</summary>
        <description>Zacht gerecycled polyamide met UPF 50+ bescherming. Strikbandjes aan de zijkanten voor een verstelbare pasvorm.</description>
        <categoryDesc>Dames</categoryDesc>
        <seasonDesc>Lente/Zomer 2027</seasonDesc>
        <gender>Vrouw</gender>
        <material>80% Gerecycled Polyamide, 20% Elastaan</material>
      </translation>
      <translation language="fr">
        <name>Bas de bikini Atoll</name>
        <summary>Bas de bikini taille mi-haute en Corail signature — la pièce assortie au haut de bikini Atoll.</summary>
        <description>Polyamide recyclé doux avec protection UPF 50+. Liens latéraux pour un ajustement réglable.</description>
        <categoryDesc>Femmes</categoryDesc>
        <seasonDesc>Printemps/Été 2027</seasonDesc>
        <gender>Femme</gender>
        <material>80% Polyamide recyclé, 20% Élasthanne</material>
      </translation>
    </translations>

    <prices>
      <price>
        <currencyCode>EUR</currencyCode>
        <retailPrice>69.95</retailPrice>
        <wholesalePrice>35.00</wholesalePrice>
        <net>false</net>
      </price>
      <price>
        <currencyCode>EUR</currencyCode>
        <priceGroup>WHOLESALE-EU</priceGroup>
        <retailPrice>69.95</retailPrice>
        <wholesalePrice>31.50</wholesalePrice>
        <net>false</net>
      </price>
    </prices>

    <sizes>
      <size>
        <name>XS</name>
        <sortCode>10</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001003001</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>24</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>S</name>
        <sortCode>20</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001003002</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>48</quantity></stockLevel>
          <stockLevel><startDate>20280201</startDate><quantity>80</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>M</name>
        <sortCode>30</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001003003</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>60</quantity></stockLevel>
          <stockLevel><startDate>20280201</startDate><quantity>100</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>L</name>
        <sortCode>40</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001003004</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>36</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>XL</name>
        <sortCode>50</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001003005</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>18</quantity></stockLevel>
        </stockLevels>
      </size>
    </sizes>

    <deliveryWindows>
      <deliveryWindow>
        <code>SS27-D1</code>
        <description>Spring 2027 — Drop 1</description>
        <startDate>20270201</startDate>
        <endDate>20270331</endDate>
        <sortCode>1</sortCode>
      </deliveryWindow>
      <deliveryWindow>
        <code>SS27-D2</code>
        <description>Summer 2027 — Drop 2</description>
        <startDate>20270501</startDate>
        <endDate>20270630</endDate>
        <sortCode>2</sortCode>
      </deliveryWindow>
    </deliveryWindows>

    <labels>
      <label>
        <text>New</text>
        <backgroundColor>FF7F50</backgroundColor>
      </label>
    </labels>
  </product>

  <!-- ============================================================
       ATL-BKB / BLK  —  Atoll Bikini Bottom, Black
       Matches ATL-BKT/BLK — see atoll_product_relations.xml
       ============================================================ -->
  <product>
    <uniqueId>ATL-BKB</uniqueId>
    <colorCode>BLK</colorCode>
    <colorDesc>Black</colorDesc>
    <styleAttributes>SS27</styleAttributes>
    <status>ACTIVE</status>
    <searchText>bikini bottom swimwear black women swimsuit</searchText>
    <name>Atoll Bikini Bottom</name>
    <summary>Mid-rise bikini bottom in Black — the matching set partner for the Atoll Bikini Top.</summary>
    <description>Soft-touch recycled polyamide with UPF 50+ protection. Fully lined. Mid-rise cut.</description>
    <sortCode>21</sortCode>
    <brandName>Atoll</brandName>
    <brandId>ATL</brandId>
    <material>80% Recycled Polyamide, 20% Elastane</material>
    <seasonCode>SS27</seasonCode>
    <seasonDesc>Spring/Summer 2027</seasonDesc>
    <categoryCode>WOMENS</categoryCode>
    <categoryDesc>Women's</categoryDesc>
    <collectionId>ATOLL-SS27</collectionId>
    <collectionDesc>Atoll SS27</collectionDesc>
    <minimalOrderQuantity>0</minimalOrderQuantity>
    <deliveryStartDate>20270201</deliveryStartDate>
    <deliveryEndDate>20270630</deliveryEndDate>
    <firstReceiptDate>20270201</firstReceiptDate>
    <productGroupCode>BIKINI-BOTTOMS</productGroupCode>
    <productGroupDesc>Bikini Bottoms</productGroupDesc>
    <marginGroupCode>STD</marginGroupCode>
    <discountGroupCode>SWIM</discountGroupCode>
    <noos>0</noos>
    <gender>FEMALE</gender>

    <translations>
      <translation language="nl">
        <name>Atoll Bikinibroek</name>
        <summary>Hoge bikinibroek in klassiek Zwart — het bijpassende stuk voor de Atoll Bikinitop.</summary>
        <description>Zacht gerecycled polyamide met UPF 50+ bescherming. Strikbandjes aan de zijkanten voor een verstelbare pasvorm.</description>
        <categoryDesc>Dames</categoryDesc>
        <seasonDesc>Lente/Zomer 2027</seasonDesc>
        <gender>Vrouw</gender>
        <material>80% Gerecycled Polyamide, 20% Elastaan</material>
      </translation>
      <translation language="fr">
        <name>Bas de bikini Atoll</name>
        <summary>Bas de bikini taille mi-haute en Noir classique — la pièce assortie au haut de bikini Atoll.</summary>
        <description>Polyamide recyclé doux avec protection UPF 50+. Liens latéraux pour un ajustement réglable.</description>
        <categoryDesc>Femmes</categoryDesc>
        <seasonDesc>Printemps/Été 2027</seasonDesc>
        <gender>Femme</gender>
        <material>80% Polyamide recyclé, 20% Élasthanne</material>
      </translation>
    </translations>

    <prices>
      <price>
        <currencyCode>EUR</currencyCode>
        <retailPrice>69.95</retailPrice>
        <wholesalePrice>35.00</wholesalePrice>
        <net>false</net>
      </price>
      <price>
        <currencyCode>EUR</currencyCode>
        <priceGroup>WHOLESALE-EU</priceGroup>
        <retailPrice>69.95</retailPrice>
        <wholesalePrice>31.50</wholesalePrice>
        <net>false</net>
      </price>
    </prices>

    <sizes>
      <size>
        <name>XS</name>
        <sortCode>10</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001004001</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>28</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>S</name>
        <sortCode>20</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001004002</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>55</quantity></stockLevel>
          <stockLevel><startDate>20280201</startDate><quantity>90</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>M</name>
        <sortCode>30</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001004003</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>70</quantity></stockLevel>
          <stockLevel><startDate>20280201</startDate><quantity>110</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>L</name>
        <sortCode>40</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001004004</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>40</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>XL</name>
        <sortCode>50</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001004005</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>20</quantity></stockLevel>
        </stockLevels>
      </size>
    </sizes>

    <deliveryWindows>
      <deliveryWindow>
        <code>SS27-D1</code>
        <description>Spring 2027 — Drop 1</description>
        <startDate>20270201</startDate>
        <endDate>20270331</endDate>
        <sortCode>1</sortCode>
      </deliveryWindow>
      <deliveryWindow>
        <code>SS27-D2</code>
        <description>Summer 2027 — Drop 2</description>
        <startDate>20270501</startDate>
        <endDate>20270630</endDate>
        <sortCode>2</sortCode>
      </deliveryWindow>
    </deliveryWindows>
  </product>

  <!-- ============================================================
       ATL-OPS / SND  —  Atoll One-Piece, Sand
       ============================================================ -->
  <product>
    <uniqueId>ATL-OPS</uniqueId>
    <colorCode>SND</colorCode>
    <colorDesc>Sand</colorDesc>
    <styleAttributes>SS27</styleAttributes>
    <status>ACTIVE</status>
    <searchText>one piece swimsuit sand women swimwear monokini</searchText>
    <name>Atoll One-Piece</name>
    <summary>A clean, minimal one-piece in warm Sand — effortless from pool to beach.</summary>
    <description>Soft-touch recycled polyamide with UPF 50+ protection. Fully lined. Open back with adjustable tie.</description>
    <sortCode>30</sortCode>
    <brandName>Atoll</brandName>
    <brandId>ATL</brandId>
    <material>80% Recycled Polyamide, 20% Elastane</material>
    <seasonCode>SS27</seasonCode>
    <seasonDesc>Spring/Summer 2027</seasonDesc>
    <categoryCode>WOMENS</categoryCode>
    <categoryDesc>Women's</categoryDesc>
    <collectionId>ATOLL-SS27</collectionId>
    <collectionDesc>Atoll SS27</collectionDesc>
    <minimalOrderQuantity>0</minimalOrderQuantity>
    <deliveryStartDate>20270201</deliveryStartDate>
    <deliveryEndDate>20270630</deliveryEndDate>
    <firstReceiptDate>20270201</firstReceiptDate>
    <productGroupCode>ONE-PIECE</productGroupCode>
    <productGroupDesc>One-Piece</productGroupDesc>
    <marginGroupCode>STD</marginGroupCode>
    <discountGroupCode>SWIM</discountGroupCode>
    <noos>0</noos>
    <gender>FEMALE</gender>

    <translations>
      <translation language="nl">
        <name>Atoll Badpak</name>
        <summary>Strak badpak in warm Zand — UPF 50+ bescherming van zwembad tot strand.</summary>
        <description>Zacht gerecycled polyamide met UPF 50+ bescherming. Uitneembare cups. Verstevigde achterkant.</description>
        <categoryDesc>Dames</categoryDesc>
        <seasonDesc>Lente/Zomer 2027</seasonDesc>
        <gender>Vrouw</gender>
        <material>80% Gerecycled Polyamide, 20% Elastaan</material>
      </translation>
      <translation language="fr">
        <name>Maillot une pièce Atoll</name>
        <summary>Maillot de bain une pièce élancé en Sable chaleureux — protection UPF 50+ de la piscine à la plage.</summary>
        <description>Polyamide recyclé doux avec protection UPF 50+. Bonnets amovibles. Dos renforcé.</description>
        <categoryDesc>Femmes</categoryDesc>
        <seasonDesc>Printemps/Été 2027</seasonDesc>
        <gender>Femme</gender>
        <material>80% Polyamide recyclé, 20% Élasthanne</material>
      </translation>
    </translations>

    <prices>
      <price>
        <currencyCode>EUR</currencyCode>
        <retailPrice>129.95</retailPrice>
        <wholesalePrice>65.00</wholesalePrice>
        <net>false</net>
      </price>
      <price>
        <currencyCode>EUR</currencyCode>
        <priceGroup>WHOLESALE-EU</priceGroup>
        <retailPrice>129.95</retailPrice>
        <wholesalePrice>58.50</wholesalePrice>
        <net>false</net>
      </price>
    </prices>

    <sizes>
      <size>
        <name>XS</name>
        <sortCode>10</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001005001</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>14</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>S</name>
        <sortCode>20</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001005002</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>30</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>M</name>
        <sortCode>30</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001005003</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>38</quantity></stockLevel>
          <stockLevel><startDate>20280201</startDate><quantity>60</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>L</name>
        <sortCode>40</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001005004</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>22</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>XL</name>
        <sortCode>50</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001005005</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>10</quantity></stockLevel>
        </stockLevels>
      </size>
    </sizes>

    <deliveryWindows>
      <deliveryWindow>
        <code>SS27-D1</code>
        <description>Spring 2027 — Drop 1</description>
        <startDate>20270201</startDate>
        <endDate>20270331</endDate>
        <sortCode>1</sortCode>
      </deliveryWindow>
    </deliveryWindows>

    <labels>
      <label>
        <text>New</text>
        <backgroundColor>FF7F50</backgroundColor>
      </label>
    </labels>
  </product>

  <!-- ============================================================
       ATL-SHS / NVY  —  Atoll Swim Shorts, Navy
       ============================================================ -->
  <product>
    <uniqueId>ATL-SHS</uniqueId>
    <colorCode>NVY</colorCode>
    <colorDesc>Navy</colorDesc>
    <styleAttributes>SS27</styleAttributes>
    <status>ACTIVE</status>
    <searchText>swim shorts navy men swimwear board shorts</searchText>
    <name>Atoll Swim Shorts</name>
    <summary>Quick-dry swim shorts in classic Navy. Side pockets, back zip pocket, adjustable waistband.</summary>
    <description>Lightweight recycled polyester with a fast-dry finish. Mesh liner. Adjustable drawstring waist.</description>
    <sortCode>40</sortCode>
    <brandName>Atoll</brandName>
    <brandId>ATL</brandId>
    <material>100% Recycled Polyester</material>
    <seasonCode>SS27</seasonCode>
    <seasonDesc>Spring/Summer 2027</seasonDesc>
    <categoryCode>MENS</categoryCode>
    <categoryDesc>Men's</categoryDesc>
    <collectionId>ATOLL-SS27</collectionId>
    <collectionDesc>Atoll SS27</collectionDesc>
    <minimalOrderQuantity>0</minimalOrderQuantity>
    <deliveryStartDate>20270201</deliveryStartDate>
    <deliveryEndDate>20270630</deliveryEndDate>
    <firstReceiptDate>20270201</firstReceiptDate>
    <productGroupCode>SWIM-SHORTS</productGroupCode>
    <productGroupDesc>Swim Shorts</productGroupDesc>
    <marginGroupCode>STD</marginGroupCode>
    <discountGroupCode>SWIM</discountGroupCode>
    <noos>0</noos>
    <gender>MALE</gender>

    <translations>
      <translation language="nl">
        <name>Atoll Zwembroek</name>
        <summary>Sneldrogende zwembroek in klassiek Marine — voor het water en daarbuiten.</summary>
        <description>Sneldrogend gerecycled polyester met UPF 50+ bescherming. Elastische taille met koord. Zijzakken.</description>
        <categoryDesc>Heren</categoryDesc>
        <seasonDesc>Lente/Zomer 2027</seasonDesc>
        <gender>Man</gender>
        <material>100% Gerecycled Polyester</material>
      </translation>
      <translation language="fr">
        <name>Short de bain Atoll</name>
        <summary>Short de bain à séchage rapide en Marine classique — dans et hors de l'eau.</summary>
        <description>Polyester recyclé à séchage rapide avec protection UPF 50+. Ceinture élastique avec cordon. Poches latérales.</description>
        <categoryDesc>Hommes</categoryDesc>
        <seasonDesc>Printemps/Été 2027</seasonDesc>
        <gender>Homme</gender>
        <material>100% Polyester recyclé</material>
      </translation>
    </translations>

    <prices>
      <price>
        <currencyCode>EUR</currencyCode>
        <retailPrice>79.95</retailPrice>
        <wholesalePrice>40.00</wholesalePrice>
        <net>false</net>
      </price>
      <price>
        <currencyCode>EUR</currencyCode>
        <priceGroup>WHOLESALE-EU</priceGroup>
        <retailPrice>79.95</retailPrice>
        <wholesalePrice>36.00</wholesalePrice>
        <net>false</net>
      </price>
    </prices>

    <sizes>
      <size>
        <name>S</name>
        <sortCode>10</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001006001</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>30</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>M</name>
        <sortCode>20</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001006002</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>60</quantity></stockLevel>
          <stockLevel><startDate>20280201</startDate><quantity>100</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>L</name>
        <sortCode>30</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001006003</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>55</quantity></stockLevel>
          <stockLevel><startDate>20280201</startDate><quantity>90</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>XL</name>
        <sortCode>40</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001006004</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>35</quantity></stockLevel>
        </stockLevels>
      </size>
    </sizes>

    <deliveryWindows>
      <deliveryWindow>
        <code>SS27-D1</code>
        <description>Spring 2027 — Drop 1</description>
        <startDate>20270201</startDate>
        <endDate>20270331</endDate>
        <sortCode>1</sortCode>
      </deliveryWindow>
      <deliveryWindow>
        <code>SS27-D2</code>
        <description>Summer 2027 — Drop 2</description>
        <startDate>20270501</startDate>
        <endDate>20270630</endDate>
        <sortCode>2</sortCode>
      </deliveryWindow>
    </deliveryWindows>

    <labels>
      <label>
        <text>New</text>
        <backgroundColor>FF7F50</backgroundColor>
      </label>
    </labels>
  </product>

  <!-- ============================================================
       ATL-SHS / OLV  —  Atoll Swim Shorts, Olive
       ============================================================ -->
  <product>
    <uniqueId>ATL-SHS</uniqueId>
    <colorCode>OLV</colorCode>
    <colorDesc>Olive</colorDesc>
    <styleAttributes>SS27</styleAttributes>
    <status>ACTIVE</status>
    <searchText>swim shorts olive men swimwear board shorts</searchText>
    <name>Atoll Swim Shorts</name>
    <summary>Quick-dry swim shorts in earthy Olive. Side pockets, back zip pocket, adjustable waistband.</summary>
    <description>Lightweight recycled polyester with a fast-dry finish. Mesh liner. Adjustable drawstring waist.</description>
    <sortCode>41</sortCode>
    <brandName>Atoll</brandName>
    <brandId>ATL</brandId>
    <material>100% Recycled Polyester</material>
    <seasonCode>SS27</seasonCode>
    <seasonDesc>Spring/Summer 2027</seasonDesc>
    <categoryCode>MENS</categoryCode>
    <categoryDesc>Men's</categoryDesc>
    <collectionId>ATOLL-SS27</collectionId>
    <collectionDesc>Atoll SS27</collectionDesc>
    <minimalOrderQuantity>0</minimalOrderQuantity>
    <deliveryStartDate>20270201</deliveryStartDate>
    <deliveryEndDate>20270630</deliveryEndDate>
    <firstReceiptDate>20270201</firstReceiptDate>
    <productGroupCode>SWIM-SHORTS</productGroupCode>
    <productGroupDesc>Swim Shorts</productGroupDesc>
    <marginGroupCode>STD</marginGroupCode>
    <discountGroupCode>SWIM</discountGroupCode>
    <noos>0</noos>
    <gender>MALE</gender>

    <translations>
      <translation language="nl">
        <name>Atoll Zwembroek</name>
        <summary>Sneldrogende zwembroek in aards Olijf — veelzijdig en duurzaam.</summary>
        <description>Sneldrogend gerecycled polyester met UPF 50+ bescherming. Elastische taille met koord. Zijzakken.</description>
        <categoryDesc>Heren</categoryDesc>
        <seasonDesc>Lente/Zomer 2027</seasonDesc>
        <gender>Man</gender>
        <material>100% Gerecycled Polyester</material>
      </translation>
      <translation language="fr">
        <name>Short de bain Atoll</name>
        <summary>Short de bain à séchage rapide en Olive terreux — polyvalent et durable.</summary>
        <description>Polyester recyclé à séchage rapide avec protection UPF 50+. Ceinture élastique avec cordon. Poches latérales.</description>
        <categoryDesc>Hommes</categoryDesc>
        <seasonDesc>Printemps/Été 2027</seasonDesc>
        <gender>Homme</gender>
        <material>100% Polyester recyclé</material>
      </translation>
    </translations>

    <prices>
      <price>
        <currencyCode>EUR</currencyCode>
        <retailPrice>79.95</retailPrice>
        <wholesalePrice>40.00</wholesalePrice>
        <net>false</net>
      </price>
      <price>
        <currencyCode>EUR</currencyCode>
        <priceGroup>WHOLESALE-EU</priceGroup>
        <retailPrice>79.95</retailPrice>
        <wholesalePrice>36.00</wholesalePrice>
        <net>false</net>
      </price>
    </prices>

    <sizes>
      <size>
        <name>S</name>
        <sortCode>10</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001007001</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>25</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>M</name>
        <sortCode>20</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001007002</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>50</quantity></stockLevel>
          <stockLevel><startDate>20280201</startDate><quantity>80</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>L</name>
        <sortCode>30</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001007003</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>45</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>XL</name>
        <sortCode>40</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001007004</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>28</quantity></stockLevel>
        </stockLevels>
      </size>
    </sizes>

    <deliveryWindows>
      <deliveryWindow>
        <code>SS27-D1</code>
        <description>Spring 2027 — Drop 1</description>
        <startDate>20270201</startDate>
        <endDate>20270331</endDate>
        <sortCode>1</sortCode>
      </deliveryWindow>
      <deliveryWindow>
        <code>SS27-D2</code>
        <description>Summer 2027 — Drop 2</description>
        <startDate>20270501</startDate>
        <endDate>20270630</endDate>
        <sortCode>2</sortCode>
      </deliveryWindow>
    </deliveryWindows>
  </product>

  <!-- ============================================================
       ATL-RSG / WHT  —  Atoll Rash Guard, White
       ============================================================ -->
  <product>
    <uniqueId>ATL-RSG</uniqueId>
    <colorCode>WHT</colorCode>
    <colorDesc>White</colorDesc>
    <styleAttributes>SS27</styleAttributes>
    <status>ACTIVE</status>
    <searchText>rash guard white men swimwear surf long sleeve UPF</searchText>
    <name>Atoll Rash Guard</name>
    <summary>Long-sleeve rash guard in White. UPF 50+ sun protection, quick-dry stretch fabric.</summary>
    <description>Flatlock stitching for comfort against skin. Thumb loops to keep sleeves in place. Pairs with Atoll Swim Shorts.</description>
    <sortCode>50</sortCode>
    <brandName>Atoll</brandName>
    <brandId>ATL</brandId>
    <material>88% Recycled Polyester, 12% Elastane</material>
    <seasonCode>SS27</seasonCode>
    <seasonDesc>Spring/Summer 2027</seasonDesc>
    <categoryCode>MENS</categoryCode>
    <categoryDesc>Men's</categoryDesc>
    <collectionId>ATOLL-SS27</collectionId>
    <collectionDesc>Atoll SS27</collectionDesc>
    <minimalOrderQuantity>0</minimalOrderQuantity>
    <deliveryStartDate>20270201</deliveryStartDate>
    <deliveryEndDate>20270630</deliveryEndDate>
    <firstReceiptDate>20270201</firstReceiptDate>
    <productGroupCode>RASH-GUARDS</productGroupCode>
    <productGroupDesc>Rash Guards</productGroupDesc>
    <marginGroupCode>STD</marginGroupCode>
    <discountGroupCode>SWIM</discountGroupCode>
    <noos>0</noos>
    <gender>MALE</gender>

    <translations>
      <translation language="nl">
        <name>Atoll Rashguard</name>
        <summary>Strakke rashguard in helder Wit — UPF 50+ bescherming voor surfers en zwemmers.</summary>
        <description>Sneldrogende stof met UPF 50+ bescherming. Platte naden voorkomen schuren. Raglanmouwen voor vrije beweging.</description>
        <categoryDesc>Heren</categoryDesc>
        <seasonDesc>Lente/Zomer 2027</seasonDesc>
        <gender>Man</gender>
        <material>92% Gerecycled Polyester, 8% Elastaan</material>
      </translation>
      <translation language="fr">
        <name>Rashguard Atoll</name>
        <summary>Rashguard ajusté en Blanc lumineux — protection UPF 50+ pour surfeurs et nageurs.</summary>
        <description>Tissu à séchage rapide avec protection UPF 50+. Coutures plates anti-irritation. Manches raglan pour une liberté de mouvement.</description>
        <categoryDesc>Hommes</categoryDesc>
        <seasonDesc>Printemps/Été 2027</seasonDesc>
        <gender>Homme</gender>
        <material>92% Polyester recyclé, 8% Élasthanne</material>
      </translation>
    </translations>

    <prices>
      <price>
        <currencyCode>EUR</currencyCode>
        <retailPrice>89.95</retailPrice>
        <wholesalePrice>45.00</wholesalePrice>
        <net>false</net>
      </price>
      <price>
        <currencyCode>EUR</currencyCode>
        <priceGroup>WHOLESALE-EU</priceGroup>
        <retailPrice>89.95</retailPrice>
        <wholesalePrice>40.50</wholesalePrice>
        <net>false</net>
      </price>
    </prices>

    <sizes>
      <size>
        <name>S</name>
        <sortCode>10</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001008001</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>20</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>M</name>
        <sortCode>20</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001008002</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>40</quantity></stockLevel>
          <stockLevel><startDate>20280201</startDate><quantity>70</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>L</name>
        <sortCode>30</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001008003</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>35</quantity></stockLevel>
          <stockLevel><startDate>20280201</startDate><quantity>60</quantity></stockLevel>
        </stockLevels>
      </size>
      <size>
        <name>XL</name>
        <sortCode>40</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <eanCode>8719001008004</eanCode>
        <stockLevels>
          <stockLevel><startDate>20270201</startDate><quantity>22</quantity></stockLevel>
        </stockLevels>
      </size>
    </sizes>

    <deliveryWindows>
      <deliveryWindow>
        <code>SS27-D1</code>
        <description>Spring 2027 — Drop 1</description>
        <startDate>20270201</startDate>
        <endDate>20270331</endDate>
        <sortCode>1</sortCode>
      </deliveryWindow>
      <deliveryWindow>
        <code>SS27-D2</code>
        <description>Summer 2027 — Drop 2</description>
        <startDate>20270501</startDate>
        <endDate>20270630</endDate>
        <sortCode>2</sortCode>
      </deliveryWindow>
    </deliveryWindows>

    <extraFields>
      <extraField>
        <name>upf</name>
        <description>UV protection</description>
        <value>UPF 50+</value>
        <group>materials</group>
        <important>true</important>
      </extraField>
      <extraField>
        <name>fabric</name>
        <description>Fabric composition</description>
        <value>88% Recycled Polyester, 12% Elastane</value>
        <group>materials</group>
        <important>false</important>
      </extraField>
      <extraField>
        <name>care</name>
        <description>Care instructions</description>
        <value>Machine wash cold. Do not tumble dry. Do not bleach.</value>
        <group>materials</group>
        <important>false</important>
      </extraField>
    </extraFields>
  </product>

</products>
```
{% endcode %}

</details>

***

## Step — Push it

```bash
sftp your-username@sftp.colect.services
sftp> cd datafiles
sftp> put atoll_products.xml
sftp> bye
```

***

## Step — Verify

Open your collection and look for the 8 Atoll products. All of them should appear within a minute — sold out, since there's no stock yet. That's expected; the [next step](stock.md) fixes it.

![](<../.gitbook/assets/atoll-catalog-8-products.png>)

{% hint style="info" %}
No product images in this walkthrough's XML. Most customers manage images through backend settings instead of the `<media>` block — see [Media objects](../documents/products.md#media-objects) if you want the XML-driven alternative.
{% endhint %}

Backend-managed images pick up automatically once uploaded — no XML change needed. Products without an uploaded image just show the placeholder icon until one is added:

![](<../.gitbook/assets/atoll-backend-images.png>)

***

Once all 8 products show up, move on to [Stock](stock.md).
