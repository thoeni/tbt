# The Birth Tailor Website Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single-page static website for The Birth Tailor midwife consultancy with a warm minimal design.

**Architecture:** Plain HTML/CSS/JS with no build tools. Single `index.html` with 4 anchored sections (hero, about, services, contact). CSS custom properties for theming. Vanilla JS for smooth scroll, sticky nav, mobile menu, and scroll-triggered fade-ins. Deployed on GitHub Pages with custom domain.

**Tech Stack:** HTML5, CSS3 (custom properties, grid, flexbox), vanilla JavaScript, Google Fonts (Playfair Display, Source Sans Pro).

**Spec:** `docs/superpowers/specs/2026-03-12-the-birth-tailor-website-design.md`

**Existing assets repo:** `/tmp/thebirthtailor-assets/` (cloned from thoeni/thebirthtailor.co.uk)

---

## File Structure

```
tbt/
├── index.html              # Single page -- all 4 sections + nav + footer
├── css/
│   └── style.css           # All styles (variables, base, nav, sections, responsive)
├── js/
│   └── main.js             # Smooth scroll, sticky nav, mobile menu, fade-ins
├── images/
│   ├── dahlia.png          # Dahlia illustration (hero watermark)
│   ├── becks.jpg           # B&W photo (about section)
│   ├── fedant.png          # FEDANT badge (about section)
│   └── instagram.svg       # Instagram icon (contact section)
├── fonts/
│   └── aveutan.ttf         # Script font for logo text
├── favicon.svg             # SVG favicon (primary)
├── favicon.ico             # ICO fallback
├── favicon-32x32.png       # PNG fallback
├── CNAME                   # GitHub Pages custom domain
└── docs/                   # Specs and plans (already exists)
```

---

## Chunk 1: Foundation

### Task 1: Project Setup and Asset Migration

**Files:**
- Create: `images/dahlia.png`, `images/becks.jpg`, `images/fedant.png`, `images/instagram.svg`
- Create: `fonts/aveutan.ttf`
- Create: `favicon.svg`, `favicon-32x32.png`, `favicon.ico`
- Create: `CNAME`
- Create: `.gitignore`

- [ ] **Step 1: Create directory structure**

```bash
mkdir -p images fonts css js
```

- [ ] **Step 2: Copy assets from existing repo**

The existing repo is cloned at `/tmp/thebirthtailor-assets/`. Copy the needed files:

```bash
cp /tmp/thebirthtailor-assets/website/static/images/illustrations/dhalia1.png images/dahlia.png
cp /tmp/thebirthtailor-assets/website/static/images/illustrations/becks.jpg images/becks.jpg
cp /tmp/thebirthtailor-assets/website/static/images/illustrations/fedant-logo-4.png images/fedant.png
cp /tmp/thebirthtailor-assets/website/static/images/social/Instagram.svg images/instagram.svg
cp /tmp/thebirthtailor-assets/website/static/fonts/aveutan.ttf fonts/aveutan.ttf
cp /tmp/thebirthtailor-assets/website/static/favicon-32x32.svg favicon.svg
```

- [ ] **Step 3: Generate favicon fallbacks**

Convert the SVG favicon to PNG and ICO formats. Use `rsvg-convert` (from `librsvg`) or ImageMagick:

```bash
# PNG fallback -- try rsvg-convert first (handles SVG properly), fall back to ImageMagick
rsvg-convert -w 32 -h 32 favicon.svg -o favicon-32x32.png 2>/dev/null \
  || convert favicon.svg -resize 32x32 favicon-32x32.png 2>/dev/null \
  || echo "WARN: PNG conversion failed -- install librsvg or ImageMagick"

# ICO fallback
convert favicon-32x32.png favicon.ico 2>/dev/null \
  || echo "ICO conversion skipped -- SVG + PNG provide sufficient coverage"

# Verify at least the PNG exists
ls -la favicon-32x32.png
```

Note: if neither tool is available, install with `brew install librsvg imagemagick`.

- [ ] **Step 4: Create CNAME for GitHub Pages custom domain**

