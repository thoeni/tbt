# The Birth Tailor -- Website Design Spec

Status: Approved
Date: 2026-03-12

## Overview

Static informational website for "The Birth Tailor", a maternity/pregnancy consulting service run by Becks, a senior midwife and FEDANT-approved antenatal educator with 6+ years of experience. The site presents her services, builds trust, and provides contact details.

## Goals

- Present The Birth Tailor as a modern, professional, and approachable service
- Provide clear information about services and pricing
- Make it easy for prospective clients to get in touch
- Work beautifully on all devices

## Constraints

- Static site: plain HTML, CSS, and vanilla JS only -- no build tools or frameworks
- Hosted on GitHub Pages (repo: thoeni/tbt)
- Domain: thebirthtailor.co.uk
- No booking system, no contact form -- contact via email and Instagram only
- Existing assets to incorporate: dahlia illustration (PNG), B&W photo of Becks (JPG), FEDANT badge (PNG)
- Logo will be redesigned to match the new visual direction

## Design Direction: Warm Minimal

### Colour Palette

| Role | Colour | Hex |
|------|--------|-----|
| Background | Warm cream | #f8f6f3 |
| Text (primary) | Dark charcoal | #2f2f41 |
| Accent (primary) | Blue | #4B7BE5 |
| Accent (secondary) | Pink | #f24088 |
| Text (secondary) | Grey | #5C5A5A |
| Footer background | Slightly darker cream | #f0ece7 |

### Typography

| Role | Font | Source |
|------|------|--------|
| Headings | Playfair Display (600 weight) | Google Fonts |
| Body | Inter or Source Sans Pro (400, 700) | Google Fonts |
| Logo | Aveutan script (or redesigned) | Custom/local |

### Visual Characteristics

- Generous whitespace throughout
- Warm, off-white/cream tones -- feels like a high-end wellness brand
- Serif headings for elegance, clean sans-serif body for readability
- Blue and pink accents used sparingly for highlights and interactive elements
- Subtle animations: smooth scroll, gentle fade-ins on scroll

## Site Structure

Single-page design with smooth-scroll navigation across 4 sections.

### Navigation

- Minimal top bar: logo left, section links right (About, Services, Contact)
- Sticky on scroll with a subtle cream background + shadow
- Mobile: hamburger menu with slide-in overlay
- Smooth scroll to anchored sections on click

### Section 1: Hero

- Full viewport height
- Warm cream background
- Logo centred or top-left, rendered in brand colours (dark text or blue accent)
- Large serif headline: "Personalised guidance for your pregnancy journey"
- Subtitle: "Bespoke antenatal support from a senior midwife and FEDANT-approved educator"
- Dahlia illustration as a large decorative watermark -- approximately 40% opacity, offset to one side/corner, providing depth without competing with text
- Single CTA button: "Get in touch" -- smooth-scrolls to contact section
- Generous vertical spacing

### Section 2: About

- Two-column layout, stacks vertically on mobile
- Left: Becks' B&W photo with subtle brand-tinted border or shadow
- Right: heading ("Meet Becks") + condensed bio
  - Senior midwife, 6+ years experience
  - Why she founded The Birth Tailor
  - Personal, confident, short
- FEDANT badge displayed below the text as a small credential marker

### Section 3: Services

- Section heading: "How I can help" (or similar)
- Card grid: 2 columns on desktop, 1 column on mobile
- Each card has:
  - Small colour accent (alternating blue/pink line)
  - Title
  - Brief one-liner description
- 7 topic cards:
  1. Low risk vs. high risk pregnancy
  2. Natural vs. medicalised birth
  3. Caesarean section in labour
  4. Maternity triage explained
  5. Induction of labour
  6. Breastfeeding -- reality vs. ideals
  7. First 48 hours of parenthood
- Below cards: highlighted box with:
  - Pricing: £140 per hour
  - Free non-committal introductory call offer
  - Note: available in person and virtually

### Section 4: Contact

- Centred layout, minimal and warm
- Heading: "Let's talk" (or similar)
- Warm copy: "Whether you're expecting or planning, I'd love to hear from you. Book a free, no-commitment introductory call."
- Email: becks@thebirthtailor.co.uk (clickable mailto: link, displayed prominently)
- Instagram: icon + link
- Generous whitespace -- simplicity signals confidence

### Footer

- Minimal: copyright line ("The Birth Tailor 2026"), small logo mark
- Slightly darker cream background (#f0ece7) to subtly separate from content

## Responsive Behaviour

- **Desktop (>992px)**: full two-column layouts, horizontal nav, large hero typography
- **Tablet (768-992px)**: reduced whitespace, two-column layouts may narrow
- **Mobile (<768px)**: single column throughout, hamburger nav, stacked hero, stacked about section, single-column service cards

## Performance

- No build tools -- all assets served directly
- Optimised images (compressed JPG/PNG, appropriate dimensions)
- Google Fonts loaded with `display=swap` for fast text rendering
- Minimal JS: smooth scroll, sticky nav, mobile menu, optional scroll-triggered fade-ins
- Target: sub-2s first meaningful paint

## SEO

- Semantic HTML5 structure (header, main, section, footer)
- Meta title: "The Birth Tailor -- Personalised Pregnancy & Birth Support"
- Meta description based on homepage content
- Open Graph tags for social sharing
- Existing Google Analytics ID: G-8SB95KXBHV

## Assets to Migrate

From existing repo (thoeni/thebirthtailor.co.uk):

- `website/static/images/illustrations/dhalia1.png` -- dahlia illustration
- `website/static/images/illustrations/becks.jpg` -- B&W photo
- `website/static/images/illustrations/fedant-logo-4.png` -- FEDANT badge
- `website/static/images/social/Instagram.svg` -- Instagram icon
- `website/static/fonts/aveutan.ttf` -- script font (for logo if retained)
- `website/static/favicon-32x32.svg` -- favicon (may redesign)

## Out of Scope

- Blog/resources section
- Booking/scheduling integration (Calendly)
- Contact form
- Multiple pages (single-page design)
- Postnatal or birth attendance services
- E-commerce or payment processing
