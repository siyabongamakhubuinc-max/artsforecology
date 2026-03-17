# Arts For Ecology — SEO Guide
**Checklist for search engine optimisation**

## What's already built in

The website includes the following SEO features out-of-the-box:

| Feature | Status | Notes |
|---------|--------|-------|
| Title tag with keywords | ✓ | "Arts For Ecology — Creative Front for Sustainability \| South Africa" |
| Meta description | ✓ | 155 characters, keyword-rich |
| Open Graph tags | ✓ | Facebook, LinkedIn, WhatsApp preview |
| Twitter/X card | ✓ | Large image card |
| JSON-LD structured data | ✓ | Organization schema |
| lang="en-ZA" | ✓ | South African English |
| Canonical URL | ✓ | Update to your actual domain |
| viewport-fit=cover | ✓ | iOS notch support |
| Skip link (accessibility) | ✓ | Screen reader friendly |
| Focus-visible ring | ✓ | Keyboard navigation |
| Reduced motion support | ✓ | Respects OS preference |
| Semantic HTML (main landmark) | ✓ | Wraps all page content |
| Dynamic page titles | ✓ | Changes per route |
| Favicon SVG | ✓ | Crisp at all sizes |
| Apple touch icon | ✓ | iOS home screen |
| font-display: optional | — | Add to Google Fonts URL |
| Sitemap.xml | — | Create separately (see below) |
| robots.txt | — | Create separately (see below) |

---

## Actions you must take

### 1. Update canonical URL
In `index.html`, find and replace every instance of:
```
artsforecology.co.za
```
with your actual domain.

### 2. Update OG image
Replace `assets/og-image.jpg` with a properly designed 1200×630 px image.
Use Canva → set canvas to 1200×630 → export JPEG.

### 3. Create sitemap.xml
Create a file named `sitemap.xml` in your repository root:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url><loc>https://artsforecology.co.za/</loc><priority>1.0</priority></url>
  <url><loc>https://artsforecology.co.za/#/initiatives</loc><priority>0.8</priority></url>
  <url><loc>https://artsforecology.co.za/#/articles</loc><priority>0.8</priority></url>
  <url><loc>https://artsforecology.co.za/#/auction</loc><priority>0.7</priority></url>
  <url><loc>https://artsforecology.co.za/#/donate</loc><priority>0.7</priority></url>
  <url><loc>https://artsforecology.co.za/#/join</loc><priority>0.9</priority></url>
</urlset>
```

Note: hash-based URLs (`/#/`) are not ideal for SEO but are standard for
single-page apps hosted on GitHub Pages without a server.

### 4. Create robots.txt
Create a file named `robots.txt` in your repository root:

```
User-agent: *
Allow: /

Sitemap: https://artsforecology.co.za/sitemap.xml
```

### 5. Submit to Google Search Console
1. Go to https://search.google.com/search-console
2. Add property → URL prefix → `https://artsforecology.co.za`
3. Verify via HTML tag method (add meta tag to `<head>` in index.html)
4. Submit your sitemap

### 6. Optimise article content
Each article has its own route (`/#/articles/a1` etc.) but uses client-side
rendering. For better indexing:
- Keep article titles descriptive and keyword-rich
- Write excerpts with full sentences (these appear in search results)
- Articles about climate, South Africa, art, sustainability rank for relevant terms

### 7. LinkedIn company page
- Ensure your LinkedIn company page links to `artsforecology.co.za`
- Share new articles as LinkedIn posts to build backlinks
- The JSON-LD includes your LinkedIn page already

---

## Performance optimisation

### Image compression
Before uploading artwork images, compress them:
- **Squoosh**: https://squoosh.app (free, browser-based)
- Target: JPEG quality 80–85%, file size under 300 KB per image

### Font loading
The Google Fonts link already uses `preconnect`. To further optimise,
add `&display=swap` to the Google Fonts URL:
```
...family=Playfair+Display:...&display=swap
```

### Core Web Vitals
The site is designed for good Core Web Vitals:
- Reduced motion prevents layout shifts
- Images use `loading="lazy"` 
- CSS uses `will-change` sparingly
- No JavaScript frameworks — fast first load

---

## Local SEO (South Africa)

- Add to **Google Business Profile** if you have a physical office
- Target keywords: "climate art South Africa", "arts ecology NGO", 
  "creative sustainability South Africa", "environmental art auction SA"
- Get listed in South African NGO directories:
  - NGO Pulse: ngopulse.org
  - SA NGO Network: sangoco.org.za

---

## Tracking

Add Google Analytics or Plausible Analytics:

**Plausible (privacy-first, recommended):**
```html
<script defer data-domain="artsforecology.co.za"
  src="https://plausible.io/js/script.js"></script>
```
Add this just before `</head>` in index.html.

**Google Analytics 4:**
```html
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer=window.dataLayer||[];
  function gtag(){dataLayer.push(arguments);}
  gtag('js',new Date());
  gtag('config','G-XXXXXXXXXX');
</script>
```
Replace `G-XXXXXXXXXX` with your GA4 Measurement ID.
