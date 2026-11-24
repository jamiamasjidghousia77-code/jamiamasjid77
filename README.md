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
The inline JavaScript is minified, but the settings are still easy to find: search for the name, the value follows it.
- SPREADSHEET_URL    — Google Apps Script link for Fajr, Zuhr, Asr, Isha and Jumma
- MONTHS             — the masjid's permanent 12-month chart; Maghrib Jamaat = Azan + 4 min
- HIJRI_OFFSET_DAYS  — currently -1 (Pakistan moon sighting is ~1 day after Umm al-Qura). Re-check each month: 0, -1 or 1

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
- JavaScript: Google Analytics is queued immediately but gtag.js is fetched only after the page has loaded and the browser is idle.
- Images: hero art is served as AVIF (WebP fallback, 800w and 1200w); the logo is a 128px WebP (PNG kept for favicon/apple-touch-icon).
- Compression: Vercel serves HTML/CSS/JS/SVG/XML with Brotli (Gzip fallback) automatically; nothing to switch on.
  Check it with:  curl -sI -H "Accept-Encoding: br" https://jamiamasjid77.vercel.app/ | grep -i content-encoding
- vercel.json only adds a 7-day cache header to the static images listed in it. If you replace one of those images
  with a new picture under the same file name, visitors may see the old one for up to 7 days; use a new file name instead.
- Other hosts: Netlify and Cloudflare Pages compress automatically. On Apache enable mod_brotli/mod_deflate,
  on Nginx enable `brotli on;` / `gzip on;` for text/html, text/css, application/javascript, application/xml.
