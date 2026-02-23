# Landing Page Dra. Tayrine Duarte - Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task. Use @frontend-design for all HTML/CSS tasks.

**Goal:** Build a static, SEO-optimized landing page for Dra. Tayrine Duarte's medical practice focused on skin, hair, and nail care in Marabá/PA.

**Architecture:** Single-page HTML + Tailwind CSS (CLI build) with CSS-only carousel (scroll-snap), minimal vanilla JS for mobile menu and carousel arrows. Hosted on GitHub Pages with automated deploy via GitHub Actions.

**Tech Stack:** HTML5, Tailwind CSS 3.x (CLI), Vanilla JS, Google Fonts (Cormorant Garamond + Inter), GitHub Actions

**Design doc:** `docs/plans/2026-02-23-landing-page-design.md`

**CRITICAL CONSTRAINT:** Never use "dermatologista", "dermatologia" or similar terms anywhere in the site. Use "cuidados com pele, cabelos e unhas" instead.

---

### Task 1: Project Scaffolding

**Files:**
- Create: `package.json`
- Create: `tailwind.config.js`
- Create: `src/input.css`
- Create: `.gitignore`

**Step 1: Initialize package.json**

```json
{
  "name": "landing-page-tayrine-duarte",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "build": "npx @tailwindcss/cli -i ./src/input.css -o ./dist/output.css --minify",
    "watch": "npx @tailwindcss/cli -i ./src/input.css -o ./dist/output.css --watch"
  },
  "devDependencies": {
    "@tailwindcss/cli": "^4"
  }
}
```

**Step 2: Create tailwind.config.js with custom palette**

Note: Tailwind v4 uses CSS-based configuration in the input.css file, not a JS config file. The custom colors, fonts, and theme extensions go directly in `src/input.css` using `@theme`.

**Step 3: Create src/input.css**

```css
@import "tailwindcss";

@theme {
  --color-rose-gold: #C4956A;
  --color-blush: #F5E6E0;
  --color-warm-white: #FEFAF8;
  --color-warm-gray: #3D3530;
  --color-muted: #8B7E74;
  --color-accent: #D4A088;

  --font-heading: "Cormorant Garamond", serif;
  --font-body: "Inter", sans-serif;
}
```

**Step 4: Create .gitignore**

```
node_modules/
dist/
.DS_Store
```

**Step 5: Install dependencies and do first build**

Run: `npm install`
Run: `npm run build`
Expected: `dist/output.css` is generated.

**Step 6: Commit**

```bash
git add package.json src/input.css .gitignore
git commit -m "Scaffolding: Tailwind CSS, paleta customizada, scripts de build"
```

---

### Task 2: Optimize Images

**Files:**
- Input: `imagens/*.jpg`
- Output: `imagens/` (optimized WebP + resized JPG originals)

**Step 1: Install image tools**

Run: `brew install webp` (if not already installed — cwebp command)

**Step 2: Rename files to clean names (remove .jpg.jpg double extension)**

```bash
cd imagens
mv foto_perfil_tayrine.jpg.jpg perfil.jpg
mv foto_carrosel_1.jpg.jpg carrossel-1.jpg
mv foto_carrosel_2.jpg.jpg carrossel-2.jpg
mv foto_carrossel_3.jpg.jpg carrossel-3.jpg
mv foto_carrossel_4.jpg.jpg carrossel-4.jpg
mv foto_carrossel_5.jpg.jpg carrossel-5.jpg
```

**Step 3: Convert to WebP (quality 80, resize to max 1200px wide for carousel, 800px for profile)**

```bash
# Profile photo
cwebp -q 80 -resize 800 0 perfil.jpg -o perfil.webp

# Carousel photos
for i in 1 2 3 4 5; do
  cwebp -q 80 -resize 1200 0 carrossel-$i.jpg -o carrossel-$i.webp
done

# Logo (keep small, high quality)
cwebp -q 90 logo_tayrine.jpg -o logo.webp
```

