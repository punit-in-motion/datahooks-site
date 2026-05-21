# Datahooks Website — CLAUDE.md

## Project Overview

The **datahooks-net/website** repo is the marketing website for **Datahooks** (datahooks.net), a data agency that helps AI-driven companies align their teams, clean their data, and build reliable dashboards and automations.

**Live site**: https://datahooks.net  
**Hosting**: GitHub Pages with custom domain (`CNAME` → `datahooks.net`)  
**Tech stack**: Pure static HTML/CSS/JS — no build system, no framework, no npm

---

## Repository Structure

```
/
├── index.html              # Homepage
├── styles.css              # Single global stylesheet (all pages share this)
├── sitemap.xml             # SEO sitemap
├── robots.txt              # Search engine directives
├── favicon.ico             # Root-level favicon for browser default discovery
├── CNAME                   # GitHub Pages custom domain (datahooks.net)
├── assets/                 # Images, logos, icons
│   ├── logo-d-white.png    # Logo mark (white) — used on dark/navy backgrounds
│   ├── logo-d-navy.png     # Logo mark (navy) — used on light backgrounds
│   ├── logo-d-color.png    # Logo mark (coral/color variant)
│   ├── logo-square-coral.png  # 512px square logo on coral bg (also used as favicon-512)
│   ├── logo-square-navy.png   # Square logo on navy bg
│   ├── favicon-16.png
│   ├── favicon-32.png
│   ├── favicon-512.png
│   └── apple-touch-icon.png
├── about/index.html        # /about/ — Company about page
├── blog/index.html         # /blog/ — Blog listing page
├── contact/index.html      # /contact/ — Contact/lead capture form
├── solutions/index.html    # /solutions/ — What We Do / services page
├── whitepaper/index.html   # /whitepaper/ — Interactive whitepaper with revenue leakage calculator
├── security/index.html     # /security/ — Security information page
├── privacy/index.html      # /privacy/ — Privacy policy
└── terms/index.html        # /terms/ — Terms of service
```

Each page lives in its own directory as `index.html`, enabling clean URLs (e.g., `/contact/` rather than `/contact.html`).

---

## Design System

All styles live in the single root-level `styles.css`. There is no preprocessor, no CSS modules, and no component library.

### Brand Colors (CSS Variables)

```css
--navy: #1a1a2e          /* Primary dark — text, navbars, footers */
--navy-light: #2d2d44    /* Slightly lighter navy */
--accent: #d6456b        /* Coral/pink — CTAs, highlights, links, active states */
--accent-dark: #b83a5a   /* Hover state for accent elements */
--accent-light: #e8668a  /* Lighter accent (used in dark sections like CTA box) */
--accent-glow: rgba(214, 69, 107, 0.15)   /* Soft glow for shadows */
--accent-bg: rgba(214, 69, 107, 0.06)     /* Subtle background tint */
--bg-primary: #ffffff    /* Page background */
--bg-secondary: #f7f7fa  /* Alternate section background */
--bg-card: #ffffff       /* Card background */
--text-primary: #1a1a2e  /* Primary body text */
--text-secondary: #555568 /* Secondary/supporting text */
--text-muted: #8888a0    /* Muted / meta text */
--border-color: #e5e5ee  /* Standard borders */
--border-light: #f0f0f5  /* Lighter borders */
```

### Typography

- **Font**: Inter (Google Fonts, weights 400/500/600/700/800/900)
- **Headings**: `font-weight: 700`, `letter-spacing: -0.02em`, `color: var(--navy)`
  - h1: `clamp(2.4rem, 5vw, 4rem)`
  - h2: `clamp(1.8rem, 3.5vw, 2.8rem)`
  - h3: `clamp(1.2rem, 2vw, 1.5rem)`
- **Body**: `font-size: 16px`, `line-height: 1.7`
- **Fallback stack**: `Inter, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif`

### Layout

- **Max width**: `1200px` (`var(--max-width)`)
- **Container padding**: `2rem` on each side (`.container` class)
- **Section padding**: `6rem 0` desktop, `4rem 0` mobile
- **Grid helpers**: `.grid-2`, `.grid-3`, `.grid-4` — all collapse to 1 column on mobile (`≤768px`)

### Spacing & Shapes

- `--radius: 12px` — cards, inputs, integration items
- `--radius-lg: 20px` — large cards, CTA box, value-prop visuals
- `--shadow`: `0 4px 24px rgba(26, 26, 46, 0.08)`
- `--shadow-lg`: `0 8px 40px rgba(26, 26, 46, 0.12)`
- `--transition`: `0.3s cubic-bezier(0.4, 0, 0.2, 1)`

---

## Reusable Components & Patterns

These CSS classes are defined in `styles.css` and used across pages. Avoid duplicating them in page-level `<style>` blocks. Only add page-level styles for truly unique, one-off layouts.

### Buttons

```html
<!-- Primary (coral, filled) -->
<a href="/contact/" class="btn btn-primary">CTA Text <span>→</span></a>

<!-- Secondary (bordered, transparent) -->
<a href="/solutions/" class="btn btn-secondary">Learn More</a>

<!-- Ghost (text-only, accent color) -->
<a href="/solutions/" class="btn btn-ghost">See how we work <span class="arrow">→</span></a>
```

