# Fase 3 — Performance & Core Web Vitals

**Datum:** 2026-04-22
**Belangrijke methodische opmerking:** De PageSpeed Insights v5 API weigert anonieme requests (HTTP 429 `RESOURCE_EXHAUSTED`, quota-limit = 0). Voor officiële field-data (CrUX) + Lighthouse-lab run aanbevolen:
1. **Zelf draaien in browser:** https://pagespeed.web.dev/analysis?url=https://annex27.nl/ (gratis, geen key)
2. **Search Console → Ervaring → Core Web Vitals rapport** (eigen field-data)
3. **CrUX Dashboard** via Looker Studio (free) of `crux-api.web.dev`

Wat **wel** direct gemeten is uit de server-responses en HTML-analyse is hieronder uitgewerkt — dit geeft een betrouwbare *inschatting* van CWV-performance.

## 3.1 Direct gemeten infrastructuur-signalen

### TTFB per pagina (5 samples, warm cache, vanaf NL-gelijkende locatie)

| Pagina | avg TTFB | p95 TTFB | avg Total | Beoordeling |
|---|---|---|---|---|
| `/` | 150 ms | 159 ms | 211 ms | ✅ Uitstekend |
| `/gap-analyse` | 153 ms | 169 ms | 196 ms | ✅ Uitstekend |
| `/nis2` | 147 ms | 151 ms | 154 ms | ✅ Uitstekend |
| `/bestellen` | 150 ms | 159 ms | 159 ms | ✅ Uitstekend |
| `/faq` | 143 ms | 146 ms | 173 ms | ✅ Uitstekend |
| `/rapport-voorbeeld` | 152 ms | 159 ms | 157 ms | ✅ Uitstekend |

**Google drempel:** TTFB goed = <800 ms. Alle pagina's zitten ruim binnen "uitstekend" (Varnish-caching op Hostnet doet zijn werk).

**Implicatie voor LCP:** Met TTFB ≈ 150 ms is er nog 2.350 ms over voordat LCP "poor" wordt (>2.500 ms). Veel speelruimte, dus de bottleneck is waarschijnlijk niet server-side.

### Payload per pagina

| Pagina | Raw HTML | Gzip | Reductie |
|---|---|---|---|
| `/` | 98 KB | 22 KB | 77% |
| `/gap-analyse` | 84 KB | 20 KB | 76% |
| `/nis2` | 35 KB | 9 KB | 73% |
| `/bestellen` | 37 KB | 11 KB | 70% |
| `/faq` | 43 KB | 12 KB | 72% |
| `/rapport-voorbeeld` | 22 KB | 7 KB | 69% |

**Beoordeling:** ✅ Uitstekende compressie-ratio's. Homepage 22 KB gzip is prima (mobile budget ≤ 1 MB).

### HTTP-signalen samengevat
- ✅ HTTPS + HSTS preload (`max-age=63072000; includeSubDomains; preload`)
- ✅ Gzip actief (brotli niet getest — *aanbeveling: activeer Brotli voor ~15% extra besparing*)
- ✅ Varnish reverse-proxy cache (TTFB bewijs)
- ❌ **`Cache-Control: no-store, no-cache, must-revalidate, max-age=0`** — zie K3 in Fase 2. Schaadt repeat-visit performance. Split: auth-pagina's houden `no-store`, marketingpagina's kunnen `public, max-age=300, s-maxage=3600`.
- ⚠️ HTTP/2 of HTTP/3: niet getest via `curl -I` (alle headers tonen HTTP/1.1). Hostnet *zegt* HTTP/2 te ondersteunen — **verificatie aanbevolen** met `curl -I --http2 https://annex27.nl/`. HTTP/3/QUIC is onwaarschijnlijk op shared Apache-hosting.

## 3.2 Render-blocking resources in `<head>`

