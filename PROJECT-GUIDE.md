# CNR Car Rental — Phase 1 SEO project guide

This is a step-by-step handoff for completing the four-item SEO scope on
**cnrcarrental.com** (Joey Li, June 2026). Quote agreed at $340 fixed bid,
8 hours of work, 5–7 business days. 50% on start, 50% on completion.

The visual deliverable (what Joey has already seen and approved) lives at:

- HTML file in this repo: `cnr-phase1-mockup.html`
- Live preview: https://camster91.github.io/cnr-mockup/cnr-phase1-mockup.html

**Read both before you start.** The mockup shows the final state. This guide
shows how to get there without breaking anything.

---

## 0. What's in scope

| # | Fix | Visible to visitors? | Hours | Cost |
|---|-----|----------------------|------:|-----:|
| 1 | Reservations page rewrite (`/reservations/`) | yes | 3.5 | $140 |
| 2 | Main menu "Business Collaboration" 404 fix | yes | 0.5 | $20 |
| 3 | LocalBusiness JSON-LD schema on homepage | no (Google only) | 2.5 | $100 |
| 4 | Sitemap redirect fix (`/sitemap.xml` 301→404) | no (Google only) | 1.5 | $60 |
| 5 | Validation + Search Console submission (all fixes) | no | 0.5 | $20 |
| | **Total** | | **8.0** | **$340** |

The order below is the order to do the work in. Items 1 and 2 are visible and
Joey will check them first, so get them right and get them early. Items 3 and 4
are background code — Joey won't see them, but Google will, and he's paying
for both.

---

## 1. Before you touch anything

### 1.1 Get access

Ask Joey for all three. If he only sends one, do not start — the project needs
all three to ship.

- [ ] **WordPress admin login** (or cPanel + FTP if WP-admin is locked down).
      Role needed: administrator. Don't proceed with editor-level access —
      you'll need to install schema and edit the menu.
- [ ] **Google Search Console access.** Read + submit is enough. Property
      should be the URL-prefix `https://cnrcarrental.com/`, not the domain
      property. If it's not set up, have Joey verify first via the
      Search Console verification page.
- [ ] **Business info confirmation.** You need this to write the schema and
      the new Reservations page copy correctly. Ask for:

  | Field | Value (placeholder — confirm with Joey) |
  |-------|-----------------------------------------|
  | Official business name | CNR Car Rental |
  | Street address | 7900 Westminster Hwy, Unit 111, Richmond, BC V6X 1A5 |
  | Phone | +1-604-370-1779 |
  | Email | service@cnrauto.ca |
  | Counter hours | Mon–Sun 9:00 AM – 5:00 PM |
  | Self pick-up | Available 24/7 with confirmed booking |
  | Google Business Profile URL | **must get from Joey — required for `sameAs`** |
  | Geo coordinates | 49.1666, -123.1336 (Richmond city center — confirm) |

### 1.2 Take a backup

Do this **before** any change, even before opening the editor. WordPress
backups are not automatic unless something is set up.

- [ ] **Full site backup.** Use the host's backup tool (most Hostinger /
      shared hosts have a one-click backup in cPanel) **or** a plugin like
      UpdraftPlus. Save the zip somewhere outside the server.
- [ ] **Database backup.** At minimum `wp_options`, `wp_posts`, `wp_postmeta`,
      `wp_rank_math*` tables. Use phpMyAdmin → Export → Custom → select
      these tables → SQL → "Add DROP TABLE" = yes.
- [ ] **Screenshot the current state** of:
      - Homepage (`cnrcarrental.com/`)
      - Reservations page (`cnrcarrental.com/reservations/`)
      - The XML sitemap URL (`cnrcarrental.com/sitemap.xml`)
      - Google Search Console → Pages report (if accessible)

Store the screenshots in a folder named `before/` inside your working
directory. You'll diff against these at the end.

### 1.3 Inventory the current state

Run these three checks and record the results. They are your "before" baseline
and they catch the exact bugs the client is paying you to fix.

- [ ] **Sitemap check.**
      ```bash
      curl -sI -L https://cnrcarrental.com/sitemap.xml
      ```
      Expected today: `301 → /sitemap_index.xml → 404`. If you see anything
      else, stop and tell Cam before doing anything else — the fix below
      assumes Rank Math is the SEO plugin and that exact redirect path.
