# babylook-privacy

Marketing site and privacy policy for BabyLook, the iPhone and iPad app for
artistic AI baby portraits and an on-device Baby Journal.

## Where this repo is published

This repo is served from two places, both from the `main` branch:

| URL | Host | How it deploys |
| --- | --- | --- |
| https://babylookapp.com | Cloudflare Workers (`babylook`) | Cloudflare Workers Builds, automatically on every push |
| https://sahinlokman.github.io/babylook-privacy/ | GitHub Pages | GitHub Pages, automatically on every push |

GitHub Pages serves the repository root, which is why the HTML and
`assets/` live at the top level rather than in a subdirectory.

## Pages

- `index.html` — single-page landing page (portraits, Baby Journal,
  screenshots, pricing, privacy summary, FAQ). It shows only a short privacy
  summary and links to `privacy.html` for the full policy.
- `privacy.html` — standalone privacy policy
- `assets/` — logo and compressed JPEG screenshots (`shot-*.jpg` for iPhone,
  `ipad-*.jpg` for iPad), exported from the App Store screenshots with
  `sips -s format jpeg -s formatOptions 80 -Z 900`

On babylookapp.com, `/privacy.html` redirects to the clean URL `/privacy`.

## Cloudflare build

Cloudflare runs `npm run build` and then `npx wrangler deploy`.

`npm run build` copies only the public files into `dist/`, which is what
`wrangler.jsonc` publishes. This keeps repo-only files (`README.md`,
`_config.yml`, `.nojekyll`, `wrangler.jsonc`) out of the deployed site
while leaving them in place for GitHub Pages.

`wrangler.jsonc` declares both custom domains, so a deploy cannot
accidentally drop `babylookapp.com` or `www.babylookapp.com`.

To check a build locally before pushing (needs Node 22+):

```sh
npm run build
npx wrangler deploy --dry-run
```

## Updating the privacy policy

The full policy text lives only in `privacy.html`. It mirrors the canonical
policy the app links to, https://appterms.site/babylook/privacy/ (source:
`legal-site/babylook/privacy/index.html`), plus a Baby Journal paragraph.
When the canonical policy changes, update `privacy.html` to match and bump its
"Updated" date.

`index.html` contains only a short privacy summary (in the `#privacy`
section and the FAQ). Check that it still agrees with `privacy.html` and does
not promise more than the policy does.

The iOS app links to the appterms.site policy; this site's copy is
user-facing as soon as it is pushed.
