🍎 PSYCHO APPLE — Lucid Streetwear

A single-file, zero-dependency streetwear storefront for Cape Town's cold nights.

Psycho Apple is a fully self-contained HTML/CSS/JS e-commerce landing page built for a Cape Town streetwear brand. No frameworks. No build step. No backend. Just open the file and the store is live — orders flow straight into WhatsApp.

---

📖 Table of Contents

· Overview
· Feature Breakdown
· Design System
· Architecture
· Product Catalog
· WhatsApp Order Flow
· Interactive Systems
· Animation & Motion
· Accessibility
· Performance
· Customization Guide
· Deployment
· Browser Support
· Known Limitations
· Roadmap
· Credits & License

---

🎯 Overview

Attribute Value
Type Static single-page storefront
File Psycho_Apple_V6.html
Dependencies Google Fonts only (no JS libraries)
Backend None — WhatsApp as the checkout layer
Payload One HTML file, ~65 KB
Brand Aesthetic Juice WRLD / lucid-dream purple, neon magenta, void black
Target Market Cape Town streetwear, unisex, S–XXL
Order Channel WhatsApp (+27 79 465 2918)

The site replaces a traditional cart/checkout with a conversational commerce model: every product button deep-links into WhatsApp with a pre-filled, product-aware message including the customer's selected size and color. This removes payment gateway friction entirely while keeping the brand personal.

---

✨ Feature Breakdown

Core Commerce

· 11 products across 4 categories (Tops, Bottoms, Gear, Cyber)
· Dynamic size selectors — chips auto-switch between Size and Waist labels based on the product's data
· Color swatches rendered per-product from data attributes
· Live stock indicators — "Only X left" badges appear automatically when stock ≤ 5
· Category filter bar with aria-pressed state and auto-hiding empty sections
· WhatsApp deep links that rebuild the message on every option change

Brand & Content

· Animated glitch headline (PSYCHO APPLE) with RGB-split pseudo-elements
· Infinite marquee ticker — PSYCHO APPLE // 021 // LUCID DROP // COLD NIGHTS
· Live countdown timer to the next drop (auto-switches to LIVE NOW)
· Featured Drop banner with animated conic-gradient border flow
· Cyberpunk collection with scanline hover, clipped corners, and flicker art
· Size guide with two tables (tops in cm, denims in inches/cm)

Visual Systems

· Floating glass bubbles rising through the background
· Dual orbiting edge lights — purple and pink tracing the viewport perimeter
· Cursor glow follower (desktop only)
· 3D tilt on product cards (desktop only, pointer-fine)
· Scanline CRT overlay across the whole page
· Scroll progress bar with gradient fill
· Floating WhatsApp FAB with ping animation

---

🎨 Design System

Color Tokens

```css
--bg:          #09030f   /* Deep void background        */
--bg-raised:   #140822   /* Card / surface elevation    */
--text:        #f5eeff   /* Primary text                */
--text-dim:    #a395b5   /* Secondary / muted text      */
--accent:      #b53cff   /* Neon purple — primary       */
--accent-dim:  #6a1b80   /* Purple shadow tone          */
--accent-alt:  #ff3cba   /* Neon pink — secondary       */
--line:        #2d1844   /* Borders and dividers        */
```

Cyberpunk section adds local tokens:

```css
--cy: #00f0ff   /* Cyan    */
--mg: #ff2bd6   /* Magenta */
--yl: #fcee09   /* Yellow  */
```

Typography

Family Role Weights
Big Shoulders Display Headlines, product names, numerals 600–900
Rubik Wet Paint Drippy brand marks, emblems 400
Work Sans Body, buttons, UI 400–900

All three load from Google Fonts with preconnect hints for speed.

Spacing & Layout

· Container max-width: 1200px
· Fluid padding via clamp(20px, 5vw, 56px)
· Product grid: repeat(auto-fill, minmax(260px, 1fr))
· Mobile grid collapses to minmax(160px, 1fr) under 720px

