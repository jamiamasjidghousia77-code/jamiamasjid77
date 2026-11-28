# Jamia Masjid Ghousia School Wali — website

Pages: index.html, prayer-timings.html, imam-muazzin.html, location-contact.html
(each is a single self-contained file: CSS and JavaScript are inline, tuned per page).

## Optional photos (currently commented out in the HTML)
The three photo files below do not exist yet. Loading a missing file caused a 404 and a console error on every
visit (this lowers the Lighthouse Best Practices score), so the `<img>` tags are now HTML comments.
Add the file, then replace the comment with the `<img>` tag written inside it:
- hero.webp (1600x900)  — index.html, search for "OPTIONAL hero photo"
- imam.webp, muazzin.webp (about 304x368) — index.html and imam-muazzin.html, search for "OPTIONAL portrait"
Until then the emblem icon is shown, exactly as before.

## Editable settings (index.html and prayer-timings.html)
The inline JavaScript in these files is minified for speed, but the setting names below are unchanged: search for the name and edit the value that follows it. A readable, commented copy of every page is in the separate readable-source folder (keep it on your computer; do not upload it).
- SPREADSHEET_URL    — Google Apps Script link for Fajr, Zuhr, Asr, Isha and Jumma
- MONTHS             — the masjid's permanent 12-month chart; Maghrib Jamaat = Azan + 4 min
- HIJRI_OFFSET_DAYS  — currently -1 (Pakistan moon sighting is ~1 day after Umm al-Qura). Re-check each month: 0, -1 or 1

## Maghrib from the Google Sheet (new, optional)
Maghrib Azan always follows the 12-month chart (automatic). Jamaat is Azan + 4 minutes by default. You can now change this from the sheet,
without touching the website. Add these two rows to the same sheet that already has Fajr / Zuhr / Asr / Isha / Jumma:
- Maghrib Gap     — minutes after Azan, e.g. 3, 4 or 5 (0 to 60). Blank = 4. Jamaat stays automatic: it still moves daily with sunset.
- Maghrib Manual  — a fixed Jamaat time, e.g. 6:05 PM (or 18:05). It wins over the gap and replaces today's Maghrib Jamaat.
                    DELETE the value to go back to automatic.
Both rows are optional; with neither of them the site behaves exactly as before (Azan + 4 min).
The home page and the prayer-timings page (card, the "Jamaat is N minutes after Azan" notes and the Maghrib Jamaat column of the 12-month table)
all follow the sheet. A manual time is shown only on today's row of the table. The default gap (4) is MAGHRIB_JAMAAT_GAP in the page script.
If the Apps Script only returns fixed rows, it must also return these two rows (the row names are not case sensitive).

## Contact
Only the masjid email exists in the original project. To add a phone or WhatsApp row, see the comment inside the
contact block (tel: / https://wa.me/ links).

## Share preview
og-image.jpg (1200x630) is used for WhatsApp/Facebook/Twitter link previews. It stays a JPEG on purpose
(social crawlers do not reliably read WebP/AVIF).

## Performance notes
- Fonts: only Cairo (Latin/UI, one variable file) and Amiri (Urdu/Arabic). Cormorant Garamond and Manrope were never visible.
- CSS: one merged <style> per page; rules that page does not use are removed.
  If you add NEW sections or utility classes later and they look unstyled, the page-specific CSS needs regenerating.
- Fonts: one single stylesheet link per page (the duplicate <noscript> copy was removed).
- JavaScript: Google Analytics is queued immediately but gtag.js is fetched only after the page has loaded and the browser is idle.
- Images: hero art is served as AVIF (WebP fallback) in 480/640/800/1200w. File names contain a content hash (hero-art-800.f22c87de.avif) so they are cached for 1 year (immutable). If you replace a hero image, give it a NEW name and update index.html (preload + <picture>). The logo is a 128px WebP (PNG kept for favicon/apple-touch-icon).
- Hero preload is the first thing in index.html <head>. Vercel Insights and Google Analytics both load only after the page has loaded.
- Compression: Vercel serves HTML/CSS/JS/SVG/XML with Brotli (Gzip fallback) automatically; nothing to switch on.
  Check it with:  curl -sI -H "Accept-Encoding: br" https://jamiamasjid77.vercel.app/ | grep -i content-encoding
- vercel.json: hashed hero images = 1 year immutable; logo.* and og-image.jpg = 30 days; plus nosniff / referrer / permissions headers on every file. HTML keeps Vercel's default (always revalidated).
- /_vercel/insights/script.js only exists once Web Analytics is enabled in the Vercel project (Project > Analytics). If it is not enabled the request 404s and logs a console error, which costs Best Practices points.
- Other hosts: Netlify and Cloudflare Pages compress automatically. On Apache enable mod_brotli/mod_deflate,
  on Nginx enable `brotli on;` / `gzip on;` for text/html, text/css, application/javascript, application/xml.
- Final performance pass: the inline JavaScript is minified; the header "scrolled" shadow uses an IntersectionObserver (no forced reflow); the two pulsing dots animate transform/opacity (compositor) instead of box-shadow; sections below the first screen use content-visibility:auto so the browser skips their layout until they are near the viewport.
- Third-party timing: Google Fonts are requested right after the page's load event (not during first paint), the first Google Sheets fetch also starts after load (cached/default timings still show instantly), and Google Analytics (gtag.js) loads on the visitor's first interaction or GA_FALLBACK_MS (5000 ms) after load, whichever comes first. To make analytics count very short visits, lower GA_FALLBACK_MS (search for it in each page).
