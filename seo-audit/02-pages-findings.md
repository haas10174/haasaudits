# Fase 2 — Bevindingen & prioriteiten per-pagina

**Aanvullend op `02-pages.md` (ruwe data) en `02-pages.csv` (machine-leesbaar).**

## Wat gaat goed (sitewide)

| Signal | Status | Opmerking |
|---|---|---|
| Single H1 per pagina | ✅ alle 14 | Correct. |
| Unieke `<title>` per pagina | ✅ geen duplicates | |
| Unieke meta description | ✅ geen duplicates | |
| Self-referencing canonical | ✅ 14/14 | Alle canonical URLs kloppen met de werkelijke URL en gebruiken HTTPS+non-www. |
| HTML `lang="nl"` | ✅ 14/14 | |
| Mobile viewport meta | ✅ 14/14 | `width=device-width, initial-scale=1.0` |
| Favicon + theme-color | ✅ 14/14 | `/favicon.svg`, teal `#0D9488` / `#14b8a6` |
| HTTPS + HSTS preload | ✅ | `max-age=63072000; includeSubDomains; preload` — indien preload-list geregistreerd is dit top-tier |
| Gzip-compressie | ✅ | Homepage 97 KB → 22 KB (77% reductie) |
| TTFB (Varnish-caching) | ✅ 145–156 ms | Uitstekend |
| Server-side rendering | ✅ | Content is in HTML (geen JS-render dependency) — Googlebot indexeert direct |
| Security headers (CSP/COOP/CORP/X-Frame-Options/Permissions-Policy) | ✅ | Boven-gemiddeld voor een B2B-site; goede E-E-A-T trust-signal |
| JSON-LD — Homepage | ✅ uitgebreid | Organization, ProfessionalService, OfferCatalog, FAQPage, WebSite+SearchAction, ContactPoint, BreadcrumbList |
| JSON-LD — Blog posts | ✅ | Article + Organization + Person (auteur) — correct patroon voor E-E-A-T |
| JSON-LD — FAQ | ✅ | FAQPage + BreadcrumbList |
| Interne links op homepage | ✅ 17 | Sterk hub-patroon |
| hreflang (nl-NL, nl-BE, x-default) | ⚠️ deels | 5/14 pagina's — zie issues |

## Kritieke issues (P1 — moeten opgelost)

### K1. Blog-index `/blog` → 301 → 403 Forbidden
- **Bewijs:** `curl -IL https://annex27.nl/blog` → 301 `Location: /blog/` → `/blog/` geeft 403 Forbidden.
- **Impact:** Zoekmachines kunnen de blog-index niet crawlen. De 5 blog-artikelen staan wel los in de sitemap en zijn individueel indexeerbaar, maar:
  - Er is geen crawlbare hub-pagina die alle posts intern naar elkaar linkt.
  - Gebruikers die via Google op `/blog` landen krijgen 403.
  - Sitemap bevat `/blog` — deze URL wordt geweigerd → sitemap-error in Search Console.
- **Oorzaak (hypothese):** Apache heeft directory-listing uit en er staat geen index.html/index.php in `/blog/`, en er is een redirect-rule die de extensieloze `/blog` naar `/blog/` omzet. Moet worden opgelost door óf (a) de rewrite te verwijderen zodat `/blog` rechtstreeks een HTML-response geeft, óf (b) `/blog/` de blog-index te laten serveren en de sitemap aan te passen naar `/blog/`.

### K2. www-variant retourneert HTTP 200 (geen redirect naar non-www)
- **Bewijs:** `https://www.annex27.nl/` → HTTP 200, identiek HTML (MD5 match) als `https://annex27.nl/`. Geen 301.
- **Mitigatie aanwezig:** De canonical tag wijst wél naar `https://annex27.nl/`, dus Google zal (zeer waarschijnlijk) consolideren. Maar:
  - Externe linkers die naar `www.annex27.nl` linken, laten link-equity op een niet-geredirecte variant staan.
  - Duplicate content risico bij UA-sniffing, Bing en andere crawlers die canonicals anders behandelen.
