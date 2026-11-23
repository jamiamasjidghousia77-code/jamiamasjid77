# Jamia Masjid Ghousia School Wali — premium redesign

Pages: index.html, prayer-timings.html, imam-muazzin.html, location-contact.html (each is a single self-contained file; no separate CSS/JS).

## Optional photos (drop into this folder, no code change needed)
- hero.webp  — mosque photo behind the homepage hero (1600x900 recommended)
- imam.webp, muazzin.webp — portraits (about 304x368); until added, an emblem is shown

## Editable settings (inside prayer-timings.html and index.html)
- SPREADSHEET_URL — Google Apps Script link for Fajr, Zuhr, Asr, Isha and Jumma (unchanged)
- MONTHS — the masjid's permanent 12-month chart (unchanged); Maghrib Jamaat = Azan + 4 min
- HIJRI_OFFSET_DAYS — set to -1 or 1 if the moon-sighting date differs from the calendar shown

## Contact
Only the masjid email exists in the original project. To add a phone or WhatsApp row, see the comment inside the contact block (tel: / https://wa.me/ links).
