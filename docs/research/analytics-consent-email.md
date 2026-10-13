# Analytics, consent and email capture for an Australia-only Shopify store

Research for [What analytics, consent and email capture involve for an Australian Shopify store](https://github.com/Kintukka2/FOSLN-WATCH/issues/52). Done on 13 October 2026.

The setup assumed throughout: Shopify Basic, one Market (Australia) priced in USD, a custom Online Store 2.0 theme, apps added only when needed, and an owner who never uses a terminal. The owner sells as an Individual with no ABN yet ([ADR 0005](../adr/0005-usd-store-currency-on-basic.md), [ADR 0006](../adr/0006-trade-as-fosln-before-registering-the-business-name.md)), with turnover far under A$3 million.

This note reports what the sources say. Where I draw my own conclusion, it's marked **Inference**. It is not legal advice.

**A note on sources.** acma.gov.au returned HTTP 503 to every request from this environment. ACMA guidance below was read through search-engine extracts and is marked "(search extract)". The Spam Act itself was read in full from legislation.gov.au. The Privacy Act (compilation 104, 4 June 2026), the Privacy and Other Legislation Amendment Act 2024 and the 2026 exposure draft bill were read in full as PDFs.

## Short answer

- **Analytics.** Shopify Analytics comes with Basic and covers sales, sessions, conversion funnels, referrers, locations, devices, landing pages, site search and Core Web Vitals. GA4, Google Ads and Merchant Center conversion tracking install through Shopify's **Google & YouTube** app, and the Meta pixel through the **Facebook & Instagram** app. Both run as Shopify app pixels, managed under Settings > Customer events, with no theme code. Each vendor library is about 110 to 150 KB compressed (about 430 KB uncompressed). Shopify runs app pixels in a sandboxed web worker, off the main thread. I could not measure the real cost on a Shopify PDP.
- **Consent.** No Australian law in force by October 2026 requires a cookie banner or consent before non-sensitive analytics or ad tags load.
  - **Small-business exemption.** FOSLN is very likely exempt from the Privacy Act. A new business qualifies on its current-year turnover (s 6D(2)), and selling through Shopify doesn't change that.
  - **The one way to lose it.** A small business that "discloses personal information about another individual to anyone else for a benefit, service or advantage" is not exempt (s 6D(4)(c)), unless it has the person's consent (s 6D(7)). Meta's Enhanced and Maximum data sharing and Shopify Network Intelligence both send customer data to a third party in return for a service. No source I found says whether that counts. **Unresolved.**
  - **The 2024 amendments.** The statutory tort (in force 10 June 2025) applies to everyone, small businesses included, but needs a serious, intentional or reckless invasion. The automated-decision privacy policy rule starts 10 December 2026 and binds only APP entities.
  - **Tranche 2.** It is an exposure draft only (consultation closed 18 September 2026). It does not remove the small-business exemption. It would redefine trading to cover disclosure "for the purposes of direct marketing", which the consultation paper says can include pixels.
  - **Shopify's banner.** For an Australia-only store it is off by default. Every consent purpose is allowed, and it can be switched on for Australia.
  - **Google.** Consent Mode v2 is required for EEA, UK and Swiss users only.
- **Email capture.** The Spam Act applies to every sender, whatever its size. It requires consent (express or inferred, with the burden on the sender), sender identification that stays valid for 30 days, and a working unsubscribe honoured within 5 business days.
  - **Shopify's tools.** Shopify Messaging (formerly Shopify Email) handles unsubscribes and puts the store address in the footer. Shopify's signup tools (the theme newsletter form, Shopify Forms and the checkout checkbox) can collect express consent if the checkbox isn't preselected.
  - **Identification gap.** ACMA says to identify the sender by the business's legal name, or the person's name and ABN (search extract). Before registration "FOSLN" is neither. This conflicts with ADR 0006's "nowhere else carries the owner's name".
  - **Cost.** 10,000 emails a month are free on Basic. After that it's US$1 per 1,000. Abandoned-checkout emails are always free. The app requires Shopify Network Intelligence to be on.
- **Privacy policy.** If FOSLN is exempt, the Privacy Act doesn't require one, but the contracts do:
  - Shopify's Terms (§9.2, §9.7, §9.11): say the store is hosted by Shopify and that data goes to Shopify and to third parties overseas, link to Shopify's Consumer Privacy Policy and an opt-out, and link to the policy in every email.
  - Google Analytics Terms (§7): give notice of cookies and disclose GA and how it collects data.
  - Meta's Business Tools Terms: a notice on every page with the pixel, explaining third-party pixels and cookies and how to opt out.
  - Shopify's automated policy covers many of these, but Shopify says it must be reviewed.

---

## 1. Analytics

### What Shopify Analytics covers on Basic

- **Plan.** "Shopify Analytics' main features are available to merchants on any Shopify subscription plan" ([Shopify reports][sh-reports]). The Australian pricing page lists "200+ real-time reports, plus custom analytics" as included on Basic, at A$56 a month paid monthly or A$42 a month paid yearly ([Shopify AU pricing][sh-pricing]).
- **Dashboard.** "Key sales, sessions, and fulfillment metrics", updated within about a minute. It has customisable metric cards, automatic Insights and targets ([Analytics dashboard][sh-dash]).
- **Acquisition reports.** Sessions by referrer, sessions by location, sessions over time and visitors over time. "These reports apply to the Online Store sales channel only" ([Acquisition reports][sh-acq]).
- **Behaviour reports** ([Behaviour reports][sh-beh]):
  - conversion rate breakdown (a funnel) and conversion rate over time;
  - web performance (LCP, INP and CLS, each over time, by URL or by page type), which is useful for the PDP;
  - product recommendation conversions, site search reports, sessions by landing page and sessions by device.
- **Other default reports.** Customers, finance, fraud, inventory, marketing, order, profit and sales ([Default reports][sh-default]).
- **Limits.**
  - Session data goes back only to 1 October 2022.
  - A session measurement change rolled out on 21 to 23 September 2026, so session numbers may shift ([Behaviour reports][sh-beh]).
  - No plan restrictions are stated on any report page I read.

**Inference.** For a single-Market launch, Shopify Analytics already answers the main questions: how many visit, from where, how many buy, and how fast the PDP is. It needs no app and adds no script, because Shopify's own tracking is part of every storefront.

### What GA4, Merchant Center conversion tracking and the Meta pixel add

| Tool | What it adds over Shopify Analytics | How it installs | Source |
|---|---|---|---|
| **GA4** | Google's event model, audiences, explorations and a link to Google Ads. "Certain ecommerce events are tracked automatically", such as product views, add to cart and purchase. Merchant Center is not needed for GA4 | Install the **Google & YouTube** sales channel, then connect a GA4 web stream from the app's Overview page. The app "installs the Google tag" | [Shopify GA4 setup][sh-ga4]; [Google: set up your Google tag][g-tag] |
| **Google Ads / Merchant Center conversion tracking** | Shows "which ads or free listings lead to actions on your store, such as sales". It maps Shopify events (purchase, add_to_cart, begin_checkout) to conversion actions. A Google Ads account is recommended but not required. Opting out turns off tracking "for both ads and free listings" | The same Google & YouTube app. "The easiest way to set up conversion tracking is through the Google & Youtube app on Shopify" | [Google: conversion tracking with the app][g-conv] |
| **Meta pixel and Conversions API** | Measurement and retargeting for Facebook and Instagram ads. **Standard** sends browsing behaviour through the pixel only. **Enhanced** and **Maximum** add the server-to-server Conversions API and share "name, location, email, phone number" to match users on Meta | **Facebook & Instagram** app: Sales channels > Facebook & Instagram > Settings > Data sharing settings | [Shopify: Meta data sharing][sh-meta] |

**No theme code, through Customer events.**

- Both apps create **app pixels**, managed at Settings > Customer events ([App pixels][sh-apppx]; [Pixels overview][sh-px]).
- App pixels "honor the consent signals customers choose". "In regions where tracking is on by default, callbacks run as events occur until the customer opts out" ([shopify.dev: Web pixels][dev-px]).
- Shopify warns that hand-added code "can be used to bypass customer consent requirements, which violates the Shopify Terms of Service" ([Pixels overview][sh-px]).
- Google warns against tracking the same conversions in the app and in "the storefront or custom pixel", because that counts them twice ([Google: set up your Google tag][g-tag]).

**Inference.** For an owner who never touches code, the two first-party apps are the only sensible route. Hand-pasted gtag or Meta snippets in the theme, or Google Tag Manager, would need code, would skip Shopify's consent handling and would risk double counting.

### Page weight on the PDP

Measured on 13 October 2026 by fetching each vendor's library with a placeholder ID:

| Library | Transfer size (compressed) | Uncompressed |
|---|---|---|
| `gtag.js` for a GA4 ID (brotli) | 151.8 KB | 439.6 KB |
| `gtag.js` for a Google Ads ID (brotli) | 148.9 KB | 433.6 KB |
| Meta `fbevents.js` (gzip) | 114.0 KB | 433.4 KB |

- **Where they run.** App pixels load "in a strict sandbox", which is a web worker. Custom pixels load in an iframe ([shopify.dev: Web pixels][dev-px]). So the script runs off the main thread, but its bytes still download on each page.
- **Shopify's own claim.** It is qualitative: pixel extensions help "avoid performance and privacy alerts" with "smaller pixel code libraries" ([shopify.dev: Web pixels][dev-px]).
- **Inference.**
  - GA4 and Google Ads probably share one Google tag load, so Google adds roughly 150 KB compressed, not 300 KB.
  - Meta adds about 115 KB, plus a per-pixel config file I couldn't measure.
  - Shopify Analytics adds nothing extra.
  - Shopify's own web performance report will show whether LCP or INP move after each app goes in.
- **Not measured.** The real cost inside Shopify's sandbox on a live PDP. There was no browser in this environment, so the figures above are the libraries alone.

---

## 2. Consent

### Does any Australian law require a cookie banner or consent before tags load?

**No, not for FOSLN's kind of tracking, as at October 2026.**

- **No cookie law.** Australia has no cookie-specific law like the EU's ePrivacy rules. I found no Commonwealth law that requires a banner (a negative finding). The Privacy Act is technology-neutral.
- **Pixels are allowed, even for APP entities.** OAIC: "The Privacy Act does not prohibit the use of tracking pixels."
  - Consent is required for sensitive information (APP 3.3), which watches don't involve.
  - For other data, an APP entity must give notice, keep disclosures within the purpose or the person's reasonable expectations (APP 6), and give a simple opt-out from targeted ads (APP 7).
  - The OAIC gives a first-visit banner or pop-up with an opt-out as an example, not a requirement ([OAIC: Tracking pixels][oaic-px], published 4 November 2024).
- **Penalties for APP entities.** Infringement notices can now be issued for breaching APP 1.3 and 1.4 (privacy policy) and APP 7.2(c) and 7.3(c) (opt-out from direct marketing) ([Privacy Act s 13K][pa]).

### The small-business exemption and FOSLN

The rules (Privacy Act, compilation 104, current at 4 June 2026 ([Privacy Act][pa])):

- **Turnover test.** "A business is a small business ... if its annual turnover for the previous financial year is $3,000,000 or less" (s 6D(1)). For a new business, "if there was no time in the previous financial year when the business was carried on, the business is a small business at the test time only if its annual turnover for the current year is $3,000,000 or less" (s 6D(2)). Turnover is all operating income of the business (s 6DA). The OAIC says a new business should "make a projection of full year annual turnover" ([OAIC: Small business][oaic-sb]).
- **Who is covered anyway** (s 6D(4)). A small business is not exempt if it:
  - provides a health service and holds health information;
  - "discloses personal information about another individual to anyone else for a benefit, service or advantage" (para (c));
  - "provides a benefit, service or advantage to collect personal information" (para (d));
  - is a Commonwealth contracted service provider or a credit reporting body.
- **Consent exception.** Paragraph (c) doesn't apply where the disclosure is made "with the consent of the other individual" or as required or authorised by law (s 6D(7)). The OAIC says "Consent can either be express or implied" ([OAIC: Small business][oaic-sb]).
- **What OAIC calls trading.** "Buying, selling or bartering personal information", for example "disclosing customer details to someone else for some commercial gain". A business is not trading if it has "the consent of all the individuals concerned" ([OAIC: Trading in personal information][oaic-trade]).
- **Opting in.** A small business can choose to be covered (s 6EA; [OAIC: Opting in][oaic-optin]).

**What this means for FOSLN.**

- **Turnover: exempt.** FOSLN's projected turnover is far under A$3 million, and it runs no other business. It is a small business operator, so the APPs, the notifiable data breach scheme and the new infringement notices don't apply to it. **Inference** from the Act's text.
- **Trading through Shopify changes nothing.** The test looks at the person who carries on the business. The owner carries on the FOSLN business, and Shopify is a separate entity with its own obligations. **Inference**. No source deals with marketplaces or platforms directly.
- **The open question is paragraph (c).** Three set-ups send customer data to a third party, which then gives FOSLN a service:
  - **Meta Enhanced or Maximum data sharing** sends customers' "name, location, email, phone number" to Meta to match them for ads ([Shopify: Meta data sharing][sh-meta]).
  - **Shopify Network Intelligence** lets Shopify "use your customer data together with customer data from other merchants" to power "Enhanced Services". It must be on to use Shopify Messaging (Shopify Email) ([Shopify Terms FAQ][sh-tos-faq]; [Shopify Messaging][sh-msg]).
  - **The Standard Meta pixel and GA4** send browsing data and device identifiers. Whether that is "personal information" in FOSLN's hands is a further question.

  No OAIC guidance or decision I found says whether a merchant sending data to an ad platform or to its own e-commerce platform is a disclosure "for a benefit, service or advantage". **Unresolved.**
- **Inference: three ways to lower the risk.**
  1. Keep Meta data sharing at **Standard**, or leave Meta off until it is needed.
  2. Get consent, which takes even a paragraph (c) disclosure out of the test (s 6D(7)). A clear notice at signup or checkout, plus Shopify's cookie banner switched on for Australia, would support an argument for consent. Whether implied consent from a banner is enough is untested.
  3. Opt in to the Privacy Act and follow the APPs anyway. The cost is a compliant privacy policy and an opt-out, most of which FOSLN needs anyway for the contracts in section 4.

### The 2024 amendments in force by October 2026

From the commencement table in s 2 of the [Privacy and Other Legislation Amendment Act 2024][pola] (assent 10 December 2024):

| Change | Starts | Applies to FOSLN? |
|---|---|---|
| Schedule 1, Parts 1 to 7 and 9 to 14: emergency declarations, children's privacy code mandate, security, overseas data flows, data breaches, tiered civil penalties and infringement notices, court orders, OAIC powers | 11 December 2024 | No, while exempt. These bind APP entities |
| Schedule 1, Part 15: APP 1.7 to 1.9, privacy policy must describe automated decisions that "could reasonably be expected to significantly affect the rights or interests of an individual" | **10 December 2026** | No, while exempt. Even if covered, FOSLN makes no such decisions (**Inference**) |
| Schedule 2: statutory tort for serious invasions of privacy | Proclamation, or 6 months after assent at the latest. The OAIC confirms "commenced on 10 June 2025" ([OAIC: Statutory tort][oaic-tort]) | **Yes**, see below |
| Schedule 3: doxxing offences (Criminal Code) | 11 December 2024 | Not relevant |
| Children's Online Privacy Code: the OAIC must register it within 24 months of assent | By 10 December 2026 | No, while exempt. It binds APP entities |

**The statutory tort.**

- **Who can be sued.** The tort extends to "individuals and other entities that may not necessarily be an Australian Privacy Principle entity" ([OAIC: Statutory tort][oaic-tort]). The exemptions are journalists, agencies, intelligence and law-enforcement bodies, and persons under 18, not small businesses (Schedule 2, cl 15 to 18, [POLA Act][pola]).
- **What must be shown.** All of these (cl 7):
  - an intrusion on seclusion or a misuse of information;
  - a reasonable expectation of privacy;
  - the invasion was "intentional or reckless" and "serious";
  - the public interest in privacy outweighs any countervailing interest.
- **Inference.** Standard analytics and ad pixels on a watch store, disclosed in a privacy policy, are a long way from that bar. A data leak caused by recklessness, or misusing a customer's details, could reach it.

### Tranche 2: not law

- **What it is.** The Attorney-General's Department published the exposure draft **Privacy Amendment (Personal Data Protection) Bill 2026** and a consultation paper on 31 August 2026. Submissions closed 18 September 2026. "The Bill remains subject to further consideration by government" ([AGD consultation][agd]).
- **Status.** I found no sign that it has been introduced to Parliament. Its commencement table is blank (draft s 2). It does not remove the small-business exemption, and the consultation paper doesn't mention the exemption.
- **What would change for a small business like FOSLN, if it passes as drafted** ([Exposure draft][agd-ed]; [Consultation paper][agd-cp]):
  - New s 6FC would make a disclosure a "trade" if made "for money or other consideration" or "for the purposes of direct marketing". The consultation paper says this "may include disclosures of cookies or pixels in programmatic advertising processes". Disclosures to a processor acting on the business's behalf would be carved out.
  - Paragraph 6D(4)(c) would be replaced with "trades personal information". So a small business that shares pixel data for ad targeting could fall outside the exemption.
  - The consent exception in s 6D(7) isn't among the draft's listed amendments (items 2 to 6 change s 6D(4) and (8) only), so it would still apply. **Inference** from reading the draft.
  - For APP entities, the draft replaces APP 7 with a simpler direct-marketing scheme that doesn't require consent but does require an opt-out in every communication.
- **Inference.** Nothing here changes FOSLN's position at launch. If the bill passes in this form, consent for pixels becomes the clean way for a small business to stay exempt. Revisit when it is introduced.

### Shopify's customer privacy settings for an Australia-only store

- **Where.** Settings > Customer privacy. It holds three automated items: the privacy policy, the cookie banner and the data sharing opt-out page ([Shopify: Privacy settings][sh-priv]).
- **Default banner regions.** "We will automatically configure your cookie banner for visitors in the UK and EEA regions", if the store has active markets there ([Shopify: Privacy settings][sh-priv]). FOSLN has only Australia, so **no banner shows by default**.
- **Turning it on for Australia.** Settings > Customer privacy > Cookie banner > Regions > Edit lets you "select the regions where the cookie banner should display" ([Shopify: Privacy settings][sh-priv]).
- **Behaviour without a banner.** In regions set to require consent, non-essential purposes are off until the visitor agrees. "For other regions, the default behavior is to allow all processing purposes" (analytics, marketing, preferences and sale of data) ([shopify.dev: Customer Privacy API][dev-cpa]).
- **Behaviour with a banner.** Pixels wait for consent, then replay the queued events ([shopify.dev: Web pixels][dev-px]). Shopify warns this "may reduce analytics, marketing, and conversion data" ([Shopify: Privacy settings][sh-priv]).
- **Third-party tools.** They respect consent only if integrated with the Customer Privacy API. The banner regions must match in Shopify and in any third-party app ([Shopify: Privacy settings][sh-priv]). The Google & YouTube and Facebook & Instagram app pixels go through Shopify's consent handling ([shopify.dev: Web pixels][dev-px]).
- **Data sharing opt-out page.** It is for US state laws and doesn't matter for an Australia-only store ([shopify.dev: Customer Privacy API][dev-cpa]).

### Google's consent requirements for Australian traffic

- **Where the policy applies.** Google's EU user consent policy covers "end users in the European Economic Area, the UK and Switzerland" ([Google EU user consent policy][g-eucp]). Shopify says EEA data "won't be used for personalized advertising unless Google Consent Mode v2 is activated and valid consent is obtained" ([Shopify: Understanding privacy settings][sh-upriv]).
- **Elsewhere.** Google's consent mode article recommends a default consent state per region, "since current privacy laws are region-specific", and names no requirement outside those regions ([Google: Consent mode][g-cm]).
- **If the banner is turned on.** "After you've enabled your consent banner, consent mode is enabled automatically" with the Google & YouTube app ([Google: Set up Shopify to obtain user consent][g-shopconsent]).
- **Inference.** Consent Mode v2 is not required for Australian visitors. If FOSLN turns on Shopify's banner for Australia, the app handles consent mode with no extra work.

---

## 3. Email capture

### What the Spam Act 2003 requires

Read from the current compilation (No. 10, 10 March 2016, still the latest on the register) ([Spam Act][spam]):

- **Every message to Australians is covered.** A message has an "Australian link" if it originates in Australia, or the sender, or the device or account it is read on, is in Australia (s 7). The Act has no size or turnover exemption.
- **Consent (s 16).** "A person must not send, or cause to be sent, a commercial electronic message" with an Australian link unless "the relevant electronic account-holder consented" (s 16(1) and (2)).
  - Consent is "express consent" or consent that "can reasonably be inferred from ... the conduct; and ... the business and other relationships" of the person (Sch 2, cl 2).
  - A sender relying on consent "bears an evidential burden" (s 16(5)).
  - ACMA: "it is up to you to prove that you have a person's consent", and businesses must be able to produce evidence of consent (search extract, [ACMA: Avoid sending spam][acma-avoid]).
- **Identify the sender (s 17).** The message must "clearly and accurately" identify "the individual or organisation who authorised the sending of the message". It must include "accurate information about how the recipient can readily contact" them, and that information must be "reasonably likely to be valid for at least 30 days after the message is sent".
  - ACMA: use "the correct legal name of your business, or your name and Australian Business Number (ABN)" (search extract, [ACMA: Avoid sending spam][acma-avoid]).
- **Functional unsubscribe (s 18).** The message must have a "clear and conspicuous" statement that the recipient can unsubscribe. The unsubscribe address must keep working "for at least 30 days after the message is sent".
  - **The 5-day rule.** A withdrawal of consent takes effect "at the end of the period of 5 business days" after the unsubscribe is sent (Sch 2, cl 6).
- **Messages with mixed content.** ACMA's position is that any message with promotional content is commercial. In March 2026 Lululemon paid A$702,900 for over 370,000 emails, including order and delivery emails "that also had a clear marketing purpose", sent without an unsubscribe (search extract, [ACMA: Lululemon][acma-lulu]).
- **Pre-ticked boxes.** Secondary sources say ACMA rejects pre-ticked boxes as consent. I could not read ACMA's own wording. **Not verified.**

### Does Shopify's tooling meet it?

| Requirement | Shopify tool and setting | Meets it? | Source |
|---|---|---|---|
| Express consent at signup | **Theme newsletter form.** Liquid `{% form 'customer' %}` creates "a new customer without registering a new account", for "building a list of emails from a newsletter signup". The subscriber lands in the Email subscribers segment | Yes, if the form says what people are signing up for | [shopify.dev: form tag][dev-form]; [Subscriber list][sh-subs] |
| Express consent in a popup | **Shopify Forms** (free app). Installs as a theme **app embed**, with no code. Consent can be "implicitly after they submit a form" or by "a marketing consent checkbox ... that a customer needs to select" | Yes. A popup whose whole purpose is signup is express consent either way. Use the checkbox on any form with another purpose (**Inference**) | [Forms settings][sh-forms]; [Create a form][sh-forms-create] |
| Express consent at checkout | Settings > Checkout > Marketing opt-in > Email. "Preselect the sign-up option" is optional. "By default, no regions are selected" | Yes, **only if not preselected** | [Customer contact information][sh-contact] |
| Proof of consent | Shopify stores each customer's `consentUpdatedAt` and `marketingOptInLevel` | Helps meet the evidential burden (**Inference**) | [shopify.dev: CustomerEmailMarketingConsentState][dev-consent] |
| Double opt-in (optional) | Settings > Notifications > Customer notifications > Marketing double opt-in > Customer marketing confirmation | Not required by the Spam Act, but it is the strongest evidence | [Customer contact information][sh-contact] |
| Unsubscribe | Unsubscribes, spam reports and hard bounces "are automatically suppressed". Custom Liquid emails must include `unsubscribe_link` or `unsubscribe_url` | Yes. Suppression is immediate, well inside 5 business days | [Subscriber list][sh-subs] |
| Sender identification and address | "By default, your store address is included in the footer of your emails." It can be changed in Settings > General, and a PO box can be entered | Partly. See the gap below | [Messaging legal compliance][sh-msg-legal] |
| Shopify's own rules | Shopify's Terms prohibit "using non-permission based email lists", "purchased or rented email lists", "failing to include a working "unsubscribe" link", "failing to include in each email your valid physical mailing address" and failing to include "a link to the then-current privacy policy" | Contractual, on top of the Act | [Shopify Terms §9.7][sh-tos] |

**The identification gap (Inference).**

- **The conflict.** ADR 0006 keeps the owner's name off everything except the warranty, so emails would say "FOSLN". Until registration, "FOSLN" is not a registered business name and there is no ABN. So a footer that says only "FOSLN" may not "accurately" identify the person who authorised the send (s 17), on ACMA's reading.
- **A low-cost fix.** Put "[legal name], trading as FOSLN" and a contact email in the marketing email footer, as the warranty already does. After registration it becomes the registered name and ABN.
- **Decision needed.** This needs a decision against ADR 0006.
- **The address.** Shopify's Terms ask for a "valid physical mailing address", and the PO box question is covered in the earlier [PO box research](https://github.com/Kintukka2/FOSLN-WATCH/blob/research/po-box-warranty-address/docs/research/po-box-warranty-address.md).

**Inference: keep order emails clean.** Leave discount codes and product promotions out of Shopify's order, shipping and delivery notifications. Otherwise they become commercial messages that need consent and an unsubscribe, as in the Lululemon case.

### Cost of Shopify Email (now Shopify Messaging) on Basic

From [Shopify Messaging email pricing][sh-msg-price]:

- **Free allowance.** "All stores on these plans have 10,000 free emails to send at the beginning of every month." Basic is one of those plans.
- **After that.** "$1 USD per 1,000 additional emails up to 300,000 emails". Then US$0.65 per 1,000, and US$0.55 per 1,000 after 750,000. The charge is billed on the next Shopify bill as "Marketing emails".
- **What counts.** Each recipient counts as one email, and automations count toward the 10,000. "Abandoned checkout automations are always free and don't count towards the monthly limit." Unused emails don't carry over.
- **Requirement.** Shopify Messaging is installed from the App Store and "requires Shopify Network Intelligence to be enabled" in privacy settings ([Shopify Messaging][sh-msg]). See section 2 for why that matters.

**Inference.** At launch volumes FOSLN would pay nothing. For example, 1,000 subscribers emailed four times a month is 4,000 emails.

---

## 4. What the privacy policy must say

**If FOSLN is exempt,** the Privacy Act doesn't require a privacy policy. One is still needed, for three contractual reasons and one legal one:

- **Shopify's Terms.** When Network Intelligence (Enhanced Services) is on, the policy "must disclose that the store is hosted by Shopify", that Shopify collects and processes customer data, and that data may go to Shopify and third parties in other countries. It must link to Shopify's Consumer Privacy Policy and to an opt-out, and the store must link to the policy prominently (§9.2). Every marketing email must link to the policy (§9.7). For pixels, the merchant must "obtain all necessary consents from every site visitor and customer whose events you track" (§9.11) ([Shopify Terms][sh-tos]; [Network Intelligence requirements][sh-sni]).
- **Google Analytics Terms §7.** "You must post a Privacy Policy and that Privacy Policy must provide notice of Your use of cookies". "You must disclose the use of Google Analytics, and how it collects and processes data", for example with a link to Google's "How Google uses information from sites or apps that use our services" page ([GA Terms][g-terms]).
- **Meta Business Tools Terms §3.** A "clear and prominent notice on each web page where our pixels are used that links to a clear explanation". The explanation must say that FOSLN may use "pixels, web beacons, and other storage technologies from third parties, including Meta", how users can opt out of ad targeting, and where to find the opt-out ([Meta Business Tools Terms][meta-terms]).
- **Accuracy.** Whatever the policy says must be true. A false privacy statement could be misleading conduct under s 18 of the Australian Consumer Law. **Inference.** I didn't research this further.

**Per tool, the policy should cover:**

| Tool | What to say | Driven by |
|---|---|---|
| Shopify (always) | Store hosted by Shopify. Shopify processes customer data, including overseas. Link to Shopify's Consumer Privacy Policy. Network Intelligence use, if on | Shopify Terms §9.2 |
| Shopify Analytics | Cookies and similar technologies used to measure visits and sales | GA Terms (cookies notice), OAIC pixel guidance |
| GA4 and Google Ads | Use of Google Analytics and Google Ads conversion tracking, cookies, and the link to Google's "How Google uses information" page | GA Terms §7 |
| Meta pixel | Third-party pixels and cookies including Meta's, used for measurement and ad targeting. How to opt out. If Enhanced or Maximum: that name, email, phone and location are shared with Meta | Meta Business Tools Terms §3; [Shopify: Meta data sharing][sh-meta] ("It's important to tell your customers how you share data") |
| Email signup | What is collected, that it's used for marketing, that every email has an unsubscribe, and who the sender is | Spam Act s 17 and 18 (in the emails); Shopify Terms §9.7 |
| Shopify Messaging | Customer data used with Shopify Network Intelligence | Shopify Terms §9.2 |

**If FOSLN is or becomes an APP entity** (by opting in, by losing the exemption under s 6D(4)(c), or when turnover passes A$3 million), the APP 1.4 contents also apply:

- **APP 1.4 contents** ([OAIC: APP 1 guidelines][oaic-app1]):
  - kinds of personal information collected and held;
  - how it is collected and held;
  - purposes of collection, use and disclosure;
  - how to access and correct it;
  - how to complain and how complaints are handled;
  - whether it is likely to go overseas, and to which countries if practicable.
- **Pixel guidance.** The OAIC adds that the policy should "clearly disclose the use of third-party pixels, the kinds of personal information the pixel collects, and the purposes" ([OAIC: Tracking pixels][oaic-px]).
- **From 10 December 2026.** The automated-decision disclosures under APP 1.7 to 1.9 also apply.

**Shopify's automated policy.** At Settings > Customer privacy > Privacy policy, "Use automated policy" is on by default. It "automatically updates as your settings change". Shopify says "there are some things we can't automate and you will need to review and add" ([Shopify: Privacy settings][sh-priv]). **Inference.** Start from it, then check it against the table above, especially the Meta notice and the Network Intelligence wording.

---

## Not verified

- **Whether sending data to Meta (Enhanced or Maximum) or enabling Shopify Network Intelligence is a disclosure "for a benefit, service or advantage" under s 6D(4)(c).** No OAIC guidance, determination or case found. This decides whether FOSLN keeps the small-business exemption while using them.
- **Whether a cookie banner gives "consent" for s 6D(7).** This includes whether implied consent from a notice is enough.
- **ACMA's own wording** on pre-ticked boxes, sender identification and mixed-content emails. acma.gov.au returned HTTP 503. The points are from search extracts and secondary sources.
- **Real page weight on a Shopify PDP** for the Google & YouTube and Facebook & Instagram app pixels inside Shopify's sandbox. Only the vendor libraries were measured. Whether the app pixels load these exact libraries, and Meta's per-pixel config size, are unconfirmed.
- **Whether a standard (non-Liquid) Shopify Messaging template always inserts the unsubscribe link.** The Help Center confirms suppression and the Liquid requirement only.
- **Whether Shopify Network Intelligence is on by default** for a new store. Shopify's pages don't say.
- **Whether the exposure draft bill has been introduced to Parliament** since consultation closed on 18 September 2026. No evidence found.
- **Shopify Basic's exact report list by plan.** No help page states per-plan limits. The pricing page lists "200+ real-time reports, plus custom analytics" on Basic.
- **Whether ACMA accepts "FOSLN" alone** as sender identification for an unregistered sole trader. **Inference** only.

## Sources

[pa]: https://www.legislation.gov.au/C2004A03712/latest/text "Privacy Act 1988, compilation 104 (4 June 2026), Federal Register of Legislation"
[pola]: https://www.legislation.gov.au/C2024A00128/asmade/text "Privacy and Other Legislation Amendment Act 2024, as made"
[spam]: https://www.legislation.gov.au/C2004A01214/latest/text "Spam Act 2003, compilation 10"
[agd]: https://consultations.ag.gov.au/rights-and-protections/privacy-reform/ "AGD: Privacy Reform consultation on exposure draft"
[agd-ed]: https://consultations.ag.gov.au/rights-and-protections/privacy-reform/user_uploads/exposure-draft-bill-2026.pdf "Exposure Draft Privacy Amendment (Personal Data Protection) Bill 2026"
[agd-cp]: https://consultations.ag.gov.au/rights-and-protections/privacy-reform/user_uploads/consultation_paper.pdf "AGD: Privacy Reform consultation paper"
[oaic-sb]: https://www.oaic.gov.au/privacy/privacy-guidance-for-organisations-and-government-agencies/organisations/small-business "OAIC: Small business"
[oaic-trade]: https://www.oaic.gov.au/privacy/privacy-guidance-for-organisations-and-government-agencies/organisations/trading-in-personal-information "OAIC: Trading in personal information"
[oaic-optin]: https://www.oaic.gov.au/privacy/privacy-guidance-for-organisations-and-government-agencies/organisations/opting-in-to-the-privacy-act "OAIC: Opting in to the Privacy Act"
[oaic-px]: https://www.oaic.gov.au/privacy/privacy-guidance-for-organisations-and-government-agencies/organisations/tracking-pixels-and-privacy-obligations "OAIC: Tracking pixels and privacy obligations"
[oaic-tort]: https://www.oaic.gov.au/privacy/your-privacy-rights/more-privacy-rights/statutory-tort-for-serious-invasions-of-privacy "OAIC: Statutory tort for serious invasions of privacy"
[oaic-app1]: https://www.oaic.gov.au/privacy/australian-privacy-principles/australian-privacy-principles-guidelines/chapter-1-app-1-open-and-transparent-management-of-personal-information "OAIC: APP Guidelines chapter 1"
[acma-avoid]: https://www.acma.gov.au/avoid-sending-spam "ACMA: Avoid sending spam (search extract)"
[acma-lulu]: https://www.acma.gov.au/articles/2026-03/lululemon-penalised-702k-spam-breaches "ACMA: Lululemon penalised $702k (search extract)"
[sh-reports]: https://help.shopify.com/en/manual/reports-and-analytics/shopify-reports "Shopify Help: Shopify reports"
[sh-dash]: https://help.shopify.com/en/manual/reports-and-analytics/shopify-reports/overview-dashboard "Shopify Help: Analytics dashboard"
[sh-default]: https://help.shopify.com/en/manual/reports-and-analytics/shopify-reports/report-types/default-reports "Shopify Help: Default reports"
[sh-acq]: https://help.shopify.com/en/manual/reports-and-analytics/shopify-reports/report-types/default-reports/acquisition-reports "Shopify Help: Acquisition reports"
[sh-beh]: https://help.shopify.com/en/manual/reports-and-analytics/shopify-reports/report-types/default-reports/behaviour-reports "Shopify Help: Behaviour reports"
[sh-pricing]: https://www.shopify.com/au/pricing "Shopify AU pricing"
[sh-ga4]: https://help.shopify.com/en/manual/reports-and-analytics/google-analytics/google-analytics-setup "Shopify Help: Setting up Google Analytics 4"
[sh-meta]: https://help.shopify.com/en/manual/promoting-marketing/analyze-marketing/meta-data-sharing "Shopify Help: Meta customer data sharing"
[sh-px]: https://help.shopify.com/en/manual/promoting-marketing/pixels/overview "Shopify Help: Pixels overview"
[sh-apppx]: https://help.shopify.com/en/manual/promoting-marketing/pixels/app-pixels "Shopify Help: App pixels"
[sh-priv]: https://help.shopify.com/en/manual/privacy-and-security/privacy/customer-privacy-settings/privacy-settings "Shopify Help: Configuring customer privacy settings"
[sh-upriv]: https://help.shopify.com/en/manual/privacy-and-security/privacy/customer-privacy-settings/understanding-customer-privacy-settings "Shopify Help: Understanding customer privacy settings"
[sh-sni]: https://help.shopify.com/en/manual/privacy-and-security/privacy/shopify-network-intelligence-requirements "Shopify Help: Shopify Network Intelligence requirements"
[sh-tos]: https://www.shopify.com/au/legal/terms "Shopify Terms of Service"
[sh-tos-faq]: https://www.shopify.com/au/legal/terms-faq "Shopify: Updates to Terms of Service (25 July 2025)"
[sh-msg]: https://help.shopify.com/en/manual/promoting-marketing/create-marketing/shopify-messaging "Shopify Help: Shopify Messaging"
[sh-msg-price]: https://help.shopify.com/en/manual/promoting-marketing/create-marketing/shopify-messaging/email/pricing "Shopify Help: Shopify Messaging email pricing"
[sh-msg-legal]: https://help.shopify.com/en/manual/promoting-marketing/create-marketing/shopify-messaging/email/data-privacy-legal-compliance "Shopify Help: Data privacy and legal compliance for email marketing"
[sh-subs]: https://help.shopify.com/en/manual/promoting-marketing/create-marketing/shopify-messaging/email/subscriber-list-management "Shopify Help: Subscriber list management"
[sh-contact]: https://help.shopify.com/en/manual/promoting-marketing/create-marketing/customer-contact-information "Shopify Help: Collecting customer contact information"
[sh-forms]: https://help.shopify.com/en/manual/promoting-marketing/create-marketing/forms-app/settings/all-forms "Shopify Help: Shopify Forms settings"
[sh-forms-create]: https://help.shopify.com/en/manual/promoting-marketing/create-marketing/forms-app/create "Shopify Help: Create a form"
[dev-px]: https://shopify.dev/docs/apps/build/marketing-analytics/pixels "shopify.dev: Web pixels"
[dev-cpa]: https://shopify.dev/docs/api/customer-privacy "shopify.dev: Customer Privacy API"
[dev-form]: https://shopify.dev/docs/api/liquid/tags/form "shopify.dev: Liquid form tag"
[dev-consent]: https://shopify.dev/docs/api/admin-graphql/latest/objects/CustomerEmailMarketingConsentState "shopify.dev: CustomerEmailMarketingConsentState"
[g-tag]: https://support.google.com/merchants/answer/12183125 "Google: Shopify, set up your Google tag"
[g-conv]: https://support.google.com/merchants/answer/13494537?hl=en "Google: Set up conversion tracking with the Google & YouTube app on Shopify"
[g-cm]: https://support.google.com/analytics/answer/9976101 "Google: About consent mode"
[g-eucp]: https://www.google.com/about/company/user-consent-policy/ "Google: EU user consent policy"
[g-shopconsent]: https://support.google.com/google-ads/answer/14563069?hl=en "Google: Set up Shopify to obtain user consent"
[g-terms]: https://marketingplatform.google.com/about/analytics/terms/us/ "Google Analytics Terms of Service"
[meta-terms]: https://www.facebook.com/legal/terms/businesstools "Meta Business Tools Terms"

- Legislation: [Privacy Act 1988][pa] (compilation 104, 4 June 2026, read as PDF), [Privacy and Other Legislation Amendment Act 2024][pola], [Spam Act 2003][spam].
- Reform: [AGD consultation][agd], [exposure draft bill][agd-ed], [consultation paper][agd-cp].
- OAIC: [Small business][oaic-sb], [Trading in personal information][oaic-trade], [Opting in][oaic-optin], [Tracking pixels][oaic-px], [Statutory tort][oaic-tort], [APP 1 guidelines][oaic-app1].
- ACMA (search extracts): [Avoid sending spam][acma-avoid], [Lululemon penalty][acma-lulu].
- Shopify: [Reports][sh-reports], [Dashboard][sh-dash], [Default reports][sh-default], [Acquisition][sh-acq], [Behaviour][sh-beh], [AU pricing][sh-pricing], [GA4 setup][sh-ga4], [Meta data sharing][sh-meta], [Pixels][sh-px], [App pixels][sh-apppx], [Privacy settings][sh-priv], [Understanding privacy settings][sh-upriv], [Network Intelligence requirements][sh-sni], [Terms][sh-tos], [Terms FAQ][sh-tos-faq], [Messaging][sh-msg], [Messaging pricing][sh-msg-price], [Messaging legal compliance][sh-msg-legal], [Subscriber list][sh-subs], [Customer contact information][sh-contact], [Forms settings][sh-forms], [Create a form][sh-forms-create].
- shopify.dev: [Web pixels][dev-px], [Customer Privacy API][dev-cpa], [form tag][dev-form], [CustomerEmailMarketingConsentState][dev-consent].
- Google: [Set up your Google tag][g-tag], [Conversion tracking][g-conv], [Consent mode][g-cm], [EU user consent policy][g-eucp], [Shopify user consent][g-shopconsent], [GA Terms][g-terms].
- Meta: [Business Tools Terms][meta-terms].