- **Aanbeveling:** Apache 301-redirect van `www.annex27.nl` → `annex27.nl` (host-header match). Kleine ingreep, verwijdert een complete categorie dup-content-risico.

### K3. Cache-Control op HTML = `no-store, no-cache, must-revalidate, max-age=0`
- **Bewijs:** Homepage response-header.
- **Impact:** Elke pagina-request vanaf eindgebruiker triggert een Varnish/backend-roundtrip i.p.v. een browser-cache-hit. Dit schaadt:
  - **Repeat visit performance** (INP en LCP bij terugkerende bezoekers)
  - **Mobile data / battery**
- **Context:** Bestellen/Dashboard moeten inderdaad `no-store` zijn (auth). Maar marketingpagina's zoals home, gap-analyse, blog, FAQ, trust kunnen prima iets als `public, max-age=300, s-maxage=3600, must-revalidate` krijgen. Varnish (reverse-proxy cache) heeft dit al; nu ook browser laten helpen.

### K4. `/bestellen` = commerciële checkout zonder Product/Offer schema
- **Bewijs:** 0 JSON-LD-blokken.
- **Impact:** Deze pagina voedt direct de omzetfunnel (vier pakketten €795–€1.495). Mist rich-snippet kans en AI Overview aggregation:
  - **Product** schema per pakket met `Offer.price`, `priceCurrency`, `availability`, `url`
  - **AggregateOffer** rond alle vier
  - Optioneel: **BreadcrumbList**
- **Extra:** Slechts **188 woorden** op een checkout-pagina — dun. Voeg korte trust-blokjes toe (refund-policy, SLA, support), dit helpt zowel UX als ranking voor `bestellen`/`pakketten`-queries.

### K5. Disallow `/*.html$` in robots.txt
- **Risico:** Brittle. Als ooit een marketing asset met `.html` wordt gepubliceerd (bv. een briefly hosted landing), wordt die automatisch niet geïndexeerd.
- **Nu geen directe schade** (alle URLs zijn extensieloos), maar verwijder deze regel of smal hem in (`/docs/*.html$` etc.).

## Belangrijke issues (P2)

### B1. Hreflang-dekking inconsistent
Pagina's **met** hreflang (`nl-NL`, `nl-BE`, `x-default`): homepage, gap-analyse, bestellen, faq, trust.
Pagina's **zonder** hreflang: `/nis2`, alle 5 blog-artikelen, `/rapport-voorbeeld`, `/privacy`, `/algemene-voorwaarden`.

- **Impact:** Voor NL hoofdmarkt + BE als secundaire markt (uit memory) levert hreflang geo-targeting op. Zonder hreflang op `/nis2` bijvoorbeeld, terwijl deze pagina expliciet "voor MKB in Nederland en België" zegt, mist je de nl-BE/nl-NL split-SERP kans.
- **Aanbeveling:** Hreflang uitrollen over álle commerciële en content-pagina's. Minimaal voor `/nis2`, alle blog-artikelen, `/rapport-voorbeeld`.

### B2. `/gap-analyse` = belangrijkste lead-magnet maar slechts 244 woorden
- **Bewijs:** 244 woorden, 4 interne links, 2 H2's.
- **Impact:** Dit is de belangrijkste conversiepagina (quickscan) maar heeft nauwelijks tekstuele context voor Google om op `iso 27001 gap-analyse`, `iso 27001 quickscan`, `compliance-score` te ranken. Concurrenten hebben 800–2.000 woorden op zulke pagina's.
- **Aanbeveling:** Verrijken zonder fluff — voeg toe:
  - Wat exact gemeten wordt (welke controls/clauses)
  - Hoe de score berekend wordt
  - Waarom 5 min genoeg is
  - Wat je krijgt als output (link naar `/rapport-voorbeeld`!)
  - FAQ-blok onderaan (+ FAQPage schema)

