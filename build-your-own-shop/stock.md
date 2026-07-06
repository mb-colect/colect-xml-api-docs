# Stock

The catalog is in, but every size shows sold out — [sizesLocation is configured](../documents/sizes-stock.md) on this collection, so the connector reads stock only from the dedicated stock sub-feed, ignoring any `<stockLevels>` that happen to be inline in the products file. This step pushes that sub-feed.

***

## Step — Write the document

Create `atoll_stock.xml`. Same root element (`<products>`) and `uniqueId` + `colorCode` identification as the Products document, but each `<product>` here only carries `<sizes>` with stock — no names, prices, or media.

{% code title="atoll_stock.xml" %}
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<products xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

  <!-- ============================================================
       Atoll SS27 — Stock sub-feed
       Preorder quantities (20270201) + SS28 restock (20280201)
       ============================================================ -->

  <!-- ATL-BKT / CRL  —  Bikini Top Coral  (75–90 × C–F) -->
  <product>
    <uniqueId>ATL-BKT</uniqueId>
    <colorCode>CRL</colorCode>
    <sizes>
      <size><name>75</name><subSizeName>C</subSizeName><eanCode>8719001001101</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>6</quantity></stockLevel></stockLevels></size>
      <size><name>75</name><subSizeName>D</subSizeName><eanCode>8719001001102</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>10</quantity></stockLevel></stockLevels></size>
      <size><name>75</name><subSizeName>E</subSizeName><eanCode>8719001001103</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>6</quantity></stockLevel></stockLevels></size>
      <size><name>75</name><subSizeName>F</subSizeName><eanCode>8719001001104</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>4</quantity></stockLevel></stockLevels></size>
      <size><name>80</name><subSizeName>C</subSizeName><eanCode>8719001001201</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>12</quantity></stockLevel></stockLevels></size>
      <size><name>80</name><subSizeName>D</subSizeName><eanCode>8719001001202</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>20</quantity></stockLevel><stockLevel><startDate>20280201</startDate><quantity>40</quantity></stockLevel></stockLevels></size>
      <size><name>80</name><subSizeName>E</subSizeName><eanCode>8719001001203</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>14</quantity></stockLevel></stockLevels></size>
      <size><name>80</name><subSizeName>F</subSizeName><eanCode>8719001001204</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>8</quantity></stockLevel></stockLevels></size>
      <size><name>85</name><subSizeName>C</subSizeName><eanCode>8719001001301</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>10</quantity></stockLevel></stockLevels></size>
      <size><name>85</name><subSizeName>D</subSizeName><eanCode>8719001001302</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>18</quantity></stockLevel><stockLevel><startDate>20280201</startDate><quantity>36</quantity></stockLevel></stockLevels></size>
      <size><name>85</name><subSizeName>E</subSizeName><eanCode>8719001001303</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>14</quantity></stockLevel></stockLevels></size>
      <size><name>85</name><subSizeName>F</subSizeName><eanCode>8719001001304</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>10</quantity></stockLevel></stockLevels></size>
      <size><name>90</name><subSizeName>C</subSizeName><eanCode>8719001001401</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>6</quantity></stockLevel></stockLevels></size>
      <size><name>90</name><subSizeName>D</subSizeName><eanCode>8719001001402</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>10</quantity></stockLevel></stockLevels></size>
      <size><name>90</name><subSizeName>E</subSizeName><eanCode>8719001001403</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>8</quantity></stockLevel></stockLevels></size>
      <size><name>90</name><subSizeName>F</subSizeName><eanCode>8719001001404</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>6</quantity></stockLevel></stockLevels></size>
    </sizes>
  </product>

  <!-- ATL-BKT / BLK  —  Bikini Top Black  (75–90 × C–F) -->
  <product>
    <uniqueId>ATL-BKT</uniqueId>
    <colorCode>BLK</colorCode>
    <sizes>
      <size><name>75</name><subSizeName>C</subSizeName><eanCode>8719001002101</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>8</quantity></stockLevel></stockLevels></size>
      <size><name>75</name><subSizeName>D</subSizeName><eanCode>8719001002102</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>12</quantity></stockLevel></stockLevels></size>
      <size><name>75</name><subSizeName>E</subSizeName><eanCode>8719001002103</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>8</quantity></stockLevel></stockLevels></size>
      <size><name>75</name><subSizeName>F</subSizeName><eanCode>8719001002104</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>5</quantity></stockLevel></stockLevels></size>
      <size><name>80</name><subSizeName>C</subSizeName><eanCode>8719001002201</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>15</quantity></stockLevel></stockLevels></size>
      <size><name>80</name><subSizeName>D</subSizeName><eanCode>8719001002202</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>25</quantity></stockLevel><stockLevel><startDate>20280201</startDate><quantity>50</quantity></stockLevel></stockLevels></size>
      <size><name>80</name><subSizeName>E</subSizeName><eanCode>8719001002203</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>18</quantity></stockLevel></stockLevels></size>
      <size><name>80</name><subSizeName>F</subSizeName><eanCode>8719001002204</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>10</quantity></stockLevel></stockLevels></size>
      <size><name>85</name><subSizeName>C</subSizeName><eanCode>8719001002301</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>12</quantity></stockLevel></stockLevels></size>
      <size><name>85</name><subSizeName>D</subSizeName><eanCode>8719001002302</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>22</quantity></stockLevel><stockLevel><startDate>20280201</startDate><quantity>44</quantity></stockLevel></stockLevels></size>
      <size><name>85</name><subSizeName>E</subSizeName><eanCode>8719001002303</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>16</quantity></stockLevel></stockLevels></size>
      <size><name>85</name><subSizeName>F</subSizeName><eanCode>8719001002304</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>12</quantity></stockLevel></stockLevels></size>
      <size><name>90</name><subSizeName>C</subSizeName><eanCode>8719001002401</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>8</quantity></stockLevel></stockLevels></size>
      <size><name>90</name><subSizeName>D</subSizeName><eanCode>8719001002402</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>12</quantity></stockLevel></stockLevels></size>
      <size><name>90</name><subSizeName>E</subSizeName><eanCode>8719001002403</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>10</quantity></stockLevel></stockLevels></size>
      <size><name>90</name><subSizeName>F</subSizeName><eanCode>8719001002404</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>8</quantity></stockLevel></stockLevels></size>
    </sizes>
  </product>

  <!-- ATL-BKB / CRL  —  Bikini Bottom Coral  (XS–XL) -->
  <product>
    <uniqueId>ATL-BKB</uniqueId>
    <colorCode>CRL</colorCode>
    <sizes>
      <size><name>XS</name><eanCode>8719001003001</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>24</quantity></stockLevel></stockLevels></size>
      <size><name>S</name><eanCode>8719001003002</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>48</quantity></stockLevel><stockLevel><startDate>20280201</startDate><quantity>80</quantity></stockLevel></stockLevels></size>
      <size><name>M</name><eanCode>8719001003003</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>60</quantity></stockLevel><stockLevel><startDate>20280201</startDate><quantity>100</quantity></stockLevel></stockLevels></size>
      <size><name>L</name><eanCode>8719001003004</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>36</quantity></stockLevel></stockLevels></size>
      <size><name>XL</name><eanCode>8719001003005</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>18</quantity></stockLevel></stockLevels></size>
    </sizes>
  </product>

  <!-- ATL-BKB / BLK  —  Bikini Bottom Black  (XS–XL) -->
  <product>
    <uniqueId>ATL-BKB</uniqueId>
    <colorCode>BLK</colorCode>
    <sizes>
      <size><name>XS</name><eanCode>8719001004001</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>28</quantity></stockLevel></stockLevels></size>
      <size><name>S</name><eanCode>8719001004002</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>55</quantity></stockLevel><stockLevel><startDate>20280201</startDate><quantity>90</quantity></stockLevel></stockLevels></size>
      <size><name>M</name><eanCode>8719001004003</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>70</quantity></stockLevel><stockLevel><startDate>20280201</startDate><quantity>110</quantity></stockLevel></stockLevels></size>
      <size><name>L</name><eanCode>8719001004004</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>40</quantity></stockLevel></stockLevels></size>
      <size><name>XL</name><eanCode>8719001004005</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>20</quantity></stockLevel></stockLevels></size>
    </sizes>
  </product>

  <!-- ATL-OPS / SND  —  One-Piece Sand  (XS–XL) -->
  <product>
    <uniqueId>ATL-OPS</uniqueId>
    <colorCode>SND</colorCode>
    <sizes>
      <size><name>XS</name><eanCode>8719001005001</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>14</quantity></stockLevel></stockLevels></size>
      <size><name>S</name><eanCode>8719001005002</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>30</quantity></stockLevel></stockLevels></size>
      <size><name>M</name><eanCode>8719001005003</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>38</quantity></stockLevel><stockLevel><startDate>20280201</startDate><quantity>60</quantity></stockLevel></stockLevels></size>
      <size><name>L</name><eanCode>8719001005004</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>22</quantity></stockLevel></stockLevels></size>
      <size><name>XL</name><eanCode>8719001005005</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>10</quantity></stockLevel></stockLevels></size>
    </sizes>
  </product>

  <!-- ATL-SHS / NVY  —  Swim Shorts Navy  (S–XL) -->
  <product>
    <uniqueId>ATL-SHS</uniqueId>
    <colorCode>NVY</colorCode>
    <sizes>
      <size><name>S</name><eanCode>8719001006001</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>30</quantity></stockLevel></stockLevels></size>
      <size><name>M</name><eanCode>8719001006002</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>60</quantity></stockLevel><stockLevel><startDate>20280201</startDate><quantity>100</quantity></stockLevel></stockLevels></size>
      <size><name>L</name><eanCode>8719001006003</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>55</quantity></stockLevel><stockLevel><startDate>20280201</startDate><quantity>90</quantity></stockLevel></stockLevels></size>
      <size><name>XL</name><eanCode>8719001006004</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>35</quantity></stockLevel></stockLevels></size>
    </sizes>
  </product>

  <!-- ATL-SHS / OLV  —  Swim Shorts Olive  (S–XL) -->
  <product>
    <uniqueId>ATL-SHS</uniqueId>
    <colorCode>OLV</colorCode>
    <sizes>
      <size><name>S</name><eanCode>8719001007001</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>25</quantity></stockLevel></stockLevels></size>
      <size><name>M</name><eanCode>8719001007002</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>50</quantity></stockLevel><stockLevel><startDate>20280201</startDate><quantity>80</quantity></stockLevel></stockLevels></size>
      <size><name>L</name><eanCode>8719001007003</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>45</quantity></stockLevel></stockLevels></size>
      <size><name>XL</name><eanCode>8719001007004</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>28</quantity></stockLevel></stockLevels></size>
    </sizes>
  </product>

  <!-- ATL-RSG / WHT  —  Rash Guard White  (S–XL) -->
  <product>
    <uniqueId>ATL-RSG</uniqueId>
    <colorCode>WHT</colorCode>
    <sizes>
      <size><name>S</name><eanCode>8719001008001</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>20</quantity></stockLevel></stockLevels></size>
      <size><name>M</name><eanCode>8719001008002</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>40</quantity></stockLevel><stockLevel><startDate>20280201</startDate><quantity>70</quantity></stockLevel></stockLevels></size>
      <size><name>L</name><eanCode>8719001008003</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>35</quantity></stockLevel><stockLevel><startDate>20280201</startDate><quantity>60</quantity></stockLevel></stockLevels></size>
      <size><name>XL</name><eanCode>8719001008004</eanCode><stockLevels><stockLevel><startDate>20270201</startDate><quantity>22</quantity></stockLevel></stockLevels></size>
    </sizes>
  </product>

</products>
```
{% endcode %}

Two dates appear throughout: `20270201` is preorder stock (available now, ships when the season starts), `20280201` is the SS28 restock a year out. Some sizes only have the first.

***

## Step — Push it

```bash
sftp your-username@sftp.colect.services
sftp> cd datafiles
sftp> put atoll_stock.xml
sftp> bye
```

***

## Step — Verify

Refresh your collection and open one of the Atoll products — the Coral Bikini Top, for example. Its sizes should now show real stock quantities instead of sold out, and the band/cup sizes with two `<stockLevel>` entries (like `80`/`D`) should show a preorder quantity now and a larger restock quantity from Feb 2028.

<!-- screenshot: order grid / size grid showing stock quantities, ideally the 80/D preorder + restock split -->

***

Once stock shows up, move on to [Matching Sets](relations.md).
