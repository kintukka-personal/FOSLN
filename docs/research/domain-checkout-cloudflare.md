# Putting checkout on fosln.com via Cloudflare

Research for [#8](https://github.com/Kintukka2/FOSLN-WATCH/issues/8). Researched 2026-10-09. The decision itself belongs to [#22](https://github.com/Kintukka2/FOSLN-WATCH/issues/22) (domain and checkout setup) and [#21](https://github.com/Kintukka2/FOSLN-WATCH/issues/21) (platform and architecture).

## Short answer

- **Shopify theme:** Shopify serves both the store and checkout on fosln.com. Cloudflare only answers DNS. You need three records, all set to **DNS only** (grey cloud): `A @ 23.227.38.65`, `AAAA @ 2620:0127:f00f:5::` and `CNAME www shops.myshopify.com`. Shopify's "Connect automatically" flow now supports Cloudflare, so this can be done entirely in the two dashboards. Shopify issues the TLS certificate itself and redirects whichever of apex or www isn't primary.
- **Headless:** the storefront lives on fosln.com, hosted on Shopify's Oxygen or somewhere else. Checkout stays on Shopify, on a subdomain such as `checkout.fosln.com`. I found no Shopify document that puts checkout on the same hostname as a headless storefront.
- **Main pitfalls:** turning Cloudflare's proxy on (Cloudflare now silently enables "Orange-to-Orange", which Shopify says it doesn't support), stray A, AAAA, CAA or wildcard records, Cloudflare redirect rules that can't act on DNS-only records, and email. Email needs its own records: inbound MX, plus Shopify's sender CNAMEs and one DMARC record.

## Context

- fosln.com is registered at Cloudflare Registrar, and its DNS is on Cloudflare (`kristina.ns.cloudflare.com`, `moura.ns.cloudflare.com`). Cloudflare Registrar domains must use Cloudflare nameservers. Using anyone else's nameservers means moving the domain to another registrar ([Cloudflare Registrar FAQ](https://developers.cloudflare.com/registrar/faq/)). Every option below therefore works through DNS records in Cloudflare's dashboard, not through a nameserver change.
- The owner works only in GUIs. Every step below is a Cloudflare dashboard or Shopify admin step.
- This note assumes Shopify, the working assumption on the map. Whether to use Shopify, and whether to go headless, are decisions for #7 and #21.

---

## 1. Shopify theme (Online Store)

### 1.1 How it fits together

With a Shopify theme, Shopify hosts the storefront, the cart and checkout. Cloudflare's only job is to answer DNS lookups with Shopify's addresses. Shopify's Cloudflare guide says you can keep Cloudflare as your DNS provider "as long as you set every record that points to Shopify to DNS only" ([Shopify: troubleshooting domains](https://help.shopify.com/en/manual/domains/troubleshoot-issues-with-domains)).

### 1.2 DNS records

From Shopify's Cloudflare-specific guide ([Shopify: Connecting a Cloudflare domain](https://help.shopify.com/en/manual/domains/add-a-domain/connecting-domains/connecting-to-cloudflare)) and its general manual guide ([Shopify: connect manually](https://help.shopify.com/en/manual/domains/add-a-domain/connecting-domains/connect-domain-manual)):

| Type | Name | Content | Proxy status |
|---|---|---|---|
| A | `@` (fosln.com) | `23.227.38.65` | DNS only |
| AAAA | `@` (fosln.com) | `2620:0127:f00f:5::` (expanded form `2620:0127:f00f:0005:0000:0000:0000:0000` if the short form is rejected) | DNS only |
| CNAME | `www` | `shops.myshopify.com` | DNS only |
| TXT | `shopify_verification` | value shown in Shopify admin | only if Shopify asks for it |

Rules that go with these:

- Delete any other A or AAAA record on the apex. Shopify allows exactly one A, one AAAA and one `www` CNAME ([Shopify: connect manually](https://help.shopify.com/en/manual/domains/add-a-domain/connecting-domains/connect-domain-manual)). Extra AAAA records send some visitors elsewhere at random ([Shopify: troubleshooting domains](https://help.shopify.com/en/manual/domains/troubleshoot-issues-with-domains)).
- Don't add a wildcard record (`*.fosln.com`) pointing at Shopify ([Shopify: Connecting a Cloudflare domain](https://help.shopify.com/en/manual/domains/add-a-domain/connecting-domains/connecting-to-cloudflare)).
- Keep the default TTL. Keep records for other subdomains, such as mail ([same page](https://help.shopify.com/en/manual/domains/add-a-domain/connecting-domains/connecting-to-cloudflare)).
- After connecting, Shopify may show the A record with a different last number, such as `23.227.38.32`. Shopify says that is still valid ([Shopify: connect manually](https://help.shopify.com/en/manual/domains/add-a-domain/connecting-domains/connect-domain-manual)).

**Automatic connection.** Shopify lists Cloudflare, with GoDaddy and IONOS, as a provider that supports "Connect automatically". The flow is Settings > Domains > Connect existing domain > Connect automatically, then log in to Cloudflare and click Connect ([Shopify: connect automatically](https://help.shopify.com/en/manual/domains/add-a-domain/connecting-domains/connect-domain-automatic)). It uses the Domain Connect standard ([Shopify: email rewrites](https://help.shopify.com/en/manual/intro-to-shopify/initial-setup/email-rewrites)). Shopify's public Domain Connect template writes exactly these records: A `@` 23.227.38.65, AAAA `@` 2620:0127:f00f:5::, a `www` CNAME to a Shopify hostname, and a verification CNAME to `dns-verification.shopify.com`. It touches no MX or TXT records ([Domain Connect template `shopify.com.website.json`](https://github.com/Domain-Connect/Templates/blob/master/shopify.com.website.json)). The template doesn't say whether the records are proxied. On Cloudflare, the service provider picks a default proxy status when it registers the template, and the domain owner can change it afterwards ([Cloudflare: Domain Connect](https://developers.cloudflare.com/dns/reference/domain-connect/)). **I couldn't confirm what default Shopify chose,** so check the proxy column after an automatic connect.

### 1.3 Cloudflare proxy: off

The two vendors' docs disagree here.

- **Shopify says off.** "Cloudflare proxy setups, including Orange-to-Orange, aren't supported" ([Shopify: Connecting a Cloudflare domain](https://help.shopify.com/en/manual/domains/add-a-domain/connecting-domains/connecting-to-cloudflare)). Its troubleshooting page adds that a proxied store "might appear to function correctly", but "this setup could break at any time". It also says a proxy can block certificate validation, reduce the accuracy of Shopify's bot detection, and weaken resilience during outages. Shopify Support won't help with proxy-caused issues ([Shopify: troubleshooting domains](https://help.shopify.com/en/manual/domains/troubleshoot-issues-with-domains)). Its general guidance repeats that "Using third-party proxies isn't supported on Shopify" ([Shopify: considerations](https://help.shopify.com/en/manual/domains/add-a-domain/connecting-domains/considerations)).
- **Cloudflare says on is possible.** Its Shopify guide (updated 2026-08-25) describes "O2O". Your own Cloudflare zone sits in front of Shopify's Cloudflare zone, set up by creating a *proxied* CNAME to `shops.myshopify.com`. It works on any Cloudflare plan, and Workers and Snippets are disabled on the `/checkout` path ([Cloudflare: Shopify provider guide](https://developers.cloudflare.com/cloudflare-for-platforms/cloudflare-for-saas/saas-customers/provider-guides/shopify/)). Since June 2025, O2O is enabled automatically for anyone who uses both services, with a Shopify logo shown on the DNS record ([Cloudflare changelog, 2025-06-03](https://developers.cloudflare.com/changelog/2025-06-03-shopify-o2o-improvements/)).

**Which way this points:** proxy off. Shopify runs the store and checkout and handles support, and it calls O2O unsupported. Also, simply leaving the orange cloud on when adding a CNAME to `shops.myshopify.com` now turns O2O on without any separate step. Shopify's own edge already runs on Cloudflare (that's what O2O layers on top of), so turning the proxy off gives up only extra Cloudflare rules, caching and analytics on the store hostnames.

When a record is DNS only, Cloudflare returns the real target address and doesn't route HTTP(S) traffic through its network. That means Cloudflare's caching, WAF rules and redirect rules don't apply to that hostname ([Cloudflare: proxy status](https://developers.cloudflare.com/dns/proxy-status/)).

### 1.4 Apex vs www

- Shopify lets either the root domain or a subdomain be primary, and only one can be ([Shopify: change primary domain](https://help.shopify.com/en/manual/domains/domain-type/change-primary-domain)). Connect both. Customers who use the other one are "automatically redirected to the URL that is your primary domain" ([same page](https://help.shopify.com/en/manual/domains/domain-type/change-primary-domain)).
- In DNS the two differ only in record type: the apex uses A and AAAA records, and `www` uses a CNAME (table above). Because Shopify gives fixed apex addresses, nothing at Cloudflare forces a choice between the two.
- Do the apex↔www redirect in Shopify, not Cloudflare. Cloudflare's Single Redirects and Bulk Redirects "require that you proxy the DNS records", so they can't act on DNS-only Shopify records ([Cloudflare: redirects](https://developers.cloudflare.com/rules/url-forwarding/)).
- Choose the primary domain before adding Markets in Shopify. "One of your markets always uses your primary domain", and subfolder Market URLs change when the primary domain changes ([Shopify: change primary domain](https://help.shopify.com/en/manual/domains/domain-type/change-primary-domain)).

For the theme path, the facts don't favour apex or www. That choice is left to #22.

### 1.5 SSL

- Shopify provides TLS certificates free and automatically once the A and CNAME records point to Shopify. Issuance can take up to 48 hours, with "SSL pending" shown until then. You can't upload your own certificate ("You can't use third-party SSL certificates with your Shopify store"). HTTP visitors are redirected to HTTPS automatically ([Shopify: secure connections](https://help.shopify.com/en/manual/domains/managing-domains/secure-connections)).
- Shopify validates domain ownership with HTTP (ACME) challenges, and a Cloudflare proxy can block them, delaying or preventing issuance and renewal ([Shopify: troubleshooting domains](https://help.shopify.com/en/manual/domains/troubleshoot-issues-with-domains)). Cloudflare itself warns that, under O2O, "Always Use HTTPS" blocks Shopify's Let's Encrypt validation on `/.well-known/acme-challenge/*` ([Cloudflare: Shopify provider guide](https://developers.cloudflare.com/cloudflare-for-platforms/cloudflare-for-saas/saas-customers/provider-guides/shopify/)).
- With DNS only, Cloudflare's SSL/TLS settings (encryption mode, Universal SSL, Always Use HTTPS) don't touch Shopify traffic, because that traffic never passes through Cloudflare ([Cloudflare: proxy status](https://developers.cloudflare.com/dns/proxy-status/)). This is my inference from how DNS-only works; neither vendor states it for Shopify specifically.
- CAA records aren't needed. If anyone adds them later, each one must allow `letsencrypt.org`, `pki.goog` and `ssl.com`, or Shopify can't issue certificates ([Shopify: secure connections](https://help.shopify.com/en/manual/domains/managing-domains/secure-connections), [Shopify: troubleshooting domains](https://help.shopify.com/en/manual/domains/troubleshoot-issues-with-domains)). Once you add any CAA record, Cloudflare also adds its own CAA records, which don't appear in the dashboard ([Cloudflare: CAA records](https://developers.cloudflare.com/ssl/edge-certificates/caa-records/)).

### 1.6 Where checkout lives

On a theme store, checkout runs on the store's primary domain. Shopify moved checkout from `checkout.shopify.com` to each shop's own domain in July 2017. Its developer changelog gives the URL form `https://{shop_domain}/{shop.id}/checkouts/{token}`, and says that if the store forces redirects to its primary domain, "all checkouts occur on the primary domain" ([Shopify changelog, July 2017](https://shopify.dev/changelog/checkouts-will-occur-at-the-shop-domain-instead-of-checkout-shopify-com)). The matching Shopify blog post says the "entire shopping experience—from homepage to checkout—will take place on your domain" ([Shopify blog, 2017-07-14](https://www.shopify.com/blog/introducing-checkout-on-your-own-domain)). Shopify's current Hydrogen docs only make sense if this still holds: they move checkout to a subdomain by making that subdomain the Online Store's primary domain (section 2).

**Caveat:** the direct sources are from 2017, and the changelog URL now returns 404 (I read it from search-index copies). I couldn't test a live checkout. The first test order should confirm the checkout URL reads `fosln.com/...`.

### 1.7 GUI-only steps (theme)

1. Cloudflare dashboard > fosln.com > DNS > Records: remove any existing A or AAAA records on `@` and any existing `www` record.
2. Shopify admin > Settings > Domains > Connect existing domain > `fosln.com` > **Connect automatically**, then log in to Cloudflare and approve. If that fails, add the three records from 1.2 by hand ([Shopify: connect automatically](https://help.shopify.com/en/manual/domains/add-a-domain/connecting-domains/connect-domain-automatic)).
3. Back in Cloudflare DNS > Records, check that every Shopify record shows **DNS only** (grey cloud) and no Shopify logo. Cloudflare's docs don't say whether the Add record form defaults to proxied ([Cloudflare: create DNS records](https://developers.cloudflare.com/dns/manage-dns-records/how-to/create-dns-records/)), so check the toggle every time.
4. Wait for Shopify to show **Connected** with the certificate issued, which can take up to 48 hours. Don't change DNS in the meantime ([Shopify: Connecting a Cloudflare domain](https://help.shopify.com/en/manual/domains/add-a-domain/connecting-domains/connecting-to-cloudflare)).
5. Shopify admin > Settings > Domains > choose apex or www > Type > **Primary domain** ([Shopify: change primary domain](https://help.shopify.com/en/manual/domains/domain-type/change-primary-domain)).

---

## 2. Headless

In every headless setup Shopify documents, the storefront and checkout sit on different hostnames. "Customers are always directed back to a Shopify-hosted checkout" ([shopify.dev: migrate to Hydrogen](https://shopify.dev/docs/storefronts/headless/hydrogen/migrate)).

### 2.1 Hydrogen on Oxygen (Shopify-hosted)

- **Storefront:** `fosln.com`. In Settings > Domains, set its **Target** to the production Hydrogen storefront and its type to **Primary** ([shopify.dev: redirect traffic to Hydrogen](https://shopify.dev/docs/storefronts/headless/hydrogen/migrate/redirect-traffic)).
- **Checkout:** a subdomain, for example `checkout.fosln.com`, with **Target** set to **Online Store** and type **Primary**. Shopify's example is `hydrogen.shop` for the storefront and `checkout.hydrogen.shop` for checkout ([same page](https://shopify.dev/docs/storefronts/headless/hydrogen/migrate/redirect-traffic)). The checkout domain also goes into a `PUBLIC_CHECKOUT_DOMAIN` environment variable, set in the Hydrogen storefront's settings in Shopify admin, and into the content security policy ([shopify.dev: consent](https://shopify.dev/docs/storefronts/headless/hydrogen/analytics/consent)).
- **DNS:** both hostnames point to Shopify ([shopify.dev: redirect traffic to Hydrogen](https://shopify.dev/docs/storefronts/headless/hydrogen/migrate/redirect-traffic)). In practice that's the apex A and AAAA records from 1.2 plus `CNAME checkout shops.myshopify.com` ([Shopify: connect a subdomain](https://help.shopify.com/en/manual/domains/add-a-domain/connecting-domains/connect-subdomain)), all DNS only.
- Changing a domain's target is only possible on stores that use Hydrogen. Moving a primary domain to Hydrogen turns the Online Store's primary domain back into `*.myshopify.com` until the checkout subdomain is made primary ([Shopify: change domain target](https://help.shopify.com/en/manual/domains/domain-type/domain-target-change)).

### 2.2 Storefront hosted elsewhere (for example Cloudflare Workers)

- **Storefront:** `fosln.com` on the other host. "For custom storefronts not hosted on Oxygen, only the subdomain must point to Shopify" ([shopify.dev: redirect traffic to Hydrogen](https://shopify.dev/docs/storefronts/headless/hydrogen/migrate/redirect-traffic)). On Cloudflare Workers, adding `fosln.com` as a Custom Domain from the dashboard makes Cloudflare create the DNS record and certificate. You can't do this on a hostname that already has a CNAME record ([Cloudflare: Workers custom domains](https://developers.cloudflare.com/workers/configuration/routing/custom-domains/)). Here Cloudflare's proxy and rules do apply to the apex, because that hostname isn't Shopify's.
- **Checkout:** `checkout.fosln.com` as a DNS-only CNAME to `shops.myshopify.com`, connected in Shopify as the Online Store's primary domain ([Shopify: connect a subdomain](https://help.shopify.com/en/manual/domains/add-a-domain/connecting-domains/connect-subdomain)).
- The checkout link comes from the Storefront API cart's `checkoutUrl` field, documented only as "The URL of the checkout for the cart" ([shopify.dev: Cart object](https://shopify.dev/docs/api/storefront/latest/objects/Cart)). The docs don't say outright that it uses the checkout subdomain, so verify that with a test cart.

### 2.3 Can checkout stay on fosln.com?

- **On the fosln.com brand, yes:** `checkout.fosln.com` is the documented pattern.
- **On the exact hostname serving a headless storefront, not by any documented route.** Shopify's docs only describe a separate subdomain. Shopify doesn't support third-party proxies ([Shopify: considerations](https://help.shopify.com/en/manual/domains/add-a-domain/connecting-domains/considerations)), so serving `/checkouts/*` through a Worker on `fosln.com` would be unsupported. Cloudflare also blocks Workers and Snippets on `/checkout` for Shopify O2O hostnames ([Cloudflare: Shopify provider guide](https://developers.cloudflare.com/cloudflare-for-platforms/cloudflare-for-saas/saas-customers/provider-guides/shopify/)). Hydrogen users in late 2025 reached the same conclusion, that "the best you can do is use a sub-domain for your checkout" ([Shopify/hydrogen discussion #3256](https://github.com/Shopify/hydrogen/discussions/3256), community answers, not Shopify staff).

**Which way this points:** if "checkout on fosln.com" means the address bar reads `fosln.com` at checkout, only the theme path delivers it. Headless means `checkout.fosln.com`.

### 2.4 Things that need care with headless

- **The theme is still live.** After launch on Hydrogen "your online store remains active". Shopify recommends its Hydrogen redirect theme to send non-checkout visitors on the checkout and myshopify domains back to the storefront ([shopify.dev: redirect traffic to Hydrogen](https://shopify.dev/docs/storefronts/headless/hydrogen/migrate/redirect-traffic)).
- **Don't password-protect the Online Store.** "Online store password protection prevents Hydrogen checkouts" ([shopify.dev: migrate to Hydrogen](https://shopify.dev/docs/storefronts/headless/hydrogen/migrate)).
- **Analytics across subdomains.** Shopify's `_shopify_y` and `_shopify_s` cookies must match between the storefront and checkout. Fixing a mismatch can mean setting the cookie domain to the parent domain ([shopify.dev: analytics validation](https://shopify.dev/docs/storefronts/headless/hydrogen/analytics/validation)).
- **Reported rough edges.** Users have reported checkout links opening in new tabs, market domains not carrying through to checkout, and logged-in details not pre-filling. The issue was closed "not planned" in October 2025 ([Shopify/hydrogen issue #1203](https://github.com/Shopify/hydrogen/issues/1203), community reports).

---

## 3. Pitfalls

1. **The orange cloud.** A proxied CNAME to `shops.myshopify.com` now enables O2O automatically ([Cloudflare changelog](https://developers.cloudflare.com/changelog/2025-06-03-shopify-o2o-improvements/)), and Shopify doesn't support O2O ([Shopify: Connecting a Cloudflare domain](https://help.shopify.com/en/manual/domains/add-a-domain/connecting-domains/connecting-to-cloudflare)). Check every Shopify record after any change, including after automatic connection.
2. **Duplicate or stray records.** More than one apex A or AAAA record, a second `www` record, or a wildcard pointing at Shopify will break or confuse the connection ([Shopify: troubleshooting domains](https://help.shopify.com/en/manual/domains/troubleshoot-issues-with-domains)).
3. **CAA records.** None are needed. Any CAA record added later must allow `letsencrypt.org`, `pki.goog` and `ssl.com` ([Shopify: secure connections](https://help.shopify.com/en/manual/domains/managing-domains/secure-connections)). A CAA record whose value is only `;` blocks every CA ([Shopify: troubleshooting domains](https://help.shopify.com/en/manual/domains/troubleshoot-issues-with-domains)).
4. **DNSSEC.** Working DNSSEC is fine, but broken DNSSEC stops Shopify connecting, and turning it off in the wrong order can stop the domain resolving ([Shopify: Connecting a Cloudflare domain](https://help.shopify.com/en/manual/domains/add-a-domain/connecting-domains/connecting-to-cloudflare)). For a Cloudflare Registrar domain, it's a one-click toggle under Manage Domains > Configuration ([Cloudflare: Registrar DNSSEC](https://developers.cloudflare.com/registrar/get-started/enable-dnssec/)). I couldn't confirm whether it's on by default for new registrations.
5. **Redirects belong in Shopify.** Cloudflare redirect rules need proxied records ([Cloudflare: redirects](https://developers.cloudflare.com/rules/url-forwarding/)). Use Shopify's primary-domain redirect for apex↔www and Shopify's own URL redirects for paths.
6. **Email is separate from the store connection.** Shopify's connect template writes no MX or TXT records ([Domain Connect template](https://github.com/Domain-Connect/Templates/blob/master/shopify.com.website.json)), so email needs its own setup:
   - **Receiving mail at @fosln.com.** Cloudflare Email Routing, on the free plan, forwards inbound mail to an existing inbox. It adds MX records, an SPF TXT record and a DKIM TXT record on the apex from the dashboard ([Cloudflare: enable Email Routing](https://developers.cloudflare.com/email-routing/get-started/enable-email-routing/), [Cloudflare: Email Routing overview](https://developers.cloudflare.com/email-routing/)). MX and TXT records can't be proxied ([Cloudflare: proxy status](https://developers.cloudflare.com/dns/proxy-status/)).
   - **Shopify sending as @fosln.com.** Authenticate the domain under the sender-email settings. For Cloudflare this can be automatic through Domain Connect, and it adds CNAME records that cover DKIM and SPF. Then add exactly one DMARC TXT record at `_dmarc` with at least `v=DMARC1; p=none`. Without it, the sender address is rewritten to `store+123@shopifyemail.com` ([Shopify: email rewrites](https://help.shopify.com/en/manual/intro-to-shopify/initial-setup/email-rewrites), [Shopify: set up email](https://help.shopify.com/en/manual/intro-to-shopify/initial-setup/setup-your-email)). Automatic setup doesn't add the DMARC record ([Shopify: set up email](https://help.shopify.com/en/manual/intro-to-shopify/initial-setup/setup-your-email)). Shopify's published email template holds up to six CNAMEs and an optional DMARC entry ([Domain Connect template `shopify.com.email.json`](https://github.com/Domain-Connect/Templates/blob/master/shopify.com.email.json)), which doesn't quite match the help center's statement that DMARC isn't added. Check for a `_dmarc` record afterwards and add one only if it's missing. Keep the CNAMEs DNS only, because a proxied record answers with Cloudflare's addresses instead of its target. That last point is my inference; Shopify doesn't say it.
   - Don't strip these records later. Removing Shopify's email CNAMEs causes bounces, and two DMARC records make DMARC fail ([Shopify: email rewrites](https://help.shopify.com/en/manual/intro-to-shopify/initial-setup/email-rewrites)).
7. **Timing.** Allow up to 48 hours for the connection and certificate, and don't keep changing records in the meantime ([Shopify: Connecting a Cloudflare domain](https://help.shopify.com/en/manual/domains/add-a-domain/connecting-domains/connecting-to-cloudflare)).
8. **No transfer is needed, and an early one isn't possible.** Cloudflare can't process a transfer within 60 days of a new registration ([Cloudflare Registrar FAQ](https://developers.cloudflare.com/registrar/faq/)). For fosln.com, registered 2026-10-03, that's until about 2026-12-02. Every path above works with the domain staying at Cloudflare.
9. **Customer accounts get their own address.** New customer account pages use a `shopify.com` URL by default. `account.fosln.com` is optional: a CNAME to `shops.myshopify.com`, then set it as the primary customer-accounts domain ([Shopify: customer account domain](https://help.shopify.com/en/manual/domains/add-a-domain/connecting-domains/connect-domain-customer-account)).

## 4. What has changed recently

- Shopify now gives an **AAAA (IPv6)** record for the apex. Older guides list only A and CNAME.
- **Cloudflare is now on Shopify's automatic-connection list.** Older guides name Google Domains, GoDaddy and IONOS.
- Since **June 2025**, Cloudflare enables O2O automatically for proxied Shopify records ([Cloudflare changelog](https://developers.cloudflare.com/changelog/2025-06-03-shopify-o2o-improvements/)). Shopify's help center still calls O2O unsupported. Expect this to keep moving.
- Shopify's CAA list is now `letsencrypt.org`, `pki.goog` and `ssl.com`.
- Cloudflare pages read today show last-updated dates between April and October 2026. Shopify's help pages show no dates.

## 5. Not verified

- **That theme checkout still runs at `fosln.com/...` today.** The direct sources are from 2017, and the changelog URL now 404s. I couldn't test a live checkout. Confirm on the first test order.
- **Which proxy status Shopify's automatic connection sets on Cloudflare.** Check it by eye afterwards.
- **Whether Cloudflare's Add record form defaults to proxied.**
- **Whether `cart.checkoutUrl` returns the checkout subdomain** in a non-Oxygen headless build.
- **Whether DNSSEC is on by default** for new Cloudflare Registrar domains.
- A Shopify staff forum reply from March 2024 said checkout URL changes need Shopify Plus ([Shopify Community thread](https://community.shopify.com/t/hydrogen-storefront-checkout-domain-equal-to-storefront-domain/174139)). It conflicts with the documented subdomain method, which mentions no plan requirement, and I found no primary source for it.

## 6. Which way the facts point (for #22 and #21)

- **Proxy:** off for every record that points to Shopify. Shopify supports only DNS-only records. Cloudflare documents O2O, but Shopify calls it unsupported.
- **On-domain checkout:** a Shopify theme keeps checkout on `fosln.com` itself. Headless moves it to a subdomain such as `checkout.fosln.com`. That cost should be weighed in #21 next to the map's rule to "go headless only if a theme can't express the locked design".
- **Apex vs www:** for a theme, no technical reason favours either; Shopify redirects the other one. Shopify's Hydrogen example puts the storefront on the apex.
- **GUI rule:** the theme path is fully GUI: automatic connection plus a visual check of the proxy column. Headless adds GUI steps in Shopify (domain targets, environment variable) but also code-side work that Claude Code sessions would own.

## Sources

Shopify Help Center
- Connecting a Cloudflare domain to Shopify: https://help.shopify.com/en/manual/domains/add-a-domain/connecting-domains/connecting-to-cloudflare
- Connect your third-party domain manually: https://help.shopify.com/en/manual/domains/add-a-domain/connecting-domains/connect-domain-manual
- Connect automatically: https://help.shopify.com/en/manual/domains/add-a-domain/connecting-domains/connect-domain-automatic
- Troubleshooting domains: https://help.shopify.com/en/manual/domains/troubleshoot-issues-with-domains
- Considerations for third-party domains: https://help.shopify.com/en/manual/domains/add-a-domain/connecting-domains/considerations
- Change primary domain: https://help.shopify.com/en/manual/domains/domain-type/change-primary-domain
- Change domain target: https://help.shopify.com/en/manual/domains/domain-type/domain-target-change
- Connect a subdomain: https://help.shopify.com/en/manual/domains/add-a-domain/connecting-domains/connect-subdomain
- Secure connections (TLS): https://help.shopify.com/en/manual/domains/managing-domains/secure-connections
- Customer account domain: https://help.shopify.com/en/manual/domains/add-a-domain/connecting-domains/connect-domain-customer-account
- Email rewrites and authentication: https://help.shopify.com/en/manual/intro-to-shopify/initial-setup/email-rewrites
- Set up your email: https://help.shopify.com/en/manual/intro-to-shopify/initial-setup/setup-your-email

shopify.dev and Shopify
- Redirect traffic to Hydrogen: https://shopify.dev/docs/storefronts/headless/hydrogen/migrate/redirect-traffic
- Migrate to Hydrogen: https://shopify.dev/docs/storefronts/headless/hydrogen/migrate
- Hydrogen consent (PUBLIC_CHECKOUT_DOMAIN): https://shopify.dev/docs/storefronts/headless/hydrogen/analytics/consent
- Hydrogen analytics validation: https://shopify.dev/docs/storefronts/headless/hydrogen/analytics/validation
- Storefront API Cart object: https://shopify.dev/docs/api/storefront/latest/objects/Cart
- Changelog, July 2017 (now 404): https://shopify.dev/changelog/checkouts-will-occur-at-the-shop-domain-instead-of-checkout-shopify-com
- Shopify blog, 2017-07-14: https://www.shopify.com/blog/introducing-checkout-on-your-own-domain
- Domain Connect templates: https://github.com/Domain-Connect/Templates/blob/master/shopify.com.website.json and https://github.com/Domain-Connect/Templates/blob/master/shopify.com.email.json

Cloudflare docs
- Shopify provider guide (O2O): https://developers.cloudflare.com/cloudflare-for-platforms/cloudflare-for-saas/saas-customers/provider-guides/shopify/
- Changelog 2025-06-03, Shopify O2O: https://developers.cloudflare.com/changelog/2025-06-03-shopify-o2o-improvements/
- Proxy status: https://developers.cloudflare.com/dns/proxy-status/
- Create DNS records: https://developers.cloudflare.com/dns/manage-dns-records/how-to/create-dns-records/
- Domain Connect: https://developers.cloudflare.com/dns/reference/domain-connect/
- Registrar FAQ: https://developers.cloudflare.com/registrar/faq/
- Registrar DNSSEC: https://developers.cloudflare.com/registrar/get-started/enable-dnssec/
- CAA records: https://developers.cloudflare.com/ssl/edge-certificates/caa-records/
- Redirects: https://developers.cloudflare.com/rules/url-forwarding/
- Workers custom domains: https://developers.cloudflare.com/workers/configuration/routing/custom-domains/
- Email Routing: https://developers.cloudflare.com/email-routing/ and https://developers.cloudflare.com/email-routing/get-started/enable-email-routing/

Community (not authoritative)
- Shopify/hydrogen discussion #3256: https://github.com/Shopify/hydrogen/discussions/3256
- Shopify/hydrogen issue #1203: https://github.com/Shopify/hydrogen/issues/1203
- Shopify Community, Hydrogen checkout domain thread: https://community.shopify.com/t/hydrogen-storefront-checkout-domain-equal-to-storefront-domain/174139