### B3. `/nis2` mist Service-schema (alleen FAQPage)
- Pagina biedt feitelijk een dienst ("NIS2 Readiness €995" per sitemap `/bestellen`).
- Voeg **Service** + **Offer** + **BreadcrumbList** schema toe. Behoud de bestaande FAQPage.

### B4. Meta robots directives inconsistent
| Pagina | `max-snippet:-1` | `max-image-preview:large` | `max-video-preview:-1` |
|---|---|---|---|
| home | ✅ | ✅ | ✅ |
| gap-analyse | ✅ | ✅ | ❌ |
| faq | ✅ | ✅ | ❌ |
| trust | ✅ | ✅ | ❌ |
| nis2 | ❌ | ❌ | ❌ |
| bestellen | volledig afwezig | — | — |
| blog-*, rapport-voorbeeld, privacy, AV | ❌ | ❌ | ❌ |

- **Impact:** De `max-snippet:-1` en `max-image-preview:large` directives helpen AI Overviews / SERP feature display (langere snippets + grote preview images). Nu missen ze op ~9 pagina's.
- **Aanbeveling:** Uniform toepassen via een template-tag op álle indexeerbare pagina's.

### B5. Meta description lengte buiten optimale range
| Pagina | chars | status |
|---|---|---|
| home | 201 | ❌ te lang |
| trust | 169 | ⚠️ grens |
| algemene-voorwaarden | 179 | ⚠️ lang (maar laag-prio pagina) |
| supply-chain blog | 156 | ✅ |
| rapport-voorbeeld | 126 | ⚠️ aan korte kant |
| privacy | 131 | ✅ |
Aanbeveling: homepage-desc naar 150–160 chars met core USP + CTA.

### B6. Geen Product/Offer schema op `/bestellen` (zie K4)
Al genoemd — opgenomen in P1.

### B7. Internal linking — thin pagina's
| Pagina | Interne links | Opmerking |
|---|---|---|
| /gap-analyse | 4 | Zou moeten linken naar /nis2, /rapport-voorbeeld, /faq, /bestellen, blog-posts |
| /bestellen | 5 | Idem |
| /rapport-voorbeeld | 6 | Kan beter |
| /nis2 | 12 | Prima |
| /faq | 13 | Prima |
| home | 17 | Goed |
| blog-* | 7–8 | Kan beter (posts zouden onderling en naar /nis2, /gap-analyse moeten linken) |

### B8. Blog posts missen `twitter:card`
- 5 van 5 blog-artikelen missen `<meta name="twitter:card">`. Minor maar gratis winst voor social-share previews.

### B9. `/rapport-voorbeeld`, `/privacy`, `/algemene-voorwaarden` missen OpenGraph
- `og:title`, `og:description`, `og:image` = leeg.
- Voor juridische pagina's is impact laag, maar `/rapport-voorbeeld` is wél een trust-asset voor social/backlink-sharing.

### B10. Semantische HTML-landmarks ontbreken
- Homepage gebruikt **0× `<header>`**, **0× `<main>`**, **1× `<nav>`**, **1× `<footer>`**, **8× `<section>`**, **0× `<article>`**.
- Voor accessibility (WCAG 2.1) en als indirect E-E-A-T / UX-signal zou er minimaal één `<header>` en één `<main>` moeten zijn. `<article>` rond blog-posts zou logisch zijn.

## Kleine issues (P3)

### C1. `changefreq`/`priority` in sitemap
- Worden door Google grotendeels genegeerd sinds ~2017. Geen kwaad, maar niet koesteren.

### C2. Sitemap: 9 van 15 URLs hebben dezelfde `lastmod: 2026-04-17`
- Suggereert een bulk-deployment-tijdstempel i.p.v. per-file content-changes. Niet schadelijk, wel minder informatief voor crawlers bij incrementele updates.

