# Fase 1 — Sitemap & Inventarisatie annex27.nl

**Fetch-datum:** 2026-04-22
**Sitemap-URL:** https://annex27.nl/sitemap.xml (HTTP 200, 2.612 bytes, `Last-Modified: 2026-04-21`)
**Robots.txt:** aanwezig, verwijst correct naar sitemap
**Totaal URLs in sitemap:** 15

## Server-signalen (uit HTTP-headers)
- **Server:** Apache achter Varnish (webcache1)
- **HTTPS + HSTS:** `max-age=63072000; includeSubDomains; preload` ✅
- **Security headers:** X-Frame-Options DENY, X-Content-Type-Options nosniff, Referrer-Policy strict-origin-when-cross-origin, Permissions-Policy (incl. `interest-cohort=()`), CSP aanwezig, COOP/CORP same-origin ✅
- **Belangrijk SEO-detail:** `X-Varnish` + `Age` header — caching via Varnish is actief (goed voor TTFB)
- **CSP staat Supabase + Mollie + GA + GTM toe** — consistent met tech-stack uit memory

## Robots.txt — beoordeling
```
Allow: /  /gap-analyse  /faq  /trust  /bestellen
Disallow: /portal /dashboard /admin /success /factuur /docs/ /beleidspakket/ /supabase/
Disallow: *.sql$ *.md$ *.sh$ *.py$ *.html$
User-agent: GPTBot / CCBot / ClaudeBot → alleen /dashboard /admin /portal geblokkeerd
Sitemap: https://annex27.nl/sitemap.xml
```

**Observaties:**
- ⚠️ `Disallow: /*.html$` — dit blokkeert álle URLs die eindigen op `.html`. Risico: als ooit `.html`-extensies gebruikt worden (bv. door het auto-hosten van marketing assets), verdwijnen ze uit de index. Nu niet actief schadelijk omdat alle sitemap-URLs extensieloos zijn, maar dit patroon is brittle.
- ✅ Disallow /admin, /dashboard, /portal, /success is correct — dit zijn auth-flow / klantportaal pagina's.
- ✅ Disallow van source-bestanden (*.sql, *.md, *.sh, *.py) is goede hygiëne.
- ⚠️ AI-bot-beleid: je staat GPTBot/CCBot/ClaudeBot wél toe om /blog en dienst-pagina's te scrapen. Voor commerciële SEO in het AI Overviews-tijdperk is dit *strategisch* (content kan in AI Overviews verschijnen). Voor pure IP-bescherming zou je ze volledig kunnen weren — bewuste afweging nodig.
- ℹ️ Geen `Disallow: /cgi-bin/`, geen crawl-delay — prima.

## URL-inventarisatie

| # | URL | lastmod | changefreq | priority | Categorie |
|---|-----|---------|------------|----------|-----------|
| 1 | https://annex27.nl/ | 2026-04-17 | weekly | 1.0 | Homepage |
| 2 | https://annex27.nl/gap-analyse | 2026-04-17 | monthly | 0.9 | Dienst — tool/lead-magnet |
| 3 | https://annex27.nl/nis2 | 2026-04-17 | monthly | 0.9 | Dienst — NIS2 |
| 4 | https://annex27.nl/bestellen | 2026-04-17 | monthly | 0.8 | Commerciëel — checkout |
| 5 | https://annex27.nl/faq | 2026-04-17 | monthly | 0.8 | Support / trust |
| 6 | https://annex27.nl/blog | 2026-04-17 | weekly | 0.8 | Blog-index |
| 7 | https://annex27.nl/blog/iso-27001-kosten-mkb | 2026-04-15 | monthly | 0.7 | Blog-artikel |
| 8 | https://annex27.nl/blog/iso-27001-vs-soc-2 | 2026-04-12 | monthly | 0.7 | Blog-artikel |
| 9 | https://annex27.nl/blog/nis2-deadline-2026 | 2026-04-08 | monthly | 0.7 | Blog-artikel |
| 10 | https://annex27.nl/blog/cyberverzekering-mkb-2026 | 2026-04-19 | monthly | 0.7 | Blog-artikel |
| 11 | https://annex27.nl/blog/supply-chain-attacks-mkb | 2026-04-19 | monthly | 0.7 | Blog-artikel |
| 12 | https://annex27.nl/rapport-voorbeeld | 2026-04-17 | monthly | 0.7 | Trust / social proof |
| 13 | https://annex27.nl/trust | 2026-04-17 | monthly | 0.7 | Trust / social proof |
| 14 | https://annex27.nl/privacy | 2026-04-17 | yearly | 0.4 | Juridisch |
| 15 | https://annex27.nl/algemene-voorwaarden | 2026-04-17 | yearly | 0.4 | Juridisch |

