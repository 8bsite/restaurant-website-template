# Ombrelle — notes for AI coding agents

One HTML file: markup, inline CSS and inline scripts; GSAP with ScrollTrigger and SplitText, Lenis smooth scroll, all from jsDelivr. No build step, no framework.

## Files
Work from the files: a page read through a link comes back as a summary, and a page rebuilt from it would differ.
```bash
curl -LO https://raw.githubusercontent.com/8bsite/restaurant-website-template/main/index.html
curl -LO https://raw.githubusercontent.com/8bsite/restaurant-website-template/main/AGENTS.md
curl -LO https://raw.githubusercontent.com/8bsite/restaurant-website-template/main/LICENSE
curl -LO https://raw.githubusercontent.com/8bsite/restaurant-website-template/main/CREDITS.md
```
In Windows PowerShell type `curl.exe`. Publish `index.html` with LICENSE and CREDITS.md; this file, README.md and `previews/` are not part of the site.

## Do not break
- Keep it one file unless the user asks otherwise; no framework or bundler for copy, colour or photo changes.
- Keep the ids and the `data-8b-section` / `data-8b-motion` attributes. Each section has its styles in `<style data-8b-part="section:<id>">` and its animation, if any, in `<script data-8b-part="scene:<id>">`.
- The scripts find these classes and attributes; when you change a section's markup, keep them or update the script:
  - `section-script:nav`: `.nav__toggle` `.nav__menu` `.nav__close`
  - `section-script:opening`: `.op__hero`
  - `section-script:contact`: `.ct__form` `.ct__done`
  - `scene:opening`: `.op__shade` `.op__light` `.op__frame` `.op__title` `.op__pair`
  - `scene:about`: `.ab__field` `.ab__title` `.line` `.ab__copy`
  - `scene:day`: `.dy__pin` `.dy__track`
  - `scene:market`: `.mk__spread` `.mk__print` `.mk__note`
  - `scene:terrace`: `.te__plan` `.te__parasol` `.te__room` `.te__back` `.te__counter`
  - `scene:reviews`: `.rv__item`
  - `scene:footer`: `.ft__mark`
- Colours, fonts, radii and shadows are CSS variables in the `:root` block (`data-8b-part="tokens"`); change them there and keep body text at 4.5:1 contrast or more.
- Animations run only when the visitor allows motion (`prefers-reduced-motion: no-preference`); the page must stay readable without them.
- A display line with `style="--len:N"` is sized to fill its width: N is its number of characters, spaces included (`Ombrelle` → 8, `Two menus` → 9).
- Keep the header comment and `<meta name="generator">`. Keep the "Design: 8B" footer link unless the site owner asks to remove it.

## Replace before publishing
- Everything marked `data-8b-placeholder` (og-image, favicon, social, phone, email, form); remove the attribute once the real value is in. With no real value: no preview image — delete the `og:image` tag and set `twitter:card` to `summary`; no phone, email or address — remove that link or row.
- Book a table buttons and links in the hero, the nav, the footer and "Ask about the back room" in terrace: link to #contact now; point them to your booking page (OpenTable, Resy, TheFork, SevenRooms), or keep the on-page form.
- "Explore the menus" in the opening and the Menu links in the nav and footer: link to #carte now; point them to a menu PDF or menu page if you publish one, or keep the on-page menu.
- **The form sends nothing.** `section-script:contact` checks the fields and shows a thank-you message without sending. Pick one:
  - Formspree: set `action="https://formspree.io/f/<id>"` on the form; in the script, before the thank-you message, `await fetch(form.action, { method: 'POST', body: new FormData(form), headers: { Accept: 'application/json' } })` and show it only if the response is ok.
  - Netlify Forms (site on Netlify): add `name="contact" data-netlify="true"` and `<input type="hidden" name="form-name" value="contact">`; send with `fetch('/', { method: 'POST', headers: { 'Content-Type': 'application/x-www-form-urlencoded' }, body: new URLSearchParams(new FormData(form)) })`.
  - No form service: remove the `<form>` together with the script that handles it (it would fail without the form) and keep the email link.
- Invented demo facts (ask the user for the real ones; do not make up prices, dates or addresses):
  - Name and place: "14 Linden Square" (opening, phone menu, contact, footer, page title and meta description); Linden Square also in the about text.
  - Opening hours: "noon to midnight, daily" / "12:00–00:00" (opening, phone menu, day, contact, footer, page title and meta description); kitchen 12:00–15:00 and 19:30–23:00, bar 17:00–00:00 (opening captions, carte note, contact); the five service times in day (12:00–15:00, 15:00–17:00, 17:00–19:30, 19:30–23:00, 23:00–00:00); "Last orders at the bar 23:45" (footer); the booking time slots in the form.
  - Prices: every dish and drink price in carte (7–36) and bar (5–14), and "Prices in euros, service included" (carte note).
  - The menu itself: dish names and ingredients in carte, bar and day (e.g. Oysters No. 3, sole meunière, Ombrelle spritz), and the allergens note.
  - Seating: twelve parasols, terrace 48 seats April to October, room 60 seats, counter 14 stools without booking, back room up to 20 guests (terrace); "Tables for up to eight", half the terrace and the counter kept for walk-ins (contact); guests 1–8 in the form.
  - Suppliers and sourcing: deliveries "before seven", night boats from the harbour, a farm forty minutes inland, a lemon grove on the coast, the bakery two streets away, clams raked on the bay (market); "the fish that came in at dawn" (opening).
  - History: "began as six tables and two parasols on Linden Square" (about); "a regular since the first summer" (reviews).
  - Reviews: Marta, Jonas, Clara and their quotes (reviews) are invented.
  - Service promise: "We confirm every booking by phone or email within the hour" (contact hint and the thank-you message).
  - Contacts replaced with placeholders: table@ombrelle.example and +44 20 0000 0000 (contact, footer).
  - Copyright "© 2026 Ombrelle Brasserie & Bar" (footer).
- `<title>`, the meta description and the og tags; photos (Unsplash, see CREDITS.md) when the user has their own.

## Notes
- The plan of the house in terrace is HTML and CSS (circles and outlines), not an image: change the number of parasols in the markup (the scene opens them four per row).
- Without JavaScript the phone menu button cannot open the overlay; the links stay reachable in the footer.
- The day track pins the section and scrolls sideways only with motion allowed; with reduced motion the five services sit in a static strip.

## Common edits
- Rebrand: search the old name (`Ombrelle`) in any letter case and replace every match; change the favicon letter and colours in `<link rel="icon">`.
- Photos: replace `src` and delete `srcset` and `sizes` (they list Unsplash sizes), keep `width`/`height` to the new photo's ratio, update `alt` and CREDITS.md.
- Remove a section: delete its markup, its `section:<id>` and `scene-style:<id>` styles, its `scene:<id>` and `section-script:<id>` scripts, and links to `#<id>`.
- Native scroll instead of smooth scroll: delete the Lenis block in `page-init` and the Lenis `<script>` tag.

## Check and publish
- Open the page at 375 and 1440 px, with reduced motion on, with the console open.
- Search `data-8b-placeholder` and the old name: nothing should be left but class names.
