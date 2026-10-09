# Selling in Australia before registering a business

Research for [#29](https://github.com/Kintukka2/FOSLN-WATCH/issues/29), done on 9 October 2026. It asks whether FOSLN can launch in Australia, its only launch Market, while the owner is still an individual with no ABN. Section 6 covers a later question: showing Australians prices in USD only. It builds on the Australian section of [#10's findings](https://github.com/Kintukka2/FOSLN-WATCH/blob/research/au-uk-ca-markets/docs/research/au-uk-ca-markets.md). This note reports what the sources say. It is not legal or tax advice. Income tax planning and choosing a business structure are out of scope.

## Short answer

- **Shopify and Shopify Payments work without an ABN.** Shopify Payments Australia has an "Individual" business type for people with no ABN. The owner must live in Australia, be at least 18, and have an Australian AUD transaction account.
- **The smallest registration is an ABN as a sole trader.** It's free and done online. Trading under the name "FOSLN" rather than the owner's own name also needs a registered business name, which costs A$47 a year and needs an ABN first.
- **GST starts at A$75,000.** A resident seller must register within 21 days of reaching A$75,000 of GST turnover, measured on the past 12 months or the next 12 months. Before then FOSLN charges no GST and must not show prices as "incl. GST". At A$40 an order, A$75,000 is about 1,875 orders in 12 months.
- **Doing business doesn't depend on registering.** The ATO judges this by what you do (aiming for profit, regular sales, run like a business), and a sole trader reports business income in their own tax return.
- **Australian Consumer Law applies in full.** It covers anyone selling "in trade or commerce", and that includes any business activity. Registration isn't part of the test.
- **Pricing in USD only (section 6).**
  - The ACL's single-price rule doesn't name a currency. Its only currency wording is in the multiple-pricing rule, which ignores foreign-currency prices.
  - The ACCC's leading case: Airbnb paid a A$15 million penalty for showing USD prices to Australians with a bare "$".
  - Once FOSLN is registered, GST is worked out in AUD even when the price is in USD.
  - In Shopify, Australian buyers can be shown USD. FOSLN then pays a 2% conversion fee, with payouts in AUD.
  - The buyer's bank usually adds 2.2% to 3.5%.

## At a glance

| | Before any registration | With an ABN (sole trader) | Once registered for GST |
|---|---|---|---|
| Shopify Payments business type | Individual | Sole trader | Sole trader |
| Cost to register | Nothing | ABN free. Business name A$47 for 1 year or A$108 for 3 years | GST registration (cost not stated, see below) |
| GST on FOSLN's sales | None | None while under A$75,000 | 10% (1/11 of the price), about A$3.64 on an A$40 order |
| Price shown on the store | One total price with no GST label | Same | One total price including GST |
| GST on Shopify's own bill | 10% added | 10% added | None, once the ABN is added in Shopify billing |
| Consumer guarantees | Apply | Apply | Apply |

### Assumptions behind the figures

- **Order:** one A$30 watch plus A$10 shipping, so A$40 paid by the customer. The Supplier sends it from China straight to an Australian customer. That's well under the A$1,000 low-value limit.
- **Currency:** the store sells and pays out in AUD only. So the currency conversion fee below doesn't apply to these sales.
- **Arithmetic is mine:** fee examples and order counts are worked out from the published rates. They aren't quoted from a source.

---

## 1. Opening a store and taking payments without an ABN

### Shopify itself

- **No ABN needed to open a store.** The Terms of Service ask for a name, email and other required details. You must be at least 18, or the age of majority where you live if that's higher ([Shopify Terms of Service, updated 1 Aug 2026][shopify-tos]).
- **It's for business use.** By signing up, you confirm you use Shopify "for the purposes of carrying on a business activity and not for any personal, household or family purpose" ([same page][shopify-tos]).

### Shopify Payments onboarding

- **Individual, no ABN.** "If you don't have an ABN, then you must select the Individual business type." Shopify describes Individuals as "single-person businesses that don't have an ABN and are doing business privately" ([Shopify, Shopify Payments Australia requirements][sp-req]).
- **What an Individual provides:** legal name, date of birth, email, phone and an Australian residential address ([same page][sp-req]).
- **Residency.** For Individuals and sole traders, "you're required to live in Australia to be eligible for Shopify Payments Australia" ([same page][sp-req]).
- **Sole trader.** This type needs the ABN, the registered business name, a business phone and the registered business address ([same page][sp-req]). Shopify checks these against the Australian business register.
- **Company ("corporation").** This needs an ABN and an ACN, and lists beneficial owners and directors ([same page][sp-req]).
- **Identity checks.** Shopify may ask for photo ID (passport, driver's licence and similar) and proof of address. A business document, if asked for, must show "the legal entity name, ABN, and address of your business" ([same page][sp-req]).
- **Bank account.** It "must be with a bank in Australia in AUD currency" and be able to take BECS transfers. "Savings accounts can't be used for Shopify Payments payouts." Money transfer services that imitate bank accounts aren't accepted ([same page][sp-req]).
- **Switching later.** "You can't change the business type, country, or the person associated with individual or sole proprietor entities." Moving from Individual to sole trader means adding a new business entity in Shopify. You must switch off Shopify Payments and other financial services first, and only the store owner can do that ([Shopify, Business entities][shopify-entities]).

### The smallest registration if one is needed

- **ABN as a sole trader.** "It's free to register for an ABN through the Australian Government's Australian Business Register (ABR)." You apply online, and if all the information is given "you'll find out about your ABN as soon as you've completed your online application". Some applications go to review, which can take 20 business days ([business.gov.au, Register for an ABN][bga-abn]).
- **Who can get one.** "To get one you need to be running a business or other enterprise" ([business.gov.au][bga-abn]). The ABR's own wording says you're entitled if you're "carrying on or starting an enterprise in Australia" ([ABR, ABN entitlement][abr-entitle], read via search extract).
- **What to have ready:** your business structure, proof of identity, and details of your business activities ([business.gov.au][bga-abn]).
- **Business name.** "You need to register a business name if it's different to your own name." A sole trader trading under their own name doesn't need one ([business.gov.au, Choose a business name][bga-name-need]). ASIC says the own name means "your first name and surname and nothing else" ([ASIC, Register a business name][asic-bn]).
  - It costs **A$47 for 1 year or A$108 for 3 years**. It takes about 10 to 15 minutes online, and confirmation comes within 2 business days by card ([business.gov.au, Register your business name][bga-name]).
  - You need an ABN first, or an ABN application reference number. The Business Registration Service can do both at once ([ASIC][asic-bn]).
  - Carrying on a business under an unregistered name is an offence of 30 penalty units, unless an exception applies. For an individual, the main exception is trading under their own name ([Business Names Registration Act 2011, s 18][bnra], as made; the 1 July 2026 compilation still lists s 18 under the same heading).
- **Company.** Registering a company also takes about 15 minutes online through the Business Registration Service, with confirmation in 2 business days. It carries ongoing duties: an annual ASIC fee, keeping details up to date, and a yearly solvency resolution ([business.gov.au, Register a company][bga-co]). I couldn't read the current fee. ASIC's fee pages showed placeholders instead of amounts.
- **All in a browser?** The ABN, business name and company registrations are all online forms ([business.gov.au][bga-abn]; [business.gov.au][bga-name]; [business.gov.au][bga-co]). GST registration can also be done online, as well as by phone, through a tax or BAS agent, or on a paper form ([ATO, Registering for GST][ato-reg], read via search extract). None of these needs a terminal. I didn't confirm whether the online ABN form needs a myID login. myID is a phone app, and the ABR says it's "the only Digital ID you can use to access our online services" ([ABR][abr-home], read via search extract).

### PayPal as an alternative

- **Shopify needs a PayPal business account.** If you don't have "a PayPal professional business account" linked to the store's email, you add the email to an existing PayPal account or "sign up for a new PayPal business account" ([Shopify, Set up PayPal][shopify-paypal]).
- **What PayPal asks for.** Opening an Australian business account needs "your full legal name, email address, a password, your business registration number, business description, and your business bank account details" ([PayPal AU, Open a business account][pp-open]).
- **Not verified: whether that business registration number must be an ABN.** The page doesn't say whether an individual with no ABN can leave it out.
- **PayPal fees (AU, updated 23 July 2026).** Domestic is **2.90% + A$0.30**. International adds 1.00 percentage point. Currency conversion is 3.0% ([PayPal AU, Merchant fees][pp-fees]).
- **Shopify's charge on top.** Using an outside payment provider adds a Shopify third-party transaction fee: 2% on Basic, 1% on Grow, 0.6% on Advanced and 0.2% on Plus ([Shopify AU pricing][shopify-au-pricing]). PayPal is exempt from that fee when Shopify Payments is the main provider ([Shopify, Fees and costs][shopify-fees]).

---

## 2. What Shopify charges in Australia

### Plan prices (AUD)

From [Shopify AU pricing][shopify-au-pricing], read 9 October 2026:

| Plan | Paid monthly | Paid yearly (per month) |
|---|---|---|
| Basic | A$56 | A$42 |
| Grow | A$149 | A$114 |
| Advanced | A$575 | A$431 |
| Plus | From A$3,700 | Not listed |

- **Trial.** "Try 3 days free, then $1/month for 3 months." No credit card is needed for the trial ([same page][shopify-au-pricing]).
- **GST on Shopify's bill.** "If your store is based in Australia, then you're subject to a 10% Goods and Services Tax (GST) on your Shopify subscription and any shipping labels." You avoid it only once you're GST-registered and add your ABN under Settings > Billing. The exemption doesn't apply to invoices issued before you add it ([Shopify, GST exemptions for billing][shopify-billing-gst]).
- **Not verified:** whether the prices above already include that GST. The pricing page doesn't say.

### Shopify Payments rates (online, AUD)

From [Shopify AU pricing][shopify-au-pricing]:

| Plan | Standard cards | Amex | International cards |
|---|---|---|---|
| Basic | 1.7% + 30¢ | 2.9% + 30¢ | 3.5% + 30¢ |
| Grow | 1.55% + 30¢ | 2.8% + 30¢ | 3.4% + 30¢ |
| Advanced | 1.35% + 30¢ | 2.7% + 30¢ | 3.3% + 30¢ |
| Plus | 1.15% + 30¢ | 2.6% + 30¢ | 3.2% + 30¢ |

- **On one A$40 order (Basic, my arithmetic):** about A$0.98 for a standard card, A$1.46 for Amex, and A$1.70 for a card issued outside Australia.
- **Domestic and international.** Shopify defines domestic as cards "issued in the same country or region as your store" and cross-border as cards issued anywhere else ([Shopify, Fees and costs][shopify-fees]).
- **Accepted cards.** Visa, Mastercard, American Express and UnionPay. Apple Pay, Google Pay and Shop Pay are switched on automatically ([Shopify, Payment methods in Australia][sp-methods]).
- **Currency conversion fee.** It's **2%** for all Shopify Payments regions other than the US (1.5%) ([Shopify, Fees and costs][shopify-fees]). It applies only "when you accept payments in a currency different from your payout currency". An AUD-only store paid out in AUD doesn't pay it. Since 6 April 2026 it has been worked out on the gross order amount ([Shopify, Currency conversion calculation][shopify-ccc]).
- **GST on payment fees.** Shopify tells Australian merchants that "you need to file the amount of goods and services tax (GST) paid on transaction fees". Monthly invoices and the transaction export show it ([Shopify, Tax reporting in Australia][sp-taxrep], read via WebFetch). Not verified: whether the rates above include that GST or have it added.

### Shop Pay Installments

- **Not available in Australia.** Eligible merchants must be in the US (USD), Canada (CAD) or the UK (GBP), with Shopify Payments set to that currency ([Shopify, Shop Pay Installments eligibility][spi-elig]). So there's no Australian merchant rate to report.
- Shopify's AU pricing page doesn't list a Shop Pay Installments rate ([Shopify AU pricing][shopify-au-pricing]).

---

## 3. GST for a resident seller

### The threshold

- **Who must register.** "You are required to be registered" if you're carrying on an enterprise and your GST turnover meets the registration threshold ([GST Act s 23-5][gst-act]).
- **How much.** The Act sets A$50,000 "or such higher amount as the regulations specify" ([GST Act s 23-15][gst-act]). The regulations specify **A$75,000** ([GST Regulations 2019, s 23-15.01, compilation 1 Nov 2025][gst-regs]).
- **Two tests, either one triggers it** ([GST Act s 188-10][gst-act]):
  - **Current GST turnover:** sales in the current month plus the previous 11 months. It counts, unless the ATO accepts that projected turnover will stay below the threshold ([s 188-15][gst-act]).
  - **Projected GST turnover:** sales in the current month plus the next 11 months ([s 188-20][gst-act]).
  - Both count supplies "made, or … likely to make" ([s 188-15, 188-20][gst-act]). So a forecast of A$75,000 in the coming year triggers registration even before the sales happen.
- **What counts.** The ATO says GST turnover is total business income, not profit, minus GST, input-taxed sales and sales not connected with Australia ([ATO, Registering for GST][ato-reg], read via search extract). Shipping charged to the customer is part of what they pay, so it counts ([ATO, How Australian GST works][ato-how], cited in #10).
- **Deadline.** You must apply "within 21 days after becoming required to be registered" ([GST Act s 25-1][gst-act]). If you register late, the ATO says you may owe GST on sales since the date you were required to register, even if your prices didn't include it ([ATO][ato-noreg], read via search extract).
- **Voluntary registration.** You may register below the threshold if you carry on an enterprise ([GST Act s 23-10][gst-act]). The ATO says you generally stay registered for at least 12 months ([ATO, Registering for GST][ato-reg], read via search extract).
- **ABN first.** For a resident business, ordinary GST registration needs an ABN ([ATO, Registering for GST][ato-reg], read via search extract). The simplified ARN registration in #10's findings is for non-resident businesses, so it doesn't apply to an Australian-resident owner.
- **In FOSLN's case (my arithmetic):** at A$40 an order, A$75,000 is about 1,875 orders in 12 months.

### Is it the same as for an offshore seller?

- **Yes, for the threshold and the tests.** The low-value goods rules don't depend on where the seller lives. An "offshore supply of low value goods" is any supply where the goods are brought to Australia and the supplier delivers them or arranges delivery ([GST Act s 84-77(1)][gst-act]). Low value means a customs value of A$1,000 or less ([s 84-79(3)][gst-act]). The ATO's ruling says these rules apply to "both non-resident and Australian resident suppliers", including Australian merchants whose goods are sent straight from overseas to the customer ([ATO, LCR 2018/1, para 257][ato-lcr], read via search extract).
- **So FOSLN's sales count toward A$75,000.** Each sale to an Australian consumer is "connected with" Australia ([s 84-75(1)][gst-act]), so it counts in GST turnover ([s 188-15(3)][gst-act]).
- **What differs.** A resident registers for an ABN and full GST. A non-resident can use the simplified ARN registration ([ATO, Simplified GST registration][ato-simple], cited in #10).

### Does any GST arise before FOSLN registers?

- **On FOSLN's sales: no.** A sale is only a "taxable supply" if, among other things, "you are registered, or required to be registered" ([GST Act s 9-5(d)][gst-act]). Below A$75,000 and unregistered, FOSLN is neither.
- **At the border: no, by my reading.** The ABF says most goods worth A$1,000 or less have no duties, taxes or charges to pay at the border ([ABF, Import declarations][abf-decl], cited in #10). I couldn't find an ATO page that covers an unregistered resident seller's parcels directly. My reading of the Act and the ABF pages is that no GST is collected at the border on them.
- **On what the Supplier charges FOSLN: possibly yes.** This depends on the Supplier.
  - An unregistered buyer counts as a "consumer" under the low-value rules ([GST Act s 84-75(2)(a)][gst-act]).
  - The ATO says: "If you are an Australian business but you are not GST-registered, you will need to pay GST on the purchase of these imported services, digital products and low value imported goods" ([ATO, GST and Australian businesses][ato-aubiz], read via search extract).
  - So if the Supplier, or a platform it sells through, is registered for Australian GST, it may add 10% to its price to FOSLN until FOSLN registers and gives its ABN. A Supplier that isn't registered won't charge it ([ATO, GST and consumers][ato-consumers], read via search extract).
  - **Not verified:** whether FOSLN's Supplier is registered.
- **After registering.** "If you are an Australian GST registered supplier and you 'drop ship', you will have to account for GST on your sale of low value imported goods" ([ATO, GST and Australian businesses][ato-aubiz], read via search extract).

### How prices must be shown

- **One total price, always.** A seller must state "in a prominent way and as a single figure, the single price" ([Australian Consumer Law s 48(1)][cca]). That price includes "any tax, duty, fee, levy or charge imposed on the person making the representation", and the Act gives GST as an example ([s 48(7)(b), Example 2][cca]). Delivery charges may be shown separately, but a known minimum delivery charge must be stated ([s 48(2)–(3)][cca]).
- **Before registering.** No GST is payable, so there's no GST to include. The ATO says that if you're "not registered and are not required to be registered for GST, you don't include GST in the price" ([ATO][ato-noreg], read via search extract). Invoices "should not include the words 'tax invoice'" ([ATO, Setting up your business invoices][ato-invoice], read via search extract). The sources don't use the phrase "incl. GST". My inference is that a store label claiming prices include GST would be inaccurate before registration.
- **After registering.** The single price must include GST ([s 48(7)(b)][cca]). The ATO says "As soon as it's clear that GST applies, you must show the GST-inclusive price" ([ATO, How Australian GST works][ato-how], cited in #10). At the same A$40 price, FOSLN would then keep about A$36.36 (my arithmetic).
- **In Shopify.** Shopify collects tax only for regions you set up under Settings > Taxes and duties > Collect sales tax, with your tax number. You can leave the number blank while your application is pending ([Shopify, Setting up taxes][shopify-taxsetup]). "All prices include tax" is a setting on the same page ([Shopify, Including or excluding tax][shopify-taxincl], cited in #10). Shopify doesn't file or pay GST for you ([Shopify, Setting up taxes][shopify-taxsetup]).

---

## 4. Carrying on a business

- **One line:** whether you're in business depends on what you do, not on registering. The ATO's signs are an intention to make a profit, repeated and continuous sales, and running it like a business. A sole trader declares business income in their individual tax return, with no threshold for business income ([ATO, Are you in business?][ato-inbiz]; [ATO, Business, partnership and trust income][ato-bizinc], both read via search extract).

---

## 5. Australian Consumer Law for an unregistered seller

- **Registration isn't part of the test.** The consumer guarantees apply when "a person supplies, in trade or commerce, goods to a consumer" ([ACL s 54(1)][cca]). "Trade or commerce" includes trade between Australia and other countries, and "includes any business or professional activity (whether or not carried on for profit)" ([ACL s 2][cca]).
- **Individuals are covered.** The Commonwealth ACL applies to corporations ([CCA s 131][cca]). Section 6 extends it to people who aren't corporations, for trade between States or with other countries ([CCA s 6(2)][cca]). Each State and Territory also applies the ACL as its own law ([CCA Part XI Division 2][cca]). I didn't read the State laws.
- **The one exclusion that matters is for private sales, not unregistered businesses.** The ACCC says most consumer guarantees don't apply "to one-off sales between 2 people, where the seller isn't running a business" ([ACCC, Buying online][accc-online], read via search extract). A store selling regularly is running a business, so in my reading this exclusion wouldn't apply.
- **Who is a consumer.** A buyer of goods "ordinarily acquired for personal, domestic or household use", or of goods costing A$40,000 or less, is a consumer ([ACL s 3(1)][cca]). A watch fits both.
- **In practice:**
  - The consumer guarantees, and the rules on refunds for faulty goods, apply from the first sale. #10's findings cover what they require ([au-uk-ca-markets.md, Australia: Returns][r10]).
  - The single-price rule above applies too.
  - Overseas sellers to Australian consumers must follow the ACL as well ([ACCC, Buying online][accc-online], cited in #10).

---

## 6. Showing prices to Australians in USD only

Added on 9 October 2026 after the owner asked about showing Australian consumers USD prices only. This section reports the rules and costs. It doesn't recommend either way.

### (a) What the ACL says about currency

- **The single-price rule doesn't name a currency.** It requires "the single price for the goods" as "a single figure", including taxes such as GST ([ACL s 48(1), (7)][cca]). I found no wording in s 48 that says the figure must be in Australian dollars.
- **The only currency wording is in the multiple-pricing rule.** If goods show more than one price, the seller must charge the lowest ([ACL s 47(1)][cca]). But a price "expressed as an amount in a currency other than Australian currency", or "in a way that is unlikely to be interpreted as an amount of Australian currency", doesn't count as a "displayed price" for that rule ([s 47(4)(c)–(d)][cca]).
- **Misleading conduct.** "A person must not, in trade or commerce, engage in conduct that is misleading or deceptive or is likely to mislead or deceive" ([ACL s 18(1)][cca]).
- **False or misleading price claims.** A person must not "make a false or misleading representation with respect to the price of goods or services" ([ACL s 29(1)(i)][cca]). A pecuniary penalty can apply ([s 29, Note 1][cca]).
- **The ACCC's main currency case: Airbnb (2023).**
  - Between January 2018 and August 2021, Airbnb showed Australian users prices with a "$" sign and no indication of whether they were AUD or USD. About 70,000 consumers were charged in USD.
  - The only USD reference was "in the footer of the Website and Mobile Site (not the Apps), in small font". From 31 August 2021, USD prices were "clearly denoted through the use of the abbreviation 'USD'".
  - Airbnb admitted contravening ss 18(1) and 29(1)(i). The Federal Court ordered a **A$15 million** penalty, and Airbnb offered up to A$15 million more in compensation.
  - Sources: [ACCC media release][accc-airbnb]; [ACCC concise statement][accc-airbnb-cs]; [ACCC v Airbnb Ireland UC [2023] FCA 1633][fca-airbnb]. All three were read via search extracts, because the pages blocked automated reading.
  - The ACCC's case was that Airbnb represented prices as AUD when they were USD (the "AUD Representation") ([ACCC undertaking register][accc-airbnb-und], read via search extract). So the problem was a "$" amount Australians would read as AUD, not the fact that USD was used.
- **ACCC guidance on international transaction fees.**
  - The ACCC says it "may be considered misleading under Australian Consumer Law if consumers are charged an unexpected international transaction fee for a purchase in Australian dollars, and the business appeared to be in Australia" ([ACCC, Foreign currency and money exchange][accc-fx], read via search extract).
  - Its 2019 business guidance recommends "a prominent statement during an online order process (but prior to taking payment)" about where payment is processed ([ACCC, Disclosure of international transaction fees, Dec 2019][accc-itf], read via search extract).
  - That guidance is about AUD prices processed overseas. I didn't find ACCC guidance on an Australian business that prices only in a foreign currency.

### (b) GST when the price is in USD

- **GST is worked out in AUD.** "The value of a taxable supply is to be expressed in Australian currency." Any part of the price "expressed in … a currency other than Australian currency" is converted "in the manner determined by the Commissioner" ([GST Act s 9-85][gst-act]).
- **How to convert.** The Commissioner's determination lets a seller use the RBA rate, a rate published by a foreign exchange organisation, or an agreed rate. "You must use your particular exchange rate consistently." For a seller not accounting on a cash basis, the conversion day is the earlier of the day payment is received and the transaction or invoice date ([GST Foreign Currency Conversion Determination 2018][gst-fx]).
- **The GST share is still 1/11.** GST on a taxable supply is 10% of its value, and value is the price × 10/11 ([GST Act ss 9-70, 9-75][gst-act]). So a GST-inclusive USD price carries GST equal to 1/11 of its AUD equivalent (my arithmetic from those sections).
- **The shown price must include GST.** The single price includes GST whatever the currency ([ACL s 48(7)(b)][cca]).
- **Returns and invoices.** BAS amounts must be in Australian currency ([ATO, GSTR 2001/2][ato-gstr20012], read via search extract). A tax invoice must show the GST in AUD, or give enough information to work it out, such as the conversion rate used ([same ruling][ato-gstr20012]; [ATO, Financial services Q&A, issue 5.12][ato-fsqa], both read via search extract).
- **Before registration** nothing changes from section 3. There's no GST to include, whatever the currency.

### (c) Shopify with an Australian store and Shopify Payments AU

- **Showing USD to Australians is possible.** "Each market can have a currency customization that determines what currency is displayed to customers in that market." A single-country market starts in its local currency, but "You can change this to any supported currency" ([Shopify, Local currencies][shopify-localcur], read via WebFetch).
- **Australia as the primary market.** The primary market's currency is changed under Markets > the primary market > Products and pricing > Pricing. If it differs from the store currency, "base prices are automatically converted using a daily currency conversion rate" ([Shopify, Managing markets][shopify-markets], read via search extract). With Shopify Payments, changing the store currency doesn't change the markets' currencies ([same page][shopify-markets]).
- **Conversion fee FOSLN pays: 2%.** It's charged "when you accept payments in a currency different from your payout currency", and it's 2% for Australian stores ([Shopify, Fees and costs][shopify-fees]).
  - With automatic exchange rates, "The currency conversion fee is included in the price that's displayed to your customer."
  - With manual rates, the fee isn't added to customer prices and comes out of payouts ([Shopify, Local currencies][shopify-localcur]).
  - Conversion fees aren't returned on refunds ([Shopify, Fees and costs][shopify-fees]).
- **Card rate.** An Australian-issued card is a domestic card, because Shopify bases domestic or cross-border on where the card was issued ([Shopify, Fees and costs][shopify-fees]). So my reading is that the domestic rate (1.7% + 30¢ on Basic) applies, plus the 2% conversion fee.
- **Payout currency.** "You can receive payouts in AUD on all subscription plans." Other currencies, including USD, are available to Australian stores. Choosing a non-domestic default payout currency brings a Multi-Currency Payout fee, and multiple payout currencies need the Advanced or Plus plan ([Shopify, Payouts in Australia][sp-payouts], read via WebFetch; [Shopify, Supported payout currencies][shopify-payoutcur], read via WebFetch). The fee is 1.5% on Advanced for non-US regions, and 1% on Plus ([Shopify, Payouts in multiple currencies][shopify-mcp], read via WebFetch).
- **Example (my arithmetic, approximate).** At US$1 = A$1.4332 ([currency-converter.org.uk, 9 Oct 2026][fx], from #10), A$40 is about US$27.91. On a sale paid out in AUD, FOSLN would pay about A$0.80 in conversion fee, on top of about A$0.98 in card fees on Basic.

### (d) What an Australian cardholder pays on a USD charge

Each bank's own fee documents, read 9 October 2026:

| Bank | Card | Fee on a foreign-currency purchase | Source |
|---|---|---|---|
| CommBank | Standard credit cards (Mastercard, Visa) | **3.50%** when Mastercard or Visa converts "from a foreign currency to Australian dollars". 0% on Ultimate, Smart, Neo and Interest-free Low Fee cards | [CommBank credit card fees][cba-cc] |
| CommBank | Debit Mastercard | **3.5%** "for overseas transactions in store and online", defined as purchases made overseas or where the merchant or processor is overseas | [CommBank debit cards][cba-debit] |
| Westpac | Credit cards | **3%** "of any Foreign Transaction (includes transactions in a foreign currency…)". Not on Lite and Flex cards | [Westpac credit card fees][wbc-cc] |
| Westpac | Debit Mastercard | **3%** for foreign currency with an overseas merchant. **2.2%** "where a transaction is made in foreign currency and the merchant or financial institution accepting the card is located in Australia" | [Westpac Debit Mastercard T&Cs, 27 Aug 2026][wbc-debit] |
| NAB | Visa Debit and linked credit cards (personal) | **3.5%** of the AUD amount for a "Multi-currency international transaction", one "made in a currency other than Australian dollars". Not on Platinum Visa Debit purchases | [NAB personal fees guide, 31 Oct 2025][nab-fees] |
| ANZ | Credit cards (consumer) | **3.5%** "of any overseas transaction". Not on Rewards Travel Adventures | [ANZ credit card fees, 25 Aug 2026][anz-cc] |
| ANZ | Accounts and debit cards | **3%** of any international transaction, including one "In a currency other than Australian currency" | [ANZ account fees, changes effective 30 Oct 2026][anz-acct] |

- **On top of the fee.** The card scheme sets the exchange rate. Westpac says it is "either a government mandated exchange rate or a wholesale exchange rate, selected by Mastercard Worldwide", and that Westpac's own quoted rates aren't used ([Westpac Debit Mastercard T&Cs][wbc-debit]).
- **ACCC.** It says international transaction fees are usually around 3% ([ACCC, Foreign currency and money exchange][accc-fx], read via search extract).
- **Example (my arithmetic, approximate).** On a US$27.91 charge worth about A$40, a 3.5% fee adds about A$1.40, and a 2.2% fee about A$0.88.

---

## What I couldn't verify

- **Blocked pages.** The ATO, ABR, ACCC and Federal Court pages blocked automated reading. For those I relied on search-engine extracts, and the wording quoted from them comes from those extracts. Where I could, I read the law itself on legislation.gov.au instead.
- Whether PayPal Australia lets an individual with no ABN open the business account Shopify needs.
- Whether Shopify's AUD plan prices and Shopify Payments rates already include GST, or have it added.
- The current ASIC fee to register a company. ASIC's pages showed placeholders.
- Whether the online ABN application needs a myID login.
- Whether a store or brand called "FOSLN" alone counts as "carrying on a business under a name" for an individual with no ABN. I read s 18 in the Act as first made, not the current compilation.
- Whether FOSLN's Supplier, or the platform it sells through, is registered for Australian GST and would charge FOSLN GST while FOSLN is unregistered.
- A direct ATO statement that no GST is collected at the border on parcels from an unregistered resident seller. The finding above is my reading of the GST Act and the ABF pages.
- The GST registration cost. The extracts don't mention a fee.
- How the State and Territory laws apply the ACL to individuals. I didn't read them.
- **Section 6:**
  - Any ACCC guidance, or any case, on an Australian business that prices only in a foreign currency. The Airbnb case is about a bare "$" read as AUD, not about USD prices clearly labelled.
  - The Airbnb judgment's exact wording. The Federal Court and ACCC pages blocked reading. Reports differ on the number of consumers affected (about 63,000 or about 70,000).
  - Whether the Australian card rate or the international rate applies when an Australian-issued card pays in USD. My reading from Shopify's definitions is the domestic rate.
  - Whether CommBank's debit fee applies to a USD charge from a merchant in Australia. Its wording covers overseas merchants and processors only.
  - Which entity and country Shopify Payments AU uses to process charges. That affects how some bank fee definitions apply.
  - Whether a USD-only Australian store's 2% conversion fee is shown in its USD prices when Australia is the primary market rather than an added market.

`GLOSSARY.md` on `main` still names the US as the launch Market. This ticket treats Australia as the only launch Market. Terms used here that aren't in the glossary: *ABN*, *GST turnover*, *low-value goods*.

---

## Sources

All pages were read on 9 October 2026 unless noted. The date after each source is the page's own date where it showed one.

**Shopify**
- *Terms of Service* (updated 1 Aug 2026). https://www.shopify.com/legal/terms
- *Shopify Payments Australia requirements*. https://help.shopify.com/en/manual/payments/shopify-payments/supported-countries/australia/requirements
- *Payment methods with Shopify Payments in Australia*. https://help.shopify.com/en/manual/payments/shopify-payments/supported-countries/australia/payment-methods
- *Tax reporting with Shopify Payments for Australia* (read via WebFetch). https://help.shopify.com/en/manual/payments/shopify-payments/supported-countries/australia/tax-reporting
- *Business entities*. https://help.shopify.com/en/manual/your-account/business-entities
- *Shopify AU pricing*. https://www.shopify.com/au/pricing
- *Fees and costs* (international pricing). https://help.shopify.com/en/manual/international/pricing/fees
- *Currency conversion calculation*. https://help.shopify.com/en/manual/payments/shopify-payments/store-currency/currency-conversion-calculation
- *Shop Pay Installments eligibility*. https://help.shopify.com/en/manual/payments/shop-pay-installments/eligibility
- *Navigating GST exemptions for Shopify billing in Australia and New Zealand*. https://help.shopify.com/en/manual/your-account/manage-billing/billing-charges/types-of-charges/taxes/billing-taxes-australia-new-zealand
- *Set up PayPal*. https://help.shopify.com/en/manual/payments/paypal/set-up-paypal
- *Setting up taxes*. https://help.shopify.com/en/manual/taxes/registration/setup
- *Including or excluding tax on product prices* (cited in #10). https://help.shopify.com/en/manual/taxes/include-exclude-taxes

**PayPal**
- PayPal Australia, *Open a business account*. https://www.paypal.com/au/business/open-business-account
- PayPal Australia, *Merchant fees* (updated 23 Jul 2026). https://www.paypal.com/au/webapps/mpp/merchant-fees

**Registrations**
- business.gov.au, *Register for an Australian Business Number*. https://business.gov.au/registrations/register-for-an-australian-business-number-abn
- business.gov.au, *Register your business name*. https://business.gov.au/registrations/register-your-business-name
- business.gov.au, *Choose a business name*. https://www.business.gov.au/registrations/register-a-business-name/business-name-trading-names-legal-names
- business.gov.au, *Register a company*. https://business.gov.au/registrations/register-a-company
- ASIC, *Register a business name*. https://www.asic.gov.au/for-business-and-companies/business-names/register-a-business-name
- ABR, *ABN entitlement* (read via search extract). https://www.abr.gov.au/business-super-funds-charities/applying-abn/abn-entitlement
- ABR, *Home* (myID note, read via search extract). https://www.abr.gov.au/
- *Business Names Registration Act 2011*, s 18 (as made; current compilation 1 Jul 2026). https://www.legislation.gov.au/C2011A00126/latest/text

**GST**
- *A New Tax System (Goods and Services Tax) Act 1999*, ss 9-5, 23-5, 23-10, 23-15, 25-1, 84-75, 84-77, 84-79, 188-10, 188-15, 188-20 (compilation 1 Jan 2026). https://www.legislation.gov.au/C2004A00446/latest/text
- *A New Tax System (Goods and Services Tax) Regulations 2019*, s 23-15.01 (compilation 1 Nov 2025). https://www.legislation.gov.au/F2019L00417/latest/text
- ATO, *Registering for GST* (read via search extract). https://www.ato.gov.au/businesses-and-organisations/gst-excise-and-indirect-taxes/gst/registering-for-gst
- ATO, *When to charge GST (and when not to)* (read via search extract). https://www.ato.gov.au/Business/GST/When-to-charge-GST-(and-when-not-to)/
- ATO, *Setting up your business invoices* (read via search extract). https://www.ato.gov.au/businesses-and-organisations/preparing-lodging-and-paying/record-keeping-for-business/setting-up-and-managing-records/setting-up-your-business-invoices
- ATO, *GST and Australian businesses* (read via search extract). https://www.ato.gov.au/businesses-and-organisations/gst-excise-and-indirect-taxes/gst/in-detail/rules-for-specific-transactions/international-transactions/australian-business-importing-goods-and-services
- ATO, *GST and consumers* (read via search extract). https://www.ato.gov.au/businesses-and-organisations/gst-excise-and-indirect-taxes/gst/in-detail/rules-for-specific-transactions/international-transactions/australian-consumers-importing-goods-and-services
- ATO, *LCR 2018/1 GST on low value imported goods* (read via search extract). https://www.ato.gov.au/law/view/document?DocID=COG%2FLCR20181%2FNAT%2FATO%2F00001
- ATO, *How Australian GST works* (cited in #10). https://www.ato.gov.au/businesses-and-organisations/international-tax-for-business/gst-for-non-resident-businesses/how-australian-gst-works
- ATO, *Simplified GST registration* (cited in #10). https://ato.gov.au/businesses-and-organisations/international-tax-for-business/non-resident-businesses-and-gst/simplified-gst-registration
- ABF, *Import declarations* (cited in #10). https://www.abf.gov.au/imports/Pages/How-to-import/Import-declarations.aspx

**Income tax (one line only)**
- ATO, *Are you in business?* (read via search extract). https://www.ato.gov.au/businesses-and-organisations/starting-registering-or-closing-a-business/starting-your-own-business/are-you-in-business
- ATO, *Business, partnership and trust income* (read via search extract). https://www.ato.gov.au/individuals-and-families/income-deductions-offsets-and-records/income-you-must-declare/business-partnership-and-trust-income

**Consumer law**
- *Competition and Consumer Act 2010*, ss 6, 131 and Schedule 2 (Australian Consumer Law) ss 2, 3, 48, 54 (compilation 16 Sep 2026). https://www.legislation.gov.au/C2004A00109/latest/text
- ACCC, *Buying online* (read via search extract). https://www.accc.gov.au/consumers/buying-products-and-services/buying-online
- FOSLN, *What selling into AU, UK and CA involves* (#10 findings). https://github.com/Kintukka2/FOSLN-WATCH/blob/research/au-uk-ca-markets/docs/research/au-uk-ca-markets.md

**Section 6: USD pricing**
- *Competition and Consumer Act 2010*, Schedule 2 (ACL) ss 18, 29, 47, 48 (compilation 16 Sep 2026). https://www.legislation.gov.au/C2004A00109/latest/text
- ACCC, *Airbnb to pay $15m in penalties and up to $15m compensation for misleading consumers* (Dec 2023, read via search extract). https://www.accc.gov.au/media-release/airbnb-to-pay-15m-in-penalties-and-up-to-15m-compensation-for-misleading-consumers
- ACCC, *ACCC v Airbnb Ireland UC and Airbnb, Inc, concise statement* (read via search extract). https://www.accc.gov.au/system/files/ACCC%20v%20Airbnb%20Ireland%20UC%20and%20Airbnb,%20Inc_Concise%20Statement.pdf
- ACCC, *Undertakings register: Airbnb Ireland UC* (20 Dec 2023, read via search extract). https://www.accc.gov.au/public-registers/undertakings-registers/airbnb-ireland-uc
- Federal Court, *ACCC v Airbnb Ireland UC* [2023] FCA 1633 (blocked; read via search extract). https://www.judgments.fedcourt.gov.au/judgments/Judgments/fca/single/2023/2023fca1633
- ACCC, *Foreign currency and money exchange* (read via search extract). https://www.accc.gov.au/consumers/specific-products-and-activities/foreign-currency-and-money-exchange
- ACCC, *Disclosure of international transaction fees* (Dec 2019, read via search extract). https://www.accc.gov.au/system/files/guidance-on-the-disclosure-of-international-transaction-fees_0.pdf
- *A New Tax System (Goods and Services Tax) Act 1999*, ss 9-70, 9-75, 9-85 (compilation 1 Jan 2026). https://www.legislation.gov.au/C2004A00446/latest/text
- *Goods and Services Tax: Foreign Currency Conversion Determination 2018* (F2018L00724, from 1 Jul 2018). https://www.legislation.gov.au/F2018L00724/latest/text
- ATO, *GSTR 2001/2 Goods and Services Tax: foreign exchange conversions* (read via search extract). https://www.ato.gov.au/law/view/document?DocID=GST%2FGSTR20012%2FNAT%2FATO%2F00001
- ATO, *Financial services: questions and answers* (issue 5.12, read via search extract). https://www.ato.gov.au/law/view/print?DocID=GIR%2FFinancial-services-ch5&PiT=99991231235958
- Shopify, *Local currencies* (read via WebFetch). https://help.shopify.com/en/manual/markets/customizations/local-currencies
- Shopify, *Managing markets* (read via search extract). https://help.shopify.com/en/manual/markets/managing-markets
- Shopify, *Payouts with Shopify Payments in Australia* (read via WebFetch). https://help.shopify.com/en/manual/payments/shopify-payments/supported-countries/australia/payouts
- Shopify, *Supported payout currencies* (read via WebFetch). https://help.shopify.com/en/manual/payments/shopify-payments/store-currency/supported-payout-currencies
- Shopify, *Payouts in multiple currencies* (read via WebFetch). https://help.shopify.com/en/manual/payments/shopify-payments/store-currency/payouts-in-multiple-currencies
- CommBank, *Credit card fees and charges*. https://www.commbank.com.au/credit-cards/card-fees-charges.html
- CommBank, *Debit cards*. https://www.commbank.com.au/banking/debit-cards.html
- Westpac, *Credit card fees*. https://www.westpac.com.au/personal-banking/credit-cards/manage/fees-interest/minimising-fees/
- Westpac, *Debit Mastercard Terms and Conditions for personal customers* (effective 27 Aug 2026). https://www.westpac.com.au/content/dam/public/wbc/documents/pdf/pb/Debit_MasterCard_TandC.pdf
- NAB, *A Guide to Fees and Charges: Personal banking fees* (effective 31 Oct 2025). https://www.nab.com.au/content/dam/nab/documents/guides/banking/personal-banking-fees-and-charges-guide.pdf
- ANZ, *Consumer credit cards fees and charges* (25 Aug 2026). https://www.anz.com.au/content/dam/anzcomau/documents/pdf/credit-card-fees-charges.pdf
- ANZ, *Personal Banking: Account fees and charges* (with changes effective 30 Oct 2026). https://www.anz.com.au/content/dam/anzcomau/documents/pdf/personal-account-fees-charges.pdf
- currency-converter.org.uk, *USD rates* (9 Oct 2026; aggregator, not an official fixing; cited in #10). https://www.currency-converter.org.uk/currency-rates/NSA/USD-rate.html

[shopify-tos]: https://www.shopify.com/legal/terms
[sp-req]: https://help.shopify.com/en/manual/payments/shopify-payments/supported-countries/australia/requirements
[sp-methods]: https://help.shopify.com/en/manual/payments/shopify-payments/supported-countries/australia/payment-methods
[sp-taxrep]: https://help.shopify.com/en/manual/payments/shopify-payments/supported-countries/australia/tax-reporting
[shopify-entities]: https://help.shopify.com/en/manual/your-account/business-entities
[shopify-au-pricing]: https://www.shopify.com/au/pricing
[shopify-fees]: https://help.shopify.com/en/manual/international/pricing/fees
[shopify-ccc]: https://help.shopify.com/en/manual/payments/shopify-payments/store-currency/currency-conversion-calculation
[spi-elig]: https://help.shopify.com/en/manual/payments/shop-pay-installments/eligibility
[shopify-billing-gst]: https://help.shopify.com/en/manual/your-account/manage-billing/billing-charges/types-of-charges/taxes/billing-taxes-australia-new-zealand
[shopify-paypal]: https://help.shopify.com/en/manual/payments/paypal/set-up-paypal
[shopify-taxsetup]: https://help.shopify.com/en/manual/taxes/registration/setup
[shopify-taxincl]: https://help.shopify.com/en/manual/taxes/include-exclude-taxes
[pp-open]: https://www.paypal.com/au/business/open-business-account
[pp-fees]: https://www.paypal.com/au/webapps/mpp/merchant-fees
[bga-abn]: https://business.gov.au/registrations/register-for-an-australian-business-number-abn
[bga-name]: https://business.gov.au/registrations/register-your-business-name
[bga-name-need]: https://www.business.gov.au/registrations/register-a-business-name/business-name-trading-names-legal-names
[bga-co]: https://business.gov.au/registrations/register-a-company
[asic-bn]: https://www.asic.gov.au/for-business-and-companies/business-names/register-a-business-name
[abr-entitle]: https://www.abr.gov.au/business-super-funds-charities/applying-abn/abn-entitlement
[abr-home]: https://www.abr.gov.au/
[bnra]: https://www.legislation.gov.au/C2011A00126/latest/text
[gst-act]: https://www.legislation.gov.au/C2004A00446/latest/text
[gst-regs]: https://www.legislation.gov.au/F2019L00417/latest/text
[ato-reg]: https://www.ato.gov.au/businesses-and-organisations/gst-excise-and-indirect-taxes/gst/registering-for-gst
[ato-noreg]: https://www.ato.gov.au/Business/GST/When-to-charge-GST-(and-when-not-to)/
[ato-invoice]: https://www.ato.gov.au/businesses-and-organisations/preparing-lodging-and-paying/record-keeping-for-business/setting-up-and-managing-records/setting-up-your-business-invoices
[ato-aubiz]: https://www.ato.gov.au/businesses-and-organisations/gst-excise-and-indirect-taxes/gst/in-detail/rules-for-specific-transactions/international-transactions/australian-business-importing-goods-and-services
[ato-consumers]: https://www.ato.gov.au/businesses-and-organisations/gst-excise-and-indirect-taxes/gst/in-detail/rules-for-specific-transactions/international-transactions/australian-consumers-importing-goods-and-services
[ato-lcr]: https://www.ato.gov.au/law/view/document?DocID=COG%2FLCR20181%2FNAT%2FATO%2F00001
[ato-how]: https://www.ato.gov.au/businesses-and-organisations/international-tax-for-business/gst-for-non-resident-businesses/how-australian-gst-works
[ato-simple]: https://ato.gov.au/businesses-and-organisations/international-tax-for-business/non-resident-businesses-and-gst/simplified-gst-registration
[abf-decl]: https://www.abf.gov.au/imports/Pages/How-to-import/Import-declarations.aspx
[ato-inbiz]: https://www.ato.gov.au/businesses-and-organisations/starting-registering-or-closing-a-business/starting-your-own-business/are-you-in-business
[ato-bizinc]: https://www.ato.gov.au/individuals-and-families/income-deductions-offsets-and-records/income-you-must-declare/business-partnership-and-trust-income
[cca]: https://www.legislation.gov.au/C2004A00109/latest/text
[accc-online]: https://www.accc.gov.au/consumers/buying-products-and-services/buying-online
[r10]: https://github.com/Kintukka2/FOSLN-WATCH/blob/research/au-uk-ca-markets/docs/research/au-uk-ca-markets.md#returns
[accc-airbnb]: https://www.accc.gov.au/media-release/airbnb-to-pay-15m-in-penalties-and-up-to-15m-compensation-for-misleading-consumers
[accc-airbnb-cs]: https://www.accc.gov.au/system/files/ACCC%20v%20Airbnb%20Ireland%20UC%20and%20Airbnb,%20Inc_Concise%20Statement.pdf
[accc-airbnb-und]: https://www.accc.gov.au/public-registers/undertakings-registers/airbnb-ireland-uc
[fca-airbnb]: https://www.judgments.fedcourt.gov.au/judgments/Judgments/fca/single/2023/2023fca1633
[accc-fx]: https://www.accc.gov.au/consumers/specific-products-and-activities/foreign-currency-and-money-exchange
[accc-itf]: https://www.accc.gov.au/system/files/guidance-on-the-disclosure-of-international-transaction-fees_0.pdf
[gst-fx]: https://www.legislation.gov.au/F2018L00724/latest/text
[ato-gstr20012]: https://www.ato.gov.au/law/view/document?DocID=GST%2FGSTR20012%2FNAT%2FATO%2F00001
[ato-fsqa]: https://www.ato.gov.au/law/view/print?DocID=GIR%2FFinancial-services-ch5&PiT=99991231235958
[shopify-localcur]: https://help.shopify.com/en/manual/markets/customizations/local-currencies
[shopify-markets]: https://help.shopify.com/en/manual/markets/managing-markets
[sp-payouts]: https://help.shopify.com/en/manual/payments/shopify-payments/supported-countries/australia/payouts
[shopify-payoutcur]: https://help.shopify.com/en/manual/payments/shopify-payments/store-currency/supported-payout-currencies
[shopify-mcp]: https://help.shopify.com/en/manual/payments/shopify-payments/store-currency/payouts-in-multiple-currencies
[cba-cc]: https://www.commbank.com.au/credit-cards/card-fees-charges.html
[cba-debit]: https://www.commbank.com.au/banking/debit-cards.html
[wbc-cc]: https://www.westpac.com.au/personal-banking/credit-cards/manage/fees-interest/minimising-fees/
[wbc-debit]: https://www.westpac.com.au/content/dam/public/wbc/documents/pdf/pb/Debit_MasterCard_TandC.pdf
[nab-fees]: https://www.nab.com.au/content/dam/nab/documents/guides/banking/personal-banking-fees-and-charges-guide.pdf
[anz-cc]: https://www.anz.com.au/content/dam/anzcomau/documents/pdf/credit-card-fees-charges.pdf
[anz-acct]: https://www.anz.com.au/content/dam/anzcomau/documents/pdf/personal-account-fees-charges.pdf
[fx]: https://www.currency-converter.org.uk/currency-rates/NSA/USD-rate.html