**Step 4: Create an OG image (copy profile as og-image.jpg, 1200x630 if possible)**

Use the profile photo as base for social sharing.

**Step 5: Verify file sizes are reasonable**

Run: `ls -lh imagens/*.webp`
Expected: Each file < 200KB ideally, profile < 100KB.

**Step 6: Commit**

```bash
git add imagens/
git commit -m "Otimizar imagens: renomear, converter para WebP"
```

---

### Task 3: HTML Base Structure + Head (SEO)

**Files:**
- Create: `index.html`

**Step 1: Write the HTML head with all SEO meta tags**

The `<head>` must include:
- charset, viewport
- `<title>`: "Dra. Tayrine Duarte | Cuidados com Pele, Cabelos e Unhas em Marabá/PA"
- meta description: "Dra. Tayrine Duarte - Médica em Marabá/PA especializada em cuidados com pele, cabelos e unhas. Avaliação de pele, tratamento de acne, saúde capilar. Agende sua consulta!"
- canonical URL: https://tayrineduarte.com.br
- Open Graph tags (og:title, og:description, og:image, og:url, og:type, og:locale)
- Twitter card meta
- Google Fonts preconnect + link (Cormorant Garamond 400,600 + Inter 400,500,600)
- `<link>` to dist/output.css
- Preload hero image
- favicon (use logo)
- Theme color meta

**Step 2: Write Schema.org JSON-LD in a `<script type="application/ld+json">`**

```json
{
  "@context": "https://schema.org",
  "@type": "MedicalBusiness",
  "name": "Dra. Tayrine Duarte",
  "description": "Cuidados especializados com pele, cabelos e unhas em Marabá/PA",
  "url": "https://tayrineduarte.com.br",
  "telephone": "+5594984336074",
  "email": "consultorio@tayrineduarte.com.br",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "Av. Nagib Mutran, 229",
    "addressLocality": "Marabá",
    "addressRegion": "PA",
    "addressCountry": "BR",
    "postalCode": "68501-087"
  },
  "geo": {
    "@type": "GeoCoordinates",
    "latitude": "-5.3553",
    "longitude": "-49.1321"
  },
  "image": "https://tayrineduarte.com.br/imagens/perfil.webp",
  "sameAs": [
    "https://www.instagram.com/tayrineduartee"
  ],
  "medicalSpecialty": "Skin, Hair and Nail Care"
}
```

**Step 3: Write the `<body>` skeleton with empty sections**

```html
<body class="bg-warm-white font-body text-warm-gray">
  <header id="header"><!-- Task 4 --></header>
  <main>
    <section id="hero"><!-- Task 5 --></section>
    <section id="sobre"><!-- Task 6 --></section>
    <section id="servicos"><!-- Task 7 --></section>
    <section id="galeria"><!-- Task 8 --></section>
    <section id="localizacao"><!-- Task 9 --></section>
    <section id="contato"><!-- Task 10 --></section>
  </main>
  <footer><!-- Task 10 --></footer>
  <!-- WhatsApp float: Task 11 -->
</body>
```

**Step 4: Build and open in browser**

Run: `npm run build`
Open `index.html` in browser — should show blank page with warm-white background.

**Step 5: Commit**

```bash
git add index.html
git commit -m "HTML base: estrutura, meta tags SEO, Schema.org JSON-LD"
```

---

### Task 4: Header/Nav Section

**Files:**
- Modify: `index.html` (header section)

**Step 1: Build the header**

Structure:
- Sticky header (`sticky top-0 z-50`) with blur background
- Logo left (link to top)
- Nav links center: Sobre | Serviços | Galeria | Localização (hidden on mobile)
- WhatsApp CTA button right (hidden on mobile, visible md+)
- Hamburger button (visible only on mobile)

Anchor links use smooth scroll: `<a href="#sobre">Sobre</a>`

