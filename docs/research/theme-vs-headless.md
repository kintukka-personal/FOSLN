# Can a Shopify theme carry the FOSLN designs?

Research for [#5](https://github.com/Kintukka2/FOSLN-WATCH/issues/5), part of the [fosln.com v1 map (#4)](https://github.com/Kintukka2/FOSLN-WATCH/issues/4). Researched on 2026-10-09.

## Short answer

1. **Yes.** Everything both reference designs do runs in the browser as plain HTML, CSS and vanilla JavaScript, with no dependencies. A Shopify Online Store 2.0 theme can ship that code as it is. The product card already has a working theme section that adds to the cart through the Cart AJAX API.
2. **Nothing in either design forces a headless build.** A headless build would run the same browser code, so it would not fix the main Core Web Vitals risk. The Acier hero keeps its watch photo hidden for about 4.7 seconds, and in lab runs that pushed Largest Contentful Paint (LCP) to about 5.2 seconds, which Google rates "poor". Fixing that is a design choice on either stack.
3. **Headless costs more to run when the owner never uses a terminal.** Oxygen hosting is free on paid plans, but the owner loses the theme editor and theme-based apps. Every quarterly API upgrade and every security patch also needs a Claude Code session. Next.js on Vercel adds at least $20 a month, because Vercel's free plan bans commercial use.
4. **The facts point to a theme.** The decision belongs to [#21](https://github.com/Kintukka2/FOSLN-WATCH/issues/21).

---

## 1. What the designs need, and whether a theme can do it

I read the source of both designs: `acier-reveal/` (README, `index.html`, `style.css`, `script.js`) and `product-card-watch/` (the same files, plus `shopify/ui-wcard.liquid` and `shopify/ui-wcard-shopify.js`).

| Feature | How the code does it | Can a theme do it? |
| --- | --- | --- |
| **Timeline hero, about 17 s** | One `requestAnimationFrame` loop calls `Stage.render(t)`. The beats are constants in `script.js`: the poster at 4.69 s, the callouts from 7.49 s, the sign-off at 14.79 s, and the end at **16.79 s**. | Yes. It is browser JavaScript that would ship as a theme asset. |
| **Live hands on the visitor's local time** | A second loop reads `new Date()` on every frame and rotates the SVG hands. | Yes. Only the browser knows the visitor's local time, so server rendering gives headless no edge here. |
| **ACIER drawn as SVG steel bands** | `buildWord()` builds five letter clip paths at runtime, each holding five gradient bands. The light is a shifted `gradientTransform`. | Yes. This is plain DOM and SVG work. |
| **Light that follows the pointer** | `pointermove` sets CSS custom properties. The light is a radial gradient, masked by the watch photo through CSS `mask: url(...)`. | Yes. Theme assets are served from the store's own domain, under `/cdn/shop/...` ([Shopify: platform performance](https://shopify.dev/docs/storefronts/themes/best-practices/performance/platform)), so the mask image is same-origin. |
| **Scroll-driven exit into the product section** | Non-passive `wheel`, `touchmove` and `keydown` listeners on `window` catch the first scroll after the sign-off. The pieces then "drain" into the crown and the script scrolls the page. | Yes. I found no theme rule against it. The script's scroll target needs to allow for any sticky theme header. |
| **Product card: Dial switching** | The swatches are radio inputs. The Liquid section maps each value of the colour or Dial option to a variant, its price and its stock state. | Already built as a section: `sections/ui-wcard.liquid`. |
| **Product card: add to cart** | `ui-wcard-shopify.js` posts to `/cart/add.js`, reads `/cart.js`, and refreshes a Dawn-style `#cart-icon-bubble` through the `?sections=` parameter. | Already built. Shopify says the Ajax API "can only be used by themes that are hosted by Shopify" and "you can't use the Ajax API on a Shopify custom storefront" ([Shopify: Ajax API](https://shopify.dev/docs/api/ajax)). This part is theme-only. |

The theme platform does set some limits. None of them block these designs:

- A JSON template can hold up to 25 sections, each with up to 50 blocks ([Shopify: JSON templates](https://shopify.dev/docs/storefronts/themes/architecture/templates/json-templates)).
- A template can use another layout file, or no layout at all. However, "Templates without a layout can't be customized in the theme editor" (same source). The Acier landing page would therefore need its own layout file, not `"layout": false`.
- `assets/` cannot have subfolders, because "Subdirectories, other than the ones listed, aren't supported" ([Shopify: theme architecture](https://shopify.dev/docs/storefronts/themes/architecture)). So `images/acier-watch.webp` becomes `acier-watch.webp`, and the CSS `url()` paths change to match.

### Work needed to port the designs into a theme

These are build tasks, not decisions.

- **Acier as a section.** Move the hero and the bench into one section, and give the page-level styles (`.ui-acier-page`, the skip link and the footer) to the theme's layout. Expose the copy, images, price and Dial as section settings. Today `$97 / $225` is hard-coded, and the product card shows how to read the price from the variant instead.
- **Acier add to bag.** The button currently only fires `ui-acier:add`. Connect it to `/cart/add.js` the way the product card does.
- **Theme editor support.** The editor re-renders a section without reloading the page, so its JavaScript must start again on `shopify:section:load` and clean up on `shopify:section:unload`. `Shopify.designMode` tells the script it is running in the editor ([Shopify: integrate sections with the editor](https://shopify.dev/docs/storefronts/themes/best-practices/editor/integrate-sections-and-blocks)). The product card already handles this. The Acier script runs once over `document` and does not, and it should probably not autoplay inside the editor.
- **No flash before the script runs.** Without JavaScript, the Acier stage shows the poster, through the `:not([data-phase])` styles. Theme scripts are normally loaded with `defer`, so the browser could paint the poster before the script switches the stage to the quartz intro. A small inline guard in the section would prevent that flash.
- **Fonts.** See section 2.

### Fitting CADRE into the same sections

The map wants every decision made for both Models. Two things in the Acier code are specific to ACIER, and they are design questions for [#14](https://github.com/Kintukka2/FOSLN-WATCH/issues/14), not platform questions:

- **The round dial.** The intro's round minute ring shrinks onto "exactly the size and position of the watch dial", using a radius (`DIAL.r = 118`). CADRE has a square case, so this match cut needs a new idea for CADRE.
- **The steel letters.** They are five hand-drawn paths for A, C, I, E and R. CADRE reuses C, A, R and E, but needs a new D. The photo constants (`HANDS_C`, `DIAL`, `WATCH_W`) are also per watch. In a theme they can become section settings, so one section serves both Models.

---

## 2. Core Web Vitals

The targets: LCP of 2.5 s or less, Interaction to Next Paint (INP) of 200 ms or less, and Cumulative Layout Shift (CLS) of 0.1 or less, each measured at the 75th percentile of page loads ([web.dev: Web Vitals](https://web.dev/articles/vitals)). An LCP over 4.0 s is "poor" ([web.dev: LCP](https://web.dev/articles/lcp)).

### What the theme platform gives you

- **Streamed HTML.** Shopify sends the `<head>` before the sections finish rendering, but "only JSON-template pages qualify for now". It also sends HTTP 103 Early Hints for render-blocking CSS and JS, "on every storefront", and serves files over HTTP/3, TLS 1.3 and Brotli from a "CDN backed by Cloudflare" ([Shopify: platform performance](https://shopify.dev/docs/storefronts/themes/best-practices/performance/platform)).
- **Faster first byte.** On 2026-09-22 Shopify reported that streaming cut time to first byte for the median store by about 29% at p75. LCP improved less, by 2 to 6% ([Performance @ Shopify](https://performance.shopify.com/blogs/blog/liquid-storefronts-now-start-loading-29-faster)). This is recent, and Shopify's own measurement.
- **Field data in the admin.** Online Store › Themes and Analytics › Reports show LCP, INP and CLS from "real user data from the past 30 days" ([Shopify Help: web performance reports](https://help.shopify.com/en/manual/online-store/web-performance/web-performance-reports)). The page doesn't say whether this covers headless storefronts.
- **A cost you can't remove.** `{{ content_for_header }}` is required in `theme.liquid`, it "dynamically loads all scripts required by Shopify", and "You shouldn't try to modify or parse" it ([Shopify: layouts](https://shopify.dev/docs/storefronts/themes/architecture/layouts)). Every app embed you add increases this weight.
- **A benchmark.** Theme Store themes need "a minimum average Lighthouse performance score of 60" across the home, product and collection pages ([Shopify: Theme Store requirements](https://shopify.dev/docs/storefronts/themes/store/requirements)). That rule is for Theme Store submissions, not for a custom theme, but it shows what Shopify considers acceptable.

### What the designs do to Core Web Vitals, on any stack

**Acier hero, LCP.** The watch photo, `.ui-acier__watch-img`, sits inside `.ui-acier__camera`, which has `opacity: 0` in CSS until the phase changes to `poster` at 4.69 s. Chrome ignores paints at opacity 0 for LCP, since Chrome 86 ([Chromium LCP changelog](https://chromium.googlesource.com/chromium/src/+/refs/heads/main/docs/speed/metrics_changelog/lcp.md)). Shopify warns about exactly this pattern: "Fade-in and reveal animations delay the LCP event even after the image finishes downloading" ([Shopify: don't hide the LCP image behind animations](https://shopify.dev/docs/storefronts/themes/best-practices/performance/dont-hide-lcp-image-behind-animations)).

I measured this in the lab with Playwright and headless Chromium 141, on a 412 × 823 viewport, with the files served from a local static server:

| Run | Final LCP element | LCP |
| --- | --- | --- |
| No CPU throttling, 5 runs | `ui-acier__watch-img` | **5.13–5.24 s**, every run |
| 4× CPU throttling, 5 runs | `ui-acier__watch-img` | 0.43–0.64 s, at first paint, every run |
| Reduced motion | `ui-acier__watch-img` | 0.42 s |

Caveats on these numbers:

- **Lab only.** This is not field data. Google Fonts loaded over the network, and the latency of real hosting was not part of the test.
- **Unexplained result.** I could not explain why the throttled runs counted the hidden photo at first paint. A logging script confirmed that the stage was already in the `quartz` phase before that paint.
- **Read the result as unstable.** On fast devices the hero can report LCP at about 5 s. Because that time is set by the hero's own choreography, faster hosting, a faster server or a headless build would not change it.

The options sit with [#14](https://github.com/Kintukka2/FOSLN-WATCH/issues/14): show the photo sooner, shorten the quartz beat, start from the poster, or accept the score. In the reduced-motion path the poster appears at once, with LCP at 0.42 s.

**Product card, LCP.** The watch image is the LCP element. In the demo it painted at first paint, about 0.5 s, with or without reduced motion. The card still hides its pieces through a class that JavaScript adds (`is-pending`, then an animated "drop" about 0.9 s later). In the Shopify section the scripts load with `defer`, so the card could paint, vanish and then animate back in. Shopify's guidance is to leave the LCP image unanimated. I did not test the section in a real theme. Two more points:

- The section loads both Dial images eagerly, although only one is visible.
- It loads Manrope from Google Fonts, although Manrope is in Shopify's own font library (`manrope_n2`…`manrope_n8`) ([Shopify: font library](https://shopify.dev/docs/storefronts/themes/architecture/settings/fonts)).

**Fonts.** Acier loads three families in nine weights and styles from Google Fonts. Shopify says to "self-host it on the Shopify CDN instead of loading it from an external service such as Google Fonts", and warns that "more than 6 total font files impacts performance" ([Shopify: self-host web fonts](https://shopify.dev/docs/storefronts/themes/best-practices/performance/self-host-web-fonts)). Libre Baskerville is in Shopify's font library. Hanken Grotesk and JetBrains Mono are not, so they would be uploaded to `assets/` as WOFF2 files ([Shopify: font library](https://shopify.dev/docs/storefronts/themes/architecture/settings/fonts)).

**INP and CLS. Not measured.**

- **INP risk.** When motion is allowed, the Acier clock loop runs on every frame for as long as the page is open, even when the hero is off screen. The 17-second timeline also makes more than a hundred SVG attribute and style writes on each frame. It is worth measuring INP on a mid-range phone.
- **CLS risk.** Almost all motion uses `transform` and `opacity`, which do not move the layout, so CLS risk looks low.

The same code would behave the same way on a headless stack.

---

## 3. What would force a headless build

These are the usual reasons a theme can't do the job. None of them applies to the two designs.

- **Full control of the page head, including Shopify's own scripts.** `content_for_header` is required and can't be changed ([Shopify: layouts](https://shopify.dev/docs/storefronts/themes/architecture/layouts)). It would matter only if Shopify's injected scripts broke a performance budget, and that has not been measured here.
- **A URL structure Shopify won't serve.** `/products` and `/collections` are "fixed Shopify paths" ([Shopify Help: URL redirects](https://help.shopify.com/en/manual/online-store/menus-and-links/url-redirect)). The designs don't need any other structure.
- **Server-rendered data from somewhere other than Shopify.** Liquid renders from Shopify's own objects, such as products, metafields and metaobjects. Browser JavaScript can still call other services. This is my reading of the Liquid reference: I found no Liquid tag that calls an outside server during rendering. The designs need nothing like this. The live time is read in the browser.
- **One codebase shared with another app, or several storefronts from one codebase.** Neither is in v1 scope ([#4](https://github.com/Kintukka2/FOSLN-WATCH/issues/4)).

---

## 4. What a headless build would cost under the no-terminal rule

### Costs common to both headless options

- **The theme editor goes.** With a theme, the owner changes text, images, colours and Dial blocks in the editor. The product card section already exposes 17 section settings, plus 13 on each Dial block. Shopify's GitHub integration commits those edits back to the repo, and code pushes update the theme ([Shopify: GitHub integration](https://shopify.dev/docs/storefronts/themes/tools/github)). Headless has no editor:
  - **Metaobjects as a replacement.** Content can live in metaobjects in the admin, but a developer must build each new kind of content section in six code steps. Shopify's recipe lists "Shopify Plus or development store" as a prerequisite ([Shopify: metaobjects in Hydrogen](https://shopify.dev/docs/storefronts/headless/hydrogen/cookbook/metaobjects)). I couldn't confirm whether that Plus requirement is current, since the help centre's metaobjects page states no plan limit ([Shopify Help: metaobjects](https://help.shopify.com/en/manual/custom-data/metaobjects)).
  - **Hydrogen visual editor.** Shopify announced one in June 2024, saying "We will keep you posted as we make progress towards early access" ([Hydrogen update, June 2024](https://hydrogen.shopify.dev/update/summer-editions-24-announcing-the-hydrogen-visual-editor-and-a-ton-of-dx-improvements)). I found no general release. **Unverified.**
- **Theme-based apps go.** Apps inject into a theme through app blocks and app embeds ([Shopify: theme app extensions](https://shopify.dev/docs/apps/build/online-store/theme-app-extensions)). A headless storefront doesn't render a Shopify theme, so those apps have nowhere to appear. Each storefront app would need its own headless integration, and Shopify keeps a separate App Store list of [custom storefront apps](https://apps.shopify.com/collections/custom-storefront-apps). This is an inference from how theme app extensions work. Shopify has no single page that lists which apps break. It matters for later tickets on reviews, email capture and consent. Back-office apps, such as order routing to the Supplier, don't touch the storefront, so they should be unaffected. That is also an inference ([#26](https://github.com/Kintukka2/FOSLN-WATCH/issues/26)).
- **The product card's cart code must be rewritten.** The Ajax Cart API doesn't work on a custom storefront ([Shopify: Ajax API](https://shopify.dev/docs/api/ajax)), so the cart calls would move to the Storefront API.
- **Analytics and consent must be wired by hand.** In a theme they come in through `content_for_header`. In Hydrogen, "If you haven't configured consent through the Customer Privacy API, then analytics events won't fire and no data is tracked" ([Shopify: Hydrogen analytics tracking](https://shopify.dev/docs/storefronts/headless/hydrogen/analytics/tracking)).
- **Checkout moves to a subdomain.** Shopify's headless guide puts the storefront on the main domain and Shopify checkout on a subdomain such as `checkout.example.shop` ([Shopify: redirect traffic to Hydrogen](https://shopify.dev/docs/storefronts/headless/hydrogen/migrate/redirect-traffic)). That bears on the map's "checkout on-domain" assumption and on [#8](https://github.com/Kintukka2/FOSLN-WATCH/issues/8).
- **The server becomes FOSLN's to maintain.** With a theme, Shopify runs the renderer, and a Liquid theme does not pin a versioned API. Headless brings a framework, its dependencies and an API version, and each one ages.

### Hydrogen on Oxygen

- **Hosting is free.** "Oxygen is available at no extra charge on paid Shopify plans" ([Shopify: Hydrogen fundamentals](https://shopify.dev/docs/storefronts/headless/hydrogen/fundamentals)). The Basic, Grow and Advanced plans get one public environment each ([Shopify: environments](https://shopify.dev/docs/storefronts/headless/hydrogen/environments)). Development stores and trial plans get none, so their deployment URLs "always require a store login" ([Shopify: Hydrogen fundamentals](https://shopify.dev/docs/storefronts/headless/hydrogen/fundamentals)). Previews are private by default and need a staff login, which suits an owner who reviews in the browser ([Shopify: deployments](https://shopify.dev/docs/storefronts/headless/hydrogen/deployments)).
- **Deploying works through GUIs.** The owner sets up the Hydrogen channel in the admin and picks a GitHub repo. Shopify opens a pull request that adds a GitHub Actions workflow, and that pull request must be merged. After that, every push makes a preview deployment, and the default branch is production ([Shopify: deploy with GitHub](https://shopify.dev/docs/storefronts/headless/hydrogen/deployments/github), [Shopify: environments](https://shopify.dev/docs/storefronts/headless/hydrogen/environments)). Claude Code sessions push the code, and the owner never needs a terminal.
- **Upkeep is quarterly.** Shopify releases an API version every quarter, and each stable version lasts "a minimum of 12 months" ([Shopify: API versioning](https://shopify.dev/docs/api/usage/versioning)). Hydrogen releases follow those versions. Releases can break things: v2026.4.0, on 2026-04-09, made the Storefront API proxy always-on and changed how consent cookies work ([Shopify changelog](https://shopify.dev/changelog/posts/hydrogen-april-2026-release)). Expect at least one Claude Code upgrade session a year, and probably four.
- **Platform limits.**
  - Workers are capped at 10 MB, 128 MB of memory and 30 s of CPU per request ([Shopify: Hydrogen fundamentals](https://shopify.dev/docs/storefronts/headless/hydrogen/fundamentals)).
  - Oxygen "doesn't support proxies in front of deployments" (same source). That bears on [#8](https://github.com/Kintukka2/FOSLN-WATCH/issues/8), since fosln.com's DNS is at Cloudflare.

### Next.js with the Storefront API on Vercel

- **Storefront API access.** It comes through Shopify's Headless channel. "Each storefront has its own set of API tokens" ([Shopify: bring your own stack](https://shopify.dev/docs/storefronts/headless/bring-your-own-stack)).
- **Hosting cost.** The free Hobby plan is "restricted to non-commercial personal use only", and any site that takes payment counts as commercial ([Vercel: fair use guidelines](https://vercel.com/docs/limits/fair-use-guidelines)). Pro costs "$20/mo" with "$20 included credit". It covers 1 TB of transfer, then "starting at $0.15 per GB" ([Vercel pricing](https://vercel.com/pricing)). Vercel's seat table lists a developer seat at $20 a month, so the total depends on how many people need a seat.
- **Deploying works through GUIs.** Vercel deploys every branch push as a preview with its own URL, and merges to the production branch go live ([Vercel: Git deployments](https://vercel.com/docs/git)). A private repo inside a GitHub organisation can't deploy on Hobby anyway (same source).
- **Security patches are urgent.** CVE-2025-55182 ("React2Shell") was a CVSS 10.0 remote code execution flaw in React Server Components. Next.js was affected, and fixes followed on 2025-12-03 and 2025-12-11 ([React blog](https://react.dev/blog/2025/12/03/critical-security-vulnerability-in-react-server-components), [Next.js advisory](https://nextjs.org/blog/CVE-2025-66478)). Vercel added free automated fix pull requests on 2025-12-08, but someone still has to check and merge them ([Vercel changelog](https://vercel.com/changelog/automated-react2shell-vulnerability-patching-is-now-available)). For FOSLN, each such event means a Claude Code session within days.
- **A new, unproven option.** Hydrogen's framework-agnostic developer preview, from June 2026, includes a Next.js template that deploys to Vercel. Shopify still points to current Hydrogen as its "fully supported path" ([Shopify: Hydrogen developer preview](https://shopify.dev/docs/storefronts/headless/developer-preview)). It is a preview, so not a production basis yet.

### Running costs side by side

US prices from [shopify.com/pricing](https://www.shopify.com/pricing), checked 2026-10-09.

| | Theme | Hydrogen on Oxygen | Next.js on Vercel |
| --- | --- | --- | --- |
| Shopify plan, at least | Basic, $39/mo, or $29/mo billed yearly | Same | Same |
| Hosting | Included | $0 extra | Pro from $20/mo, plus usage |
| Theme editor for the owner | Yes | No | No |
| Theme app blocks and embeds | Yes | No | No |
| Cart code in the designs | Works as is | Rewrite for the Storefront API | Rewrite for the Storefront API |
| Forced upgrades | None found | Quarterly API versions, each lasting at least 12 months | Same, plus Next.js and React patches |
| Where the owner reviews changes | Theme preview, plus the GitHub sync | Oxygen previews (staff login) | Vercel previews |

---

## 5. Recent changes to watch

- **Canvas, launched 2026-10-01, in early access.** It is a new design workspace where "Sidekick now works directly on the theme's files" ([Shopify News](https://www.shopify.com/news/introducing-canvas)). It works with Shopify's own themes and with custom themes. It does not yet support "Markets, rollouts, translations, or app blocks and app embeds", and "Your original theme stays in the current theme editor" ([Shopify changelog](https://changelog.shopify.com/posts/design-a-fully-bespoke-store-with-canvas)). For an owner who never uses a terminal, it may become a GUI way to change theme code. It has no headless equivalent. **Unverified:** how Canvas behaves with a theme connected to GitHub, since Canvas works on a duplicate theme. It is eight days old.
- **Skeleton theme, now block-first.** Shopify's Skeleton theme "deliberately excludes the section-based model": "No sections/ directory… No JSON templates" ([Skeleton README](https://github.com/Shopify/skeleton-theme/blob/main/README.md)). Its `partial` tag also needs `shop.features.agentic_editor_enabled?`, a Shopify feature flag. Shopify recommends Skeleton as the start for new theme projects ([Horizon README](https://github.com/Shopify/horizon/blob/main/README.md)). Horizon itself still uses JSON templates with sections and theme blocks, as in its `templates/product.json`. Two consequences:
  - The product card is a classic section. It fits a theme with JSON templates and sections, such as Horizon or Dawn, as it stands. It would need converting to blocks for a Skeleton-style theme.
  - Streamed HTML currently covers JSON-template pages only (section 2), so Skeleton's `.liquid` templates miss that speed-up today.

  Which theme base to use is for [#21](https://github.com/Kintukka2/FOSLN-WATCH/issues/21).
- **Hydrogen developer preview, from June 2026.** It separates Hydrogen from React Router and supports Next.js, SvelteKit, Nuxt, SolidStart and others ([Shopify docs](https://shopify.dev/docs/storefronts/headless/developer-preview), [release notes](https://hydrogen.shopify.dev/updates)). It is not a production option yet.
- **Plan names and prices have changed.** The pricing page now lists Basic, Grow, Advanced, Plus and a $0 "Agentic" plan for selling in AI channels ([Shopify pricing](https://www.shopify.com/pricing)). Oxygen "isn't available on Agentic plans" ([Shopify: Hydrogen fundamentals](https://shopify.dev/docs/storefronts/headless/hydrogen/fundamentals)), and "The Starter plan isn't available to new stores" ([Shopify Help: Starter plan](https://help.shopify.com/en/manual/intro-to-shopify/pricing-plans/plans-features/shopify-starter-plan)).

---

## 6. Which way the facts point

They point to a **Shopify theme** built on JSON templates, for these reasons:

- Both designs are plain client-side code that a theme can host without changes. The product card's Shopify wiring only works in a theme.
- Headless doesn't improve the main Core Web Vitals risk, which comes from the hero's choreography. It does take away the owner's GUI controls: the theme editor, theme app blocks, and possibly Canvas.
- Headless adds recurring engineering work: API upgrades and urgent framework patches. Under the no-terminal rule, each one is a Claude Code session the owner has to start.

That matches the map's working assumption: "headless only if needed".

**Open or unverified:**

- Real-device and field Core Web Vitals for a built theme.
- Why the throttled lab runs counted the hidden photo early.
- Whether metaobject content sections need Plus.
- The status of the Hydrogen visual editor.
- How Canvas works with a GitHub-connected theme.
- Which storefront apps FOSLN will need. That depends on later tickets.

## Notes for other tickets

- **[#14](https://github.com/Kintukka2/FOSLN-WATCH/issues/14), visual direction.** The Acier hero's LCP of about 5 s comes from its choreography, and the round dial match cut doesn't carry over to CADRE's square case (section 1).
- **[#24](https://github.com/Kintukka2/FOSLN-WATCH/issues/24) and [#13](https://github.com/Kintukka2/FOSLN-WATCH/issues/13), price presentation.** The struck-through price differs between the designs: $225 in Acier and $249 in the product card. Once wired to Shopify, it comes from the variant's compare-at price in the admin.
- **[#8](https://github.com/Kintukka2/FOSLN-WATCH/issues/8), domain and checkout.** Headless moves checkout to a subdomain, and Oxygen doesn't support a proxy in front of it.
- **[#6](https://github.com/Kintukka2/FOSLN-WATCH/issues/6), GitHub sync.** "Only branches that match the default Shopify theme folder structure can be connected" ([Shopify: GitHub integration](https://shopify.dev/docs/storefronts/themes/tools/github)).
