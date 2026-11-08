# AetherNeo Mail

A password-protected Cloudflare Email Sending web console and REST API. It uses the native `env.EMAIL.send()` binding, so no Resend API key or Cloudflare API token is required at runtime.

## Features

- Cloudflare Workers and Pages compatible
- Password-protected web interface
- REST API with Bearer API key authentication
- Default sender: `noreply@aetherneo.com`
- Custom sender addresses supported for verified domains
- No Resend API key required

## Web interface

Open the deployed Pages site or Worker URL, sign in, and use the mail form. The signed-in page displays the API key and a ready-to-copy usage example.

## API

`POST /api/send`

Headers:

```http
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json
```

Example:

```bash
curl -X POST https://YOUR_WORKER.workers.dev/api/send \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "to": "recipient@example.com",
    "from": "noreply@aetherneo.com",
    "subject": "Hello",
    "html": "<h1>Hello</h1>"
  }'
```

The `to` field may also be an array of email addresses. The `from` field can be changed to another address on a verified Cloudflare Email Sending domain.

## Deployment

Configure the Email Sending binding in Cloudflare and set `DEFAULT_FROM` in `wrangler.toml` or Worker Variables. Then deploy with:

```bash
npm install
npm run deploy
```

For Pages, connect the repository and publish the repository root as a static site.

## Security

Change the application password before using this project for a public service. Keep the API key private and rotate it if exposed.