---

🏗 Architecture

```
Psycho_Apple_V6.html
│
├── <head>
│   ├── Meta (SEO + Open Graph + Twitter card)
│   ├── Inline SVG favicon (data URI)
│   ├── Google Fonts preconnect + stylesheet
│   └── <style>  ← entire design system (~1,200 lines)
│
├── <body>
│   ├── Ambient layers (bubbles, progress, orbs, glow, edge SVG)
│   ├── <header>  — sticky, blurred, social + WhatsApp CTA
│   ├── <main>
│   │   ├── Hero (emblem + glitch wordmark + tagline)
│   │   ├── Ticker marquee
│   │   ├── Featured Drop banner + countdown
│   │   ├── Filter bar
│   │   ├── Main product grid (5 items)
│   │   ├── Gear grid (6 items)
│   │   ├── Cyberpunk grid (4 items)
│   │   ├── Perks row (3 cards)
│   │   └── Size guide (2 tables)
│   ├── FAB (floating WhatsApp button)
│   ├── <footer>  — brand + contact + copyright
│   └── 3 × <script> blocks
│       ├── Scroll progress, reveal observer, cursor glow, 3D tilt
│       ├── Social links, countdown, size/color builders, filters
│       └── Bubble generator
```

Data-Driven Products

Each product card declares its own configuration via data-* attributes:

```html
<article class="product-card"
         data-name="Heavyweight Hoodie"
         data-cat="tops"
         data-sizes="S,M,L,XL,XXL"
         data-colors="Black:#141414|Purple:#6a1b80|Ash:#8a8795"
         data-stock="9">
```

The script parses these at runtime and injects the appropriate controls — no hardcoded UI per product.

---

🛍 Product Catalog

Tops

Product Price Sizes Stock
Heavyweight Hoodie R550 S–XXL 9
Graphic T-Shirt R250 S–XXL 4 ⚠
Flannel Shirt R350 S–XXL 9

Bottoms

Product Price Sizes Stock
Utility Joggers R450 S–XXL 12
Street Denims R600 W28–38 6

Gear

Product Price Stock
Ribbed Beanie R180 15
Snapback Cap R220 10
Bucket Hat R250 7
Canvas Tote R200 11
Crew Socks (3-pack) R90 14
Bomber Jacket R850 3 ⚠

Cyber Collection

Product Code Price
Neon Circuit Jacket ARMOR-01 R950
Cyber Cargo Pants ARMOR-02 R650
Data-Stream Hoodie ARMOR-03 R600
Glitch Tee ARMOR-04 R300

---

💬 WhatsApp Order Flow

Every order button reconstructs a deep link on the fly. Example output:

```
Hi! I'd like to order the Heavyweight Hoodie in size L, Purple. Is it available?
```

This is encoded via encodeURIComponent and routed to:

```
https://wa.me/27794652918?text=<encoded message>
```

Advantages of this model:

· Zero payment infrastructure
· Zero PCI compliance burden
· Personal, human touch at the point of sale
· Works with WhatsApp Business automation if desired
· Conversion-friendly on mobile (no app switching friction)

Where it's wired:

1. Header CTA button
2. Featured Drop banner
3. Every standard product button
4. Every cyberpunk card
5. Floating action button (FAB)
6. Footer contact link

---

⚙️ Interactive Systems

Product Option Builder

```js
group(label, cls, items, pick, isSwatch)
```

A reusable function that builds either:

· Chips for sizes (S M L XL XXL or 28 30 32 34 36 38)
· Swatches for colors (circular buttons with hex fills)

The label auto-detects numeric-first options and renames itself from Size to Waist.

Category Filter

```js
bar.addEventListener('click', ...)
```

· Sets aria-pressed on the active button
· Toggles hidden on every .product-card and .cy-card
· Auto-hides entire <section> blocks whose visible children reach zero

Drop Countdown

```js
var DROP = new Date('2026-10-09T18:00:00+02:00').getTime();
```

