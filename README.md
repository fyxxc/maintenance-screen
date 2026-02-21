# Cloudflare Maintenance Worker

Minimal maintenance screen served at the Cloudflare edge.

This Worker sits in front of a Cloudflare Pages deployment and can temporarily replace the actual site with a lightweight maintenance page.
No DNS changes, no redeploys, no origin downtime required.

---

## What it does

* intercepts requests on a subdomain
* optionally serves a static maintenance page
* otherwise forwards traffic to the normal Pages site
* exposes basic request/debug information (IP, browser, OS, timestamp)

The page is intentionally minimal and neutral so it fits most projects without looking like a template.

---

## Typical use cases

* planned maintenance windows
* backend migrations
* database changes
* broken deployments
* temporarily hiding a site without touching DNS or origin

---

## Setup

### 1. Create a Worker

Cloudflare Dashboard → **Workers & Pages** → Create Worker

Paste the script and deploy.

---

### 2. Attach it to your subdomain

Add a route like:

```
hallo.fynnhofmann.com/*
```

Make sure the Worker is executed **before** the Pages project.

---

### 3. Toggle maintenance mode

Inside the Worker:

```js
const MAINTENANCE = true;
```

Set to `false` to pass traffic through normally.

Changes apply instantly across the Cloudflare edge.

---

## Customisation

You can safely edit:

* text content
* icon (Google Symbols)
* typography
* debug output
* colours

The page is static HTML returned by the Worker, so no build step is needed.

---

## Notes

* The maintenance page is served directly from Cloudflare’s edge network.
* The origin (Pages project) stays untouched.
* This approach avoids DNS caching delays.
* Works globally within seconds.

---

## Optional improvements

If you want to extend it later, common additions are:

* allow-list for your own IP
* password gate instead of maintenance mode
* automatic refresh when origin is back
* Cloudflare colo / request ID display
* KV-based toggle instead of code change

---

## License

Use it however you like.
If it breaks, you get to keep both pieces.