```
thebirthtailor.co.uk
```

- [ ] **Step 5: Create .gitignore**

```
.DS_Store
.superpowers/
```

- [ ] **Step 6: Commit**

```bash
git add images/ fonts/ favicon.svg favicon-32x32.png CNAME .gitignore
git add favicon.ico 2>/dev/null || true  # May not exist if ImageMagick not installed
git commit -m "feat: add project structure and migrate assets from existing repo"
```

---

### Task 2: HTML Skeleton

**Files:**
- Create: `index.html`

All content text comes from the spec and the existing repo content files. Build the complete semantic HTML structure with all sections populated.

- [ ] **Step 1: Create index.html with full content**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>The Birth Tailor -- Personalised Pregnancy &amp; Birth Support</title>
  <meta name="description" content="Expert antenatal support tailored to you. Becks is a senior midwife and FEDANT-approved educator offering personalised pregnancy consultations in person and online.">

  <!-- Open Graph -->
  <meta property="og:title" content="The Birth Tailor -- Personalised Pregnancy & Birth Support">
  <meta property="og:description" content="Expert antenatal support tailored to you. Becks is a senior midwife and FEDANT-approved educator offering personalised pregnancy consultations in person and online.">
  <meta property="og:type" content="website">
  <meta property="og:url" content="https://thebirthtailor.co.uk">
  <meta property="og:image" content="https://thebirthtailor.co.uk/images/dahlia.png">

  <!-- Favicons -->
  <link rel="icon" type="image/svg+xml" href="favicon.svg">
  <link rel="icon" type="image/png" sizes="32x32" href="favicon-32x32.png">
  <link rel="shortcut icon" href="favicon.ico">

  <!-- Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600&family=Source+Sans+Pro:wght@400;700&display=swap" rel="stylesheet">

  <!-- Styles -->
  <link rel="stylesheet" href="css/style.css">

  <!-- Google Analytics -->
  <script async src="https://www.googletagmanager.com/gtag/js?id=G-8SB95KXBHV"></script>
  <script>
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);}
    gtag('js', new Date());
    gtag('config', 'G-8SB95KXBHV');
  </script>
