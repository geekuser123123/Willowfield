# Contact Form → n8n

The meeting request form is a normal HTML form that POSTs (`application/x-www-form-urlencoded`)
to an **n8n Webhook**. Because it's a full-page form post (not JavaScript), no CORS setup is needed.

## Fields the workflow receives (in `body`)

| Field | Notes |
|---|---|
| `first_name`, `last_name`, `email` | required |
| `phone` | optional |
| `business_type`, `area_of_interest`, `plan_status` | required selects |
| `meeting_place`, `preferred_time` | radio choices |
| `message` | required, up to 3,000 characters |
| `consent` | `yes` when the consent box is ticked |
| `form_name` | `consultation` |
| `business` | `Willowfield` |
| `site` | the website address |
| `redirect_to` | the website's `/thank-you/` page |
| `company_website` | **honeypot**: real visitors leave it empty; bots often fill it |

## Build the workflow

1. **Webhook** node
   - HTTP Method: **POST**
   - Path: e.g. `willowfield-meeting`
   - Respond: **Using 'Respond to Webhook' Node**
2. **IF** node: continue only when `{{ $json.body.company_website }}` **is empty** (drops spam).
3. **Your action(s)** on the "true" branch, for example:
   - **Gmail / Send Email**: subject `New meeting request — {{ $json.body.first_name }} {{ $json.body.last_name }}`,
     body listing the fields above. Set Reply-To to `{{ $json.body.email }}`.
   - and/or **Google Sheets → Append row**, a CRM node, Slack, etc.
4. **Respond to Webhook** node (connect it after your actions, and also to the IF "false" branch so bots get the same response)
   - Respond With: **Redirect**
   - Redirect URL: `{{ $('Webhook').item.json.body.redirect_to }}`
     (or type the thank-you URL directly)
5. **Activate** the workflow and copy the **Production URL** from the Webhook node
   (it contains `/webhook/`, not `/webhook-test/`).

## Connect the website

Put the production URL in `site.config.mjs`:

```js
form: { name: 'consultation', provider: 'n8n', webhookUrl: 'https://YOUR-N8N-HOST/webhook/willowfield-meeting' },
```

Commit and push. The build adds the n8n host to the Content-Security-Policy `form-action`
allowlist in `_headers` automatically and removes the "not connected yet" notice.

## Test

Submit the form on the live site. You should land on `/thank-you/` and see the execution in
n8n's **Executions** list. If the browser shows n8n's JSON instead of the thank-you page, the
Webhook node isn't set to respond via the Respond to Webhook node, or that node isn't set to Redirect.
