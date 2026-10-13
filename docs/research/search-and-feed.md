# Search demand and the Merchant Center feed

Research for [#56](https://github.com/Kintukka2/FOSLN-WATCH/issues/56). Sources read on 13 October 2026. It builds on [Structured data for FOSLN product pages](https://github.com/Kintukka2/FOSLN-WATCH/blob/research/structured-data/docs/research/structured-data.md) (#12), which covered the markup and the US Merchant Center rules. This note covers Australia, the USD price, and what the feed needs on top of the markup.

The setup assumed throughout: Shopify Basic, store currency USD, one Market (Australia) priced in USD ([ADR 0001](../adr/0001-price-in-usd-for-the-australian-market.md), [ADR 0005](../adr/0005-usd-store-currency-on-basic.md)). Two Models: ACIER (US$149, Dials Chrome and Onyx) and CADRE (US$169, Dials Noir and Champagne). Private label: the Supplier makes the watches and FOSLN brands them. The Google & YouTube channel is the planned feed. This note reports what the sources say. It is not legal advice.

## Short answer

- **Search demand in Australia is for colour and shape words, not style words.** On Google Trends, "gold watch" draws 7 to 9 times the searches of "square watch" and about 3 times "silver watch". "Mens watch" and "ladies watch" are also large. "Minimalist watch", "integrated bracelet watch" and "watches under 200" are close to zero. The most common way people narrow "square watch" is "square gold watch". Competitor brand names are small too: "daniel wellington" is an eighth to a sixth of "gold watch", and "aark" and "nordgreen" barely register. All figures are relative Trends indexes. I could not reach absolute monthly volumes.
- **Competitors title pages with the colour, shape and size words people search.** Daniel Wellington's Australian site uses "Iconic Link - Silver watch with Black dial 36mm | DW" and "Quadro Collection - Square Watch Styles | DW". Nordgreen and Cluse put the model, size, finish and dial colour in the title. AARK's product titles are bare model names. Google sets no title length limit. Shopify's admin warns above 70 characters and suggests 60.
- **A USD feed for Australia is allowed by Google, but Shopify's channel may refuse it.** Google's currency conversion is on by default. It shows the USD price with an estimated AUD price, and the site can keep charging USD. But Shopify's Google & YouTube requirements say "you need to sell in a currency that matches the country". No Shopify page says whether USD prices for Australia pass this check. **Test it before launch** (section 3.1).
- **Shipping costs are required for Australia**, in the same currency as the price (USD). Return policies are optional in Merchant Center but should be added by hand and match the site.
- **Titles in the feed:** 150 characters at most, about 70 shown, no promotional text or capitals for emphasis, and the Dial's distinguishing detail in each variant title.
- **Apparel attributes:** watches are category 201 under Apparel & Accessories (166). Free listings need `color`, `gender` and `age_group` for every product in that category, in every country, plus `item_group_id` for variants. `size` is not needed.
- **GTIN:** leave it out. Submit brand FOSLN and an MPN of FOSLN's own choosing, such as the SKU. Don't set `identifier_exists` to `no`, because FOSLN has a brand and an MPN. Google's own pages disagree on whether a self-made MPN is allowed (section 3.5).
- **The usual reasons a new store is held back** are account-level: missing or mismatched contact details, a missing return policy, and misrepresentation. At product level they are missing shipping costs, price mismatches, missing identifiers or apparel attributes, and image problems. A watch whose design recalls a famous model also carries counterfeit-policy risk if the listing names that model.

---

## 1. Search demand in Australia

### Method

Google Trends, geography Australia, all categories, web search. I pulled the data from Trends' own endpoints on 13 October 2026. Trends gives relative interest only: each term's average weekly score over the period, scaled here so that **"gold watch" = 100**. Terms were compared five at a time with a shared anchor, then rescaled to the anchor. Low terms were also re-run over five years against a smaller anchor, for better resolution.

**These are not search volumes.** Trends doesn't publish volumes. Keyword Planner, Ahrefs and Semrush do, but all need an account, and none was reachable from this session. Below about 1 on this scale, Trends rounds most weeks to zero, so those figures mean "very small", not a precise ratio.

### Generic terms (past 12 months, "gold watch" = 100)

| Term | Index | Read |
|---|---:|---|
| watch | 7,714 | Includes Apple Watch and other smartwatch searches ("apple watch" tops the related queries for "gold watch"). Not a target. |
| watches | 553 | Broad. Retailers and big brands own it. |
| mens watch | 83 | Large. ACIER is unisex, but men search this way. |
| watches australia | 50 | Shoppers adding the country. Possibly a wish for local sellers (my reading). |
| silver watch | 35 | Matches ACIER Chrome. |
| vintage watch | 29 | Adjacent. CADRE's Roman and Arabic numerals fit loosely. |
| small watch | 27 | Matches CADRE's 32 mm case. |
| watches for men | 26 | |
| mens watches | 24 | |
| watches for women | 18 | |
| quartz watch | 16 | |
| stainless steel watch | 14.5 | Matches both Models. |
| tank watch | 13.7 | The Cartier Tank's style name. See the warning in section 4. |
| square watch | 13.6 | Matches CADRE. |
| womens watches | 10.8 | Note "womens watch" (singular) is only 3.4. |
| cheap watches | 7.5 | |
| affordable watches | 1.4 | |
| unisex watch | 1.0 | |
| minimalist watch | 0.5 | Very small. |
| square gold watch | 0.4 | Very small. People type "square watch" or "gold watch". |
| dainty watch | 0.3 | |
| watch under 200 / watches under 200 | 0.1 to 0.2 | Close to zero. |
| minimalist watches | 0.1 | |
| integrated bracelet watch | 0.0 | Collector jargon. Shoppers don't search it. |

**Check over five years.** Re-run over the five years to October 2026, the mid terms land close to the 12-month figures ("gold watch" = 100): "mens watch" 53, "silver watch" 31, **"ladies watch" 31**, "watches for men" 25, "watches for women" 16, "quartz watch" 15, "stainless steel watch" 14, "tank watch" 12, "square watch" 12, "womens watches" 11, "cheap watches" 7, "mens gold watch" 6, "womens gold watch" 4. "Ladies watch" was not in the 12-month run. It draws about twice "watches for women", so Australian shoppers often use "ladies".

**Small terms against a smaller anchor** (five years, **"square watch" = 100**). So 2.0 means 2% of the searches for "square watch", which is itself about a ninth of "gold watch".

| Term | Index |
|---|---:|
| affordable watches | 5.3 |
| small gold watch | 4.9 |
| unisex watch | 3.7 |
| square gold watch | 2.0 |
| gold square watch | 2.0 |
| minimalist watch | 1.8 |
| dainty watch | 1.1 |
| minimalist watches | 0.7 |
| 37mm watch | 0.3 |
| integrated bracelet watch | 0.2 |
| watches under 200 | 0.1 |
| silver square watch | 0.0 |

**Related queries** (past 12 months; Trends' own top searches that include the term, scaled to the top one = 100):

- **"square watch":** square gold watch 100, apple watch 95, square watches 75, square face watch 69, seiko square watch 61, square watch men 50, casio square watch 50, mens square watch 45, cartier square watch 35, tissot square watch 21. So "square gold watch" is the most common way people narrow "square watch", even though most searches stop at "square watch".
- **"gold watch":** apple watch 100, rose gold watch 83, gold price 78, women gold watch 56, gold watches 55, gold watch band 46, gold mens watch 41, vintage gold watch 32, seiko, casio, rolex and cartier gold watch about 31 to 33 each, gold bracelet watch 27.
- **"silver watch":** gold watch 100, mens silver watch 54, silver watches 54, silver watch women 53, silver watch band 50, silver watch men 42, seiko silver watch 38, vintage silver watch 29.
- **"minimalist watch":** only one related query has enough data: minimalist watch women.

Many related queries name a big brand (Seiko, Casio, Rolex, Cartier, Tissot). A small brand can't win those, and must not borrow them (section 4).

### Brand terms (past 12 months, "gold watch" = 100)

| Term | Index |
|---|---:|
| seiko | 232 |
| casio | 200 |
| fossil watch | 17 |
| tissot prx | 16 |
| cartier tank | 15 |
| daniel wellington | 12.3 |
| michael kors watch | 4.6 |
| skagen | 3.9 |
| mvmt | 1.6 |
| olivia burton | 0.1 |
| aark | 0.1 |
| nordgreen | 0.0 |

Over five years, against "square watch" = 100: skagen 55, mvmt 31, olivia burton 6, nordgreen 0.6, aark 0.4. So Nordgreen and AARK draw well under 1% of the searches "square watch" gets.

"tissot prx" is worth noting: the PRX is the best-known affordable integrated-bracelet watch, and its name draws more searches than "integrated bracelet watch" ever does. FOSLN can't use that name (section 4).

### What competitors rank for

I could not get ranking data for Australia. Semrush's public overview of danielwellington.com shows only South Africa's top five keywords, all branded ("daniel wellington", "dw watch", "daniel wellington watches", "daniel wellington south africa") plus "watch" ([Semrush: danielwellington.com](https://www.semrush.com/website/danielwellington.com/overview/), updated 17 September 2026). Its public pages for aarkcollective.com and nordgreen.com returned 404. The Ahrefs and Similarweb connectors in this workspace need signing in first.

What the evidence does show:

- **Brand demand is small in Australia.** Daniel Wellington, the biggest of the three, is an eighth to a sixth of "gold watch". AARK and Nordgreen barely register. So these brands get most Australian traffic from generic terms or from paid and social channels, not from people searching their name. This is my inference from the Trends figures.
- **What they target, read from their own page titles** (section 2): colour plus "watch" ("Gold Watches", "Silver watch with Black dial"), gender ("Mens Watches", "Women's Watches"), shape ("Square Watch Styles", "Rectangular watches") and price ("Affordable Watches For Men"). Daniel Wellington runs an Australian site (au.danielwellington.com) with faceted collection pages for these, such as `/collections/watches/shape-rectangular` and `/collections/mens-watches/price-affordable`.

### What this means for FOSLN

- **Lead with colour, shape and case size.** "Square gold watch" is small as typed (about 2% of "square watch"), though it is the top refinement of "square watch". Its parts ("gold watch", "square watch", "small watch") are among the largest style terms. The same goes for "silver watch" and "stainless steel watch" for ACIER.
- **Don't build pages around "minimalist" or "integrated bracelet".** They describe the look well but almost nobody in Australia searches them.
- **Price-band terms are close to zero.** "Under $200" belongs in copy, not in titles.
- **Gender words carry real volume.** Both Models are unisex in the feed, but "mens watch" and "ladies watch" are among the largest terms. In Australia "ladies" beats "women's". Whether to write gendered collection pages is a merchandising call for another ticket.

---

## 2. How comparable DTC watch stores write titles and descriptions

Fetched from the live pages on 13 October 2026, as served to a US-based fetcher. Nordgreen's `/en-au/` URLs redirected to `/en-us/` from there, so its Australian titles may differ.

| Store | Page | `<title>` | Meta description (start) |
|---|---|---|---|
| Daniel Wellington (AU) | Home | Daniel Wellington - Watches & Jewelry Online Store \| DW | Daniel Wellington - official online store. Buy silver & gold watches for men and women… |
| Daniel Wellington (AU) | Product | Iconic Link - Silver watch with Black dial 36mm \| DW | Classic silver watch for men and women in a modern look. Stylish black dial and watch strap in steel… |
| Daniel Wellington | Product (square) | Quadro Coral - Rectangular Women's Watch Pink \| DW | Women's square watch with a nato band in light pink and a pink pearl dial… |
| Daniel Wellington (AU) | Collection | Gold Watches - Polished, Classic & Dress Styles \| DW | Shop Gold Watches at Daniel Wellington with clean lines, refined details and timeless styling… |
| Daniel Wellington (AU) | Collection | Quadro Collection - Square Watch Styles \| DW | Explore Quadro Collection at Daniel Wellington with refined details, clean design… |
| Daniel Wellington (AU) | Facet | Affordable Watches For Men \| DW | Discover our selection of affordable watches for men… |
| Daniel Wellington (AU) | Facet | Rectangular watches \| DW | Discover our selection of rectangular watches… |
| AARK Collective | Home | AARK Collective \| Unique Designs for Timeless Moments | Discover a world of unique designs at AARK Collective. Explore our collection of watches, interchangeable straps… |
| AARK Collective | Product | Era - AARK Collective | The Era revisits mid-century elegance… A petite 28mm elliptical case… |
| AARK Collective | Product | Bare Essentials Watch - Prism Champagne \| AARK Collective - Minimalistic Timepieces | The Prism series explores the idea of layering… |
| Nordgreen | Home | Nordgreen: Scandinavian Watches Designed in Denmark 🇩🇰 | Scandinavian Watches designed in Copenhagen, Denmark. Minimalist watches for men and women… |
| Nordgreen | Product | Nordgreen Succession \| 41mm Blue Dial Silver Link | A bold and elegant 41mm timepiece with a blue dial. Features Mineral Glass with Sapphire Coating, reliable Miyota 2115-3H movement, and 10 ATM… |
| Nordgreen | Product | Unika 32mm - Gold - White Dial – Nordgreen | Unika, designed by Jakob Wagner, is an elegant jewellery watch… |
| Nordgreen | Collection | All Watches \| The Complete 364-Piece Minimalist Collection – Nordgreen | Explore 8 collections across 5 sizes (28mm-42mm)… |
| Cluse | Product | CLUSE Quadrus Multifunction Watch CW26209 Blue/Silver – Official CLUSE Store | Order your CLUSE Quadrus Multifunction watch in blue and silver for men… ✓ Free shipping ✓ 2 Year guarantee |
| MVMT | Product | Chrono Monochrome Chrono Men's Watch Collection \| MVMT | Buy the Chrono Monochrome Chrono Men's Watch Collection and more watches… |
| Abbott Lyon (AU) | Collection | Women's Watches \| Ladies Watches \| Abbott Lyon | Explore women's watches made to style your wrist every day. From gold and silver designs… |
| Tissot (AU) | Home | Official Website \| Tissot® Australia | Discover all Tissot® Straps and Swiss Made Watches for Men and Women… |

**Patterns:**

- **Product titles follow "Model - descriptor | Brand".** The descriptor is colour, dial and case size: "Silver watch with Black dial 36mm", "41mm Blue Dial Silver Link", "32mm - Gold - White Dial". The brand goes last, short ("DW"), after a pipe or dash. That matches Google's advice to put the site name at the start or end with a delimiter ([Google: Title links](https://developers.google.com/search/docs/appearance/title-link)).
- **The weakest titles are bare model names** ("Era - AARK Collective"). A shopper's query ("gold watch") doesn't appear in them.
- **Collection titles name the search term first**: "Gold Watches", "Mens Watches", "Square Watch Styles", "Rectangular watches".
- **Descriptions are one or two sentences** about colour, case size and audience ("for men and women"), sometimes with specs (movement, crystal, water resistance). Cluse adds trust ticks ("✓ Free shipping ✓ 2 Year guarantee").
- **Length.** Google sets no limit on titles or meta descriptions and truncates them to the device width ([title links](https://developers.google.com/search/docs/appearance/title-link); [snippets](https://developers.google.com/search/docs/appearance/snippet)). Shopify suggests titles of 60 characters or fewer, accepts up to 70, and suggests about 160 characters for descriptions ([Shopify Help: Adding keywords](https://help.shopify.com/en/manual/promoting-marketing/seo/adding-keywords)). If the title tag is left blank, the theme adds the shop name to the content's title (same source).
- **Google rewrites titles** from the `<h1>`, `og:title`, prominent text and anchor text when the title element is poor or boilerplate ([title links](https://developers.google.com/search/docs/appearance/title-link)). Google builds most snippets from page content and uses the meta description only when it describes the page better (snippet page, updated 20 April 2026).

**Applied to FOSLN** (a sketch, not final copy; the CADRE descriptor depends on the gold frame finish, which is still TBC in [models.md](../specs/models.md)):

- ACIER page: `ACIER - 37mm Silver Steel Bracelet Watch | FOSLN` (48 characters)
- CADRE page: `CADRE - Square Gold-Tone Watch, 32mm with Date | FOSLN` (54 characters). "Gold-tone" keeps the searched word "gold" without claiming solid gold.
- Meta description: Dial colours, case size, "stainless steel bracelet", "hardened mineral crystal", 3 bar, and the delivery promise once #25 sets it. Avoid "stainless steel case" until the Supplier confirms it ([models.md](../specs/models.md)).

---

## 3. Merchant Center: what the free listings feed needs

### 3.1 Currency: a USD feed for Australia

**What Google allows**

- **Currency conversion is on by default.** It converts the prices in the product data into the target country's currency and shows both prices in ads and free listings. It is "automatically enabled in all Merchant Center accounts". The conversion is an estimate from Google Finance rates. The site keeps charging in its own currency, and shoppers should see that price clearly through checkout ([Merchant Center: About currency conversion](https://support.google.com/merchants/answer/7055540?hl=en)).
- **Google's supported-currencies page names conversion as the route.** It lists Australia with AUD, and says: "If you don't want to convert your prices into the local currency for a country, you can use the currency conversion feature" ([Supported languages and currencies](https://support.google.com/merchants/answer/160637?hl=en)).
- **The price attribute says the same.** "Submit an amount and currency that match the price on your landing page and the checkout pages." With conversion, "the prices on your landing page, checkout pages, and in your product data must still match", and the converted price doesn't need to appear on the site ([price](https://support.google.com/merchants/answer/6324371?hl=en)).
- **Conflicting line.** The same page also says "Show the price in the currency of your target country prominently on your landing page and checkout pages." It doesn't reconcile this with conversion. FOSLN's planned "≈ A$" estimate next to the USD price ([ADR 0005](../adr/0005-usd-store-currency-on-basic.md)) is the closest the site can come to both.
- **Tax rules follow the currency, not the target country.** If the currency isn't local to the target country, "you must follow the price and tax requirements for the country or region that your currency is local to" ([price](https://support.google.com/merchants/answer/6324371?hl=en); [currency conversion](https://support.google.com/merchants/answer/7055540?hl=en)). For USD, that means the US rule: "Don't include any taxes". For Australia in AUD, the rule is to include GST. Before FOSLN registers for GST this doesn't matter: there is no tax, and the feed price is the shelf price. After registration, ADR 0001 has the USD price include GST, and the feed must still match the page. I found no Google page on that case. See "Not verified".

**What Shopify's channel says**

- **Store requirement:** "You need to ship to one of the supported countries, and you need to sell in a currency that matches the country" ([Shopify Help: Google & YouTube requirements](https://help.shopify.com/en/manual/online-sales-channels/google/requirements)). No exception for currency conversion is given. The same page says that in Google ads, "Google Merchant Center automatically converts the product price to a customer's local currency."
- **Which countries sync:** the channel lets the merchant "Select the countries and languages that you want to sync from Shopify Markets to your Google Merchant Center" ([Shopify Help: Connect](https://help.shopify.com/en/manual/online-sales-channels/marketplaces/google/getting-setup/connect)). So the feed should take the Australia Market's prices, in USD.
- **Channel markets** inherit the parent market's catalogue and currency. A currency that isn't "preferred" for the channel shows a warning ([Shopify Help: Channel markets](https://help.shopify.com/en/manual/online-sales-channels/channel-markets)). The page doesn't list the preferred currencies.

**Practitioner reports (Shopify Community, not official)**

- An Australian store that switched its currency from AUD to USD in 2022 had every product disapproved with "This product is ready to serve in some but not all its target countries" and "Missing value [shipping]". The thread has no confirmed fix ([thread](https://community.shopify.com/t/changed-store-from-aud-to-usd-and-products-all-disapproved-in-google-ads/137921)).
- An AUD store's US feed was rejected for a price mismatch because the product page's structured data said AUD while the feed said USD. Appending `?currency=USD` to the feed links fixed it (2024, [thread](https://community.shopify.com/t/google-merchant-store-multi-currency-structured-data-markup-mismatch/347203)). FOSLN avoids this: its page, markup and checkout are all USD.

**What to do**

1. Before launch, connect the Google & YouTube channel and check whether it offers Australia with the USD price, and whether a currency warning appears.
2. If it does, keep the markup, page and feed all in USD. Make sure the "≈ A$" estimate is plain text outside the price element and outside the JSON-LD, and less prominent than the USD price. Google matches a multi-currency page against its most prominent price (section 4).
3. If the channel refuses, there are two fallbacks. One is Merchant Center's own website crawl, which builds products from the page's structured data ([Add products automatically](https://support.google.com/merchants/answer/12158480), cited in the #12 note). The other is a third-party feed app. Either way, Google's conversion applies.

### 3.2 Shipping and returns settings

- **Shipping costs are required for Australia** for Shopping ads and free listings ([Product data specification](https://support.google.com/merchants/answer/7052112?hl=en), shipping section; [Free listings for products](https://support.google.com/merchants/answer/13889434?hl=en)). Products without them are disapproved ([Shopify: requirements](https://help.shopify.com/en/manual/online-sales-channels/google/requirements)).
- **Currency:** set the shipping rate in the currency that matches the product price. Google converts it for display ([Set up shipping settings](https://support.google.com/merchants/answer/6069284?hl=en)). For FOSLN that is USD. A free-shipping threshold must also be in the offer's currency ([spec](https://support.google.com/merchants/answer/7052112?hl=en)).
- **Delivery times:** optional, but Google shows delivery estimates only when both handling time and transit time are set. Australia supports shipping speed by region ([Set up shipping settings](https://support.google.com/merchants/answer/6069284?hl=en)). The #12 note found that test orders support a handling time of 0 to 1 days. Nothing yet supports a transit time.
- **From Shopify:** the channel can import shipping settings automatically. "Only rates from your General shipping profile can sync", and there must be a shipping zone for the target country. Carrier-calculated rates are supported only for some carriers in Australia, Germany and the US. Otherwise use flat rates ([Shopify: Connect](https://help.shopify.com/en/manual/online-sales-channels/marketplaces/google/getting-setup/connect)).
- **Return policies are optional** in Merchant Center, but Google recommends them for free listings. Set one up with: the countries, a link to the policy, whether non-defective items are accepted, the condition (new only), the window in days, the method (by mail), the label, the restocking fee and the refund time. Then "Confirm that the return policies are consistent in Merchant Center and across your website" ([Return policies](https://support.google.com/merchants/answer/14011730?hl=en)). Shopify users add it in Merchant Center by hand ([#12 note](https://github.com/Kintukka2/FOSLN-WATCH/blob/research/structured-data/docs/research/structured-data.md), section 4).
- **For FOSLN:** a Return is change of mind for an unworn watch that hasn't been altered (see the glossary). In Merchant Center that maps to "new" condition and non-defective items accepted. Faults are fault claims, outside the return policy. The window and who pays return postage belong to #26.

### 3.3 Product title and description rules

From the title and description attribute pages ([title](https://support.google.com/merchants/answer/6324415?hl=en); [description](https://support.google.com/merchants/answer/6324468?hl=en); [spec](https://support.google.com/merchants/answer/7052112?hl=en)):

- **Title:** 1 to 150 characters. "Users will usually notice only the first 70 or fewer characters". It must describe the linked product and match the landing page title.
- **Not allowed in titles:** promotional text, or "price, sale price, sale dates, shipping, delivery date"; capitals for emphasis; "gimmicky foreign characters".
- **Variants:** "Add the distinguishing details of each variant to the title", and also submit them as attributes (`color`). Add the brand if it differentiates. Google's examples: "Sleeveless Polka Dot Skater Dress, Small, Black"; "Google Pixel 3 64 GB - Midnight Blue - unlocked".
- **Colour names:** if the Dial name isn't a standard colour, keep the landing-page name in `color` and put a standard colour word in the title ([color](https://support.google.com/merchants/answer/6324487?hl=en)).
- **Description:** up to 5,000 characters, with the important details in the first 160 to 500. Describe only the product: no links, no comparisons ("even better than X"), no business history or policies, no capitals for emphasis.

A feed title per Dial could follow this pattern (sketch only):

- `FOSLN ACIER 37mm Stainless Steel Bracelet Watch, Chrome Silver Dial`
- `FOSLN ACIER 37mm Stainless Steel Bracelet Watch, Onyx Black Dial`
- `FOSLN CADRE Square Gold-Tone Watch with Date, Noir Black Dial`
- `FOSLN CADRE Square Gold-Tone Watch with Date, Champagne Dial`

Each fits in 70 characters and puts a searched word ("silver", "black", "square", "gold") inside the visible part. The Shopify channel builds the feed title from the product and variant titles by default. Whether it can take a separate feed title is not verified.

### 3.4 The apparel attributes

**Watches are apparel.** Google's taxonomy has `201 - Apparel & Accessories > Jewelry > Watches` ([taxonomy, version 2021-09-21](https://www.google.com/basepages/producttype/taxonomy-with-ids.en-US.txt)). The gender page's list of exempt subcategories names watch bands, watch winders and watch stickers, not watches ([gender](https://support.google.com/merchants/answer/6324479?hl=en)).

**Australia isn't on the Shopping ads list, but free listings need these everywhere.** For Shopping ads, `color`, `gender`, `age_group`, `size` and `item_group_id` are required only for Brazil, France, Germany, Japan, the UK and the US. Free listings are stricter: `color`, `gender` and `age_group` are "Required for free listings for all Apparel & Accessories (ID: 166) products", and `item_group_id` is "Required for free listings for all product variants" ([spec](https://support.google.com/merchants/answer/7052112?hl=en)). The free listings page lists "color, size, age group, and gender" for Apparel & Accessories ([Free listings for products](https://support.google.com/merchants/answer/13889434?hl=en)). But the spec limits `size` to Clothing (1604), Shoes, and products sold in different sizes, so a watch doesn't need it.

| Attribute | Value for a FOSLN Dial | Rule |
|---|---|---|
| `google_product_category` | 201 | Recommended. Google assigns one if missing, "but you might encounter errors" ([Shopify](https://help.shopify.com/en/manual/online-sales-channels/google/requirements)) |
| `color` | `Chrome`, `Onyx`, `Noir`, `Champagne` as on the page, or a slash list such as `Silver/Black` | Match the landing page. Up to 3 colours, primary first, separated by `/`. No commas ([color](https://support.google.com/merchants/answer/6324487?hl=en)) |
| `gender` | `unisex` | Values `male`, `female`, `unisex`, in English ([gender](https://support.google.com/merchants/answer/6324479?hl=en)) |
| `age_group` | `adult` | Required for free listings in 166 ([spec](https://support.google.com/merchants/answer/7052112?hl=en)) |
| `item_group_id` | One ID per Model, shared by its Dials | Unique per group of variants ([spec](https://support.google.com/merchants/answer/7052112?hl=en)) |
| `size` | Leave out | Clothing and shoes only |
| `brand` | FOSLN | Private label uses the store's brand ([brand](https://support.google.com/merchants/answer/6324351?hl=en)) |

Google's colour page has no rule on which part of a watch sets the colour. For FOSLN, the bracelet is steel on every Dial and the Dial is what varies. So the Dial colour (or case/Dial, such as `Gold/Black` for CADRE Noir) is the field that tells the variants apart. That choice is mine, not Google's.

Shopify keeps these values as Google & YouTube channel fields, not in the product details page. Some data "isn't available from a product's details and needs to be added from the Google & YouTube channel" ([Shopify: Syncing products](https://help.shopify.com/en/manual/online-sales-channels/marketplaces/google/getting-setup/syncing-products)). Community posts say they are stored as metafields (namespace reported as `mm-google-shopping`). That is not confirmed by Shopify.

### 3.5 GTIN or MPN for a private-label brand

**What Google says:**

- **GTIN:** "Private-label products are products that are produced by one company, but relabeled by the company selling it." "If you're the only seller of a product or if your product is a store brand, it may not have a GTIN." "Instead, submit the brand and MPN attributes for these products." "When in doubt don't provide a GTIN." But "if the product does have a GTIN assigned and you don't submit it, then the product could be disapproved" ([gtin](https://support.google.com/merchants/answer/6324461?hl=en)).
- **Brand:** "Use your store name for the brand attribute if you manufacture the product or if your product is a private-label product", including "products manufactured by someone else and rebranded by you" ([brand](https://support.google.com/merchants/answer/6324351?hl=en)).
- **MPN for private label:** "Include a unique identifier number of your choice (for example, your product SKU) in the MPN field" ([About unique product identifiers](https://support.google.com/merchants/answer/160161?hl=en)). The brand page also says to include an MPN "with a unique identifier number of your choice instead of submitting the identifier exists attribute with a value of false" ([brand](https://support.google.com/merchants/answer/6324351?hl=en)).
- **The conflict:** the MPN page says "Unless you're the manufacturer, don't use a value that you've created", and the spec says "Only submit MPNs assigned by a manufacturer" ([mpn](https://support.google.com/merchants/answer/6324482?hl=en); [spec](https://support.google.com/merchants/answer/7052112?hl=en)). Yet the MPN page's own "Store brand and private-label products" example gives a store-brand couch with brand "MyStore" and an MPN. The two pages that deal directly with private label both say a value of the seller's choice is fine. I read the "manufacturer only" lines as aimed at resellers of other brands.
- **`identifier_exists`:** "Recommended for products that don't have a GTIN, MPN, or brand". Set it to `no` only "if you're certain that your product doesn't have any assigned unique product identifiers". The examples are custom, handmade, vintage and pre-GTIN goods ([identifier_exists](https://support.google.com/merchants/answer/6324478?hl=en)).
- **Effect:** "Products submitted without any unique product identifiers are difficult to classify" ([gtin](https://support.google.com/merchants/answer/6324461?hl=en)). Strong identifiers (GTIN, or MPN plus brand) help products join the Shopping knowledge panel, which is shown in Australia ([Free listings for products](https://support.google.com/merchants/answer/13889434?hl=en)).

**On Shopify:** a barcode in the product's Inventory section becomes the GTIN. For products without one, "add the brand name and Manufacturer Part Number" in the Google & YouTube channel. That clears the "Missing GTIN or MPN" error ([Shopify: Syncing products](https://help.shopify.com/en/manual/online-sales-channels/marketplaces/google/getting-setup/syncing-products); [requirements](https://help.shopify.com/en/manual/online-sales-channels/google/requirements)).

**Recommendation:**

1. Ask the Supplier whether the watches carry a barcode. A GTIN registered to the Supplier's own brand would describe the Supplier's product, not FOSLN's, so it shouldn't go in the feed either way. That is my reading of the private-label wording.
2. Leave the Shopify barcode field empty.
3. Set brand FOSLN and an MPN per Dial equal to the SKU, such as `FOSLN-ACIER-CHR`. The SKU is already the variant's unique ID in the markup (#12).
4. Don't set `identifier_exists` to `no`.
5. Optional, later: buying GS1 barcodes would give FOSLN real GTINs. I did not check GS1 Australia's prices.

---

## 4. Free listings: why a new store's products get disapproved

New accounts are opted in to free listings by default ([Free listings for products](https://support.google.com/merchants/answer/13889434?hl=en)). Google reviews new products within 3 to 5 business days of the first sync ([Shopify: Syncing products](https://help.shopify.com/en/manual/online-sales-channels/marketplaces/google/getting-setup/syncing-products)).

### Account-level (the whole store is held back)

- **The three common causes**, per Google: a missing return and refund policy, insufficient contact information, and misrepresentation of self or products ([Fixing warnings and suspensions](https://support.google.com/merchants/answer/13693195?hl=en)).
- **Contact details:** show at least an address, phone, email, contact form or social profile, "easily visible prior to checkout". Verify a phone number and business address in Merchant Center. Make the business name, address and phone "identical" across the footer, contact page and Merchant Center. Give "specific timeframes for delivery and procedures for returns" ([Missing contact information](https://support.google.com/merchants/answer/12472091)). This ties in with [the PO box and warranty-address note](https://github.com/Kintukka2/FOSLN-WATCH/blob/research/po-box-warranty-address/docs/research/po-box-warranty-address.md).
- **Shopify's own preconditions:** the store can't be in password mode, needs a payment provider, a refund policy and terms of service in the footer, visible contact details, and a verified domain no other account has claimed ([Shopify: requirements](https://help.shopify.com/en/manual/online-sales-channels/google/requirements)). A store still behind its password page can't pass.
- **Misrepresentation examples that fit a dropshipped store** ([Misrepresentation](https://support.google.com/merchants/answer/6150127?hl=en)):
  - "Claiming a delivery time in conspicuous text while hiding a longer order processing" delay
  - "Intentionally hiding the actual location from which your products are shipped", and "Misrepresenting or concealing the location from where products are shipped"
  - "Listing items as available when you lack the capability to fulfill the orders"
  - "Not disclosing all applicable shipping costs"
  - "Deceptively advertising a product at an enticing, often unrealistic, low price". This is relevant to any was price ([ADR 0002](../adr/0002-no-sale-at-launch.md) rules out a sale at launch).
  - "Denying return and refund for a product despite having a clear refund or return policy"
  - Google added examples about non-delivery and broken return processes in October 2025, as a clarification, not a change ([Clarifying Misrepresentation](https://support.google.com/merchants/answer/16608300?hl=en)).

  For FOSLN: say where the Supplier dispatches from, and give delivery times that cover the Supplier's handling plus transit.
- **Enforcement:** a warning email with a deadline comes first. Egregious cases are suspended without warning. A re-review takes 3 to 7 business days, and each failed re-review can lengthen the cool-down before the next ([Fixing warnings and suspensions](https://support.google.com/merchants/answer/13693195?hl=en)).

### Product-level (single products are disapproved)

- **Missing shipping cost** for Australia (section 3.2).
- **Price mismatch.** Googlebot compares the feed price with the visible price or the structured data, and "the product with the mismatch may be disapproved". "If data on your website is passed dynamically with JavaScript after the page is loaded, this will trigger an error." If a page shows several currencies, the feed's price and currency must match the one most prominently displayed ([Mismatched product price](https://support.google.com/merchants/answer/12159029?hl=en)). So keep the USD price in the server-rendered HTML and JSON-LD, and keep the "≈ A$" estimate smaller and clearly secondary. The same page also says to "use the appropriate currency for the country that you're targeting", which is the same unresolved tension as in section 3.1.
- **Missing identifiers:** "Missing GTIN or MPN" (section 3.5). Incorrect identifiers can also cause disapproval ([About unique product identifiers](https://support.google.com/merchants/answer/160161?hl=en)).
- **Missing apparel attributes:** `color`, `gender`, `age_group` and `item_group_id` (section 3.4). Variant option names must be in English to sync ([Shopify: requirements](https://help.shopify.com/en/manual/online-sales-channels/google/requirements)).
- **Image problems** ([image_link](https://support.google.com/merchants/answer/6324350?hl=en)):
  - Images must be at least 500 × 500 pixels. Google has announced this for all products from 31 January 2027, and recommends 1500 × 1500 or more.
  - No promotional overlays, prices, calls to action, watermarks, borders, or logos added in post-production. A logo is fine only when it's on the product itself, such as the FOSLN dial print.
  - No placeholders or mockups. The main image must show the exact item.
  - Formats allowed include JPEG, PNG and WebP, up to 16 MB.
- **Counterfeit risk from design references.** ACIER's faceted case on an integrated bracelet and CADRE's square case recall well-known models. Google bans terms such as "knock off, replica, imitation, clone, faux, fake, mirror image" next to a brand name. It also bans non-genuine products that imitate a brand's features to pass as the real thing. Egregious cases are suspended without warning and may be barred from Shopping ([Counterfeit products](https://support.google.com/merchants/answer/6149993?hl=en)). FOSLN's dials carry its own brand, so the products themselves look fine. The risk is in the words. Keep "Royal Oak", "PRX", "Tank", "Santos", "homage" and "inspired by" out of titles, descriptions, alt text and ads, even though "tank watch" and "tissot prx" draw searches (section 1).
- **Fixes need a review request.** "Fixing issues doesn't automatically trigger a re-review", and "Review requests can take up to seven days" ([Shopify: product disapprovals and warnings](https://help.shopify.com/en/manual/online-sales-channels/google/getting-setup/product-disapprovals-warnings)). Shopify lists the same three common account issues as Google: missing refund policy, insufficient contact information, and misrepresentation.

---

## Not verified

- **Absolute search volumes.** All demand figures are relative Google Trends indexes. Monthly volumes need Keyword Planner (free with a Google Ads account), Ahrefs or Semrush. The Ahrefs and Similarweb connectors in this workspace need signing in.
- **Competitor rankings in Australia.** I had no Australian ranking data for Daniel Wellington, AARK or Nordgreen. Section 1 infers their targets from their own page titles.
- **Trends precision.** Values below about 1 (on "gold watch" = 100) are near Trends' rounding floor. Treat them as "very small".
- **Nordgreen's Australian titles.** Its `/en-au/` pages redirected to `/en-us/` for a US-based fetcher.
- **Whether Shopify's channel accepts an Australia Market priced in USD.** The requirement says the currency must match the country. Google's conversion says it needn't. I found no Shopify page on this case. Test it during setup.
- **GST in a USD feed.** Google says to follow the tax rules of the currency's home country (US: exclude tax), but the feed price must match the page. I found nothing on a GST-inclusive USD price for Australia. This only matters after GST registration.
- **The "≈ A$" estimate and price checks.** I didn't test whether Googlebot reads the estimate as a price. Keeping it out of the JSON-LD and price element should be enough, but that is my reading.
- **MPN wording.** Google's MPN page says not to invent MPNs, while its private-label guidance says to use one of your choice, such as the SKU. I follow the private-label pages.
- **Where the Shopify channel stores Google fields.** The `mm-google-shopping` metafield namespace comes from community posts. I also didn't confirm whether the channel can send a feed title that differs from the product title.
- **Return policy sync.** As in the #12 note, I found no Shopify page saying the channel syncs return policies.
- **The Supplier's barcodes, ship-from location and transit times.** All unknown.
- **GS1 Australia barcode pricing.** Not checked.

## Sources

**Google Merchant Center Help**

- [Product data specification](https://support.google.com/merchants/answer/7052112?hl=en)
- [About currency conversion](https://support.google.com/merchants/answer/7055540?hl=en)
- [Supported languages and currencies](https://support.google.com/merchants/answer/160637?hl=en)
- [Price [price]](https://support.google.com/merchants/answer/6324371?hl=en)
- [Set up shipping settings for an entire account](https://support.google.com/merchants/answer/6069284?hl=en)
- [Return policies](https://support.google.com/merchants/answer/14011730?hl=en)
- [Title [title]](https://support.google.com/merchants/answer/6324415?hl=en)
- [Description [description]](https://support.google.com/merchants/answer/6324468?hl=en)
- [Color [color]](https://support.google.com/merchants/answer/6324487?hl=en)
- [Gender [gender]](https://support.google.com/merchants/answer/6324479?hl=en)
- [GTIN [gtin]](https://support.google.com/merchants/answer/6324461?hl=en)
- [MPN [mpn]](https://support.google.com/merchants/answer/6324482?hl=en)
- [Brand [brand]](https://support.google.com/merchants/answer/6324351?hl=en)
- [Identifier exists [identifier_exists]](https://support.google.com/merchants/answer/6324478?hl=en)
- [About unique product identifiers](https://support.google.com/merchants/answer/160161?hl=en)
- [Image link [image_link]](https://support.google.com/merchants/answer/6324350?hl=en)
- [Free listings for products](https://support.google.com/merchants/answer/13889434?hl=en)
- [Misrepresentation](https://support.google.com/merchants/answer/6150127?hl=en)
- [Clarifying Misrepresentation policy](https://support.google.com/merchants/answer/16608300?hl=en)
- [Counterfeit products](https://support.google.com/merchants/answer/6149993?hl=en)
- [How to fix: Missing contact information](https://support.google.com/merchants/answer/12472091)
- [Fixing Merchant Center warnings and account suspensions](https://support.google.com/merchants/answer/13693195?hl=en)
- [How to fix: Mismatched product price](https://support.google.com/merchants/answer/12159029?hl=en)
- [Google product taxonomy](https://www.google.com/basepages/producttype/taxonomy-with-ids.en-US.txt)

**Google Search Central**

- [Title links](https://developers.google.com/search/docs/appearance/title-link), updated 10 December 2025
- [Snippets and meta descriptions](https://developers.google.com/search/docs/appearance/snippet), updated 20 April 2026

**Shopify Help Center**

- [Google & YouTube channel requirements](https://help.shopify.com/en/manual/online-sales-channels/google/requirements)
- [Google & YouTube channel overview](https://help.shopify.com/en/manual/online-sales-channels/marketplaces/google)
- [Connect the channel](https://help.shopify.com/en/manual/online-sales-channels/marketplaces/google/getting-setup/connect)
- [Syncing products](https://help.shopify.com/en/manual/online-sales-channels/marketplaces/google/getting-setup/syncing-products)
- [Product disapprovals and warnings](https://help.shopify.com/en/manual/online-sales-channels/google/getting-setup/product-disapprovals-warnings)
- [Channel markets](https://help.shopify.com/en/manual/online-sales-channels/channel-markets)
- [Adding keywords (SEO titles and descriptions)](https://help.shopify.com/en/manual/promoting-marketing/seo/adding-keywords)

**Search demand and competitors**

- Google Trends, Australia, web search, queried 13 October 2026 for the 12 months and 5 years to that date ([trends.google.com](https://trends.google.com/trends/explore?geo=AU))
- [Semrush: danielwellington.com overview](https://www.semrush.com/website/danielwellington.com/overview/), updated 17 September 2026
- Competitor pages fetched 13 October 2026: [au.danielwellington.com](https://au.danielwellington.com/), [aarkcollective.com](https://aarkcollective.com/), [nordgreen.com](https://nordgreen.com/), [cluse.com](https://cluse.com/), [mvmt.com](https://mvmt.com/), [abbottlyon.com](https://www.abbottlyon.com/), [tissotwatches.com/en-au](https://www.tissotwatches.com/en-au/)

**Shopify Community (practitioner reports, not official)**

- [Changed store from AUD to USD and products all disapproved](https://community.shopify.com/t/changed-store-from-aud-to-usd-and-products-all-disapproved-in-google-ads/137921) (July 2022)
- [Multi-currency structured data markup mismatch](https://community.shopify.com/t/google-merchant-store-multi-currency-structured-data-markup-mismatch/347203) (August 2024)