</head>
<body>

  <!-- Navigation -->
  <header class="nav" id="nav">
    <div class="nav__container">
      <a href="#" class="nav__logo">The Birth Tailor</a>
      <nav class="nav__links" id="nav-links">
        <a href="#about" class="nav__link">About</a>
        <a href="#services" class="nav__link">Services</a>
        <a href="#contact" class="nav__link">Contact</a>
      </nav>
      <button class="nav__hamburger" id="hamburger" aria-label="Toggle menu" aria-expanded="false">
        <span></span>
        <span></span>
        <span></span>
      </button>
    </div>
  </header>

  <main>

    <!-- Hero Section -->
    <section class="hero" id="hero">
      <div class="hero__content">
        <h1 class="hero__title">Personalised guidance for your pregnancy journey</h1>
        <p class="hero__subtitle">Bespoke antenatal support from a senior midwife and FEDANT-approved educator</p>
        <a href="#contact" class="hero__cta">Get in touch</a>
      </div>
      <img src="images/dahlia.png" alt="" class="hero__watermark" aria-hidden="true">
    </section>

    <!-- About Section -->
    <section class="about" id="about">
      <div class="about__container">
        <div class="about__image-wrapper">
          <img src="images/becks.jpg" alt="Becks, senior midwife and founder of The Birth Tailor" class="about__image">
        </div>
        <div class="about__text">
          <h2 class="about__title">Meet Becks</h2>
          <p>I'm a senior midwife with over 6 years of experience helping women navigate pregnancy, birth, and beyond. I believe in empowering women through informed choices, though I understand how daunting maternity services can be.</p>
          <p>That's why I founded <strong>The Birth Tailor</strong> -- to provide tailored support and guidance throughout your pregnancy journey. My focus is on education, emotional support, and preparing you for whatever path your birth takes.</p>
          <img src="images/fedant.png" alt="FEDANT Approved Antenatal Educator" class="about__badge">
        </div>
      </div>
    </section>

    <!-- Services Section -->
    <section class="services" id="services">
      <div class="services__container">
        <h2 class="services__title">How I can help</h2>
        <p class="services__intro">Take the opportunity to dive deep into your pregnancy and labour questions with expert guidance in a one-on-one setting.</p>

        <div class="services__grid">
          <div class="services__card services__card--blue">
            <h3>Low Risk vs. High Risk Pregnancy</h3>
            <p>Clarifying medical labels and what they truly mean for your pregnancy journey.</p>
          </div>
          <div class="services__card services__card--pink">
            <h3>Natural vs. Medicalised Birth</h3>
            <p>Explore your options and understand what the expectations are.</p>
          </div>
          <div class="services__card services__card--blue">
            <h3>Caesarean Section in Labour</h3>
            <p>Understand when it's necessary and what to expect if it becomes part of your birth plan.</p>
          </div>
          <div class="services__card services__card--pink">
            <h3>Maternity Triage Explained</h3>
            <p>Get the inside scoop on what to expect and how to navigate triage.</p>
          </div>
          <div class="services__card services__card--blue">
            <h3>Induction of Labour</h3>
            <p>Understand why induction might be recommended, the methods involved, and how it affects your experience.</p>
          </div>
          <div class="services__card services__card--pink">
            <h3>Breastfeeding</h3>
            <p>Explore the real challenges and rewards beyond the perfect images, with practical advice.</p>
          </div>
          <div class="services__card services__card--blue">
            <h3>The First 48 Hours</h3>
            <p>What to expect in those first days of parenthood and how to prepare.</p>
          </div>
        </div>

        <div class="services__pricing">
          <p class="services__price"><strong>£140</strong> per hour</p>
          <p class="services__offer">Book a <strong>free, non-committal introductory call</strong> to discuss your needs.</p>
          <p class="services__note">Available in person and virtually</p>
        </div>
      </div>
    </section>

    <!-- Contact Section -->
    <section class="contact" id="contact">
      <div class="contact__container">
        <h2 class="contact__title">Let's talk</h2>
        <p class="contact__text">Whether you're expecting or planning, I'd love to hear from you. Book a free, no-commitment introductory call.</p>
        <div class="contact__links">
          <a href="mailto:becks@thebirthtailor.co.uk" class="contact__email">becks@thebirthtailor.co.uk</a>
          <a href="https://www.instagram.com/thebirthtailor/" target="_blank" rel="noopener noreferrer" class="contact__instagram" aria-label="Follow on Instagram">
            <img src="images/instagram.svg" alt="" class="contact__icon" aria-hidden="true">
            <span>@thebirthtailor</span>
          </a>
        </div>
      </div>
    </section>

  </main>

  <!-- Footer -->
  <footer class="footer">
    <div class="footer__container">
      <p class="footer__copyright">&copy; The Birth Tailor 2026</p>
    </div>
  </footer>

  <script src="js/main.js"></script>
</body>
</html>
```

- [ ] **Step 2: Verify HTML opens in browser**

```bash
open index.html
```

Expected: unstyled but readable page with all content visible, all internal anchor links functional.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add complete HTML structure with all sections and content"
```

---

### Task 3: CSS Base -- Variables, Reset, Typography

**Files:**
- Create: `css/style.css`

- [ ] **Step 1: Create style.css with custom properties, reset, and base typography**