- [ ] **404 check.** In Search Console → Pages → "Why pages aren't indexed"
      filter, confirm `business-collaboration` is in the 404 list. If it
      isn't, check why before changing anything.
- [ ] **Reservations page word count.** Open the page in the WP editor (or
      view source). Count human-readable words excluding the booking form /
      shortcode. Target is 600–800. If it's already in range, tell Cam
      before continuing — the page may already be done.

### 1.4 Read the page in the editor

The site is on the **grandcarrental** theme with **Rank Math SEO** and
**WPML** for the language switcher. Don't touch theme files — everything
goes through the WP block editor (Gutenberg) or Rank Math's settings.

The reservations page is built with a booking widget shortcode at the top
followed by Gutenberg blocks below. Confirm by clicking into the page and
looking at the block structure. If it's not Gutenberg (e.g. Elementor, WPBakery,
classic editor), tell Cam before continuing — the rewrite approach changes.

---

## 2. Item 1 — Reservations page rewrite (3.5 h, $140)

This is the biggest piece of the project. The booking widget stays at the
top, exactly where it is. You're adding 600–800 words of new content below
it.

### 2.1 Get the draft copy

The full draft copy (788 words) is in the mockup at
`cnr-phase1-mockup.html` in this repo. Open it in a browser, scroll to
section "1 · Reservations page rewrite", and you'll see the new content
blocks under the booking widget preview.

The draft has these sections, in this order:
1. Welcome to CNR Car Rental (intro paragraph)
2. Our fleet at a glance (4-card grid)
3. Pickup location and hours (info card)
4. What's included in every rental
5. Who we rent to
6. Long-term and business rentals
7. Frequently asked questions (6 Q&A)

### 2.2 Verify the assumptions in the draft

The draft copy was written from what was visible on the live site plus a
few reasonable guesses. **Joey must sign off on these before you publish.**
Send him a Google Doc with the draft and ask for the corrections. The
assumptions to flag:

- [ ] **Deductible amount** — draft says $1,000. Confirm.
- [ ] **After-hours self pick-up fee** — draft says $25, discounted to $10
      if you book a week or more in advance. Confirm or remove the price.
- [ ] **Cancellation window** — draft says 48 hours. Confirm.
- [ ] **Taxi cost YVR → shop** — draft says $15–20. Confirm or remove.
- [ ] **Canada Line instructions** — draft mentions Aberdeen Station.
      Confirm or use Bridgeport Station.
- [ ] **Monthly discount** — draft says 40–50% below daily rate. Confirm.
- [ ] **Fleet classes listed** — Economy, SUV, EV, Minivan with example
      makes/models. Confirm the list is current.

Wait for Joey's sign-off before continuing. Don't publish assumptions.

### 2.3 Build the page in Gutenberg

Open the existing `/reservations/` page in the WordPress editor. Plan: keep
the booking widget block at the top, replace everything below it with the
new content.

Order of operations:

1. [ ] **Screenshot the current block layout** before changing anything.
2. [ ] **Note the booking widget block name and shortcode.** You'll need
      to re-add it in the same position. Common names: "Booking Form",
      "Reservation Form", or it may be a shortcode block. Don't delete it.
3. [ ] **Delete all blocks below the booking widget.** Select all blocks
      below the form, delete.
4. [ ] **Add a new Heading block** — "Welcome to CNR Car Rental" (H2).
5. [ ] **Add the intro paragraph** (2 paragraphs from the mockup, draft
      version you got from Joey).
6. [ ] **Add "Our fleet at a glance"** (H3) followed by 4 paragraph
      blocks (one per class) or a Columns block with 4 columns. Use
      whatever the existing fleet pages on the site use for visual
      consistency — check the homepage or `/fleet/` if it exists.
7. [ ] **Add "Pickup location and hours"** (H3) followed by a 2x2 info
      block. A Columns block with 2 columns × 2 rows works fine. Or use
      a Table block.
8. [ ] **Add the three middle sections** (What's included / Who we rent
      to / Long-term and business rentals) as H3 + paragraph blocks.
9. [ ] **Add "Frequently asked questions"** (H3) followed by 6 Q&A pairs.
      For SEO value, wrap each Q&A in a Details block (built-in
      Gutenberg block) so they're collapsible. The `<details>` HTML is
      already what Google reads for FAQ rich results when paired with
      FAQPage schema — see step 2.4.
10. [ ] **Preview the page** (Preview button, not Update). Compare against
       the mockup section-by-section. The booking widget must still be
       at the top and still work.
