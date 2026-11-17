# Site do Debaixo do Meu Teto

The public pt-BR website for **Debaixo do Meu Teto**, an app for keeping a home inventory on your phone. The site has a landing page, a privacy policy and a support page. The app's code lives in a separate private repository; this repository holds only the website.

The `site/` folder is plain HTML and CSS. There is no build step, no JavaScript, and nothing loaded from third parties (no web fonts, analytics or trackers), which matches what the privacy policy promises. Only `site/` is published; this README and the workflow are not.

Address: **https://debaixodomeuteto.hbfelizzola.de/**

| Page | Path | Used for |
| --- | --- | --- |
| Landing | `/` | Marketing, search |
| Privacy policy | `/privacidade/` | App Store "Privacy Policy URL", Google Play "Privacy policy" |
| Support | `/suporte/` | App Store "Support URL", Google Play contact website |

## Preview locally

```bash
python3 -m http.server 8080 -d site
```

Then open http://localhost:8080. Every internal link is relative, so the site also works from a subfolder.

## Files in `site/`

- `index.html`, `privacidade/index.html`, `suporte/index.html`, `404.html`: the pages.
- `estilo.css`: one stylesheet in the app's colours, with light and dark mode.
- `img/telas/*.jpg`: screenshots of the real app (iPhone 18 Pro simulator, iOS 27) showing example data. The item "photos" in them are drawn placeholders, not real product photos.
- `img/marca.svg` and the PNG icons: a temporary house mark. Replace it with the app's real icon once the store release has one.
- `img/og.jpg`: the 1200×630 image for link previews on WhatsApp, Facebook, LinkedIn and X.
- `robots.txt`, `sitemap.xml`, `site.webmanifest`, `CNAME`, `.nojekyll`.

## Hosting: GitHub Pages on a subdomain of hbfelizzola.de

`hbfelizzola.de` is already served by GitHub Pages from `hbfelizzola/my-portfolio`, with DNS at GoDaddy. This site gets its own subdomain and is published by `.github/workflows/site.yml`. Nothing goes live until you complete the steps below.

1. **DNS.** In GoDaddy's DNS settings for `hbfelizzola.de`, add a record of type `CNAME` with name `debaixodomeuteto` and value `hbfelizzola.github.io`.
2. **Pages settings** (this repo → Settings → Pages):
   - Set the source to **GitHub Actions**.
   - Set the custom domain to `debaixodomeuteto.hbfelizzola.de`. When deploying through Actions, GitHub reads this setting and ignores the `CNAME` file, which only records the domain.
   - Once the certificate is ready, tick **Enforce HTTPS**.
   - Recommended: verify `hbfelizzola.de` under your account's Settings → Pages → Verified domains, so no one else can claim a subdomain of it.
3. **Deploy.** Go to Actions → "Site" → Run workflow. Once that works, uncomment the `push` trigger in the workflow so every change to `site/` on `main` publishes automatically.
4. **Search.** Add the domain to Google Search Console and submit `https://debaixodomeuteto.hbfelizzola.de/sitemap.xml`.

To use a different address, replace `debaixodomeuteto.hbfelizzola.de` in every file under `site/`: the canonical links, Open Graph tags, JSON-LD, sitemap, robots and CNAME.

## When the app is in the stores

Search for `TODO(lojas)`. For each store button:

1. Replace `href="#"` with the store link, remove `aria-disabled="true"`, and delete the "Em breve…" lines.
2. Swap the placeholder buttons for the official Portuguese badges: Apple's "Baixar na App Store" from the App Store Marketing Guidelines, and Google's "Disponível no Google Play" from the Google Play badge generator. Follow their size and spacing rules.
3. Optional: add `<meta name="apple-itunes-app" content="app-id=…">` to `index.html` to show Safari's smart app banner, and add the store URLs to the JSON-LD as `"sameAs"`. Add `"offers"` once the price is known.

## Keep the privacy policy true

The privacy policy describes what the app does today: it collects no data, has no account, no server, no analytics and no ads, uses the camera and photos only to attach images, sends local notifications only, and makes no cloud backup. If a version of the app adds anything that sends data off the phone (backup, sync, crash reports, analytics), update `site/privacidade/index.html` and its date **before** releasing that version. Update the App Store privacy label and the Google Play Data safety form at the same time.