Design notes:
- Background: `bg-warm-white/90 backdrop-blur-sm`
- Border bottom: subtle `border-b border-blush`
- Logo image: `h-12` on mobile, `h-14` on desktop
- Nav links: `text-muted hover:text-rose-gold transition-colors` font-body
- CTA: `bg-rose-gold text-white rounded-full px-6 py-2 hover:bg-accent`
- Add `scroll-smooth` to `<html>` tag

**Step 2: Build and verify in browser**

Run: `npm run build`
Verify: Header visible, sticky on scroll, links present.

**Step 3: Commit**

```bash
git add index.html
git commit -m "Header: nav sticky com logo, links âncora e CTA WhatsApp"
```

---

### Task 5: Hero Section

**Files:**
- Modify: `index.html` (hero section)

**Step 1: Build the hero**

Structure — two-column layout on desktop, stacked on mobile:
- Left column: text content
  - H1: "Cuidados especializados para sua pele, cabelos e unhas"
  - Subtitle p: "Atendimento humanizado e personalizado em Marabá/PA"
  - CTA button: "Agende sua Consulta" → WhatsApp link
- Right column: profile photo in a rounded/soft shape
  - `<picture>` element with WebP source + JPG fallback
  - Decorative blush circle behind the photo

Design notes:
- Section padding: `py-16 md:py-24`
- Grid: `grid md:grid-cols-2 gap-12 items-center`
- H1: `font-heading text-4xl md:text-5xl lg:text-6xl text-warm-gray leading-tight`
- Subtitle: `text-muted text-lg md:text-xl mt-4`
- CTA: large green WhatsApp-style button OR rose-gold button
- Photo: `rounded-2xl shadow-lg` with a soft blush background accent shape
- Preloaded image (already in head)

**Step 2: Build and verify**

Run: `npm run build`
Verify: Hero shows with photo, text, and CTA. Check mobile and desktop widths.

**Step 3: Commit**

```bash
git add index.html
git commit -m "Hero: seção principal com foto, headline e CTA"
```

---

### Task 6: Sobre (About) Section

**Files:**
- Modify: `index.html` (sobre section)

**Step 1: Build the about section**

Structure:
- Section heading: "Sobre a Dra. Tayrine"
- Two columns: photo left (one of the carousel photos showing her in consultation), text right
- Bio text (from design doc — properly accented PT-BR)
- Formation badges/tags: "Medicina - Univ. São Lucas/RO" and "Pós-graduação em Saúde da Pele - ISMD/BH"
- CRM tag: "CRM 17.693/PA"

Design notes:
- Background: `bg-blush/30` to create visual separation
- Section heading: `font-heading text-3xl md:text-4xl text-center mb-12`
- Decorative line under heading (thin rose-gold line)
- Photo: use `carrossel-1.webp` (Tayrine in consultation with tablet)
- Text: `text-warm-gray leading-relaxed text-lg`
- Tags: `inline-block bg-white rounded-full px-4 py-1 text-sm text-muted border border-blush`

**Step 2: Build and verify**

Run: `npm run build`
Verify: About section renders correctly with photo and bio.

**Step 3: Commit**

```bash
git add index.html
git commit -m "Sobre: seção com bio, formação e CRM"
```

---

### Task 7: Serviços (Services) Section

**Files:**
- Modify: `index.html` (servicos section)

**Step 1: Build the services section**

Structure:
- Section heading: "Nossos Serviços" or "Como Posso Cuidar de Você"
- 4 cards in a 2x2 grid (stacked on mobile, 2-col on md, 4-col on lg)
- Each card: SVG icon + title + description text

Cards:
1. **Avaliação de Pele e Mapeamento de Sinais** — icon: magnifying glass / shield
   > "Avaliação completa da saúde da sua pele com dermatoscopia digital. Identificamos e acompanhamos pintas, manchas e lesões para prevenção e diagnóstico precoce."

