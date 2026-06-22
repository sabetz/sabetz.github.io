# Sarah Betz — Academic Website

Personal academic site for Sarah Betz, PhD candidate in Economics at Stony Brook University (migration & networks research).

- **Live**: https://sarah-betz.com (custom domain → sabetz.github.io)
- **DNS**: GoDaddy (four A records → GitHub Pages IPs; CNAME www → sabetz.github.io)
- **Stack**: Hugo 0.145 + HugoBlox (blox-tailwind v0.3.2), vendored via `go.mod`, Tailwind via PostCSS, GA4 analytics (`G-V81PWEQ6ZJ`)
- **Project root**: `C:\Users\Sarah\Documents\Projects\my-website`

## Deploy

Auto-deploys on push to `main` via `.github/workflows/publish.yaml`:
1. Installs npm deps (needed for Tailwind/PostCSS)
2. Runs `hugo --minify --baseURL <pages_url>`
3. Uploads artifact and deploys via `actions/deploy-pages@v4`

Repo Pages source **must be set to "GitHub Actions"** in repo settings, not "Deploy from a branch" (the latter renders `README.md` as the homepage).

## Key paths

| Path | Purpose |
|------|---------|
| `content/_index.md` | Homepage sections (bio block, research, working papers) |
| `content/authors/admin/_index.md` | Bio, profile, education, social links |
| `content/teaching/` | Course pages (ECO 108, 500, 501) |
| `content/gallery/photos/` | Gallery photos |
| `config/_default/hugo.yaml` | `baseURL`, imaging, output formats |
| `config/_default/params.yaml` | Theme color (`emerald`), nav, analytics, theme toggle |
| `config/_default/menus.yaml` | Navbar items |
| `layouts/partials/blox/resume-biography-3.html` | Bio block override (avatar size) |
| `layouts/partials/views/article-grid.html` | No-link teaching card override |
| `layouts/shortcodes/gallery.html` | Custom gallery shortcode |
| `assets/css/custom.css` | Responsive avatar sizing |
| `assets/media/icon.png` | Favicon — must be at this exact path |
| `static/CNAME` | Custom domain for GitHub Pages |

## Gotchas

- **Favicon path is exact.** `HugoBlox/get_site_icon.html` only reads `assets/media/icon.png`. A subfolder like `assets/media/icons/icon.png` silently fails back to the default HugoBlox icon.
- **Don't hardcode block backgrounds.** Setting `background: { color: white }` on a block looks fine in light mode but breaks dark mode (white text on white). Let the theme handle it.
- **Theme toggle** is controlled by `header.navbar.show_theme_chooser` in `params.yaml`. Keep it `true`.
- **Gallery EXIF rotation** must be baked into pixels (PowerShell `System.Drawing` `Rotate90FlipNone` + strip tag 274), not just metadata, or Hugo will display photos sideways.
- **Hugo image cache** at `resources/_gen/images/` can show stale versions after asset changes. Delete it and restart `hugo server` if you see old images.
- **Vendor directory** (`_vendor/`) is `.gitignore`d. GitHub Actions resolves modules from `go.mod` at build time. If you need offline builds, run `hugo mod vendor` locally; don't commit it.
- **Gitignored**: `_vendor/`, `node_modules/`, `public/`, `resources/`, `hugo_stats.json`, `.hugo_build.lock`, `.claude/`, scratch files matching `.preview-*` / `.option-*`.

## Workflow

- Preview server config is in `.claude/launch.json` as `hugo-site` (port 1313). Use the preview MCP if available; otherwise `hugo server`.
- **Verify UI changes in the browser** before reporting done. Build success ≠ feature works.
- **Never amend commits.** Always create a new commit.
- Commit style: short subject, brief "why" body, `Co-Authored-By` footer.
- Auto-memory for this repo lives at `~/.claude/projects/` — check `project_website.md` for accumulated context before exploring blindly.