```css
/* ============================================
   Custom Properties
   ============================================ */
:root {
  /* Colours */
  --color-bg: #f8f6f3;
  --color-bg-footer: #f0ece7;
  --color-text: #2f2f41;
  --color-text-secondary: #5C5A5A;
  --color-accent-blue: #4B7BE5;
  --color-accent-pink: #f24088;
  --color-white: #ffffff;

  /* Typography */
  --font-heading: 'Playfair Display', Georgia, serif;
  --font-body: 'Source Sans Pro', -apple-system, BlinkMacSystemFont, sans-serif;

  /* Spacing */
  --section-padding: 100px 80px;
  --container-max: 1140px;

  /* Transitions */
  --transition-smooth: 0.3s ease;
}

/* ============================================
   Reset
   ============================================ */
*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
  -webkit-text-size-adjust: 100%;
}

body {
  font-family: var(--font-body);
  font-size: 18px;
  line-height: 1.7;
  color: var(--color-text);
  background-color: var(--color-bg);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

img {
  max-width: 100%;
  height: auto;
  display: block;
}

a {
  color: var(--color-accent-blue);
  text-decoration: none;
  transition: color var(--transition-smooth);
}

a:hover {
  color: var(--color-accent-pink);
}

h1, h2, h3 {
  font-family: var(--font-heading);
  font-weight: 600;
  line-height: 1.3;
  color: var(--color-text);
}

h1 { font-size: clamp(2.2rem, 5vw, 3.5rem); }
h2 { font-size: clamp(1.8rem, 4vw, 2.5rem); }
h3 { font-size: clamp(1.1rem, 2vw, 1.35rem); }

p {
  margin-bottom: 1em;
}

strong {
  font-weight: 700;
}

/* ============================================
   Logo Font
   ============================================ */
@font-face {
  font-family: 'Aveutan';
  src: url('../fonts/aveutan.ttf') format('truetype');
  font-weight: normal;
  font-style: normal;
  font-display: swap;
}
```

- [ ] **Step 2: Verify in browser**

```bash
open index.html
```

Expected: cream background, correct fonts loading, readable typography with proper sizing.

- [ ] **Step 3: Commit**

```bash
git add css/style.css
git commit -m "feat: add CSS base with custom properties, reset, and typography"
```

---

## Chunk 2: Navigation and Hero

### Task 4: Navigation Styles

**Files:**
- Modify: `css/style.css` (append navigation styles)

- [ ] **Step 1: Add navigation CSS to style.css**

Append the following to `css/style.css`:

```css
/* ============================================
   Navigation
   ============================================ */
.nav {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 100;
  padding: 20px 0;
  transition: background-color var(--transition-smooth), box-shadow var(--transition-smooth), padding var(--transition-smooth);
}

.nav--scrolled {
  background-color: rgba(248, 246, 243, 0.95);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  box-shadow: 0 1px 10px rgba(0, 0, 0, 0.05);
  padding: 12px 0;
}

.nav__container {
  max-width: var(--container-max);
  margin: 0 auto;
  padding: 0 40px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.nav__logo {
  font-family: 'Aveutan', cursive;
  font-size: 1.6rem;
  color: var(--color-text);
  text-decoration: none;
  letter-spacing: 0.5px;
}

.nav__logo:hover {
  color: var(--color-accent-blue);
}

.nav__links {
  display: flex;
  gap: 36px;
}

.nav__link {
  font-size: 0.85rem;
  font-weight: 400;
  letter-spacing: 1.5px;
  text-transform: uppercase;
  color: var(--color-text-secondary);
  text-decoration: none;
  transition: color var(--transition-smooth);
}

.nav__link:hover {
  color: var(--color-accent-blue);
}

.nav__hamburger {
  display: none;
  background: none;
  border: none;
  cursor: pointer;
  padding: 8px;
  flex-direction: column;
  gap: 5px;
}

.nav__hamburger span {
  display: block;
  width: 24px;
  height: 2px;
  background-color: var(--color-text);
  transition: transform var(--transition-smooth), opacity var(--transition-smooth);
}
```

- [ ] **Step 2: Verify in browser**

Expected: nav bar visible at top of page, logo on left in script font, links on right in small caps. Transparent background (will become sticky with JS).

- [ ] **Step 3: Commit**

```bash
git add css/style.css
git commit -m "feat: add navigation styles with sticky-ready transitions"
```

---

### Task 5: Hero Section Styles

**Files:**
- Modify: `css/style.css` (append hero styles)

- [ ] **Step 1: Add hero CSS to style.css**

Append the following:

```css
/* ============================================
   Hero Section
   ============================================ */
.hero {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  overflow: hidden;
  padding: 120px 40px 80px;
  text-align: center;
}

.hero__content {
  position: relative;
  z-index: 2;
  max-width: 700px;
}

.hero__title {
  margin-bottom: 24px;
  letter-spacing: -0.5px;
}

.hero__subtitle {
  font-size: 1.15rem;
  color: var(--color-text-secondary);
  margin-bottom: 40px;
  line-height: 1.6;
}

.hero__cta {
  display: inline-block;
  padding: 14px 36px;
  border: 1.5px solid var(--color-accent-blue);
  color: var(--color-accent-blue);
  font-size: 0.85rem;
  font-weight: 700;
  letter-spacing: 1.5px;
  text-transform: uppercase;
  text-decoration: none;
  transition: background-color var(--transition-smooth), color var(--transition-smooth);
}

.hero__cta:hover {
  background-color: var(--color-accent-blue);
  color: var(--color-white);
}

.hero__watermark {
  position: absolute;
  right: -5%;
  bottom: -5%;
  width: 45%;
  max-width: 500px;
  opacity: 0.12;
  pointer-events: none;
  z-index: 1;
}
```

- [ ] **Step 2: Verify in browser**

Expected: full-viewport hero with centred headline, subtitle, and CTA button. Dahlia visible as a subtle watermark in the bottom-right. Nav overlaps hero transparently.

- [ ] **Step 3: Commit**

```bash
git add css/style.css
git commit -m "feat: add hero section with dahlia watermark and CTA"
```

---

## Chunk 3: About and Services

### Task 6: About Section Styles

**Files:**
- Modify: `css/style.css` (append about styles)

- [ ] **Step 1: Add about CSS to style.css**

Append the following:

```css
/* ============================================
   About Section
   ============================================ */
.about {
  padding: var(--section-padding);
}

.about__container {
  max-width: var(--container-max);
  margin: 0 auto;
  display: grid;
  grid-template-columns: 1fr 1.2fr;
  gap: 60px;
  align-items: center;
}

.about__image-wrapper {
  position: relative;
}

.about__image {
  width: 100%;
  border-radius: 4px;
  box-shadow: 12px 12px 0 rgba(75, 123, 229, 0.1);
}

.about__title {
  margin-bottom: 24px;
}

.about__text p {
  color: var(--color-text-secondary);
}

.about__badge {
  margin-top: 24px;
  max-width: 100px;
  opacity: 0.8;
}
```

- [ ] **Step 2: Verify in browser**

Expected: two-column layout with photo on left (subtle blue-tinted shadow), bio text on right, FEDANT badge below text.

- [ ] **Step 3: Commit**

```bash
git add css/style.css
git commit -m "feat: add about section with two-column layout"
```

---

### Task 7: Services Section Styles

**Files:**
- Modify: `css/style.css` (append services styles)

- [ ] **Step 1: Add services CSS to style.css**

Append the following:

```css
/* ============================================
   Services Section
   ============================================ */
.services {
  padding: var(--section-padding);
}

.services__container {
  max-width: var(--container-max);
  margin: 0 auto;
}

.services__title {
  text-align: center;
  margin-bottom: 16px;
}

.services__intro {
  text-align: center;
  color: var(--color-text-secondary);
  max-width: 600px;
  margin: 0 auto 48px;
}

.services__grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 24px;
  margin-bottom: 48px;
}

/* Centre the last card when odd count */
.services__grid > :last-child:nth-child(odd) {
  grid-column: 1 / -1;
  max-width: calc(50% - 12px);
  margin: 0 auto;
}

.services__card {
  padding: 32px;
  background: var(--color-white);
  border-radius: 4px;
  border-top: 3px solid transparent;
  transition: box-shadow var(--transition-smooth), transform var(--transition-smooth);
}

.services__card:hover {
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.06);
  transform: translateY(-2px);
}

.services__card--blue {
  border-top-color: var(--color-accent-blue);
}

.services__card--pink {
  border-top-color: var(--color-accent-pink);
}

.services__card h3 {
  margin-bottom: 8px;
}

.services__card p {
  color: var(--color-text-secondary);
  font-size: 0.95rem;
  margin-bottom: 0;
}

.services__pricing {
  text-align: center;
  padding: 40px;
  background: var(--color-white);
  border-radius: 4px;
  border-left: 4px solid var(--color-accent-blue);
}

.services__price {
  font-family: var(--font-heading);
  font-size: 1.5rem;
  margin-bottom: 8px;
}

.services__price strong {
  font-size: 2rem;
  color: var(--color-accent-blue);
}

.services__offer {
  font-size: 1.05rem;
  margin-bottom: 4px;
}

.services__note {
  font-size: 0.85rem;
  color: var(--color-text-secondary);
  margin-bottom: 0;
}
```

