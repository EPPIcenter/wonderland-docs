# ChattyBox setup (PoC)

One-time dashboard steps to power the docs chatbot. The MkDocs embed lives in `overrides/main.html`; crawl rules live in `chattybox.config.json`.

## 1. Create the free project

1. Sign up at [chattybox.ai](https://chattybox.ai/) (free tier, no card).
2. Create a project named **MAD4HATTER Docs**.
3. Add the site URL: `https://eppicenter.github.io/wonderland-docs/`
4. Prefer **manual URLs** (or exclude `/blog/`) so indexed pages stay at **9** (under the free 10-page limit). Use the URL list in `chattybox.config.json`.
5. Run **Scrape now** and confirm only core docs pages appear under Content (no blog).

## 2. Validate answers

In **Test Chat**, ask:

1. How do I install and get started with MAD4HATTER?
2. How do I run the pipeline?
3. What does a key pipeline parameter do? (pick one from Parameters)
4. Where are pipeline outputs written?
5. How should I interpret QC results?

Confirm each answer cites the matching page and the link opens correctly.

## 3. Wire the widget into MkDocs

1. Open **Public Keys**, create a browser key.
2. Optionally restrict origins to:
   - `https://eppicenter.github.io`
   - `http://127.0.0.1:8000` (local `mkdocs serve`)
3. Open **Embed**, copy the **public API key** and **widget API URL** (`https://….convex.site/chat`).
4. Put them in `mkdocs.yml` under `extra`:

```yaml
extra:
  chattybox_api_key: "cb_pub_..."
  chattybox_api_url: "https://YOUR-DEPLOYMENT.convex.site/chat"
```

5. Rebuild/serve: `mkdocs serve` — the Ask widget should appear bottom-right.

## 4. Optional: config-as-code deploy

After the project exists:

```bash
export CHATTYBOX_DEPLOY_TOKEN='cb_cfg_v1_...'   # Settings → Config deployment tokens
export CHATTYBOX_API_URL='https://….convex.site/chat'
bunx @openstaticfish/chattybox-cli validate
bunx @openstaticfish/chattybox-cli deploy --environment production
```

Never commit the deployment token. The public widget key in `mkdocs.yml` is intended for the browser.

## 5. Before a funder demo

- Recrawl if docs changed.
- Walk through [CHATBOT_DEMO.md](CHATBOT_DEMO.md).