11. [ ] **Check the page on mobile** using the responsive preview in the
       editor. The 4-column fleet grid should collapse to 1 column on
       phone.
12. [ ] **Update the page** (publish).

### 2.4 Add FAQPage schema (bonus, optional)

If you used Gutenberg Details blocks for the FAQ in step 2.3.9, you can
add FAQPage schema to make the FAQs eligible for rich results. Use
**Rank Math SEO → Schema** for the page (the page-level tab), or add
manually via a child theme's `functions.php`:

```php
add_filter('rank_math/snippet/rich_snippet', function ($data, $atts) {
    if (is_page('reservations')) {
        // Build FAQPage schema from the page's <details> blocks.
        // Pull from the post content or hardcode the 6 Q&A pairs.
    }
    return $data;
});
```

Only do this if you can verify it doesn't conflict with Rank Math's
existing schema on the page. If in doubt, skip it — the schema is a
nice-to-have, the rewritten content is what matters for ranking.

### 2.5 Verify the page

- [ ] **Live URL works.** Open `cnrcarrental.com/reservations/` in an
      incognito window. The booking widget is at the top, the new
      content is below.
- [ ] **Word count.** View source, count human-readable text. Target
      600–800. Anything below 600 means you deleted draft content;
      anything above 900 means you accidentally added the headers as
      body text.
- [ ] **No console errors.** Open DevTools → Console. No red errors.
- [ ] **Mobile render.** Check on phone or Chrome DevTools device
      emulator (iPhone 14 viewport). No horizontal scroll, fleet grid
      collapses correctly.
- [ ] **Existing menu link works.** Click "Reservations" in the main
      menu. Goes to the rewritten page, not a 404.

Report back to Cam with the live URL, the word count, and the mobile
screenshot.

---

## 3. Item 2 — Business Collaboration 404 fix (0.5 h, $20)

The main menu has a "Business Collaboration" link that points to a
non-existent page. Three options, all listed in the mockup. Get Joey's
choice before implementing.

### 3.1 Get Joey's call

Send him a short message:

> For the Business Collaboration menu link, three options:
>
> A. Build a real page (recommended). Lift content from the Monthly
>    Rentals page about fleet partnerships and corporate accounts.
> B. Repoint. Make the link go to an existing page (Monthly Rentals or
>    FAQs).
> C. Remove. Delete the menu item entirely.
>
> Which do you want?

Wait for the answer. Don't guess.

### 3.2 Implement

**If A — build the page:**

1. [ ] Create a new page: **Pages → Add New**. Title: "Business
       Collaboration". Use the draft content from the Monthly Rentals
       page as a starting point.
2. [ ] In the menu editor (**Appearance → Menus**), find the "Business
       Collaboration" item. Change its target from the broken URL to
       the new page.
3. [ ] Save the menu. Test the link.

**If B — repoint:**

1. [ ] In **Appearance → Menus**, find the "Business Collaboration"
       item.
2. [ ] Open it, change the URL to the chosen existing page (or
       click "Pages" on the left and select the page).
3. [ ] Save. Test.

**If C — remove:**

1. [ ] In **Appearance → Menus**, click the arrow on the "Business
       Collaboration" menu item to expand it.
2. [ ] Click "Remove". Save the menu.

### 3.3 Verify

- [ ] Click the menu link from a fresh incognito window. Lands on a
      real page, no 404.
- [ ] The menu still shows all other items in the correct order.
- [ ] Mobile menu also shows the same behavior. (The grandcarrental
      theme has a separate mobile menu — check it.)

Report which option you implemented and the live URL of the target
page.

---

## 4. Item 3 — LocalBusiness JSON-LD schema (2.5 h, $100)

The site has no structured data. You're adding a JSON-LD block to the
homepage `<head>` describing the business. Customers won't see any
change. Google will.

### 4.1 Get the schema values

The exact block to add is in the mockup, section 3. It's also pasted
below. **Verify every value with Joey before publishing** — Google
treats structured data as a factual claim about the business and will
penalize incorrect data.

Placeholder values (currently in the mockup) that **must** be confirmed:

- [ ] `geo.latitude` and `geo.longitude` — placeholder is the Richmond
      city center. Get exact coordinates from Google Maps (right-click
      the pin → "What's here?" → copy the lat/lng).
- [ ] `sameAs` — currently a placeholder `https://www.google.com/maps`
      URL. **Must** be replaced with the live Google Business Profile
      URL Joey gives you.