· Renders Drops Sat 9 Oct, 18:00 in Cape Town time
· Updates every second
· Replaces itself with LIVE NOW when the target passes

Stock Badges

```js
if (stock > 0 && stock <= 5) { ... }
```

Automatically appends an "Only X left" pill to the product image wrapper.

3D Tilt (Desktop Only)

Uses pointermove to compute normalized cursor offset and applies:

```js
transform: perspective(700px) rotateY(x*10deg) rotateX(-y*10deg)
```

Gated behind matchMedia('(hover:hover) and (pointer:fine)').

---

🎞 Animation & Motion

Effect Technique Reduced-motion fallback
Hero fade-up @keyframes fadeUp Disabled
Emblem float @keyframes floaty Disabled
Orb drift @keyframes drift Disabled
Ticker scroll @keyframes marquee Disabled
Edge light chase SVG stroke-dasharray + stroke-dashoffset Hidden entirely
Banner border flow Animated background-position Disabled
Glitch headline Dual pseudo-element clip-path Animations off
Reveal on scroll IntersectionObserver + class toggle Elements start visible
Cursor glow Inline left/top style writes Hidden on touch
Bubble rise @keyframes rise with CSS vars Disabled
FAB ping @keyframes ping Disabled
Cyber scanline @keyframes scan Disabled
Cyber flicker @keyframes flicker Disabled

Every animation is wrapped in @media (prefers-reduced-motion: no-preference) where possible, with an explicit reduce block that neutralizes the effect.

---

♿ Accessibility

· Semantic HTML — <header>, <main>, <section>, <article>, <footer>, <nav>
· ARIA roles — radiogroup on size/color rows, radio on chips/swatches
· aria-pressed on filter buttons
· aria-label on all icon-only controls (social, FAB, swatches)
· aria-hidden="true" on all decorative layers (bubbles, edge SVG, orbs, ticker)
· Focus-visible rings — 2px solid var(--accent) with offset
· Keyboard navigable — every interactive element is a real <button> or <a>
· hidden attribute used for actual display removal (not just visual hiding)
· Color contrast — body text on #09030f exceeds WCAG AA at all sizes

---

⚡ Performance

· No JS frameworks — vanilla only, three small IIFEs
· No external images — every visual is inline SVG or CSS gradient
· Fonts preconnected to fonts.googleapis.com and fonts.gstatic.com
· font-display: swap via Google Fonts default
· Passive scroll listener — addEventListener('scroll', prog, {passive:true})
· IntersectionObserver for reveal animations (unobserves after firing)
· Animation budget — GPU-friendly transforms and opacity only
· Bubble count adapts to device: 9 on mobile / low-core, 22 on desktop
· will-change: transform applied only to product cards
· CRT scanline uses a single fixed pseudo-element, not repeated DOM

Estimated Lighthouse: 95+ Performance / 100 Best Practices on a cold cache.

---

🔧 Customization Guide

Change the WhatsApp number

Search for 27794652918 (appears in every href and in the NUM constant) and replace globally.

```js
var NUM = '27794652918';  // ← change this
```

Change the drop date

```js
var DROP = new Date('2026-10-09T18:00:00+02:00').getTime();
```

Add a product

Copy any <article class="product-card"> block and edit the data attributes:

```html
<article class="product-card reveal"
         data-name="Your Product"
         data-cat="tops"
         data-sizes="S,M,L,XL"
         data-colors="Black:#141414|Purple:#6a1b80"
         data-stock="8">
  <div class="product-img-wrap">
    <svg class="art" ...>...</svg>
    <span class="tag">NEW</span>
  </div>
  <div class="product-info">
    <h3 class="product-name">Your Product</h3>
    <div class="product-price">From R000.00</div>
    <div class="product-sizes">Sizes: S – XL</div>
    <a class="product-btn" href="...">Order via WhatsApp</a>
  </div>
</article>
```

