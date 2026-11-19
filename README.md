# Willowfield: Website

A one-page site for Willowfield, which provides Solo and self-directed 401(k) plan support from an
office in Charlotte, North Carolina (Fairview Rd). It's static with no dependencies, built with Node and hosted on
**Cloudflare** (Workers static assets via `wrangler.jsonc`, or Pages).

## Quick start

```bash
node build.mjs          # builds ./dist; checks internal links; lists missing info
STRICT=1 node build.mjs # pre-launch check (fails while placeholders remain)
```

## Deploy to Cloudflare

**Workers (current setup):** Workers & Pages → Create → Import a repository → this repo.
Build command `npm run build`, deploy command `npx wrangler deploy`. `wrangler.jsonc` serves `./dist`.
Every push to the branch redeploys automatically.

**Pages (alternative):** Build command `node build.mjs`, output directory `dist`.

Security headers and redirects live in `src/static/_headers` and `src/static/_redirects`, which
are copied into `dist/` and applied by Cloudflare. After the first deploy, set `site.url` in
`site.config.mjs` to the live address (or custom domain) so canonicals, the sitemap and the form's
thank-you redirect are correct.

## Contact form (n8n)

The meeting form posts to an **n8n Webhook**, and the n8n workflow routes each submission
(email, sheet, CRM) and redirects the visitor to `/thank-you/`. Put the webhook's production URL
in `form.webhookUrl` in `site.config.mjs`. Until then the form shows a "not connected yet" notice
and won't send. Setup steps: `docs/N8N-FORM-SETUP.md`.

## Office

**5960 Fairview Rd, Charlotte, NC 28210** (storefront office). Phone: **(980) 550-5942**.

## Structure

The homepage (`/`) holds every section, each reachable from the menu and the sticky section bar:
Services · How it works · Who does what · 401(k) basics · FAQ · About · Visit & contact (meeting form).
`/privacy-policy/`, `/terms/` and `/disclosures/` are separate pages under North Carolina law,
plus `/thank-you/` and `404.html`.

## Design

- **Palette:** walnut `#2A2420`, willow green `#4F6B3A` (buttons, white text), golden-hour amber `#E0A43A`, wheat `#F1D9A6`, linen `#FBFAF6`
- **Type:** Brygada 1918 (display) and Commissioner (text), both self-hosted
- **Logo:** an arched window with a willow tree over an open field at golden hour (`src/brand.mjs`)
- **Hero:** slow-zoom photo of a tree-lined road in golden autumn light

See `docs/` for the launch checklist, media sources, Search Console and Google Business Profile steps.