- [ ] `priceRange` — currently `"$$"`. Confirm with Joey.
- [ ] `areaServed` — Vancouver, Richmond, Burnaby, Surrey. Confirm or
      add/remove based on where Joey actually delivers.

### 4.2 The exact JSON-LD block to add

Strip the highlighted `// TODO` comment before publishing. Validated
JSON only.

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "CarRental",
  "@id": "https://cnrcarrental.com/#business",
  "name": "CNR Car Rental",
  "alternateName": "CNR Auto Rental",
  "url": "https://cnrcarrental.com",
  "telephone": "+1-604-370-1779",
  "email": "service@cnrauto.ca",
  "priceRange": "$$",
  "image": "https://cnrcarrental.com/wp-content/uploads/2024/07/cnr_logo_square.png",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "7900 Westminster Hwy #111",
    "addressLocality": "Richmond",
    "addressRegion": "BC",
    "postalCode": "V6X 1A5",
    "addressCountry": "CA"
  },
  "geo": {
    "@type": "GeoCoordinates",
    "latitude": 49.1666,
    "longitude": -123.1336
  },
  "openingHoursSpecification": [
    {
      "@type": "OpeningHoursSpecification",
      "dayOfWeek": ["Monday","Tuesday","Wednesday","Thursday","Friday","Saturday","Sunday"],
      "opens": "09:00",
      "closes": "17:00"
    }
  ],
  "areaServed": [
    { "@type": "City", "name": "Vancouver" },
    { "@type": "City", "name": "Richmond" },
    { "@type": "City", "name": "Burnaby" },
    { "@type": "City", "name": "Surrey" }
  ],
  "sameAs": [
    "https://www.google.com/maps/maps?cid=REPLACE_WITH_REAL_GBP_URL"
  ]
}
</script>
```

### 4.3 Where to add it

Two options, pick one:

**Option A — Rank Math custom schema (preferred).** Rank Math has a
custom schema builder, but it's per-post, not site-wide. For the
homepage:

1. [ ] Edit the homepage (the page set as the static front page in
       **Settings → Reading**).
2. [ ] In the Rank Math SEO sidebar, click the Schema tab.
3. [ ] Click "Schema Generator" → "Custom Schema".
4. [ ] Paste the JSON above (everything inside the `<script>` tag,
       not the tag itself). Hit "Save".

**Option B — Child theme `functions.php`.** If Rank Math's per-page
schema is too fiddly, drop a function in the child theme that prints
the JSON-LD on the homepage. Pattern:

```php
add_action('wp_head', function () {
    if (is_front_page()) {
        // print the <script type="application/ld+json">...</script>
        // block here, escaped properly
    }
});
```

Use Option A unless you can verify the child theme is in active use.
Don't edit the parent grandcarrental theme — updates will wipe your
change.

### 4.4 Verify

- [ ] **View source** on the homepage. Search for `application/ld+json`.
      The block is there.
- [ ] **Validate with Google's Rich Results test.**
      https://search.google.com/test/rich-results
      Paste the homepage URL. Expected result: "Eligible for car
      rental rich result" in green. If it shows warnings, fix them
      before reporting done.
- [ ] **Validate JSON syntax.** Copy the block, paste into
      https://jsonlint.com. Must parse without error.
- [ ] **Only the homepage.** The schema should appear on the homepage
      only. If it shows on every page, the `is_front_page()` check is
      missing — fix before publishing.

Report the Rich Results test screenshot (green) to Cam as proof.

---

## 5. Item 4 — Sitemap redirect fix (1.5 h, $60)

`/sitemap.xml` returns a 301 redirect to `/sitemap_index.xml` which
returns a 404. The redirect is from Rank Math's SEO module — Rank Math
ships its sitemap at `/sitemap_index.xml` and the `/sitemap.xml` alias
is a server-level redirect that's currently broken.

### 5.1 Diagnose first

Don't change anything until you've confirmed the exact failure mode.
Run these two checks and record the results:

```bash
# Check 1: what does the sitemap URL return?
curl -sI -L https://cnrcarrental.com/sitemap.xml
# Expected today: 301 → /sitemap_index.xml → 404

