# Is there a better platform than Shopify?

Research for [issue #7](https://github.com/Kintukka2/FOSLN-WATCH/issues/7). Checked on 2026-10-09.

## Short answer

No. None of the four alternatives beats Shopify on the six tests in the issue, and each one loses on at least one test that matters to FOSLN.

Shopify is the only one that syncs theme code with GitHub both ways, set up entirely in a browser, and the China dropshipping tools support it best.

Squarespace Core costs the same and is the GUI the owner knows, but new Squarespace sites have no Git workflow and few China-Supplier links. BigCommerce's sales caps raise its price from about 29 orders a month. Webflow has no Git and sells in one currency. WooCommerce leaves the owner looking after hosting and plugins.

The real reasons to hesitate over Shopify are smaller: below Plus you can restyle the checkout but not redesign it, apps add monthly cost, and one of its tools for selling abroad may not be open to a Supplier dispatching from China.

---

## Scope and assumptions

- **Platforms compared:** Shopify, Squarespace (Commerce features), Webflow (Ecommerce plans), BigCommerce and WooCommerce. These are the ones the issue names. Wix is also a realistic candidate, and the China dropshipping tools list it, but the issue doesn't name it, so it isn't covered here.
- **US business:** the card rates below assume FOSLN sells through a US-based business with a US bank account. If the business is based somewhere else, the rates, payout currency and available payment providers change on every platform. Check this before relying on any figure.
- **Order value:** $97 per order, with no tax or shipping on top. Domestic US cards only. American Express costs more on some platforms.
- **Not included:** app subscriptions, the domain (already at Cloudflare), shipping labels and sales tax.
- **Prices change.** Several of these platforms changed their pricing in 2026 (see [Recent changes](#recent-changes-to-watch)). Treat every figure as true on the date above.

## At a glance

| Test | Shopify | Squarespace | Webflow | BigCommerce | WooCommerce |
| --- | --- | --- | --- | --- | --- |
| US card fees | 2.9% + 30¢; no platform fee with Shopify Payments | 2.9% + 30¢ on Core; Basic adds a 2% fee | Stripe 2.9% + 30¢; Standard adds a 2% fee | Stripe or PayPal at their rates; no fee with listed providers | WooPayments 2.9% + 30¢ |
| Orders to a China Supplier | Best: DSers, AutoDS and CJ all list it; built-in email handoff | Weak: via extensions; China tools mostly absent | Weakest: email, Zapier or API only | Middling: Marketplace apps exist; patchy for China tools | Good: DSers, AutoDS and CJ all list it |
| Run from a GUI | Yes | Yes, and the owner knows it | Yes, but the Designer is a professional tool | Yes, but theme deploys need a command-line tool | Yes, but the owner looks after updates and plugins |
| Theme code in GitHub | Yes, two-way sync, set up in the admin | No (Git only on legacy version 7.0) | No (design lives in Webflow) | Partly: code can live in GitHub, but deploys use a CLI | Partly: one-way GitHub deploys on WordPress.com Business and up |
| Adding AU, UK, CA | Strong: Markets on every plan, local prices | Limited: one store currency, converted prices | Poor: one currency; Localize doesn't cover Ecommerce | Good on paper; fixed local prices only on top plan | Good: built-in multi-currency, converted prices |
| Monthly cost, 30 orders | $132 ($122 yearly) | $132 ($122 yearly) on Core | $177–194 | $132 at first, then $198 | $133–163 |

The cost row uses monthly billing, with yearly billing in brackets. The full working is under [Monthly cost at launch volume](#6-monthly-cost-at-launch-volume).

## Reasons not to use Shopify

The issue asks whether there is any reason not to use Shopify. These are the real ones:

1. **The checkout can be restyled but not redesigned below Plus.** On every plan, the checkout editor sets the logo, colours, fonts and background colours ([Shopify Help](https://help.shopify.com/en/manual/checkout-settings/checkout-style)). The Checkout Branding API, and UI extensions in the information, shipping and payment steps, are Plus-only. Plus starts at $2,300 a month ([shopify.dev](https://shopify.dev/docs/apps/build/checkout/technologies); [Shopify pricing](https://www.shopify.com/pricing)). By contrast, Webflow lets you design the checkout page and layout on every Ecommerce plan ([Webflow pricing](https://webflow.com/pricing)). WooCommerce's checkout is a page on your own WordPress site, so it can be changed like any other page. Whether this matters depends on the visual direction (#14) and the PDP layout (#18).
2. **Apps cost extra.** Shopify keeps the core small and leaves features to apps. Many dropshipping and review apps charge monthly. This is true of every platform here, but on Shopify it is the usual way to add features.
3. **A 2% penalty for other payment providers.** On Basic, Shopify adds a 2% fee to orders paid through a third-party provider instead of Shopify Payments ([Shopify pricing](https://www.shopify.com/pricing)). This only matters if Shopify Payments isn't available to the business.
4. **Free tax calculation now has a lifetime cap.** For stores created on or after 13 May 2026, Shopify Tax is free for the first $100,000 of *lifetime* sales, not $100,000 a year. After that it costs 0.35% per US order, capped at $0.99 ([Shopify Help](https://help.shopify.com/en/manual/taxes/shopify-tax/pricing)). At launch volume, it stays free for years.
5. **Managed Markets probably won't be available.** Shopify's merchant-of-record service for selling abroad needs a US business with "at least one fulfillment location in the continental US" ([Shopify Help](https://help.shopify.com/en/manual/international/managed-markets/requirements-and-considerations)). A Supplier dispatching from China probably doesn't count. Ordinary Shopify Markets still works (see [Adding Australia, the UK and Canada](#5-adding-australia-the-uk-and-canada)).
6. **Lock-in.** Shopify themes are written in Liquid, which only runs on Shopify. Moving to another platform later means rebuilding the theme. The product data can be exported.

None of these is a reason to pick one of the four alternatives instead, because each alternative has a bigger gap against the tests below.

## 1. US payments and fees

| Platform and plan | Card rate (US, domestic) | Platform fee on each sale | Notes |
| --- | --- | --- | --- |
| Shopify Basic | 2.9% + 30¢ online | 0% with Shopify Payments; 2% with other providers | Grow: 2.7% + 30¢; Advanced: 2.5% + 30¢ ([pricing](https://www.shopify.com/pricing)) |
| Squarespace Basic | 2.9% + 30¢ (Squarespace Payments) | 2% | Basic has no JavaScript or code injection ([pricing](https://www.squarespace.com/pricing)) |
| Squarespace Core | 2.9% + 30¢ | 0% | Plus: 2.7% + 30¢; Advanced: 2.5% + 30¢. Premium cards, including Amex: 3.2% + 30¢ ([pricing](https://www.squarespace.com/pricing)) |
| Webflow Ecommerce Standard | Stripe: 2.9% + 30¢ | 2%, worked out on the order total including tax | Plus and Advanced: 0% ([Webflow Help](https://help.webflow.com/hc/en-us/articles/33961361368979-Connect-a-payment-provider); [Stripe pricing](https://stripe.com/pricing)) |
| BigCommerce Core | Stripe 2.9% + 30¢, or PayPal Braintree from 2.89% + 29¢ | 0% with "Embedded Payment Providers"; 2% with others | The embedded providers include Stripe, PayPal and BigCommerce Payments. Amex on Braintree: 3.5% ([pricing](https://www.bigcommerce.com/pricing/)) |
| WooCommerce + WooPayments | 2.9% + 30¢ | None from WooCommerce | $15 dispute fee ([WooPayments fees](https://woocommerce.com/document/woopayments/fees/)) |

**What it means:** on a domestic card, every platform charges about 2.9% + 30¢, which is about $3.11 on a $97 order. What separates them is the platform's own fee. Squarespace Basic and Webflow Standard each add 2%, which is $1.94 an order. Shopify, Squarespace Core, BigCommerce and WooCommerce add nothing, as long as you use the platform's preferred payment provider.

## 2. Handing orders to a dropshipping Supplier in China

The deciding fact here is outside this research: **which platforms does FOSLN's own Supplier connect to?** Many China dropshipping agents offer an app or a "store connection" for only a few platforms. Ask the Supplier before choosing. The full order flow belongs to #26. This section only covers what each platform makes possible.

The three big China-focused tools, as listed on their own sites:

| Tool | Platforms it lists |
| --- | --- |
| DSers (AliExpress's dropshipping partner) | Shopify, Wix, WooCommerce, eBay, TikTok, Amazon, Walmart ([dsers.com](https://www.dsers.com/)) |
| AutoDS | Shopify, Facebook, Etsy, WooCommerce, eBay, Amazon, Wix, TikTok Shop ([autods.com](https://www.autods.com/)) |
| CJdropshipping | Shopify, WooCommerce, Wix, Squarespace and BigCommerce, among others. This comes from older third-party and CJ blog listings ([CJ blog](https://blog.cjdropshipping.cn/detail/we-make-drop-shipping-much-simple-dropshipping-supplier); [Mercury](https://mercury.com/blog/guide-to-cj-dropshipping)). CJ's own site was behind a bot check and couldn't be read, so this isn't verified as current. |

Platform by platform:

- **Shopify.** All three tools list it. Shopify also has a built-in option needing no app: a *custom fulfillment service*. You enter the Supplier's name and email, link the products to it, and Shopify emails the Supplier the order details when you mark the order as "fulfilled" (Shopify's word) in the admin. The catch is that tracking doesn't come back on its own. Someone types the tracking number into the order in the admin ([Shopify Help](https://help.shopify.com/en/manual/shipping/fulfillment-services/custom)). All of this is GUI-only.
- **Squarespace.** Its help centre names drop shipping *extensions*: Spocket, Syncee, Order Desk and print-on-demand services. It doesn't mention email or API handoff ([Squarespace Help](https://support.squarespace.com/hc/en-us/articles/206540877-Connecting-to-a-fulfillment-service)). DSers and AutoDS don't list Squarespace. Squarespace's pricing page lists "API integrations" from the Core plan up ([pricing](https://www.squarespace.com/pricing)). But Squarespace's developer docs tie API keys to the old "Commerce Advanced" plan ([developers.squarespace.com](https://developers.squarespace.com/commerce-apis/authentication-and-permissions)). Which plan you need is unclear.
- **Webflow.** New orders arrive by email. They can then be handled through Webflow's Ecommerce API or Zapier ([Webflow Help](https://help.webflow.com/hc/en-us/articles/33961345384339-Webflow-Ecommerce-overview)). None of DSers, AutoDS or CJ lists Webflow. A connection to a China Supplier would need Zapier, Order Desk or custom code.
- **BigCommerce.** Its App Marketplace has dropshipping apps: Syncee, Spocket, Dropified, and AliExpress apps by Webkul and CedCommerce. There is also a DSers listing page, but it has no details, and DSers' own site doesn't list BigCommerce ([BigCommerce Apps: DSers](https://www.bigcommerce.com/apps/dsers/); [AliExpress by Webkul](https://www.bigcommerce.com/apps/aliexpress-dropshipping-by-webkul/)). Support for the China tools is patchy.
- **WooCommerce.** DSers, AutoDS and CJ all list it.

One tool works with all five: **Order Desk** lists Shopify, BigCommerce, WooCommerce, Squarespace, Webflow and Wix as integrations ([orderdesk.com](https://www.orderdesk.com/integrations/)). Whether it can send orders to FOSLN's Supplier wasn't checked; that belongs to #26.

**What it means:** Shopify and WooCommerce have the most ways to get an order to a China Supplier without code. Webflow has the fewest.

## 3. Running the store entirely from a GUI

The rule is that the owner never uses a terminal, and Claude Code sessions do all the building.

- **Shopify.** The owner runs everything from the web admin: orders, products, Markets, the theme editor and apps. The GitHub connection is also set up in the admin (Online Store > Themes > Add theme > Connect from GitHub) ([shopify.dev](https://shopify.dev/docs/storefronts/themes/tools/github)). No step needs a terminal.
- **Squarespace.** Fully GUI, and the one the owner already knows. This is Squarespace's strongest point. The weak side is the build: anything beyond the templates is pasted-in CSS and JavaScript, and Squarespace says custom code "falls outside the scope of Squarespace support" ([Squarespace Help](https://support.squarespace.com/hc/en-us/articles/205815928-Add-custom-code-to-your-site)).
- **Webflow.** Fully GUI, but the Designer is a professional visual-development tool, not a simple store admin. Claude Code could edit the site through Webflow's MCP server, and most of its actions don't need the Designer open ([developers.webflow.com](https://developers.webflow.com/mcp/reference/overview)). But those edits go straight into Webflow, not through GitHub.
- **BigCommerce.** The control panel and Page Builder are GUI. But custom theme code is built and pushed with the Stencil CLI, which needs Node.js 24 or later ([BigCommerce docs](https://docs.bigcommerce.com/docs/storefront/stencil/cli)). You can upload a theme zip in the control panel, but the zip has to be made with `stencil bundle`, not an ordinary zip tool ([BigCommerce docs](https://docs.bigcommerce.com/developer/docs/storefront/stencil/deployment/upload)). A Claude Code session could run those steps, so the owner wouldn't need to, but each theme change becomes a build-and-upload job rather than a sync.
- **WooCommerce.** wp-admin is GUI. But WooCommerce is a plugin on WordPress, so the owner is in charge of WordPress, the plugins and the hosting: updates, plugin conflicts, backups and security. Managed hosting such as WordPress.com takes some of this off the owner. It is still more moving parts than a hosted platform.

**What it means:** Shopify and Squarespace fit the no-terminal rule best. BigCommerce and WooCommerce both fit it, but only because Claude Code would absorb more ongoing technical work.

## 4. Keeping custom theme code in GitHub

- **Shopify: yes.** Shopify syncs a connected branch both ways. Commits on the branch update the theme. Saves in the theme editor or code editor are committed back to the branch by a `shopify` bot, with saves within about 10 seconds grouped into one commit. Only branches that match Shopify's theme folder layout can be connected, and folders that don't match are ignored. The code editor has no conflict warning and overwrites the GitHub copy ([shopify.dev](https://shopify.dev/docs/storefronts/themes/tools/github)). How this works with this repo's layout is #6's question.
- **Squarespace: no.** The Git-based Developer Platform "is only available in version 7.0" ([Squarespace Help](https://support.squarespace.com/hc/en-us/articles/205815928-Add-custom-code-to-your-site)). New sites are version 7.1, which only offers code blocks and code injection (Core plan and up). You could keep CSS and JavaScript snippets in GitHub, but someone has to paste them in or load them from a CDN. Nothing syncs.
- **Webflow: no.** The site is stored in Webflow. Code export leaves out Ecommerce products and checkout ([Webflow Help](https://help.webflow.com/hc/en-us/articles/33961386739347-How-do-I-export-my-Webflow-site-code)). Webflow Cloud can deploy a separate Next.js or Astro app from GitHub at a path on the site ([developers.webflow.com](https://developers.webflow.com/webflow-cloud/bring-your-own-app)). That is a headless-style add-on, not the store's theme.
- **BigCommerce: partly.** Stencil themes are code and can live in GitHub, but there is no GitHub connection. Deploys go one way, through `stencil push` or a zip upload ([BigCommerce docs](https://docs.bigcommerce.com/developer/docs/storefront/stencil/deployment/upload)), and changes made in the control panel don't flow back to GitHub.
- **WooCommerce: partly.** On WordPress.com Business and Commerce plans, *GitHub Deployments* connects a repo and branch in the browser and deploys a theme or plugin. It goes one way, from GitHub to the site ([WordPress.com support](https://wordpress.com/support/github-deployments/)). Other hosts differ.

**What it means:** only Shopify meets the test as written: code in GitHub, edits in the GUI flowing back to it, and no terminal.

## 5. Adding Australia, the UK and Canada

The tax, duty, pricing-rule and returns questions belong to #10. This section covers only what each platform can do.

- **Shopify.** Shopify Markets is on every plan. Showing local currencies and local payment methods needs Shopify Payments ([pricing](https://www.shopify.com/pricing)). You can set fixed prices for each market through catalogs ([Shopify Help](https://help.shopify.com/en/manual/markets/pricing/product-prices-by-country)). The pages read didn't say whether a plan requirement applies. Shopify adds a **1.5% currency conversion fee** when a customer pays in a currency other than USD ([Shopify Help](https://help.shopify.com/en/manual/markets/pricing/fees)), and **+1%** for international cards ([pricing](https://www.shopify.com/pricing)). Managed Markets probably isn't open to FOSLN (see reason 5 above).
- **Squarespace.** The store has "one primary currency." The help centre now says "multi-currency checkout is on by default" with Squarespace Payments or Stripe, so international customers "can also see prices and pay in their own currency" ([Squarespace Help](https://support.squarespace.com/hc/en-us/articles/206540937-Choose-your-store-s-currency)). But Squarespace's Payments page still says all charges "will be processed in your store currency" ([Squarespace Help](https://support.squarespace.com/hc/en-us/articles/33482360045069-Supported-locations-payment-methods-and-currencies-for-Squarespace-Payments)). Older forum posts say there is no multi-currency checkout at all. This looks like a recent change, and the official pages don't agree with each other. There is no sign of fixed per-country prices. Fees: +1.5% for international cards and +1% for currency conversion ([pricing](https://www.squarespace.com/pricing)).
- **Webflow.** You set one store currency in Ecommerce settings ([Webflow Help](https://help.webflow.com/hc/en-us/articles/33961398627219-Set-up-your-Webflow-Ecommerce-store)). Webflow Localize, its translation and localisation add-on, doesn't cover Ecommerce products, categories or pages ([Webflow Help](https://help.webflow.com/hc/en-us/articles/33961345384339-Webflow-Ecommerce-overview)). Local prices would need a display-only currency app or a separate site per Market.
- **BigCommerce.** Multi-currency is listed on every plan ([pricing](https://www.bigcommerce.com/pricing/)). It needs a Stencil theme, Optimized One-Page Checkout and a gateway that supports multiple currencies ([BigCommerce docs](https://docs.bigcommerce.com/developer/docs/admin/store-configuration/currencies/overview)). Fixed prices for each currency use price lists. BigCommerce's own sources disagree on which plan you need: the help centre says Performance, and the developer docs say Enterprise, the old name ([help](https://support.bigcommerce.com/s/article/Price-Lists?language=en_US); [docs](https://docs.bigcommerce.com/developer/docs/admin/store-configuration/currencies/how-currencies-work)). Either way it is the top plan, from $1,499 a month. On lower plans, prices are converted from USD.
- **WooCommerce.** WooPayments has multi-currency built in, with no extra extension. It converts prices from USD at automatic or manually set exchange rates, with rounding and "charm pricing" (for example, ending prices in .99). It can switch customers to their local currency by location ([WooCommerce docs](https://woocommerce.com/document/woopayments/currencies/multi-currency-setup/)). The docs don't mention fixed per-product prices in each currency. Fees: +1.5% for international cards and +1% for currency conversion ([WooPayments fees](https://woocommerce.com/document/woopayments/fees/)).

**On fees:** a foreign card paying in its own currency costs about 5.4% + 30¢ on all of them. On Shopify that is 2.9% + 1% + 1.5%; on Stripe, Squarespace and WooPayments it is 2.9% + 1.5% + 1%. The fees don't separate the platforms here. Features do: Shopify can set a fixed local price for each Market; BigCommerce can only on its top plan; Squarespace, Webflow and WooCommerce only convert from USD, or don't support local currencies at all.

## 6. Monthly cost at launch volume

The launch volume isn't known, so this shows two cases: **30 orders a month** ($2,910 in sales) and **100 orders a month** ($9,700). Each total is the plan fee, plus card fees at 2.9% + 30¢, plus any platform fee.

| Platform and plan | Plan fee: monthly billing (yearly billing) | 30 orders | 100 orders |
| --- | --- | --- | --- |
| Shopify Basic | $39 ($29) | $132 ($122) | $350 ($340) |
| Squarespace Core | $39 ($29) | $132 ($122) | $350 ($340) |
| Squarespace Basic (2% fee, no JavaScript) | $25 ($19) | $177 ($171) | $530 ($524) |
| Webflow Ecommerce Standard (2% fee) | $42 ($29) | $194 ($181) | $547 ($534) |
| Webflow Ecommerce Plus | $84 ($74) | $177 ($167) | $395 ($385) |
| BigCommerce Core, under its $30K cap | $39 ($29) | $132 ($122) | – |
| BigCommerce Growth, under its $100K cap | $105 ($79) | $198 ($172) | $416 ($390) |
| BigCommerce Scale, above $100K | $399 ($299) | – | $710 ($610) |
| WooCommerce on WordPress.com Business | $40 ($25) | $133 ($118) | $351 ($336) |
| WooCommerce on WordPress.com Commerce | $70 ($45) | $163 ($138) | $381 ($356) |

Sources for plan fees: [Shopify](https://www.shopify.com/pricing), [Squarespace](https://www.squarespace.com/pricing), [Webflow](https://webflow.com/pricing), [BigCommerce](https://www.bigcommerce.com/pricing/), and the WordPress.com plan list from its public plans API (`public-api.wordpress.com/rest/v1.5/plans`), which matches [wordpress.com/pricing](https://wordpress.com/pricing/).

Notes on the table:

- **BigCommerce's caps bite early.** Plans are capped by sales over the last 12 months. BigCommerce counts sales including tax and shipping, then takes off 10%. Core allows up to $30,000 a year and Growth up to $100,000; above a cap, "you'll be notified and automatically upgraded" ([pricing](https://www.bigcommerce.com/pricing/)). At $97 an order, Core runs out at about **29 orders a month**, and Growth at about **95 a month**, fewer if shipping or tax is charged. A store doing 30 orders a month moves to Growth within its first year. One doing 100 a month ends up on Scale at $399.
- **Webflow Standard against Plus.** The 2% fee makes Standard cost more than Plus once sales pass about $2,100 to $2,250 a month, which is 22 to 23 orders.
- **Squarespace Basic** is cheaper than Core only below about $500 to $700 a month in sales. It also lacks JavaScript and code injection, so it can't carry a custom design.
- **Introductory offers.** Shopify currently offers a 3-day trial and then $1 a month for 3 months on most plans ([pricing](https://www.shopify.com/pricing)). Squarespace shows "20% off" labels on its plans, which may be a promotion. WordPress.com renewal prices may differ from first-term prices.
- **Tax calculation.** Shopify Tax is free up to $100,000 of lifetime sales ([Shopify Help](https://help.shopify.com/en/manual/taxes/shopify-tax/pricing)). Squarespace's automated tax costs 0.15% per order on Core, capped at $0.99 ([pricing](https://www.squarespace.com/pricing)). Webflow lists automatic tax calculation on its Ecommerce plans ([pricing](https://webflow.com/pricing)). BigCommerce and WooCommerce tax costs weren't checked.
- **Shopify's $0 "Agentic" plan** is for listing products in AI shopping channels ([pricing](https://www.shopify.com/pricing)). The page doesn't say it includes an online store, so it isn't an option for fosln.com.

**What it means:** at launch volume, Shopify Basic, Squarespace Core, BigCommerce Core (at first) and WooCommerce on WordPress.com Business all cost about the same, around $120 to $135 a month at 30 orders. Card fees are most of that cost. Webflow costs more, and BigCommerce costs more as soon as sales grow.

## Each alternative in one paragraph

- **Squarespace** is the strongest alternative for the owner: same price as Shopify, the most familiar GUI, and checkout on your own domain ([Squarespace Help](https://support.squarespace.com/hc/en-us/articles/115004908508-Checkout-on-your-domain)). It fails the GitHub test outright. Custom design is limited to injected CSS and JavaScript that Squarespace won't support. And the China dropshipping tools mostly skip it. All of this works against the plan for Claude Code sessions to build a custom, locked design.
- **Webflow** gives the most design freedom, including a fully custom checkout. But for this brief it is the weakest store: one currency, Localize not covering Ecommerce, a 2% fee on Standard, no Git, and no China dropshipping integrations.
- **BigCommerce** matches Shopify's fees at the very start, and its multi-currency is listed on every plan. But its low sales caps make it the most expensive option once FOSLN reaches about 30 orders a month. Its themes deploy through a CLI rather than a GitHub sync, and support for the China tools is patchy.
- **WooCommerce** has the least lock-in, good support from China dropshipping tools, built-in multi-currency, and one-way GitHub deploys on WordPress.com. The cost is upkeep: WordPress, plugins and hosting become the owner's responsibility, which goes against running the store from GUIs without technical help.

## Which way this points

This feeds #21 (platform and architecture) and doesn't decide it.

- The facts point to **keeping Shopify** as the working assumption. No alternative is better on balance, and Shopify is the only one that passes the GitHub-sync test without a terminal.
- **What could change that:** if the locked visual direction needs a checkout that looks unlike Shopify's branded checkout (#14, #18), that argues for Webflow or for Plus-level spending. If the owner values Squarespace familiarity over code in GitHub, Squarespace Core is the realistic fallback at the same price.
- **Ask the Supplier now** which store platforms its app or order system connects to. That answer can override everything in section 2 (#26).

## Recent changes to watch

- **BigCommerce, 1 June 2026:** plans renamed (Standard to Core, Plus to Growth, Pro to Scale, Enterprise to Performance). The Core cap is now $30,000 of yearly sales. A new fee of up to 2% applies to orders paid through providers outside BigCommerce's "embedded" list ([BigCommerce 2026 update](https://www.bigcommerce.com/dm/plan-pricing-updates-2026/)).
- **Shopify Tax, 13 May 2026:** new stores get $100,000 of *lifetime* free tax calculation, not $100,000 a year ([Shopify Help](https://help.shopify.com/en/manual/taxes/shopify-tax/pricing)).
- **Shopify, 12 May 2025:** stores created since then pay third-party fees on amounts paid by gift card or store credit ([Shopify Help](https://help.shopify.com/en/manual/markets/pricing/fees)).
- **Squarespace:** plan prices are now $19, $29, $49 and $99 a month billed yearly, which is higher than many 2025 write-ups show. Multi-currency checkout appears to be new.
- **Shopify:** the pricing page no longer shows the Starter plan, and it now lists a $0 Agentic plan.

## Not verified

- **The Supplier's integrations.** This is the biggest unknown, and only the Supplier can answer it.
- **CJdropshipping's current platform list.** Its site was behind a bot check, so the listing used here comes from older third-party pages.
- **DSers on BigCommerce.** A Marketplace listing page exists, but DSers' own site doesn't list BigCommerce.
- **The plan Squarespace's Commerce API needs:** the pricing page says Core and up, while the developer docs mention the old "Commerce Advanced" plan.
- **How Squarespace's multi-currency checkout works**, including its exchange rates and whether Markets can have fixed prices. Squarespace's own pages contradict each other.
- **Whether Shopify's fixed per-market prices need a particular plan.**
- **Whether Order Desk can send orders to FOSLN's Supplier.**
- **Tax-calculation costs on BigCommerce and WooCommerce.**
- **The US-business assumption.** If FOSLN's business isn't based in the US, every fee figure here needs checking again.
