# Lee Brady Website — Maintenance Guide

## Quick Reference: Where to Edit Things

Every editable section in `index.html` is marked with a comment block like this:

```
<!-- ══════════════════
     SECTION NAME
     Edit: what to change
══════════════════ -->
```

Open the file, search (`Ctrl+F`) for the section name, and you'll land exactly where you need to be.

---

## Common Updates

### Update phone number
Search for `5715551234` — it appears in 3 places (nav, contact section, footer). Replace all 3.

### Update email address
Search for `lee@leebrady.com` — appears in 2 places (contact section, footer).

### Replace the hero background photo
In the `<style>` block, find `.hero-bg` and change the `url(...)` value:
```css
.hero-bg {
  background: ...,
    url('YOUR_NEW_IMAGE_URL') center/cover;
}
```

### Replace Lee's headshot (About section)
Find the `<img>` tag inside `class="about-image"` and change `src` and `alt`.

### Add/edit a listing card
Each listing is an `<a class="listing-card">` block. Duplicate one and change:
- `href` — link to the MLS listing
- `src` / `alt` on the `<img>`
- Tag class: `new`, `sold`, or remove the class for "Active"
- Price, specs, address, area

### Add/edit a testimonial
Each testimonial is a `<div class="testimonial-item">` block. Duplicate one, change the quote, name, location, and photo. Then add a matching `<button class="testimonial-dot">` in `.testimonial-nav`.

### Update neighborhood cards
Each card is an `<a class="neighborhood-card">` block. Edit: image, `h3` name, state/zip, description, price range, active count.

### Update stats (About section)
Find `class="about-stats"` and change the 3 `stat-num` / `stat-label` pairs.

---

## RealScout Integration

Two placeholder slots are ready for RealScout embeds:

1. **Listings widget** — search for `REALSCOUT SLOT 1` in `index.html`
2. **Home Value widget** — search for `REALSCOUT SLOT 2` in `index.html`

Replace the entire `<div class="realscout-slot">...</div>` with the embed code RealScout provides.

---

## Contact Form Setup

The form currently points to Formspree. To activate it:

1. Go to [formspree.io](https://formspree.io) and create a free account
2. Create a new form — you'll get an ID like `xabcdefg`
3. In `index.html`, find `action="https://formspree.io/f/YOUR_FORM_ID"` and replace `YOUR_FORM_ID`

**Alternative:** Change the `action` to your Make.com webhook URL to pipe leads directly into your CRM.

---

## SEO Checklist (do once before going live)

- [ ] Update `<title>` tag if needed
- [ ] Update `<meta name="description">` (keep under 160 characters)
- [ ] Update `<link rel="canonical">` with your real domain
- [ ] Update all `og:url` and `og:image` meta tags with real URLs
- [ ] Update phone, email, address in the JSON-LD block at the top of the `<head>`
- [ ] Update social profile URLs in JSON-LD (`sameAs`) and in the footer
- [ ] Replace all placeholder photos with real photos (better for image SEO)
- [ ] Add a real `favicon` — create `/website/favicon.ico` and add `<link rel="icon" href="/favicon.ico">` in `<head>`

---

## Colors & Fonts (Design Tokens)

Edit these CSS variables in the `:root` block to change colors site-wide:

| Variable | Current Value | Use |
|----------|--------------|-----|
| `--ink` | `#0e0d0b` | Main dark color |
| `--paper` | `#f5f1ea` | Main light/cream background |
| `--paper-warm` | `#ebe5d9` | Warmer sections |
| `--gold` | `#b8924a` | Primary accent |
| `--gold-light` | `#d4b072` | Lighter accent / hover |
| `--muted` | `#6b665d` | Secondary text |

Fonts are loaded from Google Fonts:
- **Display/headlines:** Fraunces (serif, italic)
- **Body:** Inter Tight (sans-serif)

---

## Deployment Options

### Option A: GitHub Pages (free, no server needed)
1. In your GitHub repo settings → Pages
2. Set source to the `website/` folder on your branch
3. Your site will be live at `https://noryey3.github.io/noryey3/`
4. For a custom domain: add a `CNAME` file to `/website/` with your domain name

### Option B: Netlify (free tier, custom domain easy)
1. Connect your GitHub repo to Netlify
2. Set "Publish directory" to `website`
3. Add your custom domain in Netlify settings

### Option C: WordPress + Elementor (if migrating later)
The `index.html` serves as the pixel-perfect reference design.
Use it to replicate sections in Elementor — the color tokens and
font names at the top of the CSS are the source of truth.

---

## File Structure

```
website/
├── index.html        ← The entire site (one file, self-contained)
└── MAINTENANCE.md    ← This guide
```

All CSS and JavaScript are embedded in `index.html` — no build step, no npm, no dependencies. Just edit the file and deploy.
