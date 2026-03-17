# Arts For Ecology — Form Data Guide

## Overview

The website captures two types of form data:
1. **Membership Applications** — from the Join form
2. **Donations & Pledges** — from the Donate page

Both are stored in browser memory during your session and can be exported.
With the Google Sheets connection active, membership applications also save to a spreadsheet automatically.

---

## How to view submissions (Admin Panel)

1. Go to `yoursite.com/#/admin`
2. Password: `0000` (change this in the HTML under `ADMIN_PASS`)
3. Click the **Submissions** tab → see all membership applications
4. Click the **Donations** tab → see all donations and pledges

---

## Exporting as CSV

### Membership Applications
1. Admin → Submissions tab
2. Click **⬇ Export CSV**
3. A file named `afc-submissions-YYYY-MM-DD.csv` downloads
4. Open in Excel, Google Sheets, or Numbers

CSV columns:
```
First_Name, Last_Name, Email, Cell_Number, Art_Field, Education, Years_In_Practice, Timestamp
```

### Donations & Pledges
1. Admin → Donations tab
2. Click **⬇ Export CSV**
3. A file named `afc-donations-YYYY-MM-DD.csv` downloads

CSV columns:
```
type, tier, amount, purpose, name, email, phone, org, date, message, ts
```

---

## Saving as PDF (printing a register)

### From Chrome / Edge:
1. Export the CSV and open it in Google Sheets
2. File → Print → set Paper size to A4
3. Under "Headers & footers" add your org name
4. Click **Print** → choose **Save as PDF**

### From the Admin panel directly:
1. Open Admin → Submissions in Chrome
2. Press `Ctrl+P` (Windows) or `Cmd+P` (Mac)
3. Change destination to **Save as PDF**
4. Layout: Landscape works best for wide tables
5. Untick "Headers and footers" for a cleaner look

---

## Saving data as SVG (visual format)

The Admin panel does not export SVG natively, but you can create a visual register:

### Method 1 — Screenshot to SVG via Figma (free)
1. Export CSV → open in Google Sheets
2. Select your data table → take a screenshot
3. Import the screenshot into Figma (free, figma.com)
4. Figma → Export → SVG

### Method 2 — Google Sheets to PDF to SVG
1. Export CSV → open in Google Sheets
2. File → Download → PDF
3. Use a free converter: https://cloudconvert.com/pdf-to-svg
4. Download the SVG

### Method 3 — Direct SVG from browser (for the pledge form)
The pledge form on the Donate page can be printed directly:

1. Navigate to `/#/donate`
2. Scroll to the Pledge Form section
3. Press `Ctrl+P` / `Cmd+P`
4. Save as PDF → then convert to SVG using cloudconvert.com

---

## Permanent data storage (beyond session)

Browser memory resets when you close the tab. To preserve data permanently:

### Option A — Google Sheets (recommended, free)
Follow the Google Apps Script setup in `AFC_SETUP_GUIDE.md`.  
Every submission auto-saves to your spreadsheet in real time.

### Option B — Export JSON regularly
1. Admin → click **⬇ Export JSON**
2. This copies all content (articles, artworks, submissions) to your clipboard
3. Open `index.html` in a text editor
4. Find `let STORE = {`
5. Replace the `submissions: [...]` array with your exported data
6. Save and re-upload

### Option C — Netlify Forms (if hosted on Netlify)
Add `netlify` attribute to your forms:
```html
<form netlify name="membership" ...>
```
Submissions then appear in your Netlify dashboard automatically, with email alerts.

---

## Email notifications

To get an email every time someone submits the join form:

### Via Google Apps Script (already set up in your code)
In your Apps Script, add this after `sheet.appendRow(...)`:

```javascript
MailApp.sendEmail(
  'siyabongamakhubu@artsforecology.co.za',
  'New AFC Membership Application',
  'Name: ' + data.First_Name + ' ' + data.Last_Name + '\n' +
  'Email: ' + data.Email + '\n' +
  'Field: ' + data.Art_Field
);
```

### Via Formspree (alternative)
Replace the Google Sheets URL in CFG.SHEET_URL with a Formspree endpoint.  
Formspree sends you an email for every submission (free up to 50/month).

---

## Data privacy (POPIA compliance)

South Africa's Protection of Personal Information Act (POPIA) requires:
- Informing users what data you collect ✓ (the form note does this)
- Storing data securely ✓ (Google Sheets with your account)
- Not sharing data with third parties ✓
- Allowing deletion on request — honour any such requests manually via the CSV

The consent checkbox on the pledge form satisfies POPIA's consent requirement.