- [ ] **Step 2: Verify in browser**

Expected: centred heading and intro, 2-column card grid with alternating blue/pink top borders, 7th card centred, pricing box below with blue left border.

- [ ] **Step 3: Commit**

```bash
git add css/style.css
git commit -m "feat: add services section with card grid and pricing"
```

---

## Chunk 4: Contact, Footer, Responsive, and JS

### Task 8: Contact and Footer Styles

**Files:**
- Modify: `css/style.css` (append contact and footer styles)

- [ ] **Step 1: Add contact and footer CSS to style.css**

Append the following:

```css
/* ============================================
   Contact Section
   ============================================ */
.contact {
  padding: var(--section-padding);
  text-align: center;
}

.contact__container {
  max-width: 600px;
  margin: 0 auto;
}

.contact__title {
  margin-bottom: 16px;
}

.contact__text {
  color: var(--color-text-secondary);
  margin-bottom: 32px;
}

.contact__links {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
}

.contact__email {
  font-size: 1.15rem;
  font-weight: 700;
  color: var(--color-accent-blue);
  text-decoration: none;
}

.contact__email:hover {
  color: var(--color-accent-pink);
}

.contact__instagram {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  color: var(--color-text-secondary);
  text-decoration: none;
  font-size: 0.95rem;
  transition: color var(--transition-smooth);
}

.contact__instagram:hover {
  color: var(--color-accent-pink);
}

.contact__icon {
  width: 20px;
  height: 20px;
}

/* ============================================
   Footer
   ============================================ */
.footer {
  background-color: var(--color-bg-footer);
  padding: 32px 40px;
  text-align: center;
}

.footer__container {
  max-width: var(--container-max);
  margin: 0 auto;
}

.footer__copyright {
  font-size: 0.8rem;
  color: var(--color-text-secondary);
  margin-bottom: 0;
}
```

- [ ] **Step 2: Verify in browser**

Expected: centred contact section with email and Instagram links. Footer with darker cream background and small copyright text.

- [ ] **Step 3: Commit**

```bash
git add css/style.css
git commit -m "feat: add contact section and footer styles"
```

---

### Task 9: Responsive Styles

**Files:**
- Modify: `css/style.css` (append media queries)

- [ ] **Step 1: Add responsive CSS to style.css**

Append the following:

```css
/* ============================================
   Responsive -- Tablet (768-992px)
   ============================================ */
@media (max-width: 992px) {
  :root {
    --section-padding: 80px 40px;
  }

  .about__container {
    gap: 40px;
  }

  .hero__watermark {
    width: 55%;
    opacity: 0.08;
  }
}

/* ============================================
   Responsive -- Mobile (<768px)
   ============================================ */
@media (max-width: 768px) {
  :root {
    --section-padding: 60px 24px;
  }

  /* Nav -- mobile */
  .nav__links {
    position: fixed;
    top: 0;
    right: -100%;
    width: 70%;
    max-width: 300px;
    height: 100vh;
    background-color: var(--color-bg);
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 32px;
    transition: right var(--transition-smooth);
    box-shadow: -4px 0 20px rgba(0, 0, 0, 0.08);
  }

  .nav__links--open {
    right: 0;
  }

  .nav__link {
    font-size: 1rem;
  }

  .nav__hamburger {
    display: flex;
  }

  /* Hamburger animation */
  .nav__hamburger--active span:nth-child(1) {
    transform: rotate(45deg) translate(5px, 5px);
  }
  .nav__hamburger--active span:nth-child(2) {
    opacity: 0;
  }
  .nav__hamburger--active span:nth-child(3) {
    transform: rotate(-45deg) translate(5px, -5px);
  }

  /* Mobile overlay */
  .nav__overlay {
    display: none;
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.3);
    z-index: -1;
  }

  .nav__overlay--visible {
    display: block;
  }

  /* Hero -- mobile */
  .hero {
    padding: 100px 24px 60px;
    min-height: 85vh;
  }

  .hero__watermark {
    width: 70%;
    opacity: 0.06;
    right: -10%;
    bottom: -10%;
  }

  /* About -- mobile (stack) */
  .about__container {
    grid-template-columns: 1fr;
    gap: 32px;
  }

  .about__image {
    max-width: 320px;
    margin: 0 auto;
  }

  /* Services -- mobile (single column) */
  .services__grid {
    grid-template-columns: 1fr;
  }

  .services__grid > :last-child:nth-child(odd) {
    max-width: 100%;
  }

  .services__pricing {
    padding: 28px 24px;
  }
}
```

- [ ] **Step 2: Test responsive behaviour**

Open browser dev tools and test at:
- Desktop (1200px wide): two-column layouts, horizontal nav
- Tablet (900px wide): layouts still two-column but tighter
- Mobile (375px wide): single column, hamburger visible, stacked about

- [ ] **Step 3: Commit**

```bash
git add css/style.css
git commit -m "feat: add responsive styles for tablet and mobile"
```

---

### Task 10: JavaScript -- Smooth Scroll, Sticky Nav, Mobile Menu

**Files:**
- Create: `js/main.js`

- [ ] **Step 1: Create main.js with all interactive features**

```javascript
(function () {
  'use strict';

  // ========================================
  // Sticky Navigation
  // ========================================
  const nav = document.getElementById('nav');

  function handleScroll() {
    if (window.scrollY > 50) {
      nav.classList.add('nav--scrolled');
    } else {
      nav.classList.remove('nav--scrolled');
    }
  }

  window.addEventListener('scroll', handleScroll, { passive: true });

  // ========================================
  // Mobile Menu
  // ========================================
  const hamburger = document.getElementById('hamburger');
  const navLinks = document.getElementById('nav-links');
  let overlay = null;

  function createOverlay() {
    overlay = document.createElement('div');
    overlay.className = 'nav__overlay';
    nav.appendChild(overlay);
    overlay.addEventListener('click', closeMenu);
  }

  function openMenu() {
    navLinks.classList.add('nav__links--open');
    hamburger.classList.add('nav__hamburger--active');
    hamburger.setAttribute('aria-expanded', 'true');
    if (overlay) overlay.classList.add('nav__overlay--visible');
    document.body.style.overflow = 'hidden';
  }

  function closeMenu() {
    navLinks.classList.remove('nav__links--open');
    hamburger.classList.remove('nav__hamburger--active');
    hamburger.setAttribute('aria-expanded', 'false');
    if (overlay) overlay.classList.remove('nav__overlay--visible');
    document.body.style.overflow = '';
  }

  hamburger.addEventListener('click', function () {
    const isOpen = navLinks.classList.contains('nav__links--open');
    if (isOpen) {
      closeMenu();
    } else {
      openMenu();
    }
  });

  // Close menu when a link is clicked
  navLinks.querySelectorAll('.nav__link').forEach(function (link) {
    link.addEventListener('click', closeMenu);
  });

  createOverlay();

  // ========================================
  // Scroll-triggered Fade-in Animations
  // ========================================
  const fadeElements = document.querySelectorAll('.about, .services__card, .services__pricing, .contact');

  // Add initial hidden state
  fadeElements.forEach(function (el) {
    el.style.opacity = '0';
    el.style.transform = 'translateY(24px)';
    el.style.transition = 'opacity 0.6s ease, transform 0.6s ease';
  });

  const observer = new IntersectionObserver(function (entries) {
    entries.forEach(function (entry) {
      if (entry.isIntersecting) {
        entry.target.style.opacity = '1';
        entry.target.style.transform = 'translateY(0)';
        observer.unobserve(entry.target);
      }
    });
  }, {
    threshold: 0.15,
    rootMargin: '0px 0px -40px 0px'
  });

  fadeElements.forEach(function (el) {
    observer.observe(el);
  });

})();
```

