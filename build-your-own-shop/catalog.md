# The Catalog

First up: get two products into the collection — the Atoll Bikini Top and its matching Bikini Bottom, both in Coral. This is the same `<products>` document from [Your First Document](../getting-started/first-document.md), just with two real products instead of one placeholder.

{% hint style="info" %}
No stock yet on purpose. Sizes are defined here, but quantities come from a separate sub-feed in the [next step](stock.md) — that's a deliberate, commonly-used setup (see [Stock sub-feed vs inline stock](../documents/sizes-stock.md)), and it means you'll see both products appear as sold out until you push stock.
{% endhint %}

***

## Step — Write the document

Create `atoll_products.xml`:

{% code title="atoll_products.xml" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<products>

  <product>
    <uniqueId>ATL-BKT</uniqueId>
    <colorCode>CRL</colorCode>
    <colorDesc>Coral</colorDesc>
    <name>Atoll Bikini Top</name>
    <description>Soft-touch recycled polyamide with UPF 50+ protection. Fully lined. Adjustable tie straps for a custom fit.</description>
    <brandName>Atoll</brandName>
    <categoryCode>WOMENS</categoryCode>
    <categoryDesc>Women's</categoryDesc>

    <prices>
      <price>
        <currencyCode>EUR</currencyCode>
        <retailPrice>89.95</retailPrice>
        <wholesalePrice>45.00</wholesalePrice>
        <net>false</net>
      </price>
    </prices>

    <sizes>
      <size>
        <name>75</name>
        <subSizeName>C</subSizeName>
        <eanCode>8719001001101</eanCode>
      </size>
      <size>
        <name>75</name>
        <subSizeName>D</subSizeName>
        <eanCode>8719001001102</eanCode>
      </size>
      <size>
        <name>80</name>
        <subSizeName>C</subSizeName>
        <eanCode>8719001001201</eanCode>
      </size>
      <size>
        <name>80</name>
        <subSizeName>D</subSizeName>
        <eanCode>8719001001202</eanCode>
      </size>
    </sizes>

    <media>
      <medium format="image">
        <type>primary</type>
        <url>https://assets.atoll-brand.com/ATL-BKT-CRL_primary.jpg</url>
      </medium>
    </media>
  </product>

  <product>
    <uniqueId>ATL-BKB</uniqueId>
    <colorCode>CRL</colorCode>
    <colorDesc>Coral</colorDesc>
    <name>Atoll Bikini Bottom</name>
    <description>Soft-touch recycled polyamide with UPF 50+ protection. Fully lined. Mid-rise cut.</description>
    <brandName>Atoll</brandName>
    <categoryCode>WOMENS</categoryCode>
    <categoryDesc>Women's</categoryDesc>

    <prices>
      <price>
        <currencyCode>EUR</currencyCode>
        <retailPrice>69.95</retailPrice>
        <wholesalePrice>35.00</wholesalePrice>
        <net>false</net>
      </price>
    </prices>

    <sizes>
      <size>
        <name>S</name>
        <eanCode>8719001003002</eanCode>
      </size>
      <size>
        <name>M</name>
        <eanCode>8719001003003</eanCode>
      </size>
      <size>
        <name>L</name>
        <eanCode>8719001003004</eanCode>
      </size>
    </sizes>

    <media>
      <medium format="image">
        <type>primary</type>
        <url>https://assets.atoll-brand.com/ATL-BKB-CRL_primary.jpg</url>
      </medium>
    </media>
  </product>

</products>
```
{% endcode %}

`ATL-BKT` uses band + cup sub-sizes (`75`/`C`, `80`/`D`, …) — see [Sub-sizes](../documents/sizes-stock.md) for how that grid works. `ATL-BKB` uses plain sizes, which is the more common case.

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

Open your collection and look for **Atoll Bikini Top** and **Atoll Bikini Bottom** in Coral. Both should appear within a minute — sold out, since there's no stock yet. That's expected; the [next step](stock.md) fixes it.

<!-- screenshot: catalog view showing both products, sold-out state -->

***

Once both products show up, move on to [Stock](stock.md).
