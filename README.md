# Client Onboarding Kit

A high-end, interactive onboarding kit for real estate clients. Dark navy + champagne gold + serif typography. Built as a single static HTML file — no build step, no dependencies.

## Preview

Open `index.html` in a browser. The kit is a single scrolling page with:

- **Animated section reveals** on scroll
- **Expandable cards** in "What to Expect" — click to see deeper detail
- **Expandable timeline** — click any step to see what's delivered
- **FAQ accordion**
- **Live tweaks** for cover layout (Classic / Centered / Split) and section order

## Customize Before Deploying

Open `index.html` and find/replace these placeholders:

| Placeholder | What it is |
|---|---|
| `[Your Name]` | Your name (appears in cover, footer) |
| `[Your Brokerage]` | Brokerage or firm name |
| `[Year]` | Year established |
| `[555.000.0000]` | Phone |
| `[hello@yourdomain.com]` | Email |
| `[@yourhandle]` | Instagram handle |
| `[yourdomain.com]` | Website |

You can also edit the `EXPECT_CARDS`, `TIMELINE_STEPS`, and `FAQ_ITEMS` arrays inside the `<script>` tag at the bottom of `index.html` to rewrite the content.

## Deploy to GitHub Pages

1. **Create a new repo** on GitHub (e.g. `onboarding-kit`).
2. **Push these files** to the `main` branch:
   ```bash
   git init
   git add index.html README.md
   git commit -m "Initial onboarding kit"
   git branch -M main
   git remote add origin https://github.com/YOUR-USERNAME/onboarding-kit.git
   git push -u origin main
   ```
3. **Enable Pages**: Repo → Settings → Pages → Source: `Deploy from a branch` → Branch: `main` / `/ (root)` → Save.
4. Your site will be live at `https://YOUR-USERNAME.github.io/onboarding-kit/` within a minute or two.

## Custom Domain (optional)

To use your own domain (e.g. `welcome.yourdomain.com`):

1. In Repo → Settings → Pages, add your custom domain.
2. At your DNS provider, add a `CNAME` record pointing to `YOUR-USERNAME.github.io`.
3. Enable **Enforce HTTPS** once the certificate provisions.

## Notes

- No build step. No npm. Just open the HTML file.
- Fonts are loaded from Google Fonts (Cormorant Garamond + Inter).
- All interactivity is vanilla JS — no React, no frameworks.
- The kit is responsive down to ~360px width.