- [ ] **Step 2: Test all interactive features**

In the browser:
1. Scroll down -- nav should get cream background + shadow
2. Click nav links -- should smooth-scroll to sections
3. Resize to mobile width -- hamburger should appear
4. Click hamburger -- slide-in menu with overlay
5. Click a link in mobile menu -- menu closes, scrolls to section
6. Scroll down -- about, service cards, and contact should fade in

- [ ] **Step 3: Commit**

```bash
git add js/main.js
git commit -m "feat: add JS for sticky nav, mobile menu, and scroll animations"
```

---

## Chunk 5: Final Polish and Deployment

### Task 11: Visual Polish Pass

**Files:**
- Modify: `css/style.css` (minor adjustments)
- Modify: `index.html` (add scroll-margin for anchor offset)

- [ ] **Step 1: Add scroll offset for sticky nav**

Anchor links need offset to account for the fixed nav bar. Add to `css/style.css`:

```css
/* ============================================
   Scroll Offset for Sticky Nav
   ============================================ */
section[id] {
  scroll-margin-top: 80px;
}
```

- [ ] **Step 2: Add selection colours**

Add to `css/style.css`:

```css
/* ============================================
   Selection
   ============================================ */
::selection {
  background-color: rgba(75, 123, 229, 0.15);
  color: var(--color-text);
}
```

- [ ] **Step 3: Open in browser and do a full visual check**

Walk through the entire page top to bottom. Verify:
- Fonts are loading (Playfair Display, Source Sans Pro, Aveutan)
- Colour palette is consistent
- Spacing feels generous and balanced
- Dahlia watermark is subtle, not distracting
- Photo shadow looks good
- Cards have proper alternating accents
- Pricing box stands out
- Contact section is clean and centred
- Footer is subtly separated
- No layout breaks at any viewport width

- [ ] **Step 4: Commit**

```bash
git add css/style.css
git commit -m "feat: add scroll offset and selection colour polish"
```

---

### Task 12: GitHub Pages Deployment

**Files:**
- No new files (CNAME already created in Task 1)

- [ ] **Step 1: Verify CNAME file exists and is correct**

```bash
cat CNAME
```

Expected output: `thebirthtailor.co.uk`

- [ ] **Step 2: Push to GitHub**

```bash
git remote add origin git@github.com:thoeni/tbt.git 2>/dev/null || true
git push -u origin main
```

- [ ] **Step 3: Enable GitHub Pages**

Go to the repo settings on GitHub (Settings > Pages) and configure:
- Source: Deploy from a branch
- Branch: `main`, root (`/`)
- Save

Alternatively via CLI:

```bash
gh api repos/thoeni/tbt/pages -X POST -f source.branch=main -f source.path="/" 2>/dev/null || echo "Pages may already be configured"
```

- [ ] **Step 4: Configure DNS**

Set up DNS records for `thebirthtailor.co.uk` to point to GitHub Pages:
- A records: `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
- Or CNAME record: `thoeni.github.io`

(This is a manual step for the domain registrar.)

- [ ] **Step 5: Verify the site is live**

```bash
curl -I https://thoeni.github.io/tbt/ 2>/dev/null | head -5
```

Expected: HTTP 200 response. Custom domain may take a few minutes to propagate.

- [ ] **Step 6: Final commit with any last tweaks**

```bash
git add -A
git status
# Only commit if there are changes
git diff --cached --quiet || git commit -m "chore: final deployment tweaks"
git push
```
