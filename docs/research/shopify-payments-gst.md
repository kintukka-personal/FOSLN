# Shopify Payments, payment methods and GST for a USD store

Research for [Decide the Shopify Payments and GST setup](https://github.com/Kintukka2/FOSLN-WATCH/issues/42), on the map [fosln.com v1: build-ready site spec](https://github.com/Kintukka2/FOSLN-WATCH/issues/4). Done on 12 October 2026. It covers six questions: switching from Individual to Sole trader on Shopify Payments, which payment methods work with a USD charge in the Australia Market, how Shopify handles GST with a USD store currency, the ATO rules that apply, GST charged by AliExpress, and payment capture. It builds on [Charging USD from an Australian Shopify store](https://github.com/Kintukka2/FOSLN-WATCH/blob/research/shopify-usd-currency/docs/research/shopify-usd-currency.md) and [Selling in Australia before registering a business](https://github.com/Kintukka2/FOSLN-WATCH/blob/research/selling-au-unregistered/docs/research/selling-au-unregistered.md), and doesn't repeat them.

The setup assumed throughout is the one decided in [ADR 0005](../adr/0005-usd-store-currency-on-basic.md) and [ADR 0006](../adr/0006-trade-as-fosln-before-registering-the-business-name.md): Basic plan, USD store currency, AUD payouts, one Market (Australia) charged in USD, an Individual on Shopify Payments who becomes a Sole trader in the first month with more than 5 orders, not registered for GST until A$75,000. This note reports what the sources say. Where I draw my own conclusion, it's marked **Inference**. It is not tax or legal advice.

**A note on ATO sources.** ato.gov.au refused every direct request from this environment (HTTP 403), and so did the Wayback Machine. ATO pages below were read through search-engine extracts and are marked "(search extract)". Their exact wording should be checked in a browser. The GST Act, the Commissioner's determinations and the Competition and Consumer Act were read in full from legislation.gov.au.

## Short answer

- **1. Individual to Sole trader.** Shopify treats it as a new business entity, not an edit. Shopify Payments must be **deactivated first**, which removes cards, Apple Pay, Google Pay and Shop Pay from checkout until it's turned back on. The new entity goes through the banking partner's checks again against the Australian Business Register. Orders stay with the store. Shopify doesn't say what happens to pending payouts, payout history or the statement name. The "legal business name" lives on the business entity, and a Sole trader entity needs one that matches the ABR exactly.
- **2. Payment methods with a USD charge.**
  - **Work:** Shopify Payments cards (including Amex), Shop Pay, Apple Pay and Google Pay. None adds a fee beyond the card rate and the 2% conversion fee.
  - **PayPal Express works,** but costs more. PayPal charges 2.90% + US$0.30, then 3.0% to turn the USD into AUD. That's about US$8.95 (6.0%) on ACIER, against about 3.8% on Shopify Payments.
  - **Don't work:** Afterpay and Zip. Both need the store currency to match their Australian region, which means AUD. Klarna through Shopify Payments is Europe and UK only. Shop Pay Installments is US, Canada and UK only.
  - **Unresolved:** whether PayPal Australia gives a business account to an Individual with no ABN.
- **3. GST in Shopify.** Shopify only collects GST once a tax registration is entered under Settings > Taxes and duties. With a USD store currency, the tax lines, the Taxes report and all other reports are in USD. Payouts are in AUD, but they don't split out GST on sales. The only AUD GST figure Shopify gives is the GST included in its own fees. So FOSLN must convert GST to AUD itself. Before registration, turning on "include tax in prices" should change nothing at checkout, because the tax collected is nil (**Inference**, to be tested).
- **4. ATO rules.**
  - **(a) Conversion.** Use the RBA rate, a rate published by a foreign exchange organisation, or (for FOSLN's low-value goods sales) a rate under the low-value goods determination. Use it consistently. On a cash basis, the conversion day can be the day payment is received. Shopify's own capture rate doesn't clearly fit any of these. **Inference:** use the RBA rate and keep Shopify's AUD figures for reconciliation only.
  - **(b) Before registration.** Don't say prices include GST, don't call documents "tax invoice", and don't show a GST line.
  - **(c) The A$75,000 test.** It uses the past 12 months or the next 12 months, and you must apply within 21 days of crossing it. It is measured in AUD.
  - **(d) Voluntary registration.** You generally stay registered for at least 12 months. BAS can be quarterly (the default), monthly, or **annual** while turnover is under A$75,000. Simpler BAS applies.
- **5. AliExpress and GST.** I found no AliExpress page that says a buyer can enter an ABN. Under the GST Act, once FOSLN is registered and buys for its business, it isn't a "consumer", so AliExpress shouldn't charge GST if FOSLN gives its ABN and says it's registered. If AliExpress charges it anyway, the ATO says to ask the supplier for a refund (search extract). By my reading of s 11-5, no input tax credit arises, because the supply to FOSLN isn't a taxable supply.
- **6. Capture.** There are three options: automatic at checkout (the default), automatic when the whole order is fulfilled, or manual. A Shopify Payments card authorisation lasts **7 days**. Fees are charged "when you capture". Shopify's POS page says "Transaction fees don't apply to canceled orders". So a voided authorisation should cost nothing (**Inference** for online orders). With manual capture, the USD-to-AUD rate is the rate at capture, not at authorisation.

---

## 1. Individual to Sole trader on Shopify Payments (Australia)

### What Shopify says

- **It's a new entity, not an edit.** "You can't change the business type, country, or the person associated with individual or sole proprietor entities. These changes create a new business entity, which results in multiple tax forms and might require additional verification." The fix is to "add a new business instead, and then set the new business entity on any stores that you want to update" ([Business entities][be]).
- **Where.** Settings > General > Business details > ⋯ > Add entity. For a registered business, enter the "Registered business name", the address, the people who own or control it, and a proof-of-business document ([Business entities][be]).
- **Shopify Payments must be off first.** "If your store uses financial services such as Shopify Payments, Shopify Balance, Shopify Capital, or Shopify Credit, then you need to deactivate all financial services before you can switch business entities in your Shopify admin. Only the store owner can deactivate Shopify Payments." The switch itself is Settings > General > Business details > ⋯ > Switch ([Business entities][be]).
- **A conflicting page.** Shopify's Australian requirements FAQ says the owner can update details "including business type" by editing the entity. "Any changes you make here are automatically sent to our banking partners for review, and the new information is re-run through their verification process. It's possible the changes automatically re-verify, but you may also be asked for supporting documents" ([AU requirements][au-req]). The general Shopify Payments page says changing "the legal entity name, identifying tax ID, or business type, will result in a new legal entity being created. If you need assistance with this or have questions, contact Shopify Support" ([Configuring Shopify Payments][sp-config]).
- **What deactivating does** ([Configuring Shopify Payments][sp-config]):
  - "All payment methods provided by Shopify Payments, including cards, wallets, and Shop Pay, are removed from your online store."
  - "Any fixed prices are deleted from your markets, and you can't support currencies other than your store currency."
  - "After deactivating, you can reactivate Shopify Payments or activate a third-party payments provider."
  - The path is Settings > Payments > Shopify Payments > Manage > More actions > Manage payment methods > Deactivate Shopify Payments.
- **What a Sole trader must give** ([AU requirements][au-req]):
  - the owner's legal name, date of birth, job title, email, phone and Australian residential address;
  - "Registered business name: The legal business name as shown in the Australian business register";
  - the ABN, a business phone, and the "official registered address of the business as shown in the Australian business register".
  - "Our banking partners verify your information by comparing it against the business registry in Australia. Ensure that you use the correct letter case, punctuation, and symbols." If that fails, a business document is needed. The accepted documents are an ABN confirmation, an ABR extract, an ASIC record of registration, or a certificate of change of name.
- **Bank account changes pause payouts.** Changing the payout bank account puts "an automatic 3-5 business day security hold" on new payouts. Payouts already scheduled go to the old account ([AU requirements][au-req]). If a payout bank account is added "more than 24 hours after completing your Shopify Payments setup", payouts are paused for 3–5 business days ([Configuring Shopify Payments][sp-config]).
- **Payout holds.** Shopify's account-holds page lists verification as a reason: the account is held "until your details can be verified". It doesn't list a change of entity or deactivation as a reason ([Account holds][holds], read via WebFetch).
- **Archiving the old entity.** "You can't archive a business entity that's currently in use by stores, or when there's a pending payout" ([Business entities][be]).
- **Statement descriptor.** The customer statement name must be 2 to 19 characters and "must include your shop name, legal entity name, store name ("Doing Business As" name), or URL". If you don't set one, Shopify sets a compliant one. The payout statement name on your own bank statement defaults to "Shopify" ([Configuring Shopify Payments][sp-config]).
- **Store name and Payments name.** "If your store uses Shopify Payments, then you also need to contact Shopify Support to change the business name on your Shopify Payments account." The trade name on Shopify Payments comes from the store name in Settings > General > Store contact details ([Business settings][setup]; [Configuring Shopify Payments][sp-config]).
- **Legal business name.** "Your legal business name is a separate setting in the Business details section" ([Business settings][setup]). For a registered business, "This name must match the business name on any legal documentation for your business" ([Business entities][be]).

### What isn't documented

Shopify's help pages don't say whether payouts pause while Payments is off, whether pending payouts still arrive, whether the old payout history stays visible, or whether the statement name carries over. The only evidence is from forum users:

- A Shopify staff answer on the Community forum says pending payouts are still paid on schedule after a store closes, because Shopify Payments runs separately from the subscription ([Community thread][c-payout]). *Forum post, not documentation.*
- One merchant who changed from Individual to an LLC (US context) says Shopify Payments was unusable for about a week, with the account set up as the wrong entity type ([Community thread, Aug 2026][c-week]). *Forum post.*
- A UK merchant was told to "Contact Support" to change business type, and others report long waits ([Community thread][c-type]). *Forum post.*

### Inference

- **Orders stay.** Orders belong to the store, not the entity, and an entity "can be assigned to multiple stores" ([Business entities][be]). Switching the entity shouldn't touch order history. No page says this directly.
- **Expect a checkout gap.** Between deactivating and the new entity passing verification, checkout has no cards, wallets or Shop Pay. PayPal Express, if connected, would be the only way to pay. Do the switch at a quiet time, and have the ABN, ABR extract and business-name certificate ready so verification passes first time.
- **Legal business name: ask Shopify which ABR name to use.** For a sole trader, the ABR's "entity name" is the owner's own name, and "FOSLN" is a registered business name ([ABR, Business names FAQ][abr-names], search extract). Shopify's AU page asks for the "Registered business name … as shown in the Australian business register", which could mean either. Since the banking partner checks it against the ABR, confirm with Shopify Support before submitting. Either way the field can't stay blank once FOSLN is a Sole trader. ADR 0006's line "Shopify's legal business name stays blank" holds only until the switch.
- **Check the statement name after reactivating.** "FOSLN" fits the 2–19 character rule as the shop name.

---

## 2. Payment methods with a USD charge in the Australia Market

The earlier note found that Shopify Payments cards, Shop Pay, Apple Pay, Google Pay and PayPal Express support charging in the Market's currency. FOSLN's store currency and Market currency are both USD, so Shopify doesn't convert anything at checkout. The 2% fee comes out at payout. Card rates on Basic are 1.7% + 30¢ domestic, 2.9% + 30¢ Amex and 3.5% + 30¢ international, all including GST ([earlier note, section 2][prev]; [AU tax reporting][au-tax]).

| Method | Works with USD in the AU Market? | Cost to FOSLN (Basic) | How it's turned on | Source |
|---|---|---|---|---|
| **Cards** (Visa, Mastercard, **Amex**, UnionPay) | Yes | Card rate + 2% conversion | Automatic with Shopify Payments. Card brands can be chosen under Settings > Payments > Shopify Payments > Manage > Cards | [AU payment methods][au-pm]; [Configuring][sp-config] |
| **Shop Pay** | Yes | Same as cards | "Accelerated checkouts are automatically activated with Shopify Payments" | [AU payment methods][au-pm]; [Shop Pay][shoppay] |
| **Apple Pay** | Yes | "You don't pay any extra fees" beyond the card rate | Automatic with Shopify Payments. Needs at least one card type active | [Apple Pay][apple] |
| **Google Pay** | Yes | "You don't pay any extra fees" beyond the card rate | Automatic with Shopify Payments | [Google Pay][gpay] |
| **PayPal Express** | Yes, by my reading. See below | 2.90% + US$0.30, then 3.0% to convert USD to AUD. No Shopify third-party fee | Settings > Payments > PayPal. Needs a PayPal business account | [PayPal AU fees][pp-fees]; [Third-party fees][tptf] |
| **Afterpay** | **No** | Afterpay's merchant fee + Shopify's 2% third-party fee | Afterpay payment app, linked to an Afterpay Business Hub account | [Afterpay dev guide][ap-start] |
| **Zip** | **No** (search extract) | Zip's fee + Shopify's 2% third-party fee | Zip payment app | [Zip merchant help][zip] |
| **Klarna** | **No** | n/a | Klarna for Shopify Payments needs Shopify Payments in an EU country, Norway, Switzerland or the UK | [Klarna][klarna] |
| **Shop Pay Installments** | **No** | n/a | US (USD), Canada (CAD) and UK (GBP) only | [Shop Pay Installments][spi] |

### Cards and Amex

- Shopify Payments Australia accepts "Visa, Mastercard, American Express, UnionPay". "When you activate Shopify Payments, credit and debit card payments are automatically activated and no additional setup is required" ([AU payment methods][au-pm]). No separate Amex sign-up is mentioned.
- Australia's only Shopify Payments local methods are for European customers (Bancontact, BLIK, iDEAL | Wero, MB WAY, Przelewy24, Satispay). No Australian buy-now-pay-later method is listed ([AU payment methods][au-pm]).

### PayPal

- **Shopify side.** PayPal is exempt from Shopify's third-party fee when Shopify Payments is the main provider ([Third-party fees][tptf]). When an order isn't in the PayPal account's own currency, PayPal may ask you to accept each payment by hand: "By default, for each multi-currency order paid through PayPal, you need to review and accept the payment manually." You can set PayPal to "Accept all orders automatically" ([PayPal multiple currencies][pp-mc]).
- **Fees** ([PayPal AU merchant fees, updated 23 July 2026][pp-fees]):
  - The domestic rate applies "when both the sender and receiver are registered with or identified by PayPal as residents of the same market". That's **2.90% + fixed fee**, and the fixed fee for US dollars received is **0.30 USD**.
  - International payments add 1.00 percentage point.
  - Converting currency costs 3.0%. The 4.0% rate applies to things like a buyer "paying for goods or services in a currency other than the currency the goods or services are listed in", which is a buyer-side cost.
- **Getting the money to an Australian bank.** PayPal "may allow you to hold a balance in currencies other than Australian dollars". You "may only withdraw it in Australian dollars, or US dollars (only when you withdraw the US dollars into your US bank account …)". Otherwise "it will be converted for you at the time of your withdrawal" at PayPal's rate including its fee ([PayPal AU user agreement, 5 Oct 2026][pp-ua]).
- **Cost on ACIER (my arithmetic).** 2.90% × US$149 + US$0.30 = US$4.62. Then 3.0% of the remaining US$144.38 = US$4.33. Total ≈ **US$8.95 (6.0%)**, about A$12.83 at US$1 = A$1.4332. Shopify Payments costs about A$8.20 on the same sale ([earlier note][prev]).
- **USD presentment.** **Inference:** PayPal Express is compatible. Shopify lists it among the methods that support local-currency payment ([earlier note, section 3][prev]). Here the Market currency is also the store currency, so Shopify just sends PayPal a USD order. Set the PayPal account to accept USD automatically.
- **Individual with no ABN: unresolved.**
  - PayPal AU's business account FAQ asks for "your full legal name, email address, a password, your business registration number, business description, and your business bank account details" ([PayPal AU, open a business account][pp-open]). It doesn't say whether the number can be left out.
  - The user agreement says a business account must be used "primarily for a business or commercial purpose", and that applying for one is applying for commercial credit ([PayPal AU user agreement][pp-ua]).
  - Once verified, "you will not be able to change the ABN/ACN to represent a new business. For a new business, create a new PayPal business account" ([PayPal AU, change ABN][pp-abn]).
  - **Inference:** if PayPal accepts FOSLN without an ABN now, it will likely need a new business account after the Sole trader switch. Waiting until the ABN exists avoids doing it twice.

### Afterpay

- Afterpay's own Shopify guide says: "The store currency must correspond to the Afterpay account region." The allowed values are USD, AUD, NZD and CAD. "The store address must correspond to the Afterpay account region" ([Afterpay, Shopify getting started][ap-start]).
- It's added as a separate payment app ("There are two different Afterpay payment apps for Shopify—one for the United States, the other for the rest of the world") and linked to an Afterpay Business Hub merchant account ([Afterpay, add at checkout][ap-add]). The App Store listing says "Variable and fixed fee per transaction" and gives no rates ([Afterpay app listing][ap-app]).
- "Afterpay is currently unavailable on Shopify Express Checkout" ([Afterpay Shopify FAQ][ap-faq]).
- **Inference:** an Australian address with a USD store currency matches no Afterpay region. Afterpay US needs a US store address. So Afterpay won't show at FOSLN's checkout. Community threads report Afterpay disappearing when the checkout currency isn't the home currency ([Community thread][c-ap]). *Forum post.*

### Zip, Klarna, Shop Pay Installments

- **Zip.** Zip's merchant help says the default store currency must match the region of the Zip app, and the store address must too. An Australian business needs the AU gateway with AUD ([Zip merchant help][zip], search extract; the page refused direct reads). **Inference:** Zip won't work with a USD store currency.
- **Klarna.** Klarna for Shopify Payments needs Shopify Payments in one of 26 European countries or the UK, and checkout in CHF, CZK, DKK, EUR, GBP, NOK, PLN or SEK ([Klarna][klarna]). Klarna's own Shopify app "is built for enterprises with a managed Klarna account. If you are a small or medium-sized business, add Klarna through Shopify Payments" ([Klarna app listing][klarna-app]). So neither route is open to FOSLN.
- **Shop Pay Installments.** It needs "a business entity in one of the following countries: United States, with USD as the selling currency; Canada, with CAD …; United Kingdom, with GBP" ([Shop Pay Installments][spi]). A USD selling currency doesn't help, because the business entity is Australian.

---

## 3. GST in Shopify with a USD store currency

### Turning GST on

- Australia uses Shopify's **Basic Tax** service. It "requires you to input a tax registration number to calculate your taxes" ([Basic Tax][basic-tax]).
- To start collecting: Settings > Taxes and duties > Australia > Collect sales tax > enter the tax number. "If you have applied for a tax number, but don't have one yet, then leave this field blank" ([Setting up taxes][tax-setup]).

### Tax-inclusive prices

- With "Include sales tax in product price and shipping rate" on, "tax is calculated by using the formula Tax = (Tax Rate X Price) / (1 + Tax Rate). Tax is displayed as a line item for both you and your customers, even though no taxes appear to be added. The subtotal and total are the same". "The included amount of tax is displayed to customers next to the total at the checkout." "Setting up your prices to include taxes doesn't affect your tax reporting" ([Include taxes in prices][tax-incl]).
- These settings change how prices are displayed. "The way that tax is calculated and charged to customers is not" changed ([Tax collection and prices][tax-impact]).
- The store address sets the "home tax rate", which is deducted from tax-inclusive prices only when selling to *another* country ([Include taxes in prices][tax-incl]). FOSLN sells only to Australia, so no deduction happens.
- **On ACIER after registration** (my arithmetic): US$149 × 10/110 = **US$13.55 GST**. Shopify shows that in USD.

### Before any registration is entered

- Shopify only collects tax for regions set up under Collect sales tax ([Setting up taxes][tax-setup]). Each Market's duties and taxes setting also needs a registration first ([earlier note, section 3][prev]).
- **Inference:** with no registration, the tax is nil. Turning on "include tax in prices" leaves the US$149 price and total unchanged and adds no GST to the order. Whether checkout then shows a line like "Including $0.00 in taxes" isn't documented. Test it on a dev store. If such a line appears, leave the setting off until registration (see 4(b)).

### What currency the GST figures are in

- **Reports.** "Store currency … is the currency that displays in your reports" ([Exchange rates][fx-rates]). The Taxes finance report shows each tax's name, rate and "the sales taxes that were applied", with a USD example ([Finance reports][fin-rep]). With a USD store currency, GST shows in **USD** in the admin and the reports.
- **Order timeline.** "The currency conversion rate and fee for each order are included in the order's Timeline" ([Fees and costs][fees]).
- **Payouts.** They're in AUD. The payouts CSV gives Transaction Date, Type, Order, Card Brand, Card Source, Payout Status, Payout Date, Available On, **Amount, Fee, Net** ([Payout details][payout-csv]). There's no column for GST on sales.
- **Fees.** "In Australia, GST at the current rate of 10% is included in all Shopify Payments processing fees." A monthly tax invoice for fee GST is under Settings > Payments > View payouts > Documents, generated on the 5th of the next month in UTC. "The CSV file includes a GST column for Australian merchants" ([AU tax reporting][au-tax]). **Inference:** that column is GST on Shopify's fees, which is an input tax credit for FOSLN once registered. It isn't GST on sales.
- **Inference:** Shopify never gives an AUD figure for GST on sales. FOSLN has to convert each sale's GST (or each sale's price, then take 1/11) to AUD using a rate the ATO accepts (section 4(a)).

### Tax invoices and order confirmations

- Shopify Order Printer templates can show `tax_lines` with each tax's amount and title ([Order Printer templates][op-cust]). The `shop` variables include `name`, `currency` and `metafields`, but no tax-number field ([Order Printer variables][op-var]).
- **FOSLN's sales are probably not tax-invoice sales.** Under the GST Act, a supply that is an "offshore supply of low value goods", taxable "solely under section 9-5" and connected with Australia "solely because of this Subdivision", is a *supplier-taxed offshore supply of low value goods* (s 84-85(3)).
  - "You are not required to issue a tax invoice" for such a supply (s 84-87).
  - Instead, "You must give the recipient of a supply a notice of the amount of GST (if any) payable", "in the approved form, and at the time the consideration for the supply is first agreed" (s 84-89) ([GST Act][gst-act]).
  - The ATO says the notice can be an email confirmation or receipt. It must show the supplier's name and ABN, the date, what was supplied with quantity and GST-inclusive price, and the GST amount. Where every item is taxable, it can show the total price "and state that it includes GST" ([LCR 2018/1][lcr], search extract).
- **Inference:** the earlier note found the low-value goods rules apply to an Australian-resident seller whose goods ship from overseas ([earlier note, section 3][prev-unreg]). If so, once registered, FOSLN's **order confirmation email** is the document that matters. It must carry the ABN and the GST included, so edit the template to hard-code the ABN or read it from a shop metafield. Confirm this classification with a tax adviser, because it changes which document FOSLN must issue.

---

## 4. ATO rules

### (a) Converting a USD taxable supply to AUD

- **The Act.** "The value of a taxable supply is to be expressed in Australian currency." An amount in another currency "is to be treated as if it were an amount of Australian currency worked out in the manner determined by the Commissioner" ([GST Act s 9-85][gst-act]).
- **The determination** ([Foreign Currency Conversion Determination 2018][fx-det], in force from 1 July 2018):
  - Formula: amount in foreign currency × 1 / "your particular exchange rate on the conversion day". "You must use your particular exchange rate consistently."
  - "Your particular exchange rate" is one of:
    - (a) the RBA rate (the 4:00 pm AEST rate that day, or the previous RBA business day);
    - (b) "a rate published by a foreign exchange organisation", meaning "an organisation that provides exchange rates publicly";
    - (c) an agreed rate, which needs "a registered recipient", so it doesn't fit sales to consumers;
    - (d) "for taxable supplies of offshore supplies of low value goods a rate chosen under" the Customs Value of Low Value Goods Determination 2018.
  - **Conversion day:**
    - Not on a cash basis: "the earlier of (i) the day on which any of the consideration is received for the supply … or (ii) the transaction date or the invoice date".
    - On a cash basis: "(i) the transaction date; or (ii) the invoice date; or (iii) the day on which any of the consideration is received".
- **The low-value goods rate** ([Customs Value of Low Value Goods Determination 2018][lvg-det]). It allows the RBA rate or another central bank's reference rate, "an exchange rate that is consistently higher than the RBA rate published by a foreign exchange organisation", or "a mid-market rate of a foreign exchange organisation or a foreign exchange data vendor". The rate may be one "published within 7 calendar days of the conversion time", and the practice must be consistent.
- **ATO guidance.** The ATO says you should use your chosen rate consistently. You may change it "for sound commercial reasons", but switching rates to reduce GST is inconsistent use and Division 165 may apply. A foreign currency gain or loss is not an adjustment event ([GSTR 2001/2 and the determination's explanatory statement][gstr], search extract).
- **Cash or accruals.** You may choose to account on a cash basis if you are a "small business entity" (aggregated turnover under A$10 million), or on other grounds ([GST Act s 29-40][gst-act]). On a cash basis, GST is reported in the period the payment is received.
- **Is Shopify's AUD figure acceptable?**
  - **Inference:** probably not as it stands. Shopify uses "the rate at the time of the transaction" ([Exchange rates][fx-rates]), shows it on each order's timeline, and doesn't name its rate source. It doesn't clearly count as a rate "published" by an organisation that "provides exchange rates publicly".
  - Shopify's rate isn't the RBA rate, and it isn't shown to be a mid-market rate.
  - The safe course is the RBA rate on the day of capture, which counts as receipt on a cash basis, used for every sale. Keep Shopify's AUD figures to reconcile payouts. Under (d), a weekly RBA or mid-market rate may also be allowed for low-value goods sales, if used consistently.
  - Ask the accountant to confirm this, and pick one method before the first BAS.

### (b) Before registration: "includes GST", "tax invoice", GST lines

- **No GST exists to include.** A sale is a taxable supply only if, among other things, "you are registered, or required to be registered" ([GST Act s 9-5][gst-act]; [earlier note, section 3][prev-unreg]).
- **ATO on invoices.** "If you're not registered for GST, your invoices should not include the words 'tax invoice' – you must issue standard invoices" ([ATO, Setting up your business invoices][ato-inv], search extract). business.gov.au says an unregistered business doesn't include GST in its prices, and its invoices "need to show that GST was not included" ([business.gov.au, Register for GST][bga-gst], search extract).
- **A narrow exception.** GSTR 2013/1 is reported to say a supplier may issue a document headed "tax invoice" for a supply that isn't taxable if it shows the price doesn't include GST ([GSTR 2013/1][gstr2013], search extract). That doesn't help a store saying its prices *include* GST.
- **The Australian Consumer Law.** A person must not "make a false or misleading representation with respect to the price of goods or services" ([ACL s 29(1)(i)][cca]).
- **Inference:** before registration, don't say "incl. GST", don't label anything "tax invoice", and don't show a GST line, even one of $0.00. Saying a price includes GST when none is payable risks being a misleading statement about price.

### (c) The A$75,000 test

The earlier note already covers the current and projected tests (ss 188-10 to 188-20), the A$75,000 figure in the regulations, and the 21-day rule (s 25-1) ([earlier note, section 3][prev-unreg]). New points:

- **The Commissioner can set aside the current test.** Current turnover at or above A$75,000 counts only if "the Commissioner is not satisfied that your projected GST turnover is below the turnover threshold". Projected turnover at or above A$75,000 always counts ([GST Act s 188-10][gst-act]).
- **It's the value of supplies, including ones not yet made.** Turnover counts supplies "that you have made, or are likely to make" ([s 188-15, 188-20][gst-act]).
- **Before registration, the value is the full price.** For a supply that isn't taxable, "the value … is taken to be 11/10 of what would be the value of the supply if it were a taxable supply" ([s 188-30][gst-act]). That's the GST-free equivalent of the whole price paid.
- **In AUD.** **Inference:** value is expressed in Australian currency (s 9-85) and s 188-30 builds on it, so USD sales are converted using a determination rate. For monitoring, the AUD "Amount" column in Shopify's payouts CSV (gross, before fees) is a close running total. Confirm the exact figure with the RBA rate when near the threshold.
- **Example (my arithmetic).** At US$149 and US$1 = A$1.4332, A$75,000 is about 351 ACIER sales in 12 months. At US$169 it's about 310 CADRE sales.

### (d) Voluntary registration

- **Allowed.** You may register if you carry on an enterprise, even below the threshold ([GST Act s 23-10][gst-act]).
- **Minimum period.**
  - After 12 months registered, the Commissioner *must* cancel if you apply and aren't required to be registered ([s 25-55][gst-act]).
  - Within 12 months, the Commissioner *may* cancel ([s 25-57][gst-act]).
  - The ATO says you generally stay registered for at least 12 months ([earlier note][prev-unreg], search extract).
- **BAS frequency.**
  - Quarterly is the general rule ([s 27-5][gst-act]). You can elect monthly ([s 27-10][gst-act]).
  - **Annual GST reporting is open to voluntary registrants.** You're eligible if "you are not required to be registered" (s 151-5). Each financial year is then one tax period (s 151-40).
  - The annual return is due with your income tax return if you lodge one, otherwise by 28 February (s 151-45).
  - The election must be made by 28 October for quarterly lodgers, or by 21 August for others (s 151-20).
  - It "ceases to have effect" if, on 31 July in a financial year, you are required to be registered (s 151-25) ([GST Act Div 151][gst-act]).
  - The ATO describes annual reporting as for businesses "voluntarily registered for GST … and your GST turnover is under $75,000" ([ATO, Annual GST reporting][ato-annual], search extract).
- **Simpler BAS.** "Simpler BAS is the default reporting method for small businesses with a GST turnover of less than $10 million". You report only G1 (total sales), 1A (GST on sales) and 1B (GST on purchases), and the same applies to the annual GST return ([ATO, GST reporting methods][ato-simpler], search extract).

---

## 5. AliExpress and GST

- **The Act.** AliExpress is an electronic distribution platform, so it is treated as the supplier of low-value goods sold through it ([GST Act s 84-81(3)][gst-act]).
  - A low-value supply is connected with Australia, and so taxable, only if "the recipient of the supply is a consumer" (s 84-75(1)).
  - A registered entity is a consumer only if it doesn't acquire the goods "solely or partly for the purpose of an enterprise" it carries on in Australia (s 84-75(2)).
  - The platform can treat a buyer as not a consumer only once "the other entity's ABN … has been disclosed" and the buyer has given "a declaration or information that indicates that the other entity is registered" (s 84-105) ([GST Act][gst-act]).
- **ATO guidance.** "You should not be charged GST on these sales if you are registered for GST. You must give the supplier your ABN and state that you are registered for GST." If charged incorrectly, "seek a refund from the supplier". The ATO also notes that some platforms "may not be able to differentiate between business and consumer purchasers at the checkout" ([ATO, GST and Australian businesses][ato-aubiz], search extract).
- **Input tax credit.**
  - A creditable acquisition needs "the supply of the thing to you is a taxable supply" ([GST Act s 11-5(b)][gst-act]).
  - The platform isn't required to issue tax invoices for supplier-taxed low-value supplies (s 84-87), and a credit above A$82.50 needs one.
  - **Inference:** once FOSLN is registered and buys for the business, AliExpress's supply to it isn't taxable at law, so the A$4 it charges isn't GST and gives no credit. The remedy is a refund from AliExpress.
  - One search summary said the ATO allows a credit in some cases, such as purchases of A$82.50 or less with no reimbursement. I couldn't confirm that wording, so don't rely on it.
- **Does AliExpress let a buyer enter an ABN? Not confirmed.**
  - I found no AliExpress help page on this. AliExpress's own "wiki" pages mention requesting a business invoice with a company name and tax ID, but they're marketing pages, not help pages ([AliExpress VAT article][ae-vat]).
  - Alibaba.com, a sister site, has a Tax Information setting that asks for "your valid ABN number not your GST number" ([Alibaba help centre][alibaba]). That's not AliExpress.
  - Forum users in 2018 described GST-registered buyers being unable to get an exemption, or being sent a refund form ([Whirlpool][wp]; [OzBargain][ozb]). *Forum posts, old.*
- **Before registration.** FOSLN is a consumer, so AliExpress charging 10% is correct, and FOSLN can't claim it back ([earlier note, section 3][prev-unreg]).

---

## 6. Payment capture on Shopify Payments (Australia)

- **Options** ([Payment authorization and capture][capture]):
  - **Automatically at checkout.** This is the default: "Payment is captured at the time of sale".
  - **Automatically when order is fulfilled.** "Payment is Authorized at checkout and is automatically captured when the entire order is fulfilled before the payment authorization period expires." Partly fulfilled orders aren't captured. It isn't compatible with fulfilment apps that need a paid order before they start.
  - **Automatically capture per fulfillment.** This is Shopify Plus only.
  - **Manually.** You capture each Authorized order within the authorisation period.
  - A warning email can be sent one day before an authorisation expires.
- **How long an authorisation lasts.** "Shopify Payments provides an authorization period of 7 days." Longer periods (up to 30 days for Visa, Mastercard and Amex) are Plus only, and capturing after 7 days on Plus costs an extra 1.75% ([Payment authorization and capture][capture]; [Capturing payments][capturing]).
- **Exchange rate.** "When you capture a payment made in a foreign currency, the amount is converted using the current conversion rate and not the rate at the time of authorization" ([Payment authorization and capture][capture]). "If you capture payments manually, then your customer's credit card is charged when you process their order, and not when the customer submits the order" ([Exchange rates][fx-rates]).
- **Fees.**
  - "When you capture the payment for an order, you're charged a Shopify Payments fee (in your payout currency)". The currency conversion fee is charged "to convert currencies when you capture a payment" ([Fees and costs][fees]).
  - Cancelling an uncaptured order: "If the payment wasn't captured, then the payment status changes to Voided" ([Canceling orders][cancel]).
  - For in-person sales, "Transaction fees don't apply to canceled orders" ([POS card payments][pos]).
  - **Inference:** an online authorisation that is voided rather than captured incurs neither the card fee nor the 2% conversion fee, because both are charged on capture. No online help page says so in as many words. Fees on a captured then refunded order are not returned.
- **Customer side.** The authorisation hold "might display on the customer's bank statement up to 30 days after the original authorization, even if the payment was captured or the order was canceled" ([Payment authorization and capture][capture]).
- **Other methods.** Afterpay's authorisation lasts 13 days, and its instalments start at authorisation ([Afterpay manual capture][ap-cap]). Afterpay isn't usable here anyway (section 2).
- **Fit with FOSLN (Inference).** FOSLN places Supplier orders once a day. "Capture when fulfilled" or manual capture would let it void an order the Supplier can't fill, at no fee, inside 7 days. Two catches: the Supplier's Dispatch often comes days after the order, and Shopify marks an order fulfilled when tracking is added. So capture-at-fulfilment works only if tracking reaches Shopify within 7 days. Otherwise, capture manually when the Supplier accepts the order. Automatic capture at checkout is simplest, and a refund then costs the card fee and the 2%, about A$8.20 on ACIER.

---

## Open or unverified

Items closed from the earlier note:

- **"What currency Shopify's GST figure is in."** Closed: it's USD, the store currency, everywhere in the admin and reports. Payouts are AUD but carry no GST on sales (section 3).
- **"Afterpay, Zip, Klarna and PayPal in a USD-priced Australia market."** Closed: Afterpay and Zip don't work, Klarna and Shop Pay Installments aren't available, and PayPal works (section 2).

Still open:

- **What happens to pending payouts, payout history and the statement name** during the Individual-to-Sole-trader switch. Shopify's help pages are silent, and the evidence is forum posts only. Ask Shopify Support before switching.
- **Which ABR name goes in Shopify's "Registered business name" for a sole trader**: the owner's name (the ABR entity name) or "FOSLN" (the registered business name).
- **The conflict between Shopify pages.** One says business type can be edited. Others say it can't be changed and needs a new entity with Payments deactivated.
- **Whether PayPal Australia opens a business account for an Individual with no ABN.**
- **Whether checkout shows a $0.00 tax line** when "include tax in prices" is on but no registration is entered. Test on a dev store.
- **Whether FOSLN's sales are "supplier-taxed offshore supplies of low value goods"**, so that the ABN-and-GST order confirmation, not a tax invoice, is the required document. This is my reading of ss 84-77 and 84-85. Confirm with a tax adviser.
- **Whether Shopify's capture rate counts as "a rate published by a foreign exchange organisation".** My reading is no. The accountant should confirm the conversion method.
- **Whether AliExpress lets a buyer record an ABN and GST status**, and how it handles refunds now.
- **The ATO's exact wording** on credits for GST wrongly charged on low-value imports. ATO pages couldn't be read directly from here.
- **Exact ATO wording generally.** Every ATO statement here comes from search extracts. Check them in a browser.
- **Afterpay's and Zip's own help pages** refused direct reads. The Afterpay finding rests on its developer guide, and the Zip finding rests on a search extract.
- **Whether no fee applies to a voided online authorisation.** It's inferred from "fees are charged on capture" and the POS page, not stated for online orders.

---

## Sources

All pages were read on 12 October 2026, by direct download unless marked. "Search extract" means the page refused direct reads and was read through search-engine summaries.

[prev]: https://github.com/Kintukka2/FOSLN-WATCH/blob/research/shopify-usd-currency/docs/research/shopify-usd-currency.md
[prev-unreg]: https://github.com/Kintukka2/FOSLN-WATCH/blob/research/selling-au-unregistered/docs/research/selling-au-unregistered.md
[be]: https://help.shopify.com/en/manual/your-account/business-entities
[au-req]: https://help.shopify.com/en/manual/payments/shopify-payments/supported-countries/australia/requirements
[au-pm]: https://help.shopify.com/en/manual/payments/shopify-payments/supported-countries/australia/payment-methods
[au-tax]: https://help.shopify.com/en/manual/payments/shopify-payments/supported-countries/australia/tax-reporting
[sp-config]: https://help.shopify.com/en/manual/payments/shopify-payments/configuring-shopify-payments
[holds]: https://help.shopify.com/en/manual/payments/shopify-payments/payouts/account-holds
[setup]: https://help.shopify.com/en/manual/intro-to-shopify/initial-setup/setup-business-settings
[payout-csv]: https://help.shopify.com/en/manual/payments/shopify-payments/getting-paid-with-shopify-payments/view-payouts/view-details
[shoppay]: https://help.shopify.com/en/manual/payments/shop-pay
[apple]: https://help.shopify.com/en/manual/payments/accelerated-checkouts/apple-pay
[gpay]: https://help.shopify.com/en/manual/payments/accelerated-checkouts/google-pay
[klarna]: https://help.shopify.com/en/manual/payments/shopify-payments/local-payment-methods/klarna
[spi]: https://help.shopify.com/en/manual/payments/shop-pay-installments
[pp-mc]: https://help.shopify.com/en/manual/payments/paypal/multicurrency
[tptf]: https://help.shopify.com/en/manual/your-account/manage-billing/billing-charges/types-of-charges/third-party-charges/third-party-transaction-fees
[fees]: https://help.shopify.com/en/manual/international/pricing/fees
[fx-rates]: https://help.shopify.com/en/manual/international/pricing/exchange-rates
[basic-tax]: https://help.shopify.com/en/manual/taxes/registration
[tax-setup]: https://help.shopify.com/en/manual/taxes/registration/setup
[tax-incl]: https://help.shopify.com/en/manual/taxes/include-exclude-taxes
[tax-impact]: https://help.shopify.com/en/manual/taxes/tax-collection-impact-on-pricing
[fin-rep]: https://help.shopify.com/en/manual/reports-and-analytics/shopify-reports/report-types/default-reports/finances-report/taxes-report
[op-cust]: https://help.shopify.com/en/manual/fulfillment/managing-orders/printing-orders/shopify-order-printer/customizing-order-templates
[op-var]: https://help.shopify.com/en/manual/fulfillment/managing-orders/printing-orders/shopify-order-printer/liquid-variables-and-filters-reference
[capture]: https://help.shopify.com/en/manual/payments/payment-authorization
[capturing]: https://help.shopify.com/en/manual/fulfillment/managing-orders/payments/capturing-payments
[cancel]: https://help.shopify.com/en/manual/fulfillment/managing-orders/canceling-orders
[pos]: https://help.shopify.com/en/manual/sell-in-person/shopify-pos/payment-management/card-payments
[pp-fees]: https://www.paypal.com/au/business/paypal-business-fees
[pp-ua]: https://www.paypal.com/au/legalhub/paypal/useragreement-full
[pp-open]: https://www.paypal.com/au/business/open-business-account
[pp-abn]: https://www.paypal.com/au/smarthelp/article/how-do-i-change-the-name-of-my-business-faq705
[ap-start]: https://developers.afterpay.com/afterpay-online-developer/guides/platforms/shopify/getting-started
[ap-add]: https://developers.afterpay.com/afterpay-online-developer/guides/platforms/shopify/add-afterpay-at-checkout
[ap-faq]: https://developers.afterpay.com/afterpay-online-developer/guides/platforms/shopify/faq
[ap-cap]: https://developers.afterpay.com/afterpay-online-developer/guides/platforms/shopify/shopify-manual-capture
[ap-app]: https://apps.shopify.com/afterpay-payments
[klarna-app]: https://apps.shopify.com/klarna
[zip]: https://merchant-help.zip.co/hc/en-au/articles/13872038184847-I-can-t-install-the-Zip-app-on-my-Shopify-store
[gst-act]: https://www.legislation.gov.au/C2004A00446/latest/text
[fx-det]: https://www.legislation.gov.au/F2018L00724/latest/text
[lvg-det]: https://www.legislation.gov.au/F2018L00725/asmade/text
[cca]: https://www.legislation.gov.au/C2004A00109/latest/text
[gstr]: https://www.ato.gov.au/law/view/document?DocID=GST%2FGSTR20012%2FNAT%2FATO%2F00001
[gstr2013]: https://www.ato.gov.au/law/view/document?docid=GST%2FGSTR20131%2FNAT%2FATO%2F00001
[lcr]: https://www.ato.gov.au/law/view/document?DocID=COG%2FLCR20181%2FNAT%2FATO%2F00001
[ato-inv]: https://www.ato.gov.au/businesses-and-organisations/preparing-lodging-and-paying/record-keeping-for-business/setting-up-and-managing-records/setting-up-your-business-invoices
[ato-aubiz]: https://www.ato.gov.au/businesses-and-organisations/gst-excise-and-indirect-taxes/gst/in-detail/rules-for-specific-transactions/international-transactions/australian-business-importing-goods-and-services
[ato-annual]: https://www.ato.gov.au/businesses-and-organisations/gst-excise-and-indirect-taxes/gst/lodging-your-bas-or-annual-gst-return/options-for-reporting-and-paying-gst/annual-gst-reporting
[ato-simpler]: https://www.ato.gov.au/businesses-and-organisations/preparing-lodging-and-paying/business-activity-statements-bas/goods-and-services-tax-gst/gst-reporting-methods
[bga-gst]: https://business.gov.au/registrations/register-for-taxes/register-for-goods-and-services-tax-gst
[abr-names]: https://abr.business.gov.au/faq/names
[ae-vat]: https://www.aliexpress.com/s/wiki-ssr/article/vat-fees
[alibaba]: https://helpcenter.alibaba.com/s/buyer/knowledge?questionId=91b4aa4b74754ceab181dc024ab71fcd&categoryId=1000137545&language=en
[c-payout]: https://community.shopify.com/t/will-outstanding-payouts-be-received-after-deactivating-my-store/87504
[c-week]: https://community.shopify.com/t/shopify-payments-disabled-for-a-week-after-entity-change-need-escalation-help/665713
[c-type]: https://community.shopify.com/t/changing-business-type-issues-with-shopify/368381/1
[c-ap]: https://community.shopify.com/t/afterpay-not-available-in-all-countries-due-to-shopify-markets/214522/12
[wp]: https://forums.whirlpool.net.au/archive/96yzn82q
[ozb]: https://www.ozbargain.com.au/node/402863

**Shopify Help Center: Shopify Payments and payment methods**
- *Managing business entities*. https://help.shopify.com/en/manual/your-account/business-entities
- *Shopify Payments Australia requirements*. https://help.shopify.com/en/manual/payments/shopify-payments/supported-countries/australia/requirements
- *Payment methods with Shopify Payments in Australia*. https://help.shopify.com/en/manual/payments/shopify-payments/supported-countries/australia/payment-methods
- *Tax reporting with Shopify Payments for Australia*. https://help.shopify.com/en/manual/payments/shopify-payments/supported-countries/australia/tax-reporting
- *Configuring Shopify Payments* (including deactivation and statement names). https://help.shopify.com/en/manual/payments/shopify-payments/configuring-shopify-payments
- *Account holds* (read via WebFetch). https://help.shopify.com/en/manual/payments/shopify-payments/payouts/account-holds
- *Setting up business settings*. https://help.shopify.com/en/manual/intro-to-shopify/initial-setup/setup-business-settings
- *Viewing and exporting payout details*. https://help.shopify.com/en/manual/payments/shopify-payments/getting-paid-with-shopify-payments/view-payouts/view-details
- *Shop Pay*. https://help.shopify.com/en/manual/payments/shop-pay
- *Apple Pay*. https://help.shopify.com/en/manual/payments/accelerated-checkouts/apple-pay
- *Google Pay*. https://help.shopify.com/en/manual/payments/accelerated-checkouts/google-pay
- *Klarna*. https://help.shopify.com/en/manual/payments/shopify-payments/local-payment-methods/klarna
- *Local payment methods* (read via WebFetch). https://help.shopify.com/en/manual/payments/shopify-payments/local-payment-methods
- *Shop Pay Installments*. https://help.shopify.com/en/manual/payments/shop-pay-installments
- *PayPal Express Checkout*. https://help.shopify.com/en/manual/payments/paypal
- *PayPal multiple currencies*. https://help.shopify.com/en/manual/payments/paypal/multicurrency
- *Third-party transaction fees*. https://help.shopify.com/en/manual/your-account/manage-billing/billing-charges/types-of-charges/third-party-charges/third-party-transaction-fees
- *Fees and costs*. https://help.shopify.com/en/manual/international/pricing/fees
- *Exchange rates*. https://help.shopify.com/en/manual/international/pricing/exchange-rates
- *Payment authorization and capture*. https://help.shopify.com/en/manual/payments/payment-authorization
- *Capturing payments*. https://help.shopify.com/en/manual/fulfillment/managing-orders/payments/capturing-payments
- *Canceling orders*. https://help.shopify.com/en/manual/fulfillment/managing-orders/canceling-orders
- *POS card payments*. https://help.shopify.com/en/manual/sell-in-person/shopify-pos/payment-management/card-payments

**Shopify Help Center: taxes and reports**
- *Basic Tax*. https://help.shopify.com/en/manual/taxes/registration
- *Setting up taxes*. https://help.shopify.com/en/manual/taxes/registration/setup
- *Include taxes in product prices*. https://help.shopify.com/en/manual/taxes/include-exclude-taxes
- *Understanding the impact of tax collection on displayed prices*. https://help.shopify.com/en/manual/taxes/tax-collection-impact-on-pricing
- *Tax reports*. https://help.shopify.com/en/manual/taxes/tax-reports
- *Finance reports* (Taxes report). https://help.shopify.com/en/manual/reports-and-analytics/shopify-reports/report-types/default-reports/finances-report/taxes-report
- *Order Printer: customising templates* and *variables reference*. https://help.shopify.com/en/manual/fulfillment/managing-orders/printing-orders/shopify-order-printer

**Payment providers**
- PayPal AU, *Merchant fees* (last updated 23 July 2026). https://www.paypal.com/au/business/paypal-business-fees
- PayPal AU, *User agreement* (last updated 5 October 2026). https://www.paypal.com/au/legalhub/paypal/useragreement-full
- PayPal AU, *Open a business account*. https://www.paypal.com/au/business/open-business-account
- PayPal AU, *How do I change my ABN or ACN?* https://www.paypal.com/au/smarthelp/article/how-do-i-change-the-name-of-my-business-faq705
- PayPal AU, *How do I change the type of PayPal account I have?* https://www.paypal.com/au/cshelp/article/how-do-i-change-the-type-of-paypal-account-i-have-help339
- Afterpay developer docs, Shopify: *Getting started*, *Add Afterpay at checkout*, *FAQ*, *Shopify manual capture*. https://developers.afterpay.com/afterpay-online-developer/guides/platforms/shopify
- Shopify App Store, *Afterpay*. https://apps.shopify.com/afterpay-payments
- Shopify App Store, *Klarna Payments for Enterprise*. https://apps.shopify.com/klarna
- Zip merchant help, *I can't install the Zip app on my Shopify store* (search extract). https://merchant-help.zip.co/hc/en-au/articles/13872038184847-I-can-t-install-the-Zip-app-on-my-Shopify-store

**Legislation (legislation.gov.au, read in full)**
- *A New Tax System (Goods and Services Tax) Act 1999*, compilation dated 1 January 2026: ss 9-5, 9-85, 11-5, 23-10, 25-1, 25-55, 25-57, 27-5, 27-10, 29-40, 29-70, 84-75 to 84-105, 188-10 to 188-30, Div 151. https://www.legislation.gov.au/C2004A00446/latest/text
- *Goods and Services Tax: Foreign Currency Conversion Determination 2018* (F2018L00724). https://www.legislation.gov.au/F2018L00724/latest/text
- *Goods and Services Tax: Foreign Currency (Customs Value of Low Value Goods) Determination 2018* (F2018L00725). https://www.legislation.gov.au/F2018L00725/asmade/text
- *Competition and Consumer Act 2010*, Schedule 2 (Australian Consumer Law) s 29, compilation dated 16 September 2026. https://www.legislation.gov.au/C2004A00109/latest/text

**ATO, business.gov.au and ABR (search extracts; direct reads refused)**
- ATO, *GSTR 2001/2 Goods and Services Tax: foreign exchange conversions*. https://www.ato.gov.au/law/view/document?DocID=GST%2FGSTR20012%2FNAT%2FATO%2F00001
- ATO, explanatory statement to the 2018 conversion determination. https://www.ato.gov.au/law/view/pdf/esg/esforex2018-001.pdf
- ATO, *GSTR 2013/1 Goods and services tax: tax invoices*. https://www.ato.gov.au/law/view/document?docid=GST%2FGSTR20131%2FNAT%2FATO%2F00001
- ATO, *LCR 2018/1 GST on low value imported goods*. https://www.ato.gov.au/law/view/document?DocID=COG%2FLCR20181%2FNAT%2FATO%2F00001
- ATO, *Setting up your business invoices*. https://www.ato.gov.au/businesses-and-organisations/preparing-lodging-and-paying/record-keeping-for-business/setting-up-and-managing-records/setting-up-your-business-invoices
- ATO, *GST and Australian businesses* (imported low value goods). https://www.ato.gov.au/businesses-and-organisations/gst-excise-and-indirect-taxes/gst/in-detail/rules-for-specific-transactions/international-transactions/australian-business-importing-goods-and-services
- ATO, *Annual GST reporting*. https://www.ato.gov.au/businesses-and-organisations/gst-excise-and-indirect-taxes/gst/lodging-your-bas-or-annual-gst-return/options-for-reporting-and-paying-gst/annual-gst-reporting
- ATO, *GST reporting methods* (Simpler BAS). https://www.ato.gov.au/businesses-and-organisations/preparing-lodging-and-paying/business-activity-statements-bas/goods-and-services-tax-gst/gst-reporting-methods
- business.gov.au, *Register for goods and services tax (GST)*. https://business.gov.au/registrations/register-for-taxes/register-for-goods-and-services-tax-gst
- ABR, *Business names/Trading names FAQs*. https://abr.business.gov.au/faq/names

**AliExpress and Alibaba (not authoritative)**
- AliExpress, *Understanding VAT fees on AliExpress* (marketing "wiki" page, search extract). https://www.aliexpress.com/s/wiki-ssr/article/vat-fees
- Alibaba.com help centre, *What is Australian GST?* (search extract; Alibaba.com, not AliExpress). https://helpcenter.alibaba.com/s/buyer/knowledge?questionId=91b4aa4b74754ceab181dc024ab71fcd&categoryId=1000137545&language=en

**Forums (not official; used only where marked)**
- Shopify Community, *Will outstanding payouts be received after deactivating my store?* https://community.shopify.com/t/will-outstanding-payouts-be-received-after-deactivating-my-store/87504
- Shopify Community, *Shopify Payments disabled for a week after entity change* (12 Aug 2026). https://community.shopify.com/t/shopify-payments-disabled-for-a-week-after-entity-change-need-escalation-help/665713
- Shopify Community, *Changing Business Type – Issues with Shopify*. https://community.shopify.com/t/changing-business-type-issues-with-shopify/368381/1
- Shopify Community, *Afterpay not available in all countries due to Shopify Markets*. https://community.shopify.com/t/afterpay-not-available-in-all-countries-due-to-shopify-markets/214522/12
- Whirlpool, *AliExpress and GST* (2018). https://forums.whirlpool.net.au/archive/96yzn82q
- OzBargain, *AliExpress dispute and GST process?* (2018). https://www.ozbargain.com.au/node/402863
