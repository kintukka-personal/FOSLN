# What selling into AU, UK and CA involves

Research for [#10](https://github.com/Kintukka2/FOSLN-WATCH/issues/10), done on 9 October 2026. It feeds the launch-Markets decision ([#23](https://github.com/Kintukka2/FOSLN-WATCH/issues/23)). It also touches how prices are presented ([#24](https://github.com/Kintukka2/FOSLN-WATCH/issues/24)), shipping and returns terms ([#25](https://github.com/Kintukka2/FOSLN-WATCH/issues/25)), and how orders reach the Supplier ([#26](https://github.com/Kintukka2/FOSLN-WATCH/issues/26)). This note reports what the sources say. It is not legal or tax advice.

## Short answer

- **UK:** FOSLN must register for UK VAT before its first sale and add 20% VAT at checkout on parcels of £135 or less. There's no customs duty on those parcels today, but the government plans to end that relief by October 2028. Buyers have a legal 14-day right to cancel for any reason.
- **Australia:** FOSLN must register only once Australian sales reach A$75,000 a year (about 540 watches). From then on it adds 10% GST at checkout. Parcels of A$1,000 or less pay no duty, and the law doesn't require change-of-mind returns.
- **Canada:** FOSLN doesn't need to register. Instead the buyer pays 5% duty plus 5–15% sales tax, and usually a courier fee, when the parcel arrives. FOSLN can collect these at checkout only if the Supplier's carrier supports prepaid duties ("DDP").
- **All three:** AU and UK prices must include tax. A struck-through "compare-at" price must have been genuinely charged in that Market first, so a new Market can't open with one. Shopify Markets handles currency, tax display and duty estimates, but it doesn't file tax returns or provide international return labels.

## At a glance

| | Australia | United Kingdom | Canada |
|---|---|---|---|
| A US$97 watch is about | A$139 | £73 | C$138 |
| Tax on a one-watch parcel | 10% GST, added by FOSLN at checkout once registered | 20% VAT, added by FOSLN at checkout | GST/HST 5–15%, plus provincial tax in BC, MB, SK and QC, charged at the border |
| Must FOSLN register? | Only from A$75,000 of Australian sales in 12 months (voluntary before that) | Yes, from the first sale, with no threshold | Not federally, while goods are dispatched from abroad and FOSLN doesn't carry on business in Canada |
| Low-value threshold | A$1,000 | £135 | C$20 |
| Duty on one watch | None (under A$1,000) | None today (under £135). Above £135 the rate is £0.20 per watch | 5% |
| Who pays anything at the door | Nobody, for parcels of A$1,000 or less | Nobody, for £135 or less. Above that, the buyer pays the carrier | The buyer, unless FOSLN prepays (DDP) |
| How prices must be shown | One total price that includes GST | Total price including VAT and any mandatory charges | Tax may be added at checkout. Any other mandatory fee must be in the price |
| Rule for a compare-at ("was") price | Must have been genuinely charged for a reasonable period just before the sale | Must be a genuine, realistic selling price that FOSLN can prove | Must pass a volume test (most sales over 12 months) or a time test (most of the time over 6 months) |
| Legal change-of-mind returns | No | Yes, 14 days | No federal right. Some provinces give cancellation rights |

### Assumptions behind the figures

- **Exchange rates:** US$1 = A$1.4332, £0.7551 and C$1.4209 on 9 October 2026 ([currency-converter.org.uk][fx]). That site is an aggregator, not an official fixing.
- **Orders:** one watch per order, sent by courier or post straight from the Supplier in China. FOSLN holds no stock in any of the three countries.
- **Product classification:** I assume both Models fall under customs code 9102.11, for a quartz wristwatch with hands only and a case that isn't precious metal. This is my reading, not a ruling. CADRE's gold frame matters here: a case "of precious metal or of metal clad with precious metal" belongs to code 9101 instead ([UK Trade Tariff, heading 9101][uk-9101]). My understanding is that gold plating isn't "clad", but the Supplier or a customs broker should confirm the code. Shopify's duty estimates depend on it.

---

## Australia

### Tax: GST on low-value imported goods

- **Who collects it.** A non-resident business selling goods with a customs value of A$1,000 or less to Australian consumers must collect GST from the customer and pay it to the ATO ([ATO, GST on low value imported goods][ato-lvig]). These rules have applied since 1 July 2018 ([Home Affairs Notice 2018/13][abf-2018-13]). Customs value means the sale price minus freight and insurance from the place of export ([ATO][ato-lvig]).
- **Rate.** GST is 10%, so it's 1/11 of a GST-inclusive price. The ATO says "As soon as it's clear that GST applies, you must show the GST-inclusive price" ([ATO, How Australian GST works][ato-how]).
- **When registration is required.** FOSLN must register once its GST turnover from sales connected with Australia reaches **A$75,000**. Registering earlier is voluntary, and shipping charges count toward the threshold ([ATO, How Australian GST works][ato-how]; [ATO, GST on low value imported goods][ato-lvig]). At about A$139 a watch, A$75,000 is roughly 540 orders a year, or fewer if shipping is charged. That figure is my arithmetic.
- **Simplified registration.** It's done online and gives FOSLN an ATO Reference Number (ARN) instead of an Australian Business Number. Returns are lodged online every quarter. It doesn't allow GST credits or tax invoices ([ATO, Simplified GST registration][ato-simple]). It needs no software beyond the ATO's web portal, which suits an owner who works only in GUIs.
- **Paperwork on the parcel.** The ARN must appear "on invoices and customs documentation for low value goods" ([ATO][ato-simple]). In practice, the Supplier's customs paperwork must carry FOSLN's ARN. That bears on #26.
- **Threshold per item.** The A$1,000 limit applies to each item's customs value, not to the whole order ([ATO, LCR 2018/1][ato-lcr]).
- **If GST is charged twice.** Sometimes GST is also charged at the border, for example when the paperwork is missing. The customer then claims that GST back from the vendor, not from the border agency ([ATO, GST and consumers][ato-consumers]; [Home Affairs Notice 2018/13][abf-2018-13]).
- **Not verified:** what happens to GST on a parcel under A$1,000 from a vendor that isn't registered (under A$75,000). My understanding is that the border doesn't collect GST on such parcels, so none is paid. I couldn't confirm that from a primary page.

### Duties

- The ABF says most goods worth A$1,000 or less have no duties, taxes or charges to pay at the border; tobacco and alcohol are the exceptions. Above A$1,000, the importer pays GST, duty and clearance charges at the border ([ABF, Import declarations][abf-decl]; [ATO][ato-lvig]).
- So a one-watch order pays no duty, and the buyer has nothing to pay on delivery. I didn't look up the Australian duty rate on watches, because it doesn't apply at this price.

### Prices

- **Single total price.** A business "must display the total price of a product or service as a single figure", including taxes and unavoidable fees. Mandatory delivery charges must be stated but needn't be in that figure ([ACCC, Price displays][accc-display]; [ACCC, Component pricing][accc-component]). So Australian prices are shown with GST included.
- **Compare-at ("was/now", strikethrough).** The ACCC says "A critical issue is whether relevant consumers would have paid the 'was' or 'strike through' price for a reasonable period before the sale commenced." A product rarely or never sold at the higher price is risky. So is a business that discounts as a habit ([ACCC, Two-price comparison advertising][accc-twoprice]). In a live case the ACCC has alleged that Emma Sleep's strikethrough prices were misleading. Those are allegations, not findings ([ACCC v Emma Sleep, concise statement][accc-emma]).
- **Coming change: drip pricing.** The Competition and Consumer Amendment (Unfair Trading Practices) Bill 2026 adds rules on disclosing transaction-based charges, such as card surcharges, alongside the base price. It starts on **1 July 2027** ([Mallesons, 10 Feb 2026][mallesons]; [HWL Ebsworth, 10 Jul 2026][hwle]). HWL Ebsworth reports that the bill passed both Houses by July 2026. I couldn't open the Parliament page to confirm passage or Royal Assent ([APH bill page][aph], which was blocked).

### Returns

- **Australian Consumer Law applies.** The ACCC says an overseas online business that sells directly to Australian consumers must follow the Australian Consumer Law, including the consumer guarantees ([ACCC, Buying online][accc-online]).
- **No change-of-mind duty.** A business may refuse a refund when the buyer simply changes their mind. It may not display "no refunds", or say refunds stop after a set number of days ([ACCC, Consumer rights and guarantees][accc-rights]).
- **Faulty goods.** A major failure entitles the buyer to a refund or replacement. A minor failure can be fixed by repair. The buyer normally returns the goods, unless doing so would cost them a lot. Once a fault is confirmed, the business repays reasonable return costs ([ACCC, Repair, replace, refund, cancel][accc-rrr]).
- **GST on refunds.** A registered vendor that refunds a returned item adjusts the GST it reported ([ATO, Non-resident businesses making online sales][ato-online]).

---

## United Kingdom

### Tax: VAT on parcels of £135 or less

- **Who collects it.** For goods sent from outside the UK in a consignment of £135 or less, sold directly rather than through a marketplace, "The seller must charge and account for VAT at the point of sale" ([GOV.UK, VAT and overseas goods sold directly to customers, updated 13 May 2022][gov-135]). HMRC's newer guidance says the same ([GOV.UK, Charging VAT on goods sold direct to customers, updated 30 Jan 2026][gov-charging]).
- **How £135 is measured.** It applies to the whole consignment. Transport and insurance shown separately don't count ([GOV.UK][gov-135]).
- **Registration from the first sale.** A business based outside the UK must register "regardless of taxable turnover" ([GOV.UK, Register for VAT][gov-register]). The £90,000 threshold applies only to UK businesses.
- **Rate.** The standard rate is 20% ([GOV.UK, VAT rates][gov-rates]).
- **Records and filing.** Full records, including VAT invoices, must be kept for 6 years ([GOV.UK][gov-charging]). GOV.UK says "All VAT-registered businesses should now be signed up for Making Tax Digital for VAT" ([GOV.UK, Making Tax Digital][gov-mtd]), which means filing returns through compatible software. I didn't confirm how this applies to overseas sellers.
- **Orders over £135.** Normal import rules apply: the buyer pays VAT and any duty to the carrier before delivery ([GOV.UK, Tax and customs for goods sent from abroad][gov-abroad]). In Shopify, an order over £135 assigned to a location outside the UK is "treated as an import, and VAT isn't charged at checkout" ([Shopify, UK tax help][shopify-uk-migrate], read via search extract). A two-watch order (about £146) therefore switches to the import rules, so the buyer pays at the door.

### Duties

- **Today.** Customs duty isn't charged on non-excise goods worth £135 or less ([GOV.UK, Tax and customs for goods sent from abroad][gov-abroad]).
- **Above £135.** The third-country duty on code 9102 11 00 00 is £0.20 per item, with no China-specific measure that I could see ([UK Trade Tariff API, read 9 Oct 2026][uk-api]).
- **Change coming.** The government will remove the £135 duty relief. HMRC's measure, published on 13 July 2026, says it starts on a day the Treasury appoints "by October 2028 at the latest" ([GOV.UK, Reforming the customs treatment of low value imports][gov-lvi-tiin]). The Budget 2025 announcement and the consultation had said March 2029 at the latest ([GOV.UK consultation][gov-lvi-consult]).
- **Fiscal representative.** The measure introduces "a UK-based business that assumes joint and several liability for any debts incurred by an overseas seller" ([GOV.UK][gov-lvi-tiin]).
- **What the consultation response says.** According to a search extract of the July 2026 response, sellers would be liable for the duty and would pay it quarterly ([LVI consultation response][gov-lvi-response]). I couldn't confirm three things: the rate structure, whether overseas sellers must appoint a fiscal representative, and the final start date.

### Prices

- **Total price.** Under the DMCC Act 2024, an invitation to purchase must give "the total price of the product (including any mandatory fees, taxes, charges or other payments…)" and show any optional delivery charges. Drip pricing, where mandatory fees appear later in checkout, is covered too ([CMA207, 18 Nov 2025][cma207]). So UK prices are shown with VAT included.
- **No fixed rule for reference prices.** CMA207 lists "the existence of a specific price advantage" among the things a misleading action can be about ([CMA207][cma207]).
- **CMA open letter.** The CMA's open letter of 29 March 2023 says "The higher price in a price reduction claim must be a genuine and realistic selling price – and a business should be able to demonstrate this." Its example is a product launched at £120, cut to £90 two weeks later, then advertised as "save £30" for months, which is likely misleading ([CMA open letter][cma-letter]).
- **Trading Standards guidance.** The guidance from the Chartered Trading Standards Institute (CTSI) isn't statutory; it was reviewed in April 2026. It says a comparison is more likely to comply if the higher price was charged recently and was the last price charged, and if the comparison runs no longer than the higher price did. A recommended retail price "must not be created purely in order to present the appearance of a discount" ([CTSI, Guidance for Traders on Pricing Practices][ctsi]).
- **Mattress sector rules.** The CMA set rules for online mattress sellers: don't show a "was" price for longer than it was charged, and stop using it once twice as many units have sold at the sale price as at the "was" price. These are sector-specific, but the CMA presents them as a reading of the general law ([CMA, Discount and reference pricing principles][cma-mattress], read via search extract). The CMA took Emma to court over reference pricing ([GOV.UK news][cma-emma]).

### Returns

- **14-day cancellation.** For a distance sale, the cancellation period ends 14 days after the buyer receives the goods ([Consumer Contracts Regulations 2013, reg. 30(3)][ccr30]).
- **Refund deadline.** The refund is due within 14 days of the trader getting the goods back, or of the buyer showing proof of sending, whichever comes first ([reg. 34][ccr34]).
- **Delivery and return costs.** The trader must also refund standard outbound delivery ([GOV.UK, Accepting returns][gov-returns]). The buyer pays to send the goods back, unless the trader agreed to pay or didn't tell them beforehand ([reg. 35(5)][ccr35]).
- **Faulty goods.** A buyer has 30 days to reject faulty goods for a refund ([Consumer Rights Act 2015, s.22][cra22]). After that, the trader must repair or replace a fault reported within 6 months, and claims can be made for up to 6 years ([GOV.UK][gov-returns]).
- **VAT on refunds.** If FOSLN charged VAT and refunds it, it adjusts its VAT return ([GOV.UK, goods returned to the seller, updated 20 June 2025][gov-returned]).
- **Buyer-paid import charges.** For orders over £135, the buyer reclaims import VAT or duty on returned goods using form BOR 286 (Royal Mail or Parcelforce) or C285 (other carriers) ([GOV.UK][gov-abroad]).

---

## Canada

### Tax: GST/HST collected at the border

- **No federal registration.** The CRA's own example matches FOSLN's set-up: a non-resident vendor selling only through its website, holding no goods in Canada, sending a parcel by courier from abroad to an Alberta consumer. The CRA says "The vendor is not required to collect the GST/HST on the supply of the good", and "The GST applies to the importation of the good at a rate of 5%" ([CRA, Supply of qualifying goods in Canada, page dated 2024-10-16][cra-goods]).
- **Simplified regime doesn't apply.** Canada's simplified registration covers digital products and services, goods held in Canadian warehouses, and goods sold through distribution platforms. It doesn't cover goods dispatched from abroad straight to a buyer ([CRA, GST/HST for digital-economy businesses][cra-digital]).
- **Rates (CRA, 1 April 2025)** ([CRA, GST/HST rates][cra-rates]):
  - GST is 5%.
  - HST is 13% in Ontario, 14% in Nova Scotia (cut from 15% on 1 April 2025), and 15% in New Brunswick, Newfoundland and Labrador, and Prince Edward Island.
  - Provincial tax is 7% in BC, 7% in Manitoba, 6% in Saskatchewan, and Quebec's QST is 9.975%.
- **Quebec (QST).** Revenu Québec says a supplier outside Canada that isn't registered for GST/HST, sending goods from outside Quebec by mail or courier, doesn't have to register for QST. A supplier that *is* registered for GST/HST must register for QST once its sales of goods to Quebec consumers pass C$30,000 in 12 months ([Revenu Québec, specified system][rq-specified], read via search extract because the page blocked fetching). So registering for GST/HST voluntarily could also bring in QST.
- **Not researched:** whether BC, Saskatchewan or Manitoba require sellers outside the province to register for their sales taxes.

### Duties

- **C$20 threshold.** Parcels from countries other than the US and Mexico pay no duty or tax up to C$20. Above that, duty and tax are due on the full value ([CBSA, Paying duty and taxes on imported goods, modified 2025-11-25][cbsa-pay]).
- **The higher CUSMA thresholds don't apply.** The C$40 and C$150 thresholds cover only courier parcels from the US or Mexico. Goods that pass through the US without entering its commerce still get C$20 ([CBSA, Increase to low-value shipment thresholds][cbsa-cusma]).
- **No splitting.** Dividing a shipment into several parcels to stay under the thresholds isn't allowed ([CBSA, Memorandum D8-2-16][cbsa-d8216], read via search extract).
- **Duty rate.** Code 9102.11.00.00 carries a 5% MFN rate ([Customs Tariff 2026, Chapter 91, revised 1 Sep 2026][ca-tariff]). My understanding is that goods from China pay the MFN rate. China doesn't appear in that item's list of tariff-free trade partners.
- **Surtaxes.** None of the Chinese-goods surtaxes appear to reach watches:
  - The EV surtax on Chinese goods was repealed on 1 March 2026 ([CBSA, Customs Notice 24-32][cbsa-2432]).
  - The Chinese steel and aluminium list covers chapters 72, 73 and 76 ([Finance Canada, Oct 2024][fin-china]).
  - The steel derivative surtax schedule covers chapters 39, 72, 73, 76, 83 and 94 ([Order in Council P.C. 2025-0917][oic-steel]; [CBSA, Customs Notice 25-33][cbsa-2533]).
  - None includes chapter 91 (watches). I didn't check every later amendment line by line.
- **Who pays.** If the Supplier sends parcels on "DAP" terms, which is the default, the buyer pays duty and tax to the carrier on delivery, plus the courier's brokerage or handling fee. Those fees are "separate and distinct" from duty ([CBSA][cbsa-cusma]; [Shopify, Carriers and shipping terms][shopify-carriers]). A 2024 CBSA memo puts Canada Post's handling fee at C$9.95 per item ([CBSA, Memorandum D5-1-1][cbsa-d511]). I didn't confirm the current amount.
- **Illustration (approximate, my arithmetic).** On a C$138 watch, duty is about C$6.90. I've charged GST/HST on the price plus duty. On that basis the buyer would pay about C$14 in Alberta and about C$26 in Ontario. In the 15% HST provinces it would be about C$29. Brokerage fees come on top, and shipping isn't included.

### Prices

- **Taxes may be added at checkout.** The Competition Act treats a price that can't be reached because of fixed mandatory charges as misleading, unless those charges are imposed by law ([Competition Act s.74.01(1.1), current to 2026-09-21][ca-7401]). In plain terms, GST/HST and provincial tax may be added at checkout, as in the US. A mandatory fee FOSLN sets itself would have to be in the price.
- **Compare-at prices.** A seller that claims a former price must show it either sold "a substantial volume" at that price or higher within a reasonable period, or offered it at that price "in good faith for a substantial period of time". Both are judged "in the relevant geographic market" ([s.74.01(3)][ca-7401]).
- **The Bureau's tests.** The Competition Bureau's tests are "more than 50 % of sales" at or above the reference price over 12 months, or offered at or above it "for more than 50 % of the time" over 6 months ([Competition Bureau, Ordinary Price Claims, 2009, modified 2022-06-23][cb-opc]). The Bureau notes these guidelines are under review. In November 2025 it warned: "the regular price must be a price the item is genuinely sold at, and businesses must be able to prove it" ([Competition Bureau, 25 Nov 2025][cb-bf]).
- **French in Quebec.** The Charter of the French Language applies to businesses that carry on activities in Quebec. It gives consumers the right to be informed and served in French, including a French version of the website ([OQLF, Language of commerce][oqlf]). I couldn't find whether a foreign seller with only a website counts as carrying on activities in Quebec. That question is unresolved.

### Returns

- **No federal change-of-mind right.** Consumer protection is provincial.
- **Ontario as an example.** Its Consumer Protection Act, 2002 is still in force; the 2023 Act awaits a start date:
  - A buyer may cancel an internet agreement within 7 days if the seller didn't follow the disclosure rules.
  - A buyer may cancel if goods aren't delivered within 30 days of the agreed delivery date, or 30 days after the agreement if no date was set.
  - The refund is due within 15 days ([Ontario, Your rights under the Consumer Protection Act][on-cpa]).
  - The 30-day rule matters for delivery times from China.
- **Duty and tax refunds on returns.** A buyer who returns goods can reclaim the duty and tax they paid using CBSA form B2G. They need a credit note from the seller and proof the goods left Canada. Brokerage and shipping fees aren't refunded ([CBSA, Casual refunds][cbsa-refund]). The memo gives a 4-year window, but it dates from 2015 ([CBSA, Memorandum D6-2-6][cbsa-d626]).
- **Not researched:** who claims the refund when FOSLN prepaid the duty (DDP).

---

## What Shopify does and doesn't cover

- **Local currency.** By default, Markets converts prices "automatically using the current market exchange rate". It also offers manual rates, rounding, percentage adjustments, and fixed per-product prices in a catalog assigned to a Market. Only stores on Shopify Payments get every feature ([Shopify, Selling in multiple currencies][shopify-pricing]; [Shopify, Product prices by country][shopify-catalog]).
- **Compare-at prices per Market.** A catalog can hold its own compare-at price for each product. By default, a catalog's percentage adjustment also changes compare-at prices ([Shopify, Product prices by country][shopify-catalog]).
  - My inference from the rules above: converting a US compare-at price into AUD, GBP or CAD doesn't give it a price history in that Market.
  - Prices that float with exchange rates also make a steady "was" price harder to evidence than fixed local prices do.
- **Tax-inclusive prices per Market.** "You set tax inclusion for each market", so UK buyers can see VAT-inclusive prices while US buyers have tax added at checkout. Product pages and the cart reliably show the right price only with Shopify Payments multi-currency ([Shopify, Including or excluding tax][shopify-taxincl]). The store's home tax rate is deducted from tax-inclusive prices when selling into another tax-inclusive region ([same page][shopify-taxincl]). So the store address matters.
- **Tax engines for a store created after 13 May 2026.** The UK and Canada use Shopify Tax or manual rates, and Basic Tax is closed to new stores there. Australia isn't named; it falls under "remaining countries", which use Basic Tax or manual rates ([Shopify, Choosing a tax service][shopify-taxsvc]). Shopify Tax is free until £100,000 or C$100,000 in lifetime sales. After that it costs 0.25% on Basic, Grow and Advanced plans, capped at £0.99 or C$0.99 per order ([Shopify Tax pricing][shopify-taxprice]).
- **Low-value VAT and GST.** Shopify adds UK VAT to orders of £135 or less once a UK VAT registration is entered, and treats Australia's low-value GST the same way. Low-value tax applies only "if you're registered to collect tax in that country". Shopify collects but doesn't file: "Low-value goods taxes are generally remitted to tax authorities by using a tax return" ([Shopify, Understanding customs fees][shopify-customs]).
- **Duties at checkout.**
  - Available on all plans since 2 February 2025 ([Shopify changelog][shopify-changelog]).
  - Needs HS codes and country of origin on products ([Shopify, Collecting duties at checkout][shopify-duties]).
  - Parcels must use "a delivery duty paid (DDP) shipping label" ([Shopify, Considerations][shopify-considerations]). Labels bought through Shopify support DDP only with DHL Express, DHL Express Canada, DHL eCommerce, and Canada Post (US destinations only). Otherwise the label is bought outside Shopify ([Shopify, Carriers and shipping terms][shopify-carriers]).
  - Fee: a temporary 0.5% since 2 February 2025. The standard fee is 0.85% with Shopify Payments, or 1.5% otherwise ([Shopify, Collecting duties at checkout][shopify-duties]).
  - Courier brokerage and handling fees aren't included in the estimate ([Shopify, Considerations][shopify-considerations]).
  - For FOSLN, the Supplier buys the label at Dispatch. So collecting Canadian duty at checkout works only if the Supplier dispatches DDP with a carrier that bills FOSLN. That bears on #26.
- **Managed Markets doesn't fit.** This is the Shopify option where Global-e is merchant of record and handles registrations, duties and tax. It requires a business based in the continental US, or certain Canadian and UK stores, with a fulfilment location in that country ([Shopify, Managed Markets requirements][shopify-mm-req]). It also doesn't support the app or custom fulfilment locations that dropshipping apps use ([Shopify help, via search extract][shopify-mm-ful]). That doesn't match dispatch from China.
- **Refunds in local currency.** Refunds are paid in the buyer's currency at the *current* rate. "You can gain or lose money with refunds because of currency conversions", and payment fees aren't returned ([Shopify, Refunds][shopify-refunds]).
- **Returns.** Return rules can be set per Market (an early-access feature), with free returns, a flat fee, or the customer buying their own label ([Shopify, Return rules][shopify-returnrules]). Shopify's own return labels are "available only for US domestic orders" ([Shopify, Return labels][shopify-returnlabels]). International returns need a carrier or an app.

---

## Returns from abroad, in practice

The law sets the minimum: UK buyers can cancel within 14 days; in all three countries a faulty watch must be put right. How FOSLN meets that is for #25. Three ways it could work:

| Option | What it involves | Trade-offs |
|---|---|---|
| Return to the Supplier in China | The buyer, or FOSLN, pays international postage back | Nothing extra to set up, but slow and costly next to a watch of about US$97. A UK buyer can be asked to pay return postage only if told before buying. I didn't research how China's customs treats returned watches |
| A returns address in each Market | A local returns service receives parcels | Cheaper, faster postage for buyers. It adds a contract, and FOSLN ends up holding returned stock, which conflicts with the Glossary's "FOSLN never holds stock itself" |
| Refund without return | FOSLN refunds and the buyer keeps or disposes of the watch | Simple. Costs the item. Suits faulty units where postage would cost more than the watch |

Tax on a refund is handled differently in each country:
- **AU:** adjust the GST return.
- **UK:** adjust the VAT return. The buyer reclaims any border VAT or duty using BOR 286 or C285.
- **CA:** the buyer reclaims duty and tax using form B2G.

---

## Which way the facts point

These are not decisions. They show which way the evidence leans for the tickets that own each decision.

- **Launch Markets (#23).**
  - **Australia** has the lowest entry cost: no registration until about 540 orders a year, no duty, nothing to pay at the door, and no legal change-of-mind returns.
  - **The UK** needs VAT registration and filing from the first order and a 14-day cancellation right. It also faces duty, and possibly a fiscal representative, by October 2028.
  - **Canada** needs no registration but gives the worst delivery experience unless DDP works: the buyer pays roughly C$14–29 plus a courier fee at the door.
  - So the facts lean toward Australia as the easiest addition, with the UK next if its VAT admin is acceptable. Canada depends on whether the Supplier can dispatch DDP.
- **Price presentation (#24).**
  - Prices must include tax in AU and the UK. At the same headline price, FOSLN keeps less: £12.17 of a £73 VAT-inclusive price is VAT, and A$12.64 of an A$139 price is GST.
  - Canada may add tax at checkout, like the US.
  - No Market can launch with a compare-at price, and fixed local prices make a later "was" price easier to support.
- **Returns terms (#25).** The UK sets the strictest minimum: 14 days to cancel for any reason, plus a refund of standard delivery. Shopify can apply different return rules per Market.
- **The Supplier hand-off (#26).** The Supplier's paperwork needs four things:
  - FOSLN's ARN on Australian parcels.
  - Probably FOSLN's VAT number on UK parcels; I didn't confirm what goes on UK paperwork.
  - The right HS code and value on every parcel.
  - For Canada DDP, a carrier that supports DDP and bills FOSLN.

## What I couldn't verify

- **Blocked pages.** The ATO, ABF, Parliament of Australia, Revenu Québec and UK Trade Tariff web pages blocked automated reading. For those I relied on search-engine extracts, or on the UK Trade Tariff API. The wording quoted from them comes from those extracts.
- Whether GST is collected at the Australian border on parcels under A$1,000 from a vendor that isn't registered.
- Whether Australia's Unfair Trading Practices bill has received Royal Assent, and its final wording on drip pricing.
- The UK low-value duty scheme: the rate structure, whether an overseas seller must appoint a fiscal representative, and the exact start date.
- What UK carriers need on the parcel to show VAT was paid at checkout.
- Who acts as importer, and who can reclaim duty, on DDP parcels into Canada and the UK.
- Whether the Quebec French-language rules reach a foreign seller with only a website. Also BC, Saskatchewan and Manitoba registration rules for out-of-province sellers.
- Current Canada Post and courier handling fees.
- The correct customs code for CADRE's gold frame.
- China's customs treatment of returned watches.
- When Ontario's Consumer Protection Act, 2023 takes effect.
- The owner's business location. It affects Shopify Payments, Managed Markets eligibility, and the home tax rate Shopify deducts from tax-inclusive prices.
- **Product rules (outside this ticket's scope, not researched).** For example, UK rules for sellers of electrical equipment and batteries; a quartz watch contains a button cell.

Terms used here that aren't in `GLOSSARY.md`: *compare-at price*, *DDP/DAP*, *low-value threshold*. They may be worth a glossary entry if later tickets keep using them.

---

## Sources

All pages were read on 9 October 2026 unless noted. The date after each source is the page's own date where it showed one.

**Australia**
- ATO, *GST on low value imported goods* (read via search extract). https://www.ato.gov.au/businesses-and-organisations/international-tax-for-business/gst-for-non-resident-businesses/gst-on-low-value-imported-goods
- ATO, *How Australian GST works* (read via search extract). https://www.ato.gov.au/businesses-and-organisations/international-tax-for-business/gst-for-non-resident-businesses/how-australian-gst-works
- ATO, *Simplified GST registration* (read via search extract). https://ato.gov.au/businesses-and-organisations/international-tax-for-business/non-resident-businesses-and-gst/simplified-gst-registration
- ATO, *LCR 2018/1 GST on low value imported goods* (read via search extract). https://www.ato.gov.au/law/view/document?DocID=COG%2FLCR20181%2FNAT%2FATO%2F00001
- ATO, *GST and consumers* (read via search extract). https://www.ato.gov.au/businesses-and-organisations/gst-excise-and-indirect-taxes/gst/in-detail/rules-for-specific-transactions/international-transactions/australian-consumers-importing-goods-and-services
- ATO, *Non-resident businesses making online sales to Australia* (read via search extract). https://www.ato.gov.au/businesses-and-organisations/international-tax-for-business/gst-for-non-resident-businesses/non-resident-businesses-making-online-sales-to-australia
- ABF, *Import declarations* (read via search extract). https://www.abf.gov.au/imports/Pages/How-to-import/Import-declarations.aspx
- Department of Home Affairs, *Notice 2018/13: GST on low value goods* (2018, read via search extract). https://www.abf.gov.au/help-and-support-subsite/customsnotices/2018-13.pdf
- ACCC, *Price displays*. https://www.accc.gov.au/consumers/pricing/price-displays
- ACCC, *Advertising and selling guide: Component pricing*. https://www.accc.gov.au/publications/advertising-selling/advertising-and-selling-guide/pricing/component-pricing
- ACCC, *Advertising and selling guide: Two-price comparison advertising*. https://www.accc.gov.au/publications/advertising-selling/advertising-and-selling-guide/pricing/two-price-comparison-advertising
- ACCC, *ACCC v Emma Sleep, concise statement* (allegations). https://www.accc.gov.au/system/files/ACCC%20v%20Emma%20Sleep%20_%20Concise%20Statement.pdf
- ACCC, *Buying online*. https://www.accc.gov.au/consumers/buying-products-and-services/buying-online
- ACCC, *Consumer rights and guarantees*. https://www.accc.gov.au/consumers/buying-products-and-services/consumer-rights-and-guarantees
- ACCC, *Repair, replace, refund, cancel*. https://www.accc.gov.au/consumers/problem-with-a-product-or-service-you-bought/repair-replace-refund-cancel
- King & Wood Mallesons, *Unfair trading practices: arriving mid-2027* (10 Feb 2026, secondary source). https://www.mallesons.com/au/en/insights/latest-thinking/unfair-trading-practices-arriving-mid-2027-but-utp-style-enforcement-has-already-landed.html
- HWL Ebsworth, *Parliament passes unfair trading practices reforms* (10 Jul 2026, secondary source). https://hwlebsworth.com.au/parliament-passes-unfair-trading-practices-reforms-preparing-for-the-new-regime
- Parliament of Australia, *Competition and Consumer Amendment (Unfair Trading Practices) Bill 2026* (blocked; not read). https://www.aph.gov.au/Parliamentary_Business/Bills_Legislation/Bills_Search_Results/Result?bId=r7468

**United Kingdom**
- GOV.UK, *VAT and overseas goods sold directly to customers in the UK* (updated 13 May 2022). https://www.gov.uk/guidance/vat-and-overseas-goods-sold-directly-to-customers-in-the-uk
- GOV.UK, *Charging VAT on goods sold direct to customers in the UK* (updated 30 Jan 2026). https://www.gov.uk/guidance/charging-vat-on-goods-sold-direct-to-customers-in-the-uk
- GOV.UK, *Register for VAT*. https://www.gov.uk/register-for-vat
- GOV.UK, *VAT rates*. https://www.gov.uk/vat-rates
- GOV.UK, *Check when a business must follow the rules for Making Tax Digital for VAT*. https://www.gov.uk/guidance/check-when-a-business-must-follow-the-rules-for-making-tax-digital-for-vat
- GOV.UK, *Tax and customs for goods sent from abroad: Tax and duty*. https://www.gov.uk/goods-sent-from-abroad/tax-and-duty
- HMRC, *Reforming the customs treatment of low value imports into the UK* (13 Jul 2026). https://www.gov.uk/government/publications/reforming-customs-rules-for-low-value-imports/reforming-the-customs-treatment-of-low-value-imports-into-the-uk
- HM Treasury and HMRC, *Reforming the customs treatment of low value imports* (consultation, Nov 2025 to Mar 2026). https://www.gov.uk/government/consultations/reforming-the-customs-treatment-of-low-value-imports-into-the-united-kingdom
- HMRC, *LVI consultation response* (Jul 2026, read via search extract). https://assets.publishing.service.gov.uk/media/6a511a7b1228eb26a4cab7c6/LVI_Consultation_Response.pdf
- UK Trade Tariff API, commodity 9102110000 (read 9 Oct 2026). https://www.trade-tariff.service.gov.uk/api/v2/commodities/9102110000
- UK Trade Tariff, heading 9101 (read via search extract). https://www.trade-tariff.service.gov.uk/trade-tariff/headings/9101
- CMA, *Unfair commercial practices (CMA207)* (18 Nov 2025). https://assets.publishing.service.gov.uk/media/691b9bd821ef5aaa6543ee6f/Unfair_commercial_practices_CMA207_18_Nov_2025__2_.pdf
- CMA, *Urgency and price reduction claims: compliance advice* (open letter, 29 Mar 2023). https://assets.publishing.service.gov.uk/media/64232b153d885d000cdadd30/OCA_business_open_letter_FINAL.pdf
- CTSI on Business Companion, *Guidance for Traders on Pricing Practices* (reviewed Apr 2026; not statutory). https://www.businesscompanion.info/en/guidance-for-traders-on-pricing-practices
- CMA, *Discount and reference pricing principles: selling mattresses online* (2024, read via search extract). https://assets.publishing.service.gov.uk/media/66ab4347a3c2a28abb50db3c/Discount_and_reference_pricing_principles.pdf
- GOV.UK, *CMA launches court action against Emma*. https://www.gov.uk/government/news/cma-launches-court-action-against-emma-to-protect-uk-consumers
- Consumer Contracts Regulations 2013, reg. 30. https://www.legislation.gov.uk/uksi/2013/3134/regulation/30
- Consumer Contracts Regulations 2013, reg. 34. https://www.legislation.gov.uk/uksi/2013/3134/regulation/34
- Consumer Contracts Regulations 2013, reg. 35. https://www.legislation.gov.uk/uksi/2013/3134/regulation/35
- Consumer Rights Act 2015, s.22. https://www.legislation.gov.uk/ukpga/2015/15/section/22
- GOV.UK, *Accepting returns and giving refunds*. https://www.gov.uk/accepting-returns-and-giving-refunds
- GOV.UK, *VAT and overseas goods sent to the UK and returned to the seller* (updated 20 Jun 2025). https://www.gov.uk/guidance/vat-and-overseas-goods-sent-to-the-uk-and-returned-to-the-seller

**Canada**
- CRA, *Supply of qualifying goods in Canada* (2024-10-16). https://www.canada.ca/en/revenue-agency/services/tax/businesses/topics/gst-hst-businesses/digital-economy-gsthst/charge-collect/sales-good.html
- CRA, *GST/HST for digital-economy businesses* (read via search extract). https://www.canada.ca/en/revenue-agency/services/tax/businesses/topics/gst-hst-businesses/digital-economy.html
- CRA, *GST/HST rates* (2025-04-01). https://www.canada.ca/en/revenue-agency/services/tax/businesses/topics/gst-hst-businesses/charge-collect-which-rate/calculator.html
- CBSA, *Importing by mail or courier: Paying duty and/or taxes* (2025-11-25). https://www.cbsa-asfc.gc.ca/import/postal-postale/dtytx-drttx-eng.html
- CBSA, *Increase to low-value shipment thresholds* (read via search extract). https://www.cbsa-asfc.gc.ca/services/cusma-aceum/lvs-efv-eng.html
- CBSA, *Memorandum D8-2-16: Courier Imports Remission* (read via search extract). https://www.cbsa-asfc.gc.ca/publications/dm-md/d8/d8-2-16-eng.html
- CBSA, *Memorandum D5-1-1: International mail processing* (2024, read via search extract). https://www.cbsa-asfc.gc.ca/publications/dm-md/d5/d5-1-1-eng.html
- CBSA, *Customs Tariff 2026, Chapter 91* (revised 1 Sep 2026). https://www.cbsa-asfc.gc.ca/trade-commerce/tariff-tarif/2026/01-99/ch91-2026-2-eng.pdf
- CBSA, *Customs Notice 24-32: China Surtax Order (2024)* (2026-02-26). https://www.cbsa-asfc.gc.ca/publications/cn-ad/cn24-32-eng.html
- Finance Canada, *Final list of steel and aluminum products from China* (Oct 2024). https://www.canada.ca/en/department-finance/news/2024/10/final-list-of-steel-and-aluminum-products-from-china-that-will-be-subject-to-a-25-per-cent-surtax.html
- Order in Council P.C. 2025-0917, *Steel Derivative Goods Surtax Order*. https://orders-in-council.canada.ca/attachment.php?attach=47872&lang=en
- CBSA, *Customs Notice 25-33: Steel Derivative Goods Surtax Order* (2026-07-22). https://www.cbsa-asfc.gc.ca/publications/cn-ad/cn25-33-eng.html
- Revenu Québec, *Registration under the specified system* (read via search extract). https://www.revenuquebec.ca/en/businesses/consumption-taxes/gsthst-and-qst/special-cases-gsthst-and-qst/suppliers-outside-quebec/qst-registration-for-suppliers-outside-quebec/registration-under-the-specified-system-suppliers-outside-quebec-who-are-registered-under-the-general-gsthst-system/
- *Competition Act*, s.74.01 (current to 2026-09-21). https://laws-lois.justice.gc.ca/eng/acts/C-34/section-74.01.html
- Competition Bureau, *Ordinary Price Claims* (2009, modified 2022-06-23). https://competition-bureau.canada.ca/en/how-we-foster-competition/education-and-outreach/ordinary-price-claims
- Competition Bureau, *Businesses: Make sure your Black Friday "deals" are real* (25 Nov 2025). https://www.canada.ca/en/competition-bureau/news/2025/11/businesses-make-sure-your-black-friday-deals-are-real.html
- OQLF, *Langue du commerce et des affaires* (read via search extract). https://www.oqlf.gouv.qc.ca/francisation/droits_linguistiques/droits/langue-du-commerce-et-des-affaires.html
- Government of Ontario, *Your rights under the Consumer Protection Act* (read via search extract). https://www.ontario.ca/page/your-rights-under-consumer-protection-act
- CBSA, *Casual refunds for imported goods* (read via search extract). https://www.cbsa-asfc.gc.ca/import/courier/crig-robi-eng.html
- CBSA, *Memorandum D6-2-6: Refund of duties and taxes on non-commercial importations* (2015, read via search extract). https://www.cbsa-asfc.gc.ca/publications/dm-md/d6/d6-2-6-eng.html

**Shopify**
- *Selling in multiple currencies*. https://help.shopify.com/en/manual/international/pricing
- *Product prices by country*. https://help.shopify.com/en/manual/markets/pricing/product-prices-by-country
- *Including or excluding tax on product prices*. https://help.shopify.com/en/manual/taxes/include-exclude-taxes
- *Choosing a tax service*. https://help.shopify.com/en/manual/taxes/shopify-tax/choose-tax-service
- *Shopify Tax pricing*. https://help.shopify.com/en/manual/taxes/shopify-tax/pricing
- *UK tax* (read via search extract). https://help.shopify.com/en/manual/taxes/uk/uk-tax-migrate
- *Understanding customs fees*. https://help.shopify.com/en/manual/international/duties-and-import-taxes/understanding-customs-fees
- *Collecting international duties and import taxes at checkout*. https://help.shopify.com/en/manual/international/duties-and-import-taxes/charging-duties
- *Considerations for charging duties and import taxes at checkout*. https://help.shopify.com/en/manual/international/duties-and-import-taxes/considerations
- *Carriers and shipping terms for international orders*. https://help.shopify.com/en/manual/international/duties-and-import-taxes/carriers-and-shipping-terms
- Shopify Changelog, *Shopify duties and import taxes now available for all Shopify plans* (Feb 2025, read via search extract). https://changelog.shopify.com/posts/shopify-duty-calculator-now-available-for-all-shopify-plans
- *Managed Markets requirements and considerations*. https://help.shopify.com/en/manual/international/managed-markets/requirements-and-considerations
- *Managed Markets fulfillment* (read via search extract). https://help.shopify.com/en/manual/international/managed-markets/fulfillment
- *Refunds for orders in local currencies*. https://help.shopify.com/en/manual/international/pricing/refunds
- *Return rules*. https://help.shopify.com/en/manual/orders/refunds-returns/self-serve-returns/return-rules
- *Return labels*. https://help.shopify.com/en/manual/fulfillment/fulfilling-orders/shipping-labels/buying-labels/return-labels

**Exchange rates**
- currency-converter.org.uk, *USD rates* (9 Oct 2026; aggregator, not an official fixing). https://www.currency-converter.org.uk/currency-rates/NSA/USD-rate.html

[ato-lvig]: https://www.ato.gov.au/businesses-and-organisations/international-tax-for-business/gst-for-non-resident-businesses/gst-on-low-value-imported-goods
[ato-how]: https://www.ato.gov.au/businesses-and-organisations/international-tax-for-business/gst-for-non-resident-businesses/how-australian-gst-works
[ato-simple]: https://ato.gov.au/businesses-and-organisations/international-tax-for-business/non-resident-businesses-and-gst/simplified-gst-registration
[ato-lcr]: https://www.ato.gov.au/law/view/document?DocID=COG%2FLCR20181%2FNAT%2FATO%2F00001
[ato-consumers]: https://www.ato.gov.au/businesses-and-organisations/gst-excise-and-indirect-taxes/gst/in-detail/rules-for-specific-transactions/international-transactions/australian-consumers-importing-goods-and-services
[ato-online]: https://www.ato.gov.au/businesses-and-organisations/international-tax-for-business/gst-for-non-resident-businesses/non-resident-businesses-making-online-sales-to-australia
[abf-decl]: https://www.abf.gov.au/imports/Pages/How-to-import/Import-declarations.aspx
[abf-2018-13]: https://www.abf.gov.au/help-and-support-subsite/customsnotices/2018-13.pdf
[accc-display]: https://www.accc.gov.au/consumers/pricing/price-displays
[accc-component]: https://www.accc.gov.au/publications/advertising-selling/advertising-and-selling-guide/pricing/component-pricing
[accc-twoprice]: https://www.accc.gov.au/publications/advertising-selling/advertising-and-selling-guide/pricing/two-price-comparison-advertising
[accc-emma]: https://www.accc.gov.au/system/files/ACCC%20v%20Emma%20Sleep%20_%20Concise%20Statement.pdf
[accc-online]: https://www.accc.gov.au/consumers/buying-products-and-services/buying-online
[accc-rights]: https://www.accc.gov.au/consumers/buying-products-and-services/consumer-rights-and-guarantees
[accc-rrr]: https://www.accc.gov.au/consumers/problem-with-a-product-or-service-you-bought/repair-replace-refund-cancel
[mallesons]: https://www.mallesons.com/au/en/insights/latest-thinking/unfair-trading-practices-arriving-mid-2027-but-utp-style-enforcement-has-already-landed.html
[hwle]: https://hwlebsworth.com.au/parliament-passes-unfair-trading-practices-reforms-preparing-for-the-new-regime
[aph]: https://www.aph.gov.au/Parliamentary_Business/Bills_Legislation/Bills_Search_Results/Result?bId=r7468
[gov-135]: https://www.gov.uk/guidance/vat-and-overseas-goods-sold-directly-to-customers-in-the-uk
[gov-charging]: https://www.gov.uk/guidance/charging-vat-on-goods-sold-direct-to-customers-in-the-uk
[gov-register]: https://www.gov.uk/register-for-vat
[gov-rates]: https://www.gov.uk/vat-rates
[gov-mtd]: https://www.gov.uk/guidance/check-when-a-business-must-follow-the-rules-for-making-tax-digital-for-vat
[gov-abroad]: https://www.gov.uk/goods-sent-from-abroad/tax-and-duty
[gov-lvi-tiin]: https://www.gov.uk/government/publications/reforming-customs-rules-for-low-value-imports/reforming-the-customs-treatment-of-low-value-imports-into-the-uk
[gov-lvi-consult]: https://www.gov.uk/government/consultations/reforming-the-customs-treatment-of-low-value-imports-into-the-united-kingdom
[gov-lvi-response]: https://assets.publishing.service.gov.uk/media/6a511a7b1228eb26a4cab7c6/LVI_Consultation_Response.pdf
[uk-api]: https://www.trade-tariff.service.gov.uk/api/v2/commodities/9102110000
[uk-9101]: https://www.trade-tariff.service.gov.uk/trade-tariff/headings/9101
[cma207]: https://assets.publishing.service.gov.uk/media/691b9bd821ef5aaa6543ee6f/Unfair_commercial_practices_CMA207_18_Nov_2025__2_.pdf
[cma-letter]: https://assets.publishing.service.gov.uk/media/64232b153d885d000cdadd30/OCA_business_open_letter_FINAL.pdf
[ctsi]: https://www.businesscompanion.info/en/guidance-for-traders-on-pricing-practices
[cma-mattress]: https://assets.publishing.service.gov.uk/media/66ab4347a3c2a28abb50db3c/Discount_and_reference_pricing_principles.pdf
[cma-emma]: https://www.gov.uk/government/news/cma-launches-court-action-against-emma-to-protect-uk-consumers
[ccr30]: https://www.legislation.gov.uk/uksi/2013/3134/regulation/30
[ccr34]: https://www.legislation.gov.uk/uksi/2013/3134/regulation/34
[ccr35]: https://www.legislation.gov.uk/uksi/2013/3134/regulation/35
[cra22]: https://www.legislation.gov.uk/ukpga/2015/15/section/22
[gov-returns]: https://www.gov.uk/accepting-returns-and-giving-refunds
[gov-returned]: https://www.gov.uk/guidance/vat-and-overseas-goods-sent-to-the-uk-and-returned-to-the-seller
[cra-goods]: https://www.canada.ca/en/revenue-agency/services/tax/businesses/topics/gst-hst-businesses/digital-economy-gsthst/charge-collect/sales-good.html
[cra-digital]: https://www.canada.ca/en/revenue-agency/services/tax/businesses/topics/gst-hst-businesses/digital-economy.html
[cra-rates]: https://www.canada.ca/en/revenue-agency/services/tax/businesses/topics/gst-hst-businesses/charge-collect-which-rate/calculator.html
[cbsa-pay]: https://www.cbsa-asfc.gc.ca/import/postal-postale/dtytx-drttx-eng.html
[cbsa-cusma]: https://www.cbsa-asfc.gc.ca/services/cusma-aceum/lvs-efv-eng.html
[cbsa-d8216]: https://www.cbsa-asfc.gc.ca/publications/dm-md/d8/d8-2-16-eng.html
[cbsa-d511]: https://www.cbsa-asfc.gc.ca/publications/dm-md/d5/d5-1-1-eng.html
[ca-tariff]: https://www.cbsa-asfc.gc.ca/trade-commerce/tariff-tarif/2026/01-99/ch91-2026-2-eng.pdf
[cbsa-2432]: https://www.cbsa-asfc.gc.ca/publications/cn-ad/cn24-32-eng.html
[fin-china]: https://www.canada.ca/en/department-finance/news/2024/10/final-list-of-steel-and-aluminum-products-from-china-that-will-be-subject-to-a-25-per-cent-surtax.html
[oic-steel]: https://orders-in-council.canada.ca/attachment.php?attach=47872&lang=en
[cbsa-2533]: https://www.cbsa-asfc.gc.ca/publications/cn-ad/cn25-33-eng.html
[rq-specified]: https://www.revenuquebec.ca/en/businesses/consumption-taxes/gsthst-and-qst/special-cases-gsthst-and-qst/suppliers-outside-quebec/qst-registration-for-suppliers-outside-quebec/registration-under-the-specified-system-suppliers-outside-quebec-who-are-registered-under-the-general-gsthst-system/
[ca-7401]: https://laws-lois.justice.gc.ca/eng/acts/C-34/section-74.01.html
[cb-opc]: https://competition-bureau.canada.ca/en/how-we-foster-competition/education-and-outreach/ordinary-price-claims
[cb-bf]: https://www.canada.ca/en/competition-bureau/news/2025/11/businesses-make-sure-your-black-friday-deals-are-real.html
[oqlf]: https://www.oqlf.gouv.qc.ca/francisation/droits_linguistiques/droits/langue-du-commerce-et-des-affaires.html
[on-cpa]: https://www.ontario.ca/page/your-rights-under-consumer-protection-act
[cbsa-refund]: https://www.cbsa-asfc.gc.ca/import/courier/crig-robi-eng.html
[cbsa-d626]: https://www.cbsa-asfc.gc.ca/publications/dm-md/d6/d6-2-6-eng.html
[shopify-pricing]: https://help.shopify.com/en/manual/international/pricing
[shopify-catalog]: https://help.shopify.com/en/manual/markets/pricing/product-prices-by-country
[shopify-taxincl]: https://help.shopify.com/en/manual/taxes/include-exclude-taxes
[shopify-taxsvc]: https://help.shopify.com/en/manual/taxes/shopify-tax/choose-tax-service
[shopify-taxprice]: https://help.shopify.com/en/manual/taxes/shopify-tax/pricing
[shopify-uk-migrate]: https://help.shopify.com/en/manual/taxes/uk/uk-tax-migrate
[shopify-customs]: https://help.shopify.com/en/manual/international/duties-and-import-taxes/understanding-customs-fees
[shopify-duties]: https://help.shopify.com/en/manual/international/duties-and-import-taxes/charging-duties
[shopify-considerations]: https://help.shopify.com/en/manual/international/duties-and-import-taxes/considerations
[shopify-carriers]: https://help.shopify.com/en/manual/international/duties-and-import-taxes/carriers-and-shipping-terms
[shopify-changelog]: https://changelog.shopify.com/posts/shopify-duty-calculator-now-available-for-all-shopify-plans
[shopify-mm-req]: https://help.shopify.com/en/manual/international/managed-markets/requirements-and-considerations
[shopify-mm-ful]: https://help.shopify.com/en/manual/international/managed-markets/fulfillment
[shopify-refunds]: https://help.shopify.com/en/manual/international/pricing/refunds
[shopify-returnrules]: https://help.shopify.com/en/manual/orders/refunds-returns/self-serve-returns/return-rules
[shopify-returnlabels]: https://help.shopify.com/en/manual/fulfillment/fulfilling-orders/shipping-labels/buying-labels/return-labels
[fx]: https://www.currency-converter.org.uk/currency-rates/NSA/USD-rate.html