### Cards

```html
<div class="card">
  <div class="card-icon"><!-- icon, number, or emoji --></div>
  <h3>Card Title</h3>
  <p>Card body text goes here.</p>
</div>
```

Cards have a hover effect: border turns accent-colored, element lifts with `translateY(-4px)`.

### Section Label

```html
<span class="section-label">Uppercase Label</span>
```

Small uppercase text in accent color, used above section headings.

### Page Header (internal pages — not used on homepage)

```html
<section class="page-header">
  <div class="container">
    <span class="section-label">Label</span>
    <h1>Page <span class="gradient-text">Title</span></h1>
    <p>Subtitle text.</p>
  </div>
</section>
```

### CTA Banner (dark navy call-to-action)

```html
<section class="cta-banner">
  <div class="container">
    <div class="cta-box">
      <h2>Headline <span class="gradient-text">accent</span></h2>
      <p>Supporting text.</p>
      <a href="/contact/" class="btn btn-primary">CTA <span>→</span></a>
    </div>
  </div>
</section>
```

### Fade-in Scroll Animation

Add `.fade-in` to any element. The Intersection Observer script (included on every page) adds `.visible` when the element scrolls into view:

```html
<div class="fade-in">This appears on scroll.</div>
```

The JS observer is always:
```js
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) entry.target.classList.add('visible');
  });
}, { threshold: 0.1 });
document.querySelectorAll('.fade-in').forEach(el => observer.observe(el));
```

### Gradient / Accent Text

```html
<span class="gradient-text">highlighted word</span>
```

Despite the name, this renders as a solid accent color (`--accent: #d6456b`), not a true gradient.

---

## Page Anatomy

Every page follows this structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <!-- Favicons (root-relative paths) -->
  <link rel="icon" type="image/x-icon" href="/favicon.ico" />
  <link rel="icon" type="image/png" sizes="32x32" href="/assets/favicon-32.png" />
  <link rel="icon" type="image/png" sizes="16x16" href="/assets/favicon-16.png" />
  <link rel="apple-touch-icon" sizes="180x180" href="/assets/apple-touch-icon.png" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <!-- Title and meta -->
  <title>Page Name | Datahooks</title>
  <meta name="description" content="Page description." />
  <link rel="canonical" href="https://datahooks.net/page-slug/" />
  <!-- OpenGraph -->
  <meta property="og:type" content="website" />
  <meta property="og:url" content="https://datahooks.net/page-slug/" />
  <meta property="og:title" content="..." />
  <meta property="og:description" content="..." />
  <meta property="og:site_name" content="Datahooks" />
  <!-- Twitter Card -->
  <meta name="twitter:card" content="summary" />
  <meta name="twitter:title" content="..." />
  <meta name="twitter:description" content="..." />
  <!-- Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800;900&display=swap" rel="stylesheet" />
  <!-- Global stylesheet -->
  <link rel="stylesheet" href="/styles.css" />
  <!-- Page-specific styles (only if truly unique to this page) -->
  <style> /* ... */ </style>
</head>
<body>

  <!-- NAVBAR (copy from any existing page, set .active on correct link) -->

  <!-- PAGE CONTENT -->

  <!-- CTA BANNER (optional but present on most pages) -->

  <!-- FOOTER (copy from any existing page) -->

  <script>
    // Mobile menu toggle (required on every page)
    document.getElementById('mobileToggle').addEventListener('click', () => {
      document.getElementById('navLinks').classList.toggle('open');
    });
    // Fade-in observer (required on every page that uses .fade-in)
    const observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => { if (entry.isIntersecting) entry.target.classList.add('visible'); });
    }, { threshold: 0.1 });
    document.querySelectorAll('.fade-in').forEach(el => observer.observe(el));
    // Page-specific JS here
  </script>

</body>
</html>
```

---

## Navbar

The navbar is **fixed at the top** and uses `backdrop-filter: blur` for a frosted glass effect. It is copy-pasted identically into every page — there is no templating. Mark the active page with class `active` on its `<a>` tag.

```html
<nav class="navbar">
  <div class="container">
    <a href="/" class="logo">
      <span class="logo-icon"><img src="/assets/logo-d-white.png" alt="Datahooks" /></span> Datahooks
    </a>
    <ul class="nav-links" id="navLinks">
      <li><a href="/">Home</a></li>
      <li><a href="/solutions/">What We Do</a></li>
      <li><a href="/about/">About</a></li>
      <li><a href="/contact/" class="btn btn-primary" style="padding:0.6rem 1.4rem;">Let's Talk</a></li>
    </ul>
    <button class="mobile-toggle" id="mobileToggle" aria-label="Toggle menu">
      <span></span><span></span><span></span>
    </button>
  </div>