# Check 2: does the actual Rank Math sitemap exist?
curl -sI -L https://cnrcarrental.com/sitemap_index.xml
# Expected today: 404
```

If the failure mode matches, proceed. If not — for example if
`sitemap_index.xml` returns 200 but `/sitemap.xml` doesn't redirect to
it — stop and tell Cam. Different fix.

### 5.2 Try the Rank Math fix first

Rank Math's sitemap can be turned off and on, which resets the
serving path.

1. [ ] In WP-admin, go to **Rank Math SEO → Sitemap Settings**.
2. [ ] Toggle "Enable Sitemap" off. Save. Wait 5 seconds.
3. [ ] Toggle it back on. Save.
4. [ ] Run the same curl check from 5.1.

This often fixes the redirect by forcing Rank Math to regenerate its
sitemap and the alias. If it works, you're done. Move to section 5.4.

### 5.3 If the toggle didn't fix it — add a server-level rewrite

Add a redirect in the host's `.htaccess` (Apache) or Nginx config.
For Hostinger (Apache/LiteSpeed), the rule goes in the WordPress
`.htaccess` at the site root, above the `# BEGIN WordPress` block:

```apache
# BEGIN CNR Sitemap Fix
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteRule ^sitemap\.xml$ /sitemap_index.xml [L]
</IfModule>
# END CNR Sitemap Fix
```

After adding, re-run the curl check. Expected:

```
HTTP/2 200
content-type: application/xml; charset=UTF-8
```

If the `.htaccess` is owned by a security plugin (Wordfence,
Sucuri), you may need to whitelist the edit or do it via the host
file manager instead of the WP plugin.

### 5.4 Submit to Google

- [ ] In Google Search Console → Sitemaps → Add a new sitemap.
- [ ] Enter `sitemap.xml`. Submit.
- [ ] Expected result: "Success" with a non-zero "Discovered pages"
      count. If it's 0, the sitemap is empty — check the Rank Math
      settings to make sure posts and pages are included.

### 5.5 Verify

- [ ] `curl https://cnrcarrental.com/sitemap.xml` returns 200 with
      `content-type: application/xml`.
- [ ] The sitemap contains a valid XML body listing the site's pages
      (reservations, monthly rentals, FAQs, etc.).
- [ ] Search Console shows the sitemap as successful and starting to
      be crawled (this happens over the next 24–48 hours — you don't
      need to wait, just confirm the submission was accepted).

---

## 6. Item 5 — Final validation and reporting (0.5 h, $20)

This is the part where you prove everything works. Don't skip it.

### 6.1 Pre-handoff checklist

Run through this list. Every box must be checkable before you report
done to Cam.

**Visible changes (the customer-facing items)**

- [ ] `/reservations/` page renders with booking widget at top, new
      content below. 600–800 words. Mobile-responsive.
- [ ] Main menu "Business Collaboration" link works — either goes to
      a real page (option A or B) or doesn't exist (option C). Either
      way, no 404.
- [ ] No console errors on either page.
- [ ] Existing pages (homepage, monthly rentals, FAQs, contact) still
      load correctly — you didn't break anything.

**Background changes (Google-facing items)**

- [ ] `curl https://cnrcarrental.com/sitemap.xml` returns 200 with
      `application/xml` content type.
- [ ] Homepage source contains the LocalBusiness JSON-LD block.
- [ ] Google's Rich Results test shows the homepage as "Eligible for
      car rental rich result" — green, no warnings.
- [ ] Sitemap submitted in Search Console. Discovered pages count >
      0.

**Access and cleanup**

- [ ] No test posts, drafts, or scratch pages left in WP-admin.
- [ ] No debug code, console.logs, or `var_dump` left in the theme.
- [ ] All your edits are in a way that survives a theme update
      (i.e. in a child theme, not in the parent grandcarrental
      theme).

### 6.2 Take the after-screenshots

Screenshot the same URLs you screenshotted in step 1.2. Save in an
`after/` folder. You'll need these for the report.

### 6.3 Write the report

Send Cam a short report with:

1. **What changed.** One line per item, what you did.
2. **Which option for item 2** (A, B, or C) and why.
3. **What the customer sees now.** Link to the rewritten Reservations
   page and the Business Collaboration page.
4. **Rich Results test screenshot.** Green "Eligible" line.
5. **Sitemap status.** "Submitted, 12 discovered pages" or whatever
   Search Console says.
6. **Anything you couldn't do.** Be honest. If you couldn't reach
   Google Search Console, or the host blocked your `.htaccess` edit,
   say so.
