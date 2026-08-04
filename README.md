# mvdvldn.com

Personal academic site — bio, CV, and research timeline. Built with [Hugo](https://gohugo.io), no theme dependency (the whole design lives in `layouts/` and `static/css/style.css`), so there's nothing to `git submodule` or install beyond Hugo itself.

## Structure

```
content/
  _index.md          → home page bio + research interests (edit this for your "about" text)
  cv/_index.md        → CV page (content pulled from data/cv.yaml, not this file)
  timeline/_index.md  → Timeline page (content pulled from data/timeline.yaml)
data/
  cv.yaml              → all CV content: education, research, teaching, skills, grants, activities
  timeline.yaml         → publications / conferences / talks, sorted by date automatically
layouts/               → all page templates (home, cv, timeline, header, footer)
static/css/style.css   → all styling
static/CNAME            → tells GitHub Pages to serve this at mvdvldn.com
```

**To update your CV or timeline later, edit `data/cv.yaml` or `data/timeline.yaml` directly** — you don't need to touch any HTML/template files. Add a new job, publication, or talk by copying the YAML pattern of an existing entry.

## Running locally

You'll need Hugo installed (extended version, since this uses Hugo's built-in asset pipeline conventions):

```bash
# macOS
brew install hugo

# or download from https://github.com/gohugoio/hugo/releases
```

Then, from this folder:

```bash
hugo server -D
```

Visit `http://localhost:1313` — it live-reloads as you edit.

## Deploying to GitHub Pages

1. Create a new **public** GitHub repo (e.g. `mvdvldn03/mvdvldn.com` — the repo name doesn't need to match your username since you're using a custom domain).
2. Push this whole folder as the repo contents:
   ```bash
   git init
   git add .
   git commit -m "initial site"
   git branch -M main
   git remote add origin https://github.com/mvdvldn03/mvdvldn.com.git
   git push -u origin main
   ```
3. In the repo on GitHub: **Settings → Pages → Build and deployment → Source → GitHub Actions**. The included workflow (`.github/workflows/hugo.yml`) will build and deploy automatically on every push to `main`.
4. In the repo: **Settings → Pages → Custom domain**, enter `mvdvldn.com`, save. GitHub will verify the `CNAME` file that's already in this repo (`static/CNAME`) and check DNS.
5. In Cloudflare: go to your `mvdvldn.com` zone → **DNS**, and add these two records (GitHub's standard Pages setup):
   - `A` record, name `@`, pointing to GitHub Pages' IPs: `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153` (add all four as separate A records)
   - `CNAME` record, name `www`, value `mvdvldn03.github.io`
   - Set these DNS records' proxy status to **DNS only** (gray cloud, not orange) at first — GitHub Pages needs to see your real IP to issue an SSL certificate. You can switch to Cloudflare proxy (orange cloud) afterward once HTTPS is working, if you want Cloudflare's CDN/protection in front.
6. Back in GitHub Pages settings, check **Enforce HTTPS** once the certificate is issued (can take a few minutes to an hour).

After that, `mvdvldn.com` will serve the live site, auto-rebuilding every time you push a change.

## Updating content

- **Bio/about text** → edit `content/_index.md`
- **CV** → edit `data/cv.yaml`
- **Timeline** → edit `data/timeline.yaml` (add new entries under `events:`, following the existing date/type/title/venue format — the page sorts them automatically)
- **Colors/fonts** → edit the `:root` variables at the top of `static/css/style.css`
