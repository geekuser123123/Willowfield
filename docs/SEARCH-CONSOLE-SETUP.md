# Google Search Console Setup

> Search Console verification is **not** done until the site owner completes these
> steps and Google confirms it. Nothing in this repository verifies it automatically.

**Current address:** `https://willowfield.kevin-c4h.workers.dev`
(set in `site.config.mjs → site.url`)

## What the website already provides

- HTTPS-only delivery on Cloudflare Pages, with an HSTS header in `src/static/_headers`
- `/sitemap.xml`, generated on every build, listing only indexable pages
- `/robots.txt`, which allows all crawlers and points to the sitemap
- A self-referencing canonical URL on every indexable page, using `site.url`
- A unique `<title>` and meta description on every page (the build fails if two pages share one)
- Clean internal links and a custom `404.html`
- `noindex` on the thank-you and 404 pages only
- JSON-LD structured data: Organization, WebSite, WebPage, BreadcrumbList, Service, Article and FAQPage

---

## Option A: Now, on `willowfield.kevin-c4h.workers.dev` (URL-prefix property)

A **Domain property** needs a DNS TXT record. You don't control DNS for `workers.dev`,
so use a **URL-prefix property** with the **HTML tag** method.

1. Make sure the Cloudflare Pages project is deployed and not behind Cloudflare Access.
2. Open <https://search.google.com/search-console> and sign in with the Google account
   that will **also manage the Google Business Profile**.
3. Click **Add property** → **URL prefix** and enter
   `https://willowfield.kevin-c4h.workers.dev/`
4. Choose **HTML tag** as the verification method. Google shows a tag like:
   `<meta name="google-site-verification" content="AbCdEf123..." />`
5. Copy **only the content value** (`AbCdEf123...`) into `site.config.mjs`:
   ```js
   googleSiteVerification: 'AbCdEf123...',
   ```
   Commit and push. Cloudflare Pages redeploys and the tag appears on every page. (Or send the
   value to your developer to add.)
6. Back in Search Console, click **Verify**.
7. **Sitemaps** → enter `sitemap.xml` → **Submit**.
8. **URL Inspection** → inspect the homepage → **Request indexing**.

Keep the meta tag in place, because removing it un-verifies the property.

---

## Option B: Later, when a custom domain is connected (Domain property)

Use this once you own a domain such as `willowfield.com`.

### 1. Connect the domain
1. Buy the domain in Cloudflare (**Domain Registration → Register Domains**), or add an existing one to Cloudflare.
2. Cloudflare dashboard → **Workers & Pages** → the `willowfield` project → **Custom domains** → add the
   apex domain (and `www` if you want it). Cloudflare creates the DNS records and HTTPS certificate.
3. In `site.config.mjs`, change `site.url` to `https://willowfield.com` and push.
   Canonicals, the sitemap, robots.txt, structured data and the form's thank-you redirect all switch over.

### 2. Add a Domain property
1. Search Console → **Add property** → **Domain** and enter `willowfield.com`.
   Don't include `https://` or `www`.
2. Copy the **TXT record** (`google-site-verification=XXXXXXXX`).

### 3. Add the DNS TXT record

| Type | Host / Name | Value | TTL |
|---|---|---|---|
| TXT | `@` (root domain) | `google-site-verification=XXXXXXXX` | default |

Don't remove existing TXT records, such as SPF.

### 4. Verify and submit
1. Wait for DNS propagation (minutes to 48 hours), then click **Verify**.
   Check the record with `dig TXT willowfield.com +short`.
2. **Sitemaps** → submit `sitemap.xml`.
3. Keep the TXT record permanently.
4. Keep the old workers.dev URL-prefix property for reference.

---

## Ongoing checks
- Look at the **Pages** report for errors. Thank-you and 404 pages are excluded on purpose.
- Test structured data with the [Rich Results Test](https://search.google.com/test/rich-results)
  and the [Schema.org Validator](https://validator.schema.org/).
- Add other users with **Settings → Users and permissions** instead of sharing passwords.
