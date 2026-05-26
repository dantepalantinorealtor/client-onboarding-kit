# Client Onboarding Kit

A high-end, interactive onboarding kit for real estate clients. Dark navy + champagne gold + serif typography. Built as a single static HTML file — no build step, no dependencies.

## Preview

Open `index.html` in a browser. The kit is a single scrolling page with:

- **Animated section reveals** on scroll
- **Expandable cards** in "What to Expect" — click to see deeper detail
- **Expandable timeline** — click any step to see what's delivered
- **FAQ accordion**
- **Live tweaks** for cover layout (Classic / Centered / Split) and section order

## Files

- `index.html` — the onboarding kit (open in any browser to preview)
- `intake.html` + `intake.js` — the client intake wizard (buyer & seller flows)
- `README.md` — this file
- `assets/` — brand logos (Palantino key, lockup, RE/MAX One mark)

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
   git add index.html intake.html intake.js README.md assets/
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

---

## Intake Form Setup (intake.html)

The intake form is a multi-step wizard with separate buyer and seller flows. It saves progress to the client's `localStorage` so they can close the tab and come back later. Required fields are validated inline.

**Until you wire up a submission endpoint, the form simulates a successful submit** and logs the payload to the browser console — useful for testing the flow.

### Submission → Google Sheet (via Google Apps Script)

To collect responses in a Google Sheet:

1. **Create a new Google Sheet** to hold responses (e.g. "Palantino — Client Intake"). Add a single header row across the top — the column names should match the field keys from `intake.js`:
   ```
   submitted_at | type | first_name | last_name | email | phone | current_address | prompt_move | move_type | timeline | whats_important | musts_vs_nice | finance_or_cash | lender_status | price_range | monthly_payment | home_style | areas | deal_breakers | previous_homes | decision_makers
   ```
   (Add seller columns too: `property_address`, `property_type`, `beds`, `baths`, `sqft`, `year_built`, `reason`, `sell_timeline`, `occupancy`, `improvements`, `known_issues`, `previously_listed`, `target_price`, `mortgage_balance`, `flexibility`, `open_houses`, `short_notice`, `anything_else`.)

2. **In that sheet**, click `Extensions → Apps Script`. Paste this code:

   ```js
   const SHEET_ID = 'PASTE_YOUR_SHEET_ID_HERE'; // from the sheet's URL
   const SHEET_NAME = 'Sheet1';

   function doPost(e) {
     try {
       const body = JSON.parse(e.postData.contents);
       const sheet = SpreadsheetApp.openById(SHEET_ID).getSheetByName(SHEET_NAME);
       const headers = sheet.getRange(1, 1, 1, sheet.getLastColumn()).getValues()[0];
       const row = headers.map(h => body[h] || '');
       sheet.appendRow(row);
       return ContentService.createTextOutput(JSON.stringify({ ok: true }))
         .setMimeType(ContentService.MimeType.JSON);
     } catch (err) {
       return ContentService.createTextOutput(JSON.stringify({ ok: false, error: err.toString() }))
         .setMimeType(ContentService.MimeType.JSON);
     }
   }
   ```

3. **Deploy** it: `Deploy → New deployment → Type: Web app`. Set:
   - Execute as: **Me**
   - Who has access: **Anyone**

   Copy the resulting Web App URL.

4. **In `intake.js`**, replace the placeholder near the top:

   ```js
   const ENDPOINT_URL = 'PASTE_YOUR_WEB_APP_URL_HERE';
   ```

5. Push, and you're live.

### Email-on-submit (optional)

To also get an email when a form is submitted, add this inside the `try` block of `doPost` before the `return`:

```js
MailApp.sendEmail({
  to: 'dantejpalantino@gmail.com',
  subject: `New ${body.type} intake — ${body.first_name} ${body.last_name}`,
  body: Object.entries(body).map(([k,v]) => `${k}: ${v}`).join('\n'),
});
```

### Customizing the form

Questions, options, and conditional logic live in the `BUYER_STEPS` and `SELLER_STEPS` arrays at the top of `intake.js`. Each step is a group of fields. Each field has a `key`, `label`, `type` (`text` / `email` / `tel` / `textarea` / `radio` / `radio_h` / `radio_other`), and optional `required`, `help`, `placeholder`, `half` (half-width on desktop), and `showIf` (conditional logic) properties.

Reorder, add, or remove fields freely — the sidebar progress and review screen rebuild from those arrays.