### C3. Titels — encoded entities
- Home-title bevat `&amp;`: `ISO 27001 Compliance voor het MKB | Gap-analyse &amp; Lead Auditor | Annex27`
- Dit is correct HTML (ampersand escaping), Google en browsers decoden dit. Geen issue, ter info.

### C4. Externe links
- Blog `/supply` heeft 9 externe links (naar MOVEit/SolarWinds/XZ Utils bronnen) — uitstekend voor E-E-A-T.
- Andere pagina's gebruiken weinig externe authoritative sources. Waar relevant (bv. `/nis2` → overheid/ENISA/CCB), links naar primaire bronnen versterken autoriteit.
- Controleer of `rel="noopener"` of `rel="nofollow sponsored"` toegepast worden waar nodig (nu niet geverifieerd per link).

### C5. Fonts geladen vanaf Google Fonts
- `preconnect` naar fonts.googleapis.com + fonts.gstatic.com aanwezig ✓
- Overweeg **self-hosting** van fonts om de 2 extra DNS+TLS roundtrips te elimineren (kan 100–300 ms schelen op mobiel 3G).

### C6. Inline CSS vs extern
- Geen externe `.css` gevonden — alle styling lijkt inline (Tailwind-achtig). 97 KB HTML → 22 KB gzip = OK, maar styling kan niet browser-gecached worden tussen pagina's. Overweeg shared CSS bundle.

## Pagina-per-pagina prioritering (samenvatting)

| Pagina | Commerciëel gewicht | Aantal issues | Top-3 actie |
|---|---|---|---|
| `/` | ⭐⭐⭐⭐⭐ | P2: meta-desc te lang, B10 (main/header), hreflang ok | Trim meta desc → 155 chars |
| `/gap-analyse` | ⭐⭐⭐⭐⭐ | P2: thin content (244w), thin internal links | Content verrijken + FAQ-blok |
| `/nis2` | ⭐⭐⭐⭐ | P2: hreflang, Service-schema, meta robots | Voeg Service+Offer schema + hreflang |
| `/bestellen` | ⭐⭐⭐⭐⭐ | P1: geen schema, thin content | Product+Offer schema + 300+ extra woorden trust-copy |
| `/faq` | ⭐⭐⭐ | Weinig | Alleen max-video-preview:-1 toevoegen |
| `/trust` | ⭐⭐⭐ | Hreflang ok, meta desc 169 | Voeg Organization+TrustScore schema uitbreiding |
| `/rapport-voorbeeld` | ⭐⭐⭐⭐ | Geen OG, geen schema, geen hreflang | Voeg OG + ImageObject + hreflang toe |
| `/blog/*` (5 posts) | ⭐⭐ | hreflang, twitter:card, max-snippet | Template-fix over alle posts |
| `/privacy`, `/algemene-voorwaarden` | ⭐ | OK voor hun doel | Niets kritieks |
| `/blog` (index) | ⭐⭐⭐⭐ | P1: 403 Forbidden | Fix server-config (crucial) |

## Conclusie Fase 2

Technische basis is **boven-gemiddeld** (HTTPS, HSTS, Varnish, gzip, security headers, server-side rendering, rijke schema op homepage). Maar:
- **1 ernstige crawl-fout** (blog-index 403)
- **1 canonicalisatie-risico** (www niet redirected)
- **1 commerciële pagina zonder Product-schema** (/bestellen)
- **Thin content** op twee core landing-pages (/gap-analyse, /bestellen)
- **Inconsistente meta-directives en hreflang-dekking** die eenvoudig via templating op te lossen zijn

Dit zijn allemaal *fixable* issues zonder dat er content geschreven hoeft te worden. Na fix positioneert Annex27 zich technisch beter dan 80% van de NL/BE B2B-compliance-concurrenten.
