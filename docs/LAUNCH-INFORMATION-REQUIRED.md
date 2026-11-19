# Information Required Before Launch

Run `node build.mjs` to see what's still missing. Nothing on the site has been invented.

## Already set

- [x] Name: **Willowfield**
- [x] Phone: **(980) 550-5942**
- [x] Site URL: `https://willowfield.kevin-c4h.workers.dev`
- [x] Hours: **Monday–Friday, 9:00 AM – 5:00 PM (Eastern Time)**, office meetings by appointment
- [x] Business type: **storefront office** at **5960 Fairview Rd, Charlotte, NC 28210** (Google Business Profile "Local store")
- [x] Services confirmed: plan setup, records and valuations, Form 5500-EZ preparation, ongoing
      support, Self-Directed and Solo 401(k) education, and office meetings
- [x] Role: administrative support alongside the client's plan provider, at the sponsor's
      direction. The business is not a fiduciary, trustee, custodian, registered investment
      adviser or broker-dealer.
- [x] Format: a one-page site, with separate Privacy, Terms and Disclosures pages

## Required

| Item | Notes |
|---|---|
| **Contact form (n8n)** | Create the n8n workflow (see `docs/N8N-FORM-SETUP.md`) and put its production webhook URL in `form.webhookUrl` in `site.config.mjs`. Until then the form shows a notice and doesn't send. |
| **Register the business name** | North Carolina Assumed Business Name certificate with the Mecklenburg County Register of Deeds, then publish it in a local newspaper once a week for 4 weeks. Then set `business.publicNameConfirmed: true`. Check the CA Secretary of State, the USPTO and Google Maps for "Willowfield" conflicts first. |

Optional: `business.geo` with the office's real coordinates.

## Recommended before going public

- [ ] An attorney has reviewed the Privacy Policy, Terms and Disclosures (North Carolina law, administrative-services wording).
- [ ] An ERISA attorney has confirmed the administrative-services role.
- [ ] n8n: a test submission from the live site landed on the thank-you page and ran the workflow.
- [ ] Cloudflare Pages: the site is deployed. Then add the Search Console verification code to `site.googleSiteVerification`.

## Optional (hidden until set)

Business email, owner name, company history and credentials.