The script wires up sizes, colors, stock, and the WhatsApp message automatically.

Rebrand the color scheme

Edit the :root block. All UI surfaces inherit automatically:

```css
:root {
  --accent: #b53cff;       /* → your primary */
  --accent-alt: #ff3cba;   /* → your secondary */
  --bg: #09030f;           /* → your base */
}
```

Enable social links

In the second <script> block:

```js
var SOCIAL = {
  instagram: 'https://instagram.com/yourhandle',
  facebook:  'https://facebook.com/yourpage'
};
```

The header icons are hidden by default and reveal once URLs are set.

Swap fonts

Replace the Google Fonts <link> and update the three font-family references: .display, .drippy, and body.

---

🚀 Deployment

Option 1 — GitHub Pages

```bash
git add Psycho_Apple_V6.html
git commit -m "Deploy storefront"
git push origin main
```

Then enable Pages → deploy from main → root.

Option 2 — Netlify / Vercel

Drag the folder into the dashboard. No build command needed.

Option 3 — Any static host

Upload the file. It's one HTML document. That's it.

Option 4 — Local preview

```bash
python3 -m http.server 8000
# or
npx serve .
```

Note: Because everything is inline, you can also just double-click the file. But the Google Fonts request needs internet access to render correctly.

---

🌐 Browser Support

Browser Support
Chrome / Edge 105+ ✅ Full
Firefox 110+ ✅ Full
Safari 16+ ✅ Full
Mobile Safari iOS 16+ ✅ Full
Mobile Chrome Android 12+ ✅ Full
IE 11 ❌ Not supported (uses color-mix, clamp, :focus-visible)

Key modern features used:

· color-mix(in srgb, ...) — header backdrop
· clamp() — fluid typography throughout
· :focus-visible — accessibility outlines
· IntersectionObserver — reveal animations
· SVG pathLength — edge-light chase math

All are safe in evergreen browsers.

---

⚠️ Known Limitations

1. No persistent cart — each product is a one-off WhatsApp conversation
2. Stock counts are static — no backend to decrement them
3. No payment processing — by design; WhatsApp handles the transaction
4. Drop countdown is hardcoded — must be edited in source to change
5. Product images are SVG art — no real photography layer yet
6. No analytics — no tracking script is included
7. No search — filter bar is the only discovery mechanism
8. Single language — English only (ZA)
9. aria-live missing on the countdown — screen readers won't announce tick updates
10. Social links are stubs — SOCIAL object ships empty

---

🗺 Roadmap

Potential V7 directions:

☐ Product modal — click a card to open a detail view with size chart, shipping, and gallery
☐ Persistent cart via localStorage that compiles a multi-item WhatsApp message
☐ Real photography support — swap SVG art for <img> with lazy loading
☐ CMS integration (Sanity, Contentful) so the catalog updates without editing HTML
☐ Inventory sync — read stock from a Google Sheet or Airtable
☐ PWA manifest — installable on mobile, offline-capable
☐ Analytics events — track filter clicks and order-button taps
☐ Multi-currency — ZAR / USD / GBP toggle
☐ Email capture — drop notifications via Mailchimp/ConvertKit
☐ aria-live="polite" on the countdown for screen readers
☐ Internationalization — Afrikaans / isiXhosa language toggle
☐ Lookbook section — editorial photography with shoppable hotspots

---

🙏 Credits & License

Brand: Psycho Apple — Cape Town, South Africa
Design & Build: Custom, single-file architecture
Fonts: Big Shoulders Display, Rubik Wet Paint, Work Sans — all SIL Open Font License
Aesthetic inspiration: Juice WRLD's Lucid Dreams era, Cape Town night culture, 90s CRT cyberpunk

Contact:

· WhatsApp: +27 79 465 2918
· Email: Jeandreamerica49@gmail.com
· Location: Cape Town, South Africa

---

"Legendary never dies."
— Psycho Apple, CPT 021

All rights reserved © 2026 Psycho Apple.