2. **Tratamento de Acne e Cicatrizes** — icon: sparkles / face
   > "Protocolos personalizados para tratar acne ativa e suavizar cicatrizes. Devolvemos a saúde e a autoestima da sua pele com tratamentos seguros e eficazes."

3. **Saúde Capilar** — icon: hair/head silhouette
   > "Cuidamos do seu couro cabeludo e cabelos. Investigamos causas de queda capilar e problemas do couro cabeludo com abordagem clínica completa."

4. **Cuidados com as Unhas** — icon: hand/nail
   > "Diagnóstico e tratamento de alterações nas unhas, como micoses, fragilidade e deformidades. Saúde das unhas também é saúde!"

Design notes:
- Background: `bg-warm-white`
- Cards: `bg-white rounded-2xl p-8 shadow-sm hover:shadow-md transition-shadow border border-blush/50`
- Icon: inline SVG, `text-rose-gold w-10 h-10 mb-4`
- Title: `font-heading text-xl text-warm-gray mb-3`
- Description: `text-muted leading-relaxed`
- Use simple, elegant SVG icons (inline, no icon library needed)

**Step 2: Build and verify**

Run: `npm run build`
Verify: 4 cards visible, responsive grid works on mobile/desktop.

**Step 3: Commit**

```bash
git add index.html
git commit -m "Serviços: 4 cards com ícones, títulos e descrições"
```

---

### Task 8: Carrossel (Gallery) Section

**Files:**
- Modify: `index.html` (galeria section)

**Step 1: Build the CSS scroll-snap carousel**

Structure:
- Section heading: "Conheça o Consultório"
- Horizontal scrollable container with scroll-snap
- 5 photos with `<picture>` (WebP + JPG fallback)
- Navigation arrows (left/right) — styled buttons
- Dot indicators below

```html
<div class="relative">
  <div id="carousel" class="flex overflow-x-auto snap-x snap-mandatory gap-4 scrollbar-hide">
    <div class="snap-center shrink-0 w-[85%] md:w-[45%] lg:w-[30%]">
      <picture>
        <source srcset="imagens/carrossel-1.webp" type="image/webp">
        <img src="imagens/carrossel-1.jpg" alt="Dra. Tayrine em consulta"
             class="rounded-2xl w-full h-80 object-cover" loading="lazy">
      </picture>
    </div>
    <!-- repeat for 2-5 -->
  </div>
  <!-- arrows -->
  <button id="prev" class="absolute left-2 top-1/2 ...">←</button>
  <button id="next" class="absolute right-2 top-1/2 ...">→</button>
</div>
```

Design notes:
- Background: `bg-blush/20`
- Container: `overflow-x-auto snap-x snap-mandatory` with `-webkit-overflow-scrolling: touch`
- Each item: `snap-center` for centered snapping
- Hide scrollbar: add `.scrollbar-hide` utility in input.css
- Arrows: `bg-white/80 backdrop-blur-sm rounded-full w-10 h-10 shadow-md`
- Images: `rounded-2xl object-cover h-80`
- All images have descriptive `alt` text for SEO

**Step 2: Add scrollbar-hide utility to src/input.css**

```css
@utility scrollbar-hide {
  -ms-overflow-style: none;
  scrollbar-width: none;
  &::-webkit-scrollbar {
    display: none;
  }
}
```

**Step 3: Build and verify**

Run: `npm run build`
Verify: Carousel scrolls horizontally, snap works, images load.

**Step 4: Commit**

```bash
git add index.html src/input.css
git commit -m "Galeria: carrossel CSS scroll-snap com 5 fotos"
```

---

### Task 9: Localização (Location) Section

**Files:**
- Modify: `index.html` (localizacao section)

**Step 1: Build the location section**

Structure:
- Section heading: "Localização"
- Two columns: info left, map right
- Left: address, phone, email, hours (if applicable)
- Right: Google Maps iframe embed
- "Como chegar" link that opens Google Maps directions

