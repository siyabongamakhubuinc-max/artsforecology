# Arts For Ecology — Deployment & Repository Guide
**Version 2 · Updated for final release**

## Complete repository structure

```
artsforecology/
├── index.html
├── assets/
│   ├── README.md
│   ├── favicon.svg
│   ├── apple-touch-icon.png
│   ├── og-image.jpg
│   ├── artwork-001.jpg  →  artwork-008.jpg
└── docs/
    ├── DEPLOYMENT_GUIDE.md
    ├── PAYMENT_GUIDE.md
    ├── FORM_DATA_GUIDE.md
    └── SEO_GUIDE.md
```

## Pre-upload checklist

- [ ] Rename file to `index.html`
- [ ] Replace `YOUR_GOOGLE_APPS_SCRIPT_URL` with real URL
- [ ] Replace `62XXXXXXXXX` with real bank account number
- [ ] Set auction end date: Admin → ⚙ Settings
- [ ] Change admin password from `0000` (find `ADMIN_PASS`)
- [ ] Replace placeholder artwork JPGs with real photos
- [ ] Verify `artsforecology.co.za` matches your domain
- [ ] Check `assets/og-image.jpg` looks good when shared

## GitHub Pages setup

1. GitHub.com → New repository → Public → name `artsforecology`
2. Upload `index.html` + `assets/` folder + `docs/` folder
3. Settings → Pages → Deploy from branch: main / (root) → Save
4. Live at: `https://YOUR-USERNAME.github.io/artsforecology`

## Custom domain DNS records

**Apex (artsforecology.co.za):**
```
A  @  185.199.108.153
A  @  185.199.109.153
A  @  185.199.110.153
A  @  185.199.111.153
```
**WWW:**
```
CNAME  www  YOUR-USERNAME.github.io
```
After DNS propagates (~24h): Settings → Pages → tick **Enforce HTTPS**

## Updating content (recommended workflow)

1. Make changes in Admin panel (`/#/admin`, password `0000`)
2. Click **⬇ Export JSON**
3. Open `index.html` in text editor → find `let STORE = {`
4. Replace the arrays with exported JSON data
5. Save → re-upload to GitHub

## Replacing artwork images

- Prepare: 800×1067 px JPEG, under 500 KB (compress: squoosh.app)
- Name: `artwork-001.jpg` matching the lot number
- Upload to `assets/` folder in GitHub
- Admin → Artworks → Edit → Image Path: `assets/artwork-001.jpg`
- Export JSON → paste into `index.html` → re-upload

## Auction date management

Admin → ⚙ Settings → Set Auction Close Date & Time → Set Date
The countdown timer updates live. Export JSON to make permanent.

## Troubleshooting

| Problem | Fix |
|---------|-----|
| 404 error | File must be named exactly `index.html` |
| Images broken | Path `assets/artwork-001.jpg` — case sensitive |
| Countdown wrong | Admin → ⚙ Settings → update date → Export JSON |
| Domain not working | DNS propagation takes 24-48 hours |
| Social image wrong | Upload new `og-image.jpg` to `assets/` |
| Form data missing | Set `CFG.SHEET_URL` to your Google Apps Script URL |
