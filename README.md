# babylook-privacy

Marketing site and privacy policy for BabyLook, the AI baby face prediction app.

## Where this repo is published

This repo is served from two places, both from the `main` branch:

| URL | Host | How it deploys |
| --- | --- | --- |
| https://babylookapp.com | Cloudflare Workers (`babylook`) | Cloudflare Workers Builds, automatically on every push |
| https://sahinlokman.github.io/babylook-privacy/ | GitHub Pages | GitHub Pages, automatically on every push |

GitHub Pages serves the repository root, which is why the HTML and
`assets/` live at the top level rather than in a subdirectory.

## Pages

- `index.html` — landing page, with the privacy policy inlined
- `privacy.html` — standalone privacy policy

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

The policy text appears in **both** `index.html` and `privacy.html`. Update
both, and bump the "Last Updated" date in each.

The iOS app links to the published policy, so changes here are
user-facing as soon as they are pushed.
