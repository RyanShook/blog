# Cloudflare Pages Migration — Parked Plan

Notes for future-me if/when picking this up again. Captured 2026-05-09.

## Goal

Move `ryanshook.org` off GitHub Pages onto Cloudflare Pages, and make both source repos private.

## Current state (as of parking)

- DNS for `ryanshook.org` is already on Cloudflare (proxied; `server: cloudflare` on responses).
- Two separate public GitHub repos serve the site today via GitHub Pages:
  - `RyanShook/ryanshook.github.io` — vanilla HTML/CSS landing page, served at apex `ryanshook.org/`.
    - Has `CNAME` file containing `ryanshook.org`.
    - Already has a `.wrangler/` dir from prior experimentation (no `wrangler.toml` though).
  - `RyanShook/blog` (this repo) — Jekyll + Chirpy theme, served at `ryanshook.org/blog/`.
    - `_config.yml` sets `baseurl: "/blog"` and `url: "https://ryanshook.org"`.
    - Built by `.github/workflows/pages-deploy.yml` (Ruby 3.2, Jekyll build, deploy to GH Pages).
- CLI auth that was already in place:
  - `gh` logged in as `RyanShook` (scopes: `gist`, `read:org`, `repo`).
  - `wrangler` logged in as `shoook@gmail.com`, account ID `87bc7a3f658e7ab551255ca18b60b9be`.

## Architectural decision (recommended path: A)

Three options were considered:

### A. Two repos, two Pages projects, one Worker routing `/blog/*` — **recommended**

- Landing-page Pages project ← `RyanShook/ryanshook.github.io` (no build; static publish from repo root).
- Blog Pages project ← `RyanShook/blog` (Jekyll build).
- Cloudflare Worker on route `ryanshook.org/*` that proxies `/blog/*` to the blog project and everything else to the landing project.
- Pros: URLs unchanged (no SEO hit / no broken inbound links); stacks stay isolated; each repo failure is contained; both go private cleanly.
- Cons: One extra moving piece (the Worker). ~20 lines, free tier.

### B. Merge into one repo, one Pages project

Drop the landing page HTML/CSS into the Jekyll repo at root, exclude from Jekyll processing, keep blog under `/blog`. Single deploy unit. Downside: two stacks coexisting in one tree (Chirpy theme + handcrafted landing CSS) is messier to maintain, especially via agent edits.

### C. Move blog to `blog.ryanshook.org`

Two Pages projects, no Worker. Cleanest CF config but breaks every existing `/blog/...` URL — needs 301 redirects (which still means a Worker or Pages redirect rules), plus SEO/inbound-link cost. Rejected.

## Execution plan (for option A)

Order matters: stand up the new infra alongside the old, verify on `*.pages.dev`, only flip DNS at the very end. Old GH Pages stays live until the last step.

1. **Create landing Pages project**
   - Cloudflare dashboard → Workers & Pages → Create → Pages → Connect to Git.
   - Authorize the Cloudflare GitHub app on `RyanShook/ryanshook.github.io` (do this *before* the repo goes private — auth carries over).
   - Build settings: framework preset = None; build command = (empty); build output dir = `/` (or whatever folder the static files live in).
   - Verify on the assigned `*.pages.dev` URL.

2. **Create blog Pages project**
   - Same flow, repo = `RyanShook/blog`.
   - Build settings:
     - Framework preset: **Jekyll**
     - Build command: `JEKYLL_ENV=production bundle exec jekyll build`
     - Build output: `_site`
     - Env vars: `JEKYLL_ENV=production`, `RUBY_VERSION=3.2` (CF Pages default Ruby differs from the GH Actions workflow).
   - Note: `baseurl: "/blog"` is already set in `_config.yml`, so the build output will reference `/blog/...` URLs — exactly what the Worker will route to.
   - Verify on `*.pages.dev`. Spot-check that CSS/JS load and a recent post renders.

3. **Write the routing Worker**
   - Single Worker on route `ryanshook.org/*` (and `www.ryanshook.org/*` if applicable).
   - Logic: if `url.pathname` starts with `/blog`, fetch from blog Pages project; else fetch from landing Pages project. Preserve method, headers, body. Return the response untouched.
   - Easiest implementation: hard-code the two `*.pages.dev` hostnames and `fetch()` against them. (Service bindings are nicer but require both Pages projects + Worker in the same account, which we have.)
   - Sketch:
     ```js
     export default {
       async fetch(req) {
         const url = new URL(req.url);
         const target = url.pathname.startsWith("/blog")
           ? "https://<blog-project>.pages.dev"
           : "https://<landing-project>.pages.dev";
         const upstream = new URL(url.pathname + url.search, target);
         return fetch(new Request(upstream, req));
       },
     };
     ```
   - Deploy via `wrangler deploy`. Test against `*.workers.dev` URL with `Host: ryanshook.org` header before binding the route.

4. **Cut DNS / route over**
   - In the CF zone, remove or repoint the apex `A`/`AAAA`/`CNAME` records that currently target GitHub Pages.
   - Bind the Worker route `ryanshook.org/*` to the new Worker.
   - (Alternatively, if not using a Worker: attach `ryanshook.org` as the custom domain on the landing Pages project, and use Pages' "subpath" / a Pages Function for `/blog/*`. The Worker is simpler.)
   - Verify: `curl -I https://ryanshook.org/` and `https://ryanshook.org/blog/` both return 200 from the new infra. Check a few deep blog URLs.

5. **Decommission GH Pages**
   - GitHub repo Settings → Pages → disable, on both repos.
   - Remove `.github/workflows/pages-deploy.yml` from `RyanShook/blog` (CF Pages now builds on push).
   - Decide whether to keep the `CNAME` file in the landing repo (it's harmless on CF Pages but no longer needed).

6. **Make repos private**
   - `gh repo edit RyanShook/blog --visibility private --accept-visibility-change-consequences`
   - `gh repo edit RyanShook/ryanshook.github.io --visibility private --accept-visibility-change-consequences`
   - The Cloudflare GitHub app (authorized in steps 1 and 2) keeps build access after the visibility flip.

## Gotchas to remember

- **Auth-app order**: install the Cloudflare GitHub app on the repos *while public*. Easier than dealing with private-repo auth from a cold start.
- **Ruby version**: GH Actions used Ruby 3.2 with `bundler-cache: true`. CF Pages defaults to a different Ruby; pin via `RUBY_VERSION=3.2` env var or a `.tool-versions` file.
- **`baseurl: /blog`**: don't change this. The Worker routes `/blog/*` and Jekyll already emits links under `/blog/...`. Leaving it alone preserves all existing URLs.
- **The untracked `ryanshook.github.io/` directory** in this repo's working tree is just a sibling clone someone parked here — not part of this repo. Ignore.
- **`firebase-debug.log`** in the working tree is also stray — unrelated to this migration.
- **Sveltia CMS files** (`sveltia-cms-auth*.js`, `admin/`) — confirm whether they're still in use and whether the auth flow assumes a public repo before flipping to private.
- **Inbound links to `/blog/...`** stay intact under option A. Don't switch to option C without setting up 301s.

## What was *not* done

Nothing. No CF Pages projects created, no DNS touched, no repo visibility changed, no workflow files removed. This file is the only artifact.
