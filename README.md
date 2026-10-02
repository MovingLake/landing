# movinglake.com

Static site for [movinglake.com](https://www.movinglake.com), built with [Hugo](https://gohugo.io) and deployed to GitHub Pages. Migrated from Webflow.

## Local development

```sh
brew install hugo        # if not installed
hugo server -D           # http://localhost:1313
```

## Structure

| Path | What it is |
| --- | --- |
| `hugo.toml` | Site config, nav menu, contact emails, social links |
| `content/` | Pages. Industry pages (`hospitality.md`, `ecommerce.md`, `telecommunications.md`) are driven entirely by front matter |
| `content/blog/` | Blog posts in Markdown (one file per post, slug preserved from Webflow) |
| `data/connectors.yaml`, `data/destinations.yaml`, `data/partners.yaml` | Catalogs rendered on the connectors, destinations and partnerships pages |
| `layouts/` | Templates (no external theme) |
| `assets/css/main.css` | The stylesheet |
| `static/img/` | All images, downloaded from the Webflow CDN so nothing breaks when Webflow is cancelled |
| `static/CNAME` | Custom domain for GitHub Pages |
| `.github/workflows/hugo.yml` | Builds and deploys on every push to `main` |

## Deploying to GitHub Pages

1. Create a GitHub repo and push this directory to the `main` branch.
2. In the repo go to **Settings → Pages** and set **Source** to **GitHub Actions**.
3. Push (or re-run the workflow). The site is live at `https://<user>.github.io/<repo>/` within a minute or two.
4. Under **Settings → Pages → Custom domain** enter `www.movinglake.com` and tick **Enforce HTTPS** once the certificate is issued.

## DNS (move away from Webflow)

At your DNS provider:

| Type | Name | Value |
| --- | --- | --- |
| CNAME | `www` | `<user>.github.io` |
| A | `@` | `185.199.108.153` |
| A | `@` | `185.199.109.153` |
| A | `@` | `185.199.110.153` |
| A | `@` | `185.199.111.153` |

Remove the old Webflow A/CNAME records. GitHub redirects the apex domain to `www` automatically once the custom domain is set.

## Contact form

GitHub Pages has no backend. By default the form on `/contact-us/` opens the visitor's email client with the message pre-filled (`mailto:`). To get a real form, create a free [Formspree](https://formspree.io) form and set `contactFormAction = "https://formspree.io/f/xxxx"` in `hugo.toml`.

## Docs

The product documentation at `movinglake.com/docs/` was a separate Hugo site that is not part of this repo. If you want to keep it, build it and copy its output into `static/docs/`, or point the `docsUrl` param in `hugo.toml` somewhere else.

## Not migrated

The Webflow site had ~2,600 programmatic `/integrations/<source>-to-<destination>` SEO pages and one page per individual connector/destination. These were template-generated with no unique content and were left out. The connector and destination catalogs live in `data/` if you ever want to generate per-item pages from them.
