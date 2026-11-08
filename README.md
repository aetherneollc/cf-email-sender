# AetherNeo Mail

A password-protected Cloudflare Email Sending console and REST API. It uses the native `env.EMAIL.send()` Worker binding, so no Resend API key or Cloudflare API token is required at runtime.

## Features

- Cloudflare Worker API and Pages-compatible web console
- Password login and Bearer API authentication
- Explicit accepted/failed result with the Cloudflare message ID
- Recent-send history stored locally in the current browser
- Multiple file attachments with MIME type and filename support
- Default sender: `noreply@aetherneo.com`
- Custom sender addresses supported for verified Email Sending domains

The native Worker binding returns a message ID after Cloudflare accepts the request. This is an acceptance/queue result, not a final inbox-delivery receipt. The web console shows that distinction clearly.

## API

`POST /api/send`

```bash
curl -X POST https://YOUR_WORKER.workers.dev/api/send \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"to":"recipient@example.com","from":"noreply@aetherneo.com","subject":"Hello","html":"<h1>Hello</h1>","text":"Hello"}'
```

The `to` field may be a single address or an array. The `from` address must belong to a verified Cloudflare Email Sending domain.

### Attachments

Attachments are sent as base64 content:

```json
{"filename":"report.pdf","type":"application/pdf","disposition":"attachment","content":"BASE64_DATA"}
```

Add one or more objects in an `attachments` array. The total message size is limited to 5 MiB by Cloudflare Email Sending.

Successful response:

```json
{"ok":true,"status":"accepted","id":"<message-id@example.com>","sentAt":"2026-11-08T00:00:00.000Z"}
```

## Deployment

Configure the Email Sending binding and `DEFAULT_FROM` in `wrangler.toml`, then run:

```bash
npm install
npm run deploy
```

For Pages, connect the repository and publish the repository root as a static site. The Pages entry point redirects to the Worker console so both URLs always use the same deployed interface.

## Security

Change the application password before using this project for a public service. Never commit a real production password or API key. Rotate the key immediately if it is exposed.