7. **The hours you actually spent.** Quote is fixed bid at 8 hours,
   but be transparent if you went over.

Don't write a long report. Grade-12 English, sentences short, one
idea per line. Cam will forward it to Joey.

### 6.4 Hand off

Email or Slack Cam the report and the screenshots folder. Cam will
review, send the final report to Joey, and request the second 50% of
the invoice.

---

## 7. Common pitfalls

These are the things most likely to bite you on this project. Read
before starting.

- **Don't edit the parent grandcarrental theme.** All your changes
  will be wiped on the next theme update. If you need to add PHP, use
  a child theme. If there isn't one, create one first.

- **The booking widget is a shortcode, not a Gutenberg block.** Don't
  try to drag a booking form into the page — find the existing
  shortcode and reuse it. The Reservations page already has it.

- **WPML is installed.** If you write the page in English, the
  Chinese version won't auto-update. Tell Cam whether the client
  wants the Chinese version updated too — it's out of scope for this
  quote but a 30-minute add-on.

- **Rank Math and Yoast conflict.** If the site has both installed
  (it shouldn't, but check), schema from both will fire. Disable the
  one that isn't active. The site uses Rank Math per item 4's
  failure mode.

- **The host is on LiteSpeed (not Apache).** Some Apache `.htaccess`
  directives don't work on LiteSpeed. The sitemap rewrite rule in
  5.3 works on both, but if you need other rewrites, test on a
  staging URL first.

- **The site uses Cloudflare.** If Cloudflare is in front of
  cnrcarrental.com (check by running `dig +short cnrcarrental.com` —
  look for `*.cloudflare.com` in the answer), there may be cached
  versions of the sitemap 404. After fixing, hit
  `https://cnrcarrental.com/sitemap.xml?cb=N` (any random query
  string) to bypass cache, or purge the cache in Cloudflare.

- **The site uses autoptimize** (visible in the page source). CSS
  and JS are concatenated and cached. After making a change, you may
  need to purge autoptimize's cache (**Settings → Autoptimize →
  "Delete cache"**).

- **Joey's emails go to `service@cnrauto.ca`, not his `@cnrcarrental.com`.**
  He has two separate businesses. Don't get confused by the domain
  difference — this is the same person, same shop, just the email
  is on a related domain. If you need to reach him by email, ask Cam
  for the right address; don't try to guess from the site.

- **The address has a full-width hash (`＃`)** in the footer text
  ("7900 Westminster Hwy＃111") — not a regular `#`. The mockup uses
  the correct ASCII `#`. If you're copying the address from the
  live site footer, fix the character.

- **The hours say "Mon~Sun 10:00 - 17:00" in the top bar but "Mon~Sun
  09:00 - 17:00" in the footer.** Two different values on the same
  site. Use 9:00 (the footer and schema) unless Joey says otherwise.
  Tell him about the inconsistency — he may want to fix the top
  bar.

---

## 8. Reference

### Tech stack

- **CMS:** WordPress (admin bar visible on screenshots)
- **Theme:** grandcarrental (parent)
- **SEO plugin:** Rank Math
- **Cache:** LiteSpeed / autoptimize / possibly Cloudflare
- **Forms:** Contact Form 7 (not used in this scope)
- **Booking:** Custom shortcode (do not touch)
- **i18n:** WPML (English + Chinese)
- **Server:** Likely Hostinger shared or VPS (Apache with LiteSpeed)

### Test endpoints

```bash
# Sitemap (item 4)
curl -sI -L https://cnrcarrental.com/sitemap.xml

# Reservations (item 1)
curl -sI https://cnrcarrental.com/reservations/

# Business Collaboration (item 2)
curl -sI https://cnrcarrental.com/business-collaboration/

# Schema validation (item 3)
# Use: https://search.google.com/test/rich-results
```

### Useful URLs

- Google Rich Results test: https://search.google.com/test/rich-results
- Google Search Console: https://search.google.com/search-console
- JSON validator: https://jsonlint.com
- Rank Math docs: https://rankmath.com/kb/
- Schema.org CarRental: https://schema.org/CarRental

### Quote + scope reference

- Mockup: https://camster91.github.io/cnr-mockup/cnr-phase1-mockup.html
- This guide + the mockup live in: https://github.com/camster91/cnr-mockup

---

Prepared 19 June 2026 for handover to the assigned WP freelancer. Send
questions to Cam, not Joey — Cam is the point of contact and the
client-facing lead.