</nav>
```

Navigation items: Home → `/`, What We Do → `/solutions/`, About → `/about/`, Let's Talk → `/contact/` (rendered as primary button).

---

## Footer

The footer is also copy-pasted into every page. It uses the navy dark background and a 4-column grid.

```html
<footer class="footer">
  <div class="container">
    <div class="footer-grid">
      <div class="footer-brand">
        <a href="/" class="logo">
          <span class="logo-icon"><img src="/assets/logo-d-white.png" alt="Datahooks" /></span> Datahooks
        </a>
        <p>We align your teams, fix your data, and make sure your dashboards, automations, and AI actually work.</p>
      </div>
      <div>
        <h4>Services</h4>
        <ul class="footer-links">
          <li><a href="/solutions/">What We Do</a></li>
          <li><a href="/solutions/#integrations">Data Sources</a></li>
          <li><a href="/solutions/#process">Our Process</a></li>
        </ul>
      </div>
      <div>
        <h4>Company</h4>
        <ul class="footer-links">
          <li><a href="/about/">About Us</a></li>
          <li><a href="/blog/">Blog</a></li>
          <li><a href="/contact/">Contact</a></li>
        </ul>
      </div>
      <div>
        <h4>Legal</h4>
        <ul class="footer-links">
          <li><a href="/privacy/">Privacy Policy</a></li>
          <li><a href="/terms/">Terms of Service</a></li>
        </ul>
      </div>
    </div>
    <div class="footer-bottom">
      <span>&copy; 2026 Datahooks. All rights reserved.</span>
    </div>
  </div>
</footer>
```

---

## Asset Path Convention

This is a common source of bugs:

| Page location | Correct asset path style | Example |
|---|---|---|
| `/index.html` (homepage) | **Relative** | `assets/logo-d-white.png`, `styles.css` |
| `/subdir/index.html` (all other pages) | **Root-relative** | `/assets/logo-d-white.png`, `/styles.css` |

The homepage uses relative paths because it sits at the root. All subdirectory pages must use root-relative paths (starting with `/`) or assets will 404.

---

## Contact Form

The contact form (`/contact/index.html`) submits via `fetch()` to a **Google Apps Script** endpoint that writes to a Google Sheet.

- The `GOOGLE_SHEETS_URL` constant is hardcoded in the page's `<script>` block
- The form uses `FormData` (not JSON)
- CORS errors from the fetch are **expected and silently caught** — the data still posts successfully to the sheet despite the CORS error
- On submission (success or CORS error), the form is hidden and a success confirmation is shown
- Required fields: First Name, Last Name, Work Email, Company
- Optional fields: Role (select), Company Size (select), Message (textarea)

---

## SEO Conventions

Every page must include:

1. `<title>` — Format: `"Page Name | Datahooks"` (or `"Datahooks | Tagline"` for the homepage)
2. `<meta name="description">` — 1–2 sentences, unique per page
3. `<link rel="canonical">` — Full URL with trailing slash: `https://datahooks.net/page/`
4. OpenGraph: `og:type`, `og:url`, `og:title`, `og:description`, `og:site_name`
5. Twitter Card: `twitter:card` (`summary`), `twitter:title`, `twitter:description`

The homepage additionally includes Schema.org JSON-LD (`@type: Organization`) in a `<script type="application/ld+json">` block.

**Always use trailing slashes** on all internal and canonical URLs (matches GitHub Pages behavior).

When adding new pages, update `sitemap.xml` with the new URL, `changefreq`, and `priority`.

---

## Adding a New Page

1. Create the directory: `new-page-slug/`
2. Create `new-page-slug/index.html` following the page anatomy above
3. Use **root-relative paths** for assets and CSS: `/styles.css`, `/assets/...`
4. Copy the navbar HTML from an existing page; set `.active` on the correct nav link
5. Copy the footer HTML from an existing page
6. Include the mobile toggle JS and fade-in observer JS in `<script>` at the bottom
7. Add the new URL to `sitemap.xml`
8. If this page is added to the navbar, update the navbar in **every other page** as well

---

## Blog Posts

Currently the blog (`/blog/index.html`) is a listing-only page — all post content is inline HTML cards within `blog/index.html`. There are no individual post pages yet.

When adding a new blog post:
- Add a new `<article class="blog-card">` to the blog grid in `blog/index.html`
- If it gets its own page, create `blog/post-slug/index.html` and link to it from the card
- Use `.blog-category`, `.blog-meta`, `.blog-date`, `.blog-readtime` classes (defined inline in `blog/index.html`'s `<style>` block)

---

## What NOT to Do

- **No build system**: Do not introduce npm, webpack, Vite, or any framework without explicit discussion. The site is intentionally zero-dependency.
- **No duplicate CSS**: Do not add page-level `<style>` rules that replicate global patterns. Extend `styles.css` instead.
- **No wrong asset paths**: Subdirectory pages always use root-relative paths (`/assets/...`), never relative ones (`../assets/...` or `assets/...`).
- **No `.DS_Store` commits**: The repo already has one committed; do not add more. (A `.gitignore` should be added to prevent this.)
- **No trailing whitespace or mixed indentation**: The codebase uses 2-space indentation in HTML files.
- **Do not break the shared footer/navbar**: Edits to shared HTML must be propagated to every page that includes it.