### Categorisatie (aantallen)
- **Homepage:** 1
- **Commerciële dienstpagina's:** 2 (`/gap-analyse`, `/nis2`)
- **Commerciële checkout:** 1 (`/bestellen`)
- **Trust / social proof:** 2 (`/trust`, `/rapport-voorbeeld`)
- **Support:** 1 (`/faq`)
- **Blog-index + artikelen:** 6 (1 index + 5 posts)
- **Juridisch:** 2

## Crawlability-check: orphan-pagina's & kandidaten

Geprobeerd (status):
```
404 /contact  404 /over  404 /prijzen  404 /diensten  404 /demo
404 /iso-27001  404 /compliance  404 /soc-2  404 /gap-analyse-nis2
200 /success  200 /dashboard  200 /portal  200 /admin  (alle 4 Disallowed ✅)
```

**Kritieke observaties:**
1. 🚨 **Geen dedicated `/iso-27001` pagina.** Annex27's kernpropositie (digitale ISO 27001 compliance) heeft geen directe landingspagina op het primaire keyword `iso 27001`. Dat is SEO-gat #1.
2. 🚨 **Geen dedicated `/prijzen` of `/tarieven` pagina.** Prijsintentie-zoekopdrachten (`iso 27001 kosten mkb`) worden nu alleen gevangen via het blog-artikel — prima voor top-of-funnel, maar commerciële intent hoort op een prijzen-landingspagina.
3. 🚨 **Geen `/contact`, `/over-ons`, `/team`.** Voor E-E-A-T (Expertise-Authoritativeness-Trustworthiness) in B2B-compliance mist er een aantoonbaar auteurs-/team-profiel met Raoul's ISO 27001 Lead Auditor kwalificatie.
4. ⚠️ **Geen sector-landingspagina's.** Uit memory blijkt dat SaaS een gevalideerd sectorprofiel is; er is geen `/iso-27001-saas` of `/iso-27001-zorg` pagina.
5. ⚠️ **Geen lokale/geografische landingspagina's** (bv. `/iso-27001-nederland`, `/iso-27001-belgie`). Uit memory: NL is hoofdmarkt, BE secundair — geografische differentiatie ontbreekt op pagina-niveau.

## Sitemap-kwaliteit

- ✅ Alle URLs gebruiken HTTPS en de canonical host (`annex27.nl` zonder www).
- ✅ `lastmod` is recent en realistisch (niet iedereen op dezelfde datum — wat anders een "fake-lastmod" signaal zou zijn).
- ⚠️ `changefreq` en `priority` worden in 2026 door Google **grotendeels genegeerd**. Geen kwaad, maar ook geen ranking-signaal.
- ⚠️ Geen **sitemap-index** of aparte **news/image/video sitemaps**. Voor 15 URLs onnodig, maar bij groei >50 URLs overwegen.
- ⚠️ Geen `<lastmod>` ontbreekt — maar `lastmod: 2026-04-17` komt op 9/15 pagina's terug. Dat wijst op een bulk-update-build. Oké nu, maar bij echte wijzigingen moet lastmod per pagina worden bijgehouden.

## Conclusie Fase 1
Kleine, schone site (15 pagina's) met sterke technische basis (HTTPS, HSTS, security headers, Varnish-caching) maar een **beperkt commercieel pagina-oppervlak**: slechts 3 pagina's (gap-analyse, nis2, bestellen) voeden de conversie-funnel. Drie cruciale commerciële landingspagina's ontbreken: **/iso-27001** (kern-keyword), **/prijzen** en sector-/geografische varianten. Robots & sitemap zijn technisch in orde; één aandachtspunt is de brede `Disallow: /*.html$`.
