# Arts For Ecology — Complete Setup Guide

## OVERVIEW
Your website is a single HTML file. Here's what needs connecting:
1. Form submissions → your Google Sheet
2. Admin panel → manage articles & initiatives
3. Deploy to the web (free)

---

## STEP 1: Connect Form to Google Sheets (20 minutes)

### 1.1 — Create the spreadsheet
1. Go to https://sheets.google.com
2. Create a new spreadsheet, name it: **AFC Membership Applications**
3. In Row 1, add these exact column headers (one per cell):
   ```
   Timestamp | First_Name | Last_Name | Email | Cell_Number | Art_Field | Education | Years_In_Practice
   ```

### 1.2 — Create the Apps Script
1. In your spreadsheet, click **Extensions → Apps Script**
2. Delete all existing code in the editor
3. Paste this code exactly:

```javascript
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    var data = JSON.parse(e.postData.contents);
    
    sheet.appendRow([
      data.Timestamp || new Date().toISOString(),
      data.First_Name || '',
      data.Last_Name || '',
      data.Email || '',
      data.Cell_Number || '',
      data.Art_Field || '',
      data.Education || '',
      data.Years_In_Practice || ''
    ]);
    
    return ContentService
      .createTextOutput(JSON.stringify({result: 'success'}))
      .setMimeType(ContentService.MimeType.JSON);
  } catch(err) {
    return ContentService
      .createTextOutput(JSON.stringify({result: 'error', error: err.toString()}))
      .setMimeType(ContentService.MimeType.JSON);
  }
}

function doGet(e) {
  return ContentService
    .createTextOutput('AFC Membership Form — Active')
    .setMimeType(ContentService.MimeType.TEXT);
}
```

4. Click **Save** (floppy disk icon), name the project **AFC Form Handler**

### 1.3 — Deploy as Web App
1. Click **Deploy → New deployment**
2. Click the gear icon ⚙ next to "Select type" → choose **Web app**
3. Set these options:
   - Description: `AFC Form v1`
   - Execute as: **Me** (your Google account)
   - Who has access: **Anyone**
4. Click **Deploy**
5. Click **Authorize access** → choose your Google account → click **Allow**
6. **Copy the Web App URL** — it looks like:
   `https://script.google.com/macros/s/AKfycb.../exec`

### 1.4 — Add the URL to your website
1. Open `arts-for-ecology-webapp.html` in a text editor (Notepad, TextEdit, VS Code)
2. Find this line near the top:
   ```
   SHEET_URL: 'YOUR_GOOGLE_APPS_SCRIPT_URL',
   ```
3. Replace `YOUR_GOOGLE_APPS_SCRIPT_URL` with your copied URL:
   ```
   SHEET_URL: 'https://script.google.com/macros/s/AKfycb.../exec',
   ```
4. Save the file

**Test it:** Open the file, fill in the Join form, click Submit. Check your Google Sheet — the row should appear within seconds.

---

## STEP 2: Change the Admin Password

1. In `arts-for-ecology-webapp.html`, find:
   ```
   ADMIN_PASS: 'afc2024!',
   ```
2. Change `afc2024!` to your own secure password
3. Save the file

---

## STEP 3: Deploy to Netlify (10 minutes, free)

1. Go to **https://netlify.com** → Sign Up (free)
2. Once logged in, click **"Add new site" → "Deploy manually"**
3. Drag and drop your `arts-for-ecology-webapp.html` file onto the upload box
4. Your site goes live instantly at a URL like `https://random-name-123.netlify.app`

### Use your own domain
- In Netlify: **Site Settings → Domain Management → Add custom domain**
- Type your domain (e.g. `artsforecology.co.za`)
- Follow the DNS instructions Netlify gives you
- Takes 10–60 minutes to go live on your domain

### Rename the HTML file for clean URLs
Before uploading, rename the file to `index.html` — Netlify will then serve it at your root domain directly.

---

## STEP 4: Using the Admin Panel (ongoing)

**Access:** Go to `yoursite.com/#/admin`

**Login** with your password.

### Adding a new article:
1. Go to Admin → Articles tab
2. Fill in: Category, Date, Title, Author, Excerpt (2 sentences), Full Content
3. For content, use HTML tags:
   ```html
   <h2>Section Heading</h2>
   <p>Your paragraph text here.</p>
   <strong>Bold/important text</strong>
   <blockquote>A pull quote or key statement</blockquote>
   <ul><li>Bullet point one</li><li>Bullet point two</li></ul>
   ```
4. Click **Save Article**

### Making changes permanent:
1. Click **Export Content JSON** (copies to clipboard)
2. Open `arts-for-ecology-webapp.html` in a text editor
3. Find: `let STORE = {`
4. Replace the entire `articles: [...]` and `initiatives: [...]` sections with your exported data
5. Save and re-upload to Netlify (drag & drop again — takes 30 seconds)

---

## COST SUMMARY

| Item | Cost |
|------|------|
| Netlify hosting | Free |
| Google Sheets | Free |
| Google Apps Script | Free |
| Your domain (if you have one) | ~R200/year |
| **Total monthly** | **R0** |

---

## TROUBLESHOOTING

**Form submissions not appearing in Google Sheets?**
- Make sure "Who has access" is set to **Anyone** when deploying the Apps Script
- If you edit the Apps Script code, you must create a **New deployment** (not update existing)
- Check the URL in the HTML file has no extra spaces

**Site not loading on mobile?**
- Make sure you renamed the file to `index.html` before uploading to Netlify
- Clear your phone browser cache and try again

**Admin panel won't accept my password?**
- Make sure there are no extra spaces in the password when you edited the HTML
- Password is case-sensitive

**Need help?**
Contact: siyabongamakhubu@artsforecology.co.za
