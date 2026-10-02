# Frontend

This folder contains a self-contained static careers page. No build process is required.

## Run locally

From this folder, serve the file through any static HTTP server. Examples:

```bash
npx serve .
```

or:

```bash
python -m http.server 8080
```

Then open the displayed local URL.

## Connect your n8n webhook

The repository does not include the presenter's live webhook.

Choose one method:

### Temporary workshop configuration

Open the page with a URL-encoded `webhook` query parameter:

```text
http://localhost:8080/?webhook=https%3A%2F%2FYOUR-N8N-HOST%2Fwebhook%2FYOUR-PATH#apply
```

The value remains available only for the current browser session.

### Permanent deployment configuration

Edit `index.html` and replace the empty `productionWebhook` value with your own n8n Production URL.

## Hosting

Because this is a single static HTML file, it can be hosted on GitHub Pages, Cloudflare Pages, Netlify, Render Static Sites or another static host.

Never publish API keys, OAuth tokens or n8n credentials in this file. A webhook URL is not an API key, but publishing it allows anyone to submit data to the workflow, so add authentication, rate limiting and abuse controls for non-demo use.