```html
<iframe
  src="https://www.google.com/maps/embed?pb=!1m18!..."
  width="100%" height="400" style="border:0;"
  allowfullscreen="" loading="lazy"
  referrerpolicy="no-referrer-when-downgrade"
  title="Localização do consultório da Dra. Tayrine Duarte em Marabá/PA">
</iframe>
```

Note: Need to get the actual Google Maps embed URL for "Av. Nagib Mutran 229, Cidade Nova, Marabá PA".

Design notes:
- Background: `bg-warm-white`
- Map: `rounded-2xl overflow-hidden shadow-sm`
- Address text: icon + text pairs
- "Como chegar" button: outline style `border-rose-gold text-rose-gold`

**Step 2: Build and verify**

Run: `npm run build`
Verify: Map loads (lazy), address info displays correctly.

**Step 3: Commit**

```bash
git add index.html
git commit -m "Localização: mapa Google Maps e informações de endereço"
```

---

### Task 10: CTA Final + Footer

**Files:**
- Modify: `index.html` (contato section + footer)

**Step 1: Build the CTA section**

Structure:
- Full-width background with blush/rose gradient
- H2: "Cuide da Saúde da Sua Pele, Cabelos e Unhas"
- Subtitle: "Agende sua consulta e receba um atendimento personalizado"
- Large WhatsApp CTA button: "Falar com a Dra. Tayrine pelo WhatsApp"

Design notes:
- Background: gradient `from-blush to-warm-white`
- Centered text, generous padding `py-20`
- Button: large, `bg-[#25D366] text-white rounded-full px-8 py-4 text-lg font-semibold shadow-lg hover:shadow-xl`
- WhatsApp icon inline SVG in button

**Step 2: Build the footer**

Structure:
- Dark warm background `bg-warm-gray`
- Three columns: Logo + tagline | Contact info | Quick links
- Bottom bar: CRM + copyright

Design notes:
- Logo: white version or with `brightness` filter
- Links: `text-blush/70 hover:text-blush`
- Instagram link with icon
- "CRM 17.693/PA" clearly visible
- Copyright: "© 2026 Dra. Tayrine Duarte. Todos os direitos reservados."

**Step 3: Build and verify**

Run: `npm run build`
Verify: CTA section prominent, footer complete with all info.

**Step 4: Commit**

```bash
git add index.html
git commit -m "CTA final e footer com contatos e CRM"
```

---

### Task 11: WhatsApp Floating Button + JS

**Files:**
- Modify: `index.html` (before closing body)

**Step 1: Add WhatsApp floating button HTML**

```html
<a href="https://wa.me/5594984336074?text=Olá! Gostaria de agendar uma consulta com a Dra. Tayrine."
   target="_blank" rel="noopener noreferrer"
   aria-label="Fale conosco pelo WhatsApp"
   class="fixed bottom-6 right-6 z-50 bg-[#25D366] text-white rounded-full w-14 h-14 flex items-center justify-center shadow-lg hover:shadow-xl hover:scale-110 transition-all animate-pulse-slow">
  <!-- WhatsApp SVG icon -->
</a>
```

**Step 2: Add pulse animation to input.css**

```css
@utility animate-pulse-slow {
  animation: pulse-slow 3s ease-in-out infinite;
}

@keyframes pulse-slow {
  0%, 100% { box-shadow: 0 0 0 0 rgba(37, 211, 102, 0.4); }
  50% { box-shadow: 0 0 0 12px rgba(37, 211, 102, 0); }
}
```

**Step 3: Add inline JS for carousel arrows and mobile menu**

