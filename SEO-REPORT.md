# Technical SEO report: jamiamasjid77.vercel.app
Audit date: 2026-11-28. Basis: the deployable files in `jamiamasjid77-website-maghrib-fix.zip`, tested in a real browser with JavaScript on and off.

**Not verified:** I could not reach the live site from my environment, so live HTTP status codes and response headers were not checked. Section 2 gives the commands to run. Everything else comes from the files.

## 1. Summary
- **No hard blocker found.** There is no `noindex`, no robots.txt block, no redirect problem, and all four pages have a self-canonical and appear in the sitemap.
- **Likely cause of "Discovered / pending":** the three pages were weak and low-priority. Their key content was missing from the HTML. The real timings and the 12-month table came only from JavaScript. The Imam and Location pages had about 60 words of unique text. A ~700-word copyright policy was repeated in every page's HTML. Two pages largely repeated sections of the homepage.
- **Also found:** the JavaScript fallback times did not match your official timings (see 3.1). Internal "Home" links pointed to `/index.html` while the canonical is `/`.
- **Fixes:** each pending page now has substantial, unique, crawlable HTML, richer structured data and stronger internal links from the homepage. The homepage was touched minimally.
- **No indexing guarantee.** Google decides. These fixes remove the technical and quality reasons it might skip the pages.

## 2. Checks
| Check | Result |
|---|---|
| robots.txt | `User-agent: *` / `Allow: /` plus the sitemap line. Fine, unchanged. |
| Meta robots | `index, follow` on all four. Fine. |
| X-Robots-Tag | None set in `vercel.json`. Fine. |
| Canonicals | Self-referencing and absolute on all pages. Home = `https://jamiamasjid77.vercel.app/` (unchanged). |
| Sitemap | All four canonical URLs, absolute, `lastmod` updated to 2026-11-28. |
| Redirects | None configured. `/` and `/index.html` both serve the homepage and the canonical resolves it. I did not add a redirect, to avoid touching the ranking page. |
| HTTP codes (live) | Not checked. Run: `for u in "" index.html prayer-timings.html imam-muazzin.html location-contact.html robots.txt sitemap.xml; do curl -s -o /dev/null -A Googlebot -w "%{http_code} /$u\n" https://jamiamasjid77.vercel.app/$u; done` (all should be 200). |

## 3. Page by page

### 3.1 Home `/` (indexed, ranking), minimal changes only
Changed:
- Internal "Home" links now `/` instead of `index.html`, matching the canonical.
- Three short contextual links added, one under each of the Prayer Timings, Imam & Muazzin and Find the Masjid sections. The existing hero buttons, quick-link tiles, About paragraph and footer already linked to all three pages.
- JSON-LD: the Mosque entity now has `@id`, geo coordinates (the coordinates already used in your map embed), and a description. A `WebSite` entity was added. `addressRegion` is now "Punjab".
- JS fallback times corrected to the official ones (see below).

Not changed: URL, canonical, title, description, H1, copy, design, images, preloads and sitemap URL.

**Fallback timings:** `DEFAULTS` held Fajr 05:15, Asr 16:40 and Isha 19:50. With the Google Sheet unreachable, the old pages displayed Fajr 5:17 AM, Asr 4:40 PM and Isha 7:50 PM, which are wrong. Googlebot may also see these. They are now Fajr 06:05, Zuhr 13:30, Asr 16:00 and Isha 19:00. The sheet still overrides them. **Jumma stays 13:45 as before; you did not give an official Jumma time, so please confirm it.**

Note: the homepage's own prayer cards and countdown are still filled by JavaScript only. I left that alone to protect the ranking page.

### 3.2 Today's Prayer Timings (`Discovered, currently not indexed`)
Problems found: prayer cards and the 12-month table were empty in raw HTML. About 150 words with JS off. 660 words of copyright text in the HTML. Home link was `index.html`.

Fixes:
- Prayer cards now carry official times in the HTML (Maghrib shown as "Azan + 4 min", no invented clock time). The 12-month table is pre-rendered for November. JS replaces both with live data.
- New sections: "Jamaat Times at a Glance" table, "Maghrib Azan Times Through the Year" (1st, 15th and last day of each month, computed from your own chart, with Jamaat stated as +4 min), and 6 visible Q&As.
- Copyright policy moved into a `<template>` and injected on click, so it no longer sits in the HTML. The modal still works.
- JSON-LD: `WebPage` + `BreadcrumbList` + Mosque reference.
- Result: about 150 words → about 1,100 words with JS off.

