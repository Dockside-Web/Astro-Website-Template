# Dockside Web Solutions – Astro Website Template

Starter template for all Astro websites for Dockside Web Solutions, LLC.
Astro 7 + Tailwind 4, static output, deployed to Cloudflare Workers. Requires Node 22.22 or newer (`node -v`).

## After cloning

```
npm install
npx astro sync
```

Then in VS Code: `Ctrl+Shift+P` → **TypeScript: Restart TS Server**.

`npm run build` prints a warning listing any required fields still empty in `site.config.ts`.

---

## Things to fill in for every new website

Everything starts blank. Nothing from a previous client is left in here.

### `src/site.config.ts` — all SEO info lives in this one file

**Required**
- [ ] `name` – business name as it should show in Google
- [ ] `tagline` – homepage title becomes "Name | Tagline"
- [ ] `description` – fallback meta description (140–160 chars)
- [ ] `url` – production URL, `https://` and no trailing slash. Without this there's no sitemap, no sitemap line in robots.txt, and canonical URLs are wrong

**Branding**
- [ ] `legalName` – e.g. "Business Name, LLC"
- [ ] `themeColor` – mobile browser bar color (main brand color)
- [ ] `ogImage` + `ogImageAlt` – social share image in `/public`, 1200×630
- [ ] `logo` – square logo in `/public`, used by Google

**Business info** (Google structured data / local SEO)
- [ ] `business.type` – most specific type from https://schema.org/LocalBusiness ("Restaurant", "Plumber", "LodgingBusiness"…)
- [ ] `business.phone`, `business.email`
- [ ] `business.address` – leave `street` blank for service-area businesses with no storefront
- [ ] `business.geo` – lat/lng from Google Maps
- [ ] `business.mapUrl` – Google Maps / Business Profile link
- [ ] `business.hours`
- [ ] `business.priceRange`, `business.areaServed`

**Social** – full profile URLs
- [ ] `social.facebook`, `instagram`, `linkedin`, `x`, `youtube`, `tiktok`, `google`

**Contact form**
- [ ] `forms.basinId`
- [ ] `forms.turnstileSiteKey` (the form works without it, but has no spam check)

**Analytics / search console** (can wait until launch)
- [ ] `analytics.cloudflareToken` and/or `analytics.ga4Id`
- [ ] `verification.google`, `verification.bing`

### Other files

- [ ] `wrangler.jsonc` → `name` – same as the repo name (deploy fails until set)
- [ ] `src/styles/global.css` → brand colors (currently placeholder grays)
- [ ] **Fonts** (currently system fonts) – 3 places:
  1. `astro.config.mjs` → add the font to `fonts` (example in the comment)
  2. `src/layouts/Layout.astro` → uncomment the `Font` import and add a `<Font>` tag
  3. `src/styles/global.css` → set `--font-brand` / `--font-headers` to the font's `cssVariable`

### Components — empty shells, build per client

- [ ] `src/components/Navigation.astro`
- [ ] `src/components/Footer.astro` – if the business has an address, show name/address/phone exactly as on the Google Business Profile
- [ ] `src/components/PageHero.astro`
- [ ] `src/components/CTA.astro`

### Pages — each has an empty `<h1>` to fill in

- [ ] `src/pages/index.astro` – `metaDescription` + replace the "Hello, World!" heading
- [ ] `src/pages/contact.astro` – `metaDescription` + content
- [ ] `src/pages/404.astro` – content (hidden from Google)

### Images (`/public`)

- [ ] Favicons – generate a set from the client logo at https://realfavicongenerator.net and put `favicon.ico`, `favicon.svg`, `apple-touch-icon.png`, `icon-192.png`, `icon-512.png` in `/public`
- [ ] Social share image – then set `ogImage` in `site.config.ts`
- [ ] Page photos go in `src/assets/` (not `public/`) so Astro optimizes them. Always write real `alt` text

### Basin dashboard

- [ ] Redirect URL after submit (optional – Basin shows its own thank-you page otherwise)
- [ ] Turnstile enabled with the secret key; add the domain (and `localhost`) to the Turnstile widget in Cloudflare
- [ ] Honeypot field name → `_botvortex`
- [ ] Notification emails go to the client

### Launch

- [ ] Attach the custom domain to the Worker in Cloudflare. Pick www or non-www and redirect the other one
- [ ] Once the custom domain works, set `"workers_dev": false` in `wrangler.jsonc` so the `*.workers.dev` copy doesn't get indexed as duplicate content
- [ ] Replacing an old site? Add 301s for the old URLs in `public/_redirects`
- [ ] Google Search Console: verify, then submit `/sitemap-index.xml`
- [ ] Bing Webmaster Tools: import from Search Console
- [ ] Google Business Profile matches the site's name/address/phone; link it in `social.google`
- [ ] Test: https://search.google.com/test/rich-results, https://pagespeed.web.dev, and share a link in a text/Facebook to check the preview

---

## Every new page

- Unique `title` + `description` on `<Layout>` (homepage: no `title`)
- Exactly one `<h1>`
- Keep out of Google: `<Layout noindex>` and add the path to `sitemapExclude` in `astro.config.mjs`
- Extra structured data (FAQ, services, events…): `<Layout schema={{ "@type": "FAQPage", ... }}>`
- Extra head tags: `<Fragment slot="head">...</Fragment>`

## What's built in (no changes needed)

- **SEO tags** (`head/SEO.astro`): title, description, canonical, robots, Open Graph, Twitter cards, search console verification. Blank config values are skipped
- **Structured data** (`head/StructuredData.astro`): business + website JSON-LD on every page
- **Generated files**: `sitemap-index.xml`, `robots.txt`, `manifest.webmanifest`
- **Performance**: link prefetching, self-hosted fonts, immutable caching for `/_astro/*`
- **Security headers**: `public/_headers`
- **Accessibility**: skip link, `<main>` landmark, reduced-motion support, focus outlines
- **Scroll animations**: add `data-reveal` to any element to fade it in on scroll
- **Analytics** (`head/Analytics.astro`): Cloudflare Web Analytics / GA4, only when IDs are set
- **Contact form**: `ContactForm.astro` (Basin + Turnstile)

## Scripts

| Command | |
|---|---|
| `npm run dev` | Dev server |
| `npm run build` | Build to `dist/` |
| `npm run deploy` | Build + deploy to Cloudflare |