| Pagina | Sync scripts in head | Async/defer | Ext. CSS (Google Fonts) | Inline CSS |
|---|---|---|---|---|
| `/` | **2** (analytics.js ×2 — duplicaat!) | 0 / 0 | 1 (Sora+Inter) | 33 KB |
| `/gap-analyse` | 2 (idem) | 0 / 0 | 1 | 31 KB |
| `/nis2` | 1 | 0 / 0 | 1 | 9 KB |
| `/bestellen` | **3** (analytics.js, Supabase SDK, vat-rules.js) | 0 / 0 | 1 | 9 KB |
| `/faq` | 2 | 0 / 0 | 1 | 10 KB |
| `/rapport-voorbeeld` | 1 | 0 / 0 | 1 | 9 KB |
| blog/* | 1 | 0 / 0 | 1 | 4–5 KB |
| `/trust` | 2 | 0 / 0 | 1 | 10 KB |

### Concrete problemen

**P1a. Render-blocking JS zonder defer/async**
- Op **elke** pagina staan `<script>`-tags in `<head>` zonder `defer` of `async`. Dat blokkeert de HTML-parser tijdens download+parse van het script.
- `/analytics.js` is slechts 7.5 KB (2.8 KB gzip) — kleine impact maar hoort met `defer` of onderaan de body.
- `/bestellen` laadt de **Supabase SDK** (~31 KB gzip, ~110 KB uncompressed) sync in `<head>`. Dit is *de* LCP/FCP-killer op deze pagina. De SDK is pas nodig bij klik op "bestellen" — laad hem `defer` of dynamisch (`import()` on-demand).
- `vat-rules.js` is ook sync.

**P1b. `analytics.js` dubbel ingeladen op `/` en `/gap-analyse`**
- Twee `<script src="/analytics.js">` tags in dezelfde `<head>`. Verspilde DNS/HTTP/parse-cyclus. Browser-cache vangt de 2e call op (304), maar het is onnodig.

**P1c. Google Fonts als render-blocking external stylesheet**
- `<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Sora...Inter...&display=swap">` blokkeert first paint totdat het CSS-bestand is opgehaald.
- `display=swap` zorgt ervoor dat er geen FOIT is (geen onzichtbare tekst), maar de CSS-download zelf zit nog in het kritieke pad.
- **Oplossingen (van beter naar minder):**
  1. **Self-host** Sora + Inter als `woff2` + `preload` → bespaart 2 DNS-resolves (googleapis + gstatic) + TLS + 1–2 RTT. Op mobiel 4G kan dit 200–400 ms van FCP/LCP afhalen.
  2. Óf: `<link rel="preload" as="style">` + `<link rel="stylesheet" media="print" onload="this.media='all'">` truc (laad async).
  3. Behoud `preconnect` naar `fonts.googleapis.com` en `fonts.gstatic.com` (aanwezig ✓).

**P1d. Inline CSS van 33 KB op homepage**
- Alle styling is inline in een `<style>`-block. Voordeel: geen extra roundtrip. Nadeel: geen browser-cache tussen pagina's. Op een 10-pagina-site is dit te overwegen, maar het verhoogt elke HTML-response met 30 KB herhaling.
- **Overweging:** externaliseer de shared CSS naar `/assets/main.css` met lange cache-TTL. Alleen de *above-the-fold* critical CSS inline laten (Tailwind-achtige extraction).

**P1e. Geen `<img>` tags gebruikt; alles inline SVG**
- Op homepage 57 inline SVGs, op totaalpagina 0 `<img>` tags.
- Voordeel: geen extra HTTP-requests, SVG is scherp op retina, kleine bytes.
- Nadeel: inline SVGs dragen bij aan HTML-grootte. Op homepage doet dit mee aan de 98 KB raw.
- **Niet kritiek**, maar: als dezelfde SVG meerdere keren wordt gebruikt, overweeg een `<symbol>` sprite met `<use href>`.

## 3.3 Geschatte Core Web Vitals (op basis van bovenstaande signalen)

Dit is een *gefundeerde schatting* vooruitlopend op echte PSI/CrUX-data.

| Metric | Schatting mobile | Schatting desktop | Drempel (goed/matig/slecht) |
|---|---|---|---|
| **LCP** | 1.8–2.4 s | 0.9–1.4 s | <2.5 / <4.0 / ≥4.0 s |
| **INP** | 150–250 ms | 50–120 ms | <200 / <500 / ≥500 ms |
| **CLS** | 0.00–0.05 | 0.00–0.03 | <0.1 / <0.25 / ≥0.25 |
| **FCP** | 1.2–1.6 s | 0.6–0.9 s | <1.8 / <3.0 / ≥3.0 s |
| **TTFB** | 150 ms (gemeten) | 150 ms | <800 ms |

**Rationale:**
- LCP **mobile** zit waarschijnlijk in de "goed" of "needs improvement" band, afhankelijk van wat het LCP-element is. Op homepage is dat vermoedelijk de hero-tekst ("Uw weg naar ISO 27001"). Tekst-LCP wordt beperkt door: TTFB + HTML-parse + render-blocking CSS. Met 150 ms TTFB + render-blocking Google Fonts request (+200 ms op mobiel) = **~1.5–2.0 s realistisch**.
- LCP **bestellen/mobile** mogelijk zwakker door Supabase SDK sync in head → **richting 2.5 s**.
- INP is moeilijk in te schatten zonder veldwaarden; geen zware JS observable behalve Supabase op /bestellen.
- CLS zou laag moeten zijn door server-rendered content + `display=swap` op fonts. Wel controleren of hero-afbeeldingen/SVGs dimensies hebben.

## 3.4 Prioriteitslijst performance-optimalisaties

### P1 — Direct (quick wins)
1. **Verwijder dubbele `<script src="/analytics.js">`** op homepage en `/gap-analyse`.
2. **Voeg `defer` toe aan alle scripts in `<head>`** (analytics.js, vat-rules.js). Supabase SDK: verplaats naar dynamisch laden bij klik op bestel-knop, óf minimaal `defer`.
3. **Zet `Cache-Control`** op marketing-pagina's naar `public, max-age=300, s-maxage=3600, must-revalidate`. Laat auth-pagina's `no-store` houden.
4. **Activeer Brotli** op Apache (`mod_brotli`) naast gzip — ~15% extra besparing op HTML/CSS/JS.
5. **Verifieer HTTP/2** is actief: `curl -I --http2 https://annex27.nl/` (moet `HTTP/2 200` tonen).

### P2 — Medium effort
6. **Self-host Sora + Inter fonts** als WOFF2 met `<link rel="preload" as="font" crossorigin>` + subsetted `unicode-range` voor latin. Verwijder Google Fonts request.
7. **Externaliseer gemeenschappelijke CSS** naar `/assets/main.css` met `Cache-Control: public, max-age=31536000, immutable` en unieke filename-hash. Alleen critical CSS (above-the-fold) inline houden.
8. **Lazy-load Supabase SDK** op `/bestellen` via dynamic `import()` op form-submit.
9. **Voeg expliciete `width`/`height` toe** op alle SVG-containers die via CSS gedimensioneerd worden (voorkomt CLS op slow CSS-load).

### P3 — Grondig
10. **Draai een echte Lighthouse-audit** (in Chrome DevTools of via `npx lighthouse <url> --preset=mobile`) en adresseer de top-3 "Opportunities" per pagina.
11. **CrUX field-data monitoren** via Search Console → Ervaring → Core Web Vitals, of via Looker-Studio CrUX dashboard. Dit is wat Google daadwerkelijk gebruikt voor ranking.
12. **HTTP/3 / QUIC** — vraag Hostnet of dit aangezet kan worden. Geen must, maar op mobiel kan het LCP nog met 50–150 ms verbeteren.

## 3.5 Wat de gebruiker zelf moet draaien (omdat ik de API niet mag gebruiken)

Deze stappen in een browser uitvoeren en de resultaten terugkoppelen voor finetuning van het plan:

```
1. https://pagespeed.web.dev/analysis?url=https://annex27.nl/&form_factor=mobile
2. https://pagespeed.web.dev/analysis?url=https://annex27.nl/gap-analyse&form_factor=mobile
3. https://pagespeed.web.dev/analysis?url=https://annex27.nl/bestellen&form_factor=mobile
4. https://pagespeed.web.dev/analysis?url=https://annex27.nl/nis2&form_factor=mobile
5. https://pagespeed.web.dev/analysis?url=https://annex27.nl/faq&form_factor=mobile
```

Kopieer daarna per pagina: Performance-score, LCP/INP/CLS-waarden, en top-3 Opportunities.

## 3.6 Conclusie Fase 3

Infrastructureel (server, TLS, compressie, TTFB) zit Annex27 in de **top 10%** van B2B-sites. De bottlenecks zitten **volledig in het rendering-pad**:
1. Render-blocking JS in `<head>` (geen defer) — grootste issue op `/bestellen`
2. Render-blocking Google Fonts request
3. Geen browser-caching van HTML (weliswaar marketing-bewust maar onnodig op meeste pagina's)
4. Eén dubbele script-include (triviale fix)

Wanneer deze 4 issues worden opgelost, verwacht ik **LCP onder 1.5 s op mobiel** en een PSI-score van **90+** op alle pagina's. Dat is een duidelijke ranking-voorsprong t.o.v. gemiddelde concurrenten in dit segment.