```html
<script>
  // Mobile menu toggle
  const menuBtn = document.getElementById('menu-btn');
  const mobileMenu = document.getElementById('mobile-menu');
  if (menuBtn && mobileMenu) {
    menuBtn.addEventListener('click', () => {
      mobileMenu.classList.toggle('hidden');
    });
    // Close menu on anchor link click
    mobileMenu.querySelectorAll('a').forEach(link => {
      link.addEventListener('click', () => mobileMenu.classList.add('hidden'));
    });
  }

  // Carousel navigation
  const carousel = document.getElementById('carousel');
  const prevBtn = document.getElementById('prev');
  const nextBtn = document.getElementById('next');
  if (carousel && prevBtn && nextBtn) {
    const scrollAmount = carousel.offsetWidth * 0.85;
    prevBtn.addEventListener('click', () => {
      carousel.scrollBy({ left: -scrollAmount, behavior: 'smooth' });
    });
    nextBtn.addEventListener('click', () => {
      carousel.scrollBy({ left: scrollAmount, behavior: 'smooth' });
    });
  }
</script>
```

**Step 4: Build and verify**

Run: `npm run build`
Verify: WhatsApp button floats, pulses. Carousel arrows work. Mobile menu toggles.

**Step 5: Commit**

```bash
git add index.html src/input.css
git commit -m "WhatsApp flutuante, JS do carrossel e menu mobile"
```

---

### Task 12: GitHub Actions Deploy

**Files:**
- Create: `.github/workflows/deploy.yml`

**Step 1: Create the workflow**

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm ci
      - run: npm run build
      - uses: actions/upload-pages-artifact@v3
        with:
          path: .
  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - uses: actions/deploy-pages@v4
        id: deployment
```

**Step 2: Commit**

```bash
git add .github/workflows/deploy.yml
git commit -m "CI: GitHub Actions deploy para GitHub Pages"
```

---

### Task 13: Final Polish and Verification

**Files:**
- Modify: `index.html` (minor adjustments)

**Step 1: Run Tailwind build**

Run: `npm run build`

**Step 2: Open in browser and verify all sections**

Verify checklist:
- [ ] Header sticky, links work (smooth scroll)
- [ ] Hero: photo loads, CTA links to WhatsApp
- [ ] Sobre: bio text properly accented, CRM visible
- [ ] Serviços: 4 cards render, icons visible
- [ ] Galeria: carousel scrolls, arrows work
- [ ] Localização: map loads, address correct
- [ ] CTA: WhatsApp link works
- [ ] Footer: all info present, CRM visible
- [ ] WhatsApp float: visible, pulses, link works
- [ ] Mobile: hamburger menu works, all sections responsive
- [ ] No "dermatologista" or "dermatologia" anywhere in the page

**Step 3: Test mobile responsiveness**

Open browser DevTools, test at:
- 375px (iPhone SE)
- 390px (iPhone 14)
- 768px (iPad)
- 1024px (desktop)
- 1440px (large desktop)

**Step 4: Verify SEO**

- View page source: confirm `<title>`, meta description, OG tags, Schema.org JSON-LD
- Check all images have `alt` text
- Check heading hierarchy (single h1, h2 per section)

**Step 5: Final commit**

```bash
git add -A
git commit -m "Polish final: verificações de responsividade e SEO"
```

---

## Image Alt Text Reference

Use these alt texts for accessibility and SEO:

| Image | Alt Text |
|-------|----------|
| logo.webp | "Logo Dra. Tayrine Duarte - Cuidados com Pele, Cabelos e Unhas" |
| perfil.webp | "Dra. Tayrine Duarte - Médica especializada em cuidados com pele, cabelos e unhas em Marabá PA" |
| carrossel-1.webp | "Dra. Tayrine Duarte em consulta médica no consultório em Marabá" |
| carrossel-2.webp | "Dra. Tayrine realizando avaliação de couro cabeludo com dermatoscópio" |
| carrossel-3.webp | "Avaliação de pele com dermatoscopia digital no consultório" |
| carrossel-4.webp | "Exame detalhado de lesões de pele com equipamento especializado" |
| carrossel-5.webp | "Dra. Tayrine explicando camadas da pele para paciente durante consulta" |