### 3.3 Imam & Muazzin (`Pending`)
Problems found: about 85 words in total. The two person cards were word-for-word the same as the homepage section. Same copyright boilerplate.

Fixes:
- Reworded the cards so they differ from the homepage.
- Added: Roles at the Masjid, When the Imam Leads Prayer (official times), About the Masjid, contact email and two short Q&As.
- Same copyright change.
- JSON-LD: `AboutPage`, `BreadcrumbList` and two `Person` entries (name, job title, employer). No invented credentials.
- Result: about 85 words → about 350 words.

### 3.4 Location & Contact (`Pending`)
Problems found: about 75 words, with address and email duplicating the homepage's "Find the Masjid" section. The map only loads via JS.

Fixes:
- Added: Address of the Masjid (`<address>`, landmarks, coordinates, directions link), How to Find the Masjid, Planning Your Visit, Contact Details with two Q&As.
- `<noscript>` Google Maps link.
- Same copyright change.
- JSON-LD: `ContactPage`, `BreadcrumbList` and a full `Mosque` (address, geo, email, `hasMap`).
- Result: about 75 words → about 290 words.

## 4. What I did not invent
No phone number, opening hours, history, capacity, facilities, qualifications or Maghrib clock time. The only figures used are yours (Fajr 6:05, Zuhr 1:30, Asr 4:00, Isha 7:00, Maghrib +4 min) and your own existing 12-month chart and map coordinates.

## 5. Things to know / maintain
- If the standing timings change, update four places: the `DEFAULTS` line in `index.html` and `prayer-timings.html`, the static cards/table/FAQ text on `prayer-timings.html`, the lists on `imam-muazzin.html` and `location-contact.html`, and the sheet. The Maghrib gap text updates automatically (it already followed the sheet).
- `readable-source/` from your first zip was **not** updated. The deployable files are now the source of truth.
- A few Q&As use plain headings and paragraphs, not FAQ rich-result markup, because Google restricts FAQ rich results to a few site types.
- Nothing is cloaked: the static text matches what JavaScript displays.

## 6. Google Search Console, after deploying
1. Deploy the folder to Vercel (replace all files). Open the four URLs and `/sitemap.xml` to confirm they load.
2. Run the curl check in section 2 and confirm 200 on every URL.
3. **URL Inspection → homepage first.** Check "Google-selected canonical". Expect `https://jamiamasjid77.vercel.app/`. If it shows `/index.html`, tell me before doing anything else. Do not request re-indexing of the homepage unless you want Google to re-read the new links.
4. **URL Inspection for each pending URL → "Test Live URL" → View tested page.** Confirm the HTML contains the new text (screenshot and HTML tabs) and that "Page availability" says the URL is available to Google. Then click **Request Indexing**, one request per URL (the quota is small; don't repeat).
5. **Sitemaps:** open Indexing → Sitemaps. If `sitemap.xml` is not listed, submit `sitemap.xml`. If it is, it will be re-read automatically; you can resubmit.
6. Wait 1–3 weeks. "Discovered, currently not indexed" often clears slowly on small new sites. Check Indexing → Pages and the URL Inspection "Last crawl" date.
7. If a URL still says "Crawled/Discovered – currently not indexed" after 3–4 weeks, the remaining levers are off-page: link to the pages from other sites or from your Google Business Profile, and share them (WhatsApp/Facebook). A custom domain would also help in the long run.
8. Check Settings → Crawl stats and Indexing → Pages → "Why pages aren't indexed" for new reasons (for example "Soft 404" or "Duplicate, Google chose different canonical") and tell me what appears.

---

# Final audit (2026-11-28)

**What this is:** an automated audit of the deployment folder, plus browser tests with JavaScript on and off. **Not done:** I have no access to your Search Console, I did not run a live Googlebot test, and I submitted nothing to Google. I could not reach the live site from my environment, so live HTTP codes and headers are unverified.

## A. Audit results (files)
| Check | Result |
|---|---|
| Canonical, all 4 pages | Correct and self-referencing: `/`, `/prayer-timings.html`, `/imam-muazzin.html`, `/location-contact.html` |
| `og:url` | Matches each canonical |
| Meta robots | `index, follow` everywhere. No `noindex` / `nofollow` anywhere in the files |
| Mixed `/` and `/index.html` | None left. 0 references to `index.html` in any page; every Home link is `/` |
| robots.txt | `Allow: /` plus the sitemap line. No blocking. Unchanged |
| sitemap.xml | Exactly the 4 canonical URLs, `lastmod` 2026-11-28 |
| Homepage links to all 3 pages | Yes: nav, hero buttons, quick tiles, About paragraph, section links, footer |
| Crawlable text (JS off) | prayer-timings ~1,070 words, imam-muazzin ~357, location-contact ~291 (before: 154 / 85 / 75) |
| Duplicate content | Subpage text reworded or new; copyright policy removed from HTML on the 3 subpages. A final duplicate sentence on the Imam page was reworded in this pass |
| JSON-LD | Valid JSON on all pages |
| JS errors | None |
| Titles / H1 | Unique title and description per page, one H1 each |
| Timings | Fajr 6:05 AM, Zuhr 1:30 PM, Asr 4:00 PM, Isha 7:00 PM. Maghrib = sunset Azan from your own chart + 4 min. No timing taken from another mosque |

## B. Every change
**All pages**
- Home links `index.html` → `/`.
- Added a small `<style id="seo-extra">` block for the new sections (existing CSS untouched).

**index.html (minimal)**
- Three contextual link lines added to the prayers, imam and location sections.
- JSON-LD: Mosque (`@id`, geo, description) + WebSite.
- JS fallback `DEFAULTS` corrected to 06:05 / 13:30 / 16:00 / 19:00 (Jumma unchanged at 13:45).

**prayer-timings.html**
- Static prayer cards, pre-rendered November table, "Jamaat Times at a Glance", "Maghrib Azan Times Through the Year", six Q&As.
- Fallback `DEFAULTS` corrected.
- Meta description shortened from 170 to 151 characters.
- Copyright policy moved into a `<template>`.
- JSON-LD: WebPage + BreadcrumbList + Mosque.

**imam-muazzin.html**
- Reworded cards and intro; added Roles, Jamaat times, About, contact and Q&As.
- Copyright policy moved into a `<template>`.
- JSON-LD: AboutPage + BreadcrumbList + 2 Person + Mosque.

**location-contact.html**
- Added address block, how to find, planning a visit, contact details and Q&As.
- `<noscript>` map link.
- Copyright policy moved into a `<template>`.
- JSON-LD: ContactPage + BreadcrumbList + full Mosque.

**sitemap.xml:** `lastmod` updated. **robots.txt, vercel.json, images:** unchanged (they were already correct).

## C. Remaining problems / open items
1. **Live status unverified:** HTTP codes, headers and the Google-selected canonical must be checked by you (steps below).
2. **Jumma time:** 1:45 PM is the old fallback. Not given as official. Please confirm it, or make sure the sheet has it.
3. **Homepage prayer cards** are still filled only by JavaScript. Left as is to protect the ranking page. Optional later improvement.
4. **Phone number:** the site has no phone/WhatsApp. Adding one in the marked comment on the Location page would help visitors, but only if it is the masjid's real number.
5. **Domain:** a `vercel.app` subdomain has little authority. A custom domain would help long term, but moving needs 301 redirects and a Search Console change of address. Not recommended right now.
6. **`readable-source/`** from your first zip is not updated.
7. **Vercel Web Analytics:** if not enabled in the Vercel project, `/_vercel/insights/script.js` returns 404 (cosmetic console error, not an SEO issue).
8. **Static times must be kept in step** when the timings change (see section 5 above).

## D. Deploy on Vercel
**Option 1: Git (if the project is linked to a repo)**
1. Unzip the folder and copy all files over the repo's site folder (replace everything).
2. `git add -A && git commit -m "SEO fixes" && git push`. Vercel deploys automatically.

**Option 2: Vercel dashboard / CLI**
1. In the project directory with these files, run `npx vercel --prod`. Framework preset "Other", no build command, output directory = the folder with `index.html`.
2. Or in the dashboard: Project → Deployments → drag in / redeploy with the new folder.

**After deployment**
1. Open the four URLs, `/sitemap.xml` and `/robots.txt` in a browser.
2. Run the curl check from section 2 (expect 200 on all).
3. Check the homepage visually, including the countdown and prayer cards.
4. Follow the Search Console checklist: inspect the homepage canonical, Test Live URL on the three pages, one indexing request each, confirm the sitemap shows 4 URLs, and monitor for 1–3 weeks.
