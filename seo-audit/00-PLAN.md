# Annex27.nl — SEO Audit & Rankingplan 2026

**Opgesteld:** 2026-04-22
**Scope:** annex27.nl (NL hoofdmarkt, BE secundair) — 15 pagina's in sitemap
**Doel:** zo hoog mogelijk organisch ranken op commerciële zoektermen rondom ISO 27001 + NIS2 voor MKB.
**Methode:** eigen technische analyse (HTML fetches + headers) + concurrentie-onderzoek + actuele Google ranking-signalen 2025/2026.
**Ondersteunende documenten:** `01-inventory.md`, `02-pages.md` + `02-pages-findings.md`, `02-pages.csv`, `03-performance.md`, `04-recent-developments.md`, `05-competition-keywords.md`.

---

## 1. Executive summary (1 A4)

Annex27 heeft een **scherpe technische fundering** (HTTPS + HSTS preload, Varnish, security-headers, rijke homepage-schema, server-side rendering, TTFB 150 ms). Die fundering is beter dan 80% van de NL/BE compliance-concurrenten. Desondanks laat de site **significante organische ranking-winst liggen** door vijf categorieën issues:

**Top 5 issues (naar impact):**

1. **Te klein commercieel pagina-oppervlak.** 15 URLs in totaal, waarvan slechts 3 commerciële landingspagina's (`/gap-analyse`, `/nis2`, `/bestellen`). Kern-keywords zoals `iso 27001` en `prijzen` hebben géén dedicated LP. **Impact op commerciële ranking: hoog.**
2. **Blog-index `/blog` → 301 → 403 Forbidden.** De sitemap verwijst naar een niet-bereikbare pagina. Google krijgt een sitemap-fout. **Impact: hoog op crawl-gezondheid.**
3. **E-E-A-T-gap: geen `/over`, geen auteurspagina voor Raoul Haas (Lead Auditor).** In 2026 is E-E-A-T uit YMYL-segment uitgebreid naar álle sectoren; AI Overview-citaties komen voor 96% van E-E-A-T-sterke bronnen. **Impact: hoog op AI Overviews en organische trust.**
4. **Render-blocking JS in `<head>` op alle pagina's** (geen defer/async), Supabase SDK sync op `/bestellen`, Google Fonts als render-blocking external CSS. **Impact: middel op Core Web Vitals (INP/LCP).**
5. **Thin commerciële landingspagina's.** `/gap-analyse` = 244 woorden, `/bestellen` = 188 woorden, beide zonder Product/Service-schema. **Impact: middel op transactionele ranking.**

**Verwacht resultaat bij implementatie van 30-dagen actie-blok:** +40 tot +60 organische posities over de top-20 commerciële keywords en een significante CTR-verbetering door schema-driven rich results.

---

## 2. Quick wins (impact hoog — effort laag)

Acties die binnen 1 werkdag doorgevoerd kunnen worden met hoge ranking-ROI.

| # | Actie | Bron/bewijs | Impact | Effort | Prio |
|---|---|---|---|---|---|
| QW1 | Verwijder dubbele `<script src="/analytics.js">` op homepage + `/gap-analyse` | `02-pages-findings.md` §3.2, `03-performance.md` §3.2 P1b | Laag-midden | <15 min | 1 |
| QW2 | Voeg `defer` toe aan alle `<script src>` in `<head>` op álle pagina's | `03-performance.md` §3.2 P1a | Midden (INP/LCP) | 30 min | 1 |
| QW3 | Zet 301 redirect `www.annex27.nl` → `annex27.nl` (Apache vhost-config) | Fase 2 K2 | Laag-midden (dup-content-risk) | 15 min | 1 |
| QW4 | Fix `Disallow: /*.html$` in robots.txt → verwijder of inperken tot `/docs/*.html$` | Fase 1, Fase 2 K5 | Laag (preventief) | 5 min | 2 |
| QW5 | Pas homepage meta description aan van 201 → 150-160 chars met CTA | Fase 2 B5 | Midden (CTR) | 10 min | 1 |
| QW6 | Uniformeer `meta robots`: voeg `max-snippet:-1, max-image-preview:large, max-video-preview:-1` toe aan alle indexeerbare pagina's | Fase 2 B4 | Laag-midden (AI snippet-display) | 20 min | 1 |
| QW7 | Voeg `hreflang="nl-NL", "nl-BE", "x-default"` toe aan `/nis2`, alle 5 blog-posts, `/rapport-voorbeeld` | Fase 2 B1 | Midden (geo-targeting) | 30 min | 1 |
| QW8 | Voeg `twitter:card` + volledige OpenGraph toe aan blog-posts, `/rapport-voorbeeld`, `/privacy`, `/algemene-voorwaarden` | Fase 2 B8, B9 | Laag (social CTR) | 30 min | 2 |
| QW9 | Cache-Control op marketingpagina's naar `public, max-age=300, s-maxage=3600, must-revalidate`; auth-pagina's blijven `no-store` | Fase 2 K3 + Fase 3 §3.1 | Midden (repeat-visit CWV) | 30 min (server-conf) | 1 |
| QW10 | Fix `/blog` → `/blog/` → 403: óf rewrite opheffen óf `/blog/` serveren (eventueel aanpassen sitemap naar `/blog/`) | Fase 2 K1 | **Hoog** (crawlability blog-hub) | 30-60 min | 1 |

**Totaal effort quick wins: ~3-4 uur werk.**

---

## 3. Technische fundering (impact hoog — effort midden)

### 3.1 Structured data fixes
| # | Actie | Details | Impact | Effort |
|---|---|---|---|---|
| TF1 | Voeg `Product + Offer + AggregateOffer + BreadcrumbList` toe aan `/bestellen` | 4 pakketten (gap €795, NIS2 €995, beleidspakket €795, pre-audit €1.495). Zie Google Product-schema spec | Hoog (rich snippets + AI price-extraction) | 2 uur |
| TF2 | Voeg `Service + Offer + BreadcrumbList` toe aan `/nis2` naast de bestaande FAQPage | Service.areaServed: ["NL","BE"], Offer.price: 995 | Hoog (commercial intent schema) | 1 uur |
| TF3 | Breid `/trust` uit met `Organization` + `hasCredential` (Raoul's ISO 27001 Lead Auditor) | Organization.hasCredential → EducationalOccupationalCredential | Midden (E-E-A-T) | 1 uur |
| TF4 | Voeg `ImageObject + CreativeWork` toe aan `/rapport-voorbeeld` | Voorbeeldrapport is een linkable asset | Laag-midden | 30 min |
| TF5 | Audit alle Article-schema `author.Person.url` — moet naar **bestaande** auteurspagina wijzen | Nu waarschijnlijk broken (fase 4 §5) | Hoog (AI Overview + E-E-A-T) | 1 uur |

### 3.2 Performance
| # | Actie | Details | Impact | Effort |
|---|---|---|---|---|
| PF1 | Self-host Sora + Inter fonts als WOFF2 met `preload` | Verwijdert Google Fonts render-blocking request | Midden (LCP -200-400 ms mobile) | 2-3 uur |
| PF2 | Lazy-load Supabase SDK op `/bestellen` (dynamic `import()` on form-submit) | Verwijdert 31 KB sync JS uit critical path | Midden (INP) | 2 uur |
| PF3 | Activeer Brotli op Apache (`mod_brotli`) | ~15% extra compressie-winst | Laag-midden | 30 min (server-conf) |
| PF4 | Verifieer HTTP/2 is actief (`curl -I --http2`) | Verwachte win al actief — alleen confirmeren | Onbekend tot verify | 5 min |
| PF5 | Externaliseer gemeenschappelijke CSS naar `/assets/main.css` met cache-busting filename | Herhaling 30 KB inline CSS tussen pagina's verdwijnt | Laag-midden (repeat-visit) | 3-4 uur |
| PF6 | Voeg expliciete `width`/`height` toe op SVG-containers | Voorkomt CLS op slow-CSS scenario's | Laag (CLS-preventief) | 1 uur |
| PF7 | Draai Lighthouse (mobile) op homepage + `/gap-analyse` + `/bestellen`, fix top-3 opportunities per pagina | Feedback-loop; zonder PSI API | Hoog (data-gedreven) | 2-3 uur per pagina |

### 3.3 Crawl & indexering
| # | Actie | Details | Impact | Effort |
|---|---|---|---|---|
| CI1 | Update sitemap: óf `/blog` → `/blog/` óf fix server zodat `/blog` laadt | Elimineert sitemap-fout in GSC | Hoog | 15 min na QW10 |
| CI2 | Voeg Google Search Console property toe (beide `annex27.nl` en `www.annex27.nl`) | Nodig voor rank-tracking, CWV-field-data, index-coverage | Kritiek | 15 min |
| CI3 | Dien sitemap in via GSC + verifieer indexering alle 15 URLs | Baseline | Hoog | 10 min |
| CI4 | Stel Bing Webmaster Tools in (nog ~10% van NL-marktaandeel) | Extra traffic-kanaal | Laag-midden | 15 min |
| CI5 | Audit interne linking: voeg "gerelateerd/related" blok toe op blog-posts (3-5 inhoudelijke interne links per post) | Fase 2 B7 | Midden | 2 uur |

### 3.4 Semantische HTML
| # | Actie | Details | Impact | Effort |
|---|---|---|---|---|
| SH1 | Wrap page-content in `<main>` landmark; voeg `<header>` rond top-navigatie toe | Nu 0× `<main>`, 0× `<header>` | Laag-midden (A11y + indirect E-E-A-T) | 1-2 uur |
| SH2 | Wrap blog-posts in `<article>` | Correct semantisch, helpt schema-coherentie | Laag | 30 min |

---

## 4. Architectuur & interne linking

### 4.1 Silo-structuur (voorgestelde site-topologie)

```
/
├── /iso-27001                         ← NIEUW, cluster hub
│   ├── /iso-27001-implementatie       ← NIEUW (Tier 2)
│   ├── /iso-27001-beleid              ← NIEUW (Tier 2)
│   ├── /iso-27001-audit               ← NIEUW (Tier 2, pre-audit)
│   ├── /iso-27001-voor-saas           ← NIEUW (Tier 1)
│   └── /iso-27002                     ← NIEUW (Tier 2, optioneel)
├── /nis2                              ← BESTAAT
│   ├── /nis2-audit                    ← NIEUW (Tier 1)
│   ├── /nis2-cyberbeveiligingswet     ← NIEUW (Tier 1)
│   └── /nis2-supply-chain             ← NIEUW (Tier 2)
├── /gap-analyse                       ← BESTAAT (verrijken)
├── /bestellen                         ← BESTAAT (schema toevoegen)
├── /prijzen                           ← NIEUW (Tier 1) — óf alias voor /bestellen met eigen content
├── /rapport-voorbeeld                 ← BESTAAT
├── /blog                              ← FIX (403)
│   └── [posts]
├── /faq                               ← BESTAAT
├── /trust                             ← BESTAAT
├── /over                              ← NIEUW (Tier 1) — E-E-A-T
│   └── /raoul-haas                    ← NIEUW (Tier 1) — auteur-pagina
├── /iso-27001-nederland               ← NIEUW (geo-LP, Tier 1)
├── /iso-27001-belgie                  ← NIEUW (geo-LP, secondaire markt)
├── /privacy                           ← BESTAAT
└── /algemene-voorwaarden              ← BESTAAT
```

### 4.2 Interne linking rules

| Vanuit | Naar (minimaal) | Anchor-strategie |
|---|---|---|
| `/` | /iso-27001 (cluster hub), /gap-analyse, /bestellen, /over, /blog, /rapport-voorbeeld | Variatie: "ISO 27001 compliance voor MKB", "gratis gap-analyse", "bekijk pakketten" |
| `/iso-27001` | /iso-27001-implementatie, /iso-27001-beleid, /iso-27001-voor-saas, /iso-27001-nederland, /gap-analyse, /bestellen, /blog-*, /rapport-voorbeeld, /trust | Hub-spoke patroon |
| `/nis2` | /nis2-audit, /nis2-cyberbeveiligingswet, /nis2-supply-chain, /gap-analyse, /iso-27001 (voor overlap-uitleg), /blog/nis2-deadline-2026 | Hub-spoke |
| `/gap-analyse` | /rapport-voorbeeld (bewijs), /bestellen, /iso-27001, /nis2, /faq (scope-uitleg) | Conversiepad versterken |
| `/bestellen` | /gap-analyse, /nis2, /iso-27001-beleid, /iso-27001-audit, /trust, /faq | Product ↔ trust ↔ support |
| `/blog/*` (alle posts) | Minimaal: /iso-27001, /gap-analyse (CTA), 2-3 andere posts, auteur `/raoul-haas` | Breng traffic naar commerciele LP |
| `/over` | /raoul-haas, /trust, /rapport-voorbeeld, /faq | Trust-hub |
| `/raoul-haas` | Alle blogs (inkomende Person.url), /trust, /faq, LinkedIn | Autoriteit |

**Breadcrumb-rules:** toon breadcrumbs op alle pagina's dieper dan /. Schema `BreadcrumbList` is al aanwezig op homepage, gap-analyse, faq, trust — uitrollen naar alle andere.

### 4.3 Anchor text strategy
- Geen enkele "lees meer" / "klik hier" / "hier" anchors — alle interne anchors beschrijvend (bv. "bekijk ons ISO 27001 beleidspakket" i.p.v. "hier").
- Variatie per pagina: homepage kan "gratis ISO 27001 quickscan" gebruiken, /bestellen "onze pakketten", blog "onze gap-analyse" — **nooit alle interne links met exacte-match anchor** (over-optimalisatie-risico).

---

## 5. Nieuwe commerciële pagina's (geen blogs)

**Volgorde = priority.** Per pagina: minimaal 800–1200 woorden, schema (FAQ/Service/Product), 5-10 interne links, 1-2 CTAs naar /gap-analyse of /bestellen, en — waar relevant — 2-3 externe verwijzingen naar primaire bronnen (ENISA, CCB, BSI, NEN) voor E-E-A-T.

| # | Pagina | Keyword-focus | Waarom | Impact | Effort |
|---|---|---|---|---|---|
| N1 | **/iso-27001** | `iso 27001 mkb`, `iso 27001 certificering mkb`, `iso 27001 compliance mkb` | Ontbrekende kern-LP, cluster-hub | Hoog | 1-2 dagen |
| N2 | **/over** + **/raoul-haas** | Brand/E-E-A-T, `annex27 review`, `raoul haas lead auditor` | E-E-A-T kritiek voor 2026 algoritme; AI Overview citaties | Hoog | 1 dag (over) + 0.5 dag (auteur) |
| N3 | **/iso-27001-voor-saas** | `iso 27001 saas`, `iso 27001 soc 2 saas`, `iso 27001 b2b saas nederland` | Gevalideerd sectorprofiel (memory), Tier 1 keyword-gap | Hoog | 1-2 dagen |
| N4 | **/prijzen** (óf heronoemen van /bestellen + een aparte /bestellen kaart) | `iso 27001 prijs`, `iso 27001 tarief mkb`, `compliance software prijzen` | Prijs-transparantie is jouw USP | Hoog | 0.5-1 dag |
| N5 | **/nis2-audit** | `nis2 audit`, `nis2 readiness assessment`, `nis2 gap analyse` | NIS2-deadline venster 2026; concurrent (bmGRIP) staat hier al | Hoog | 1-2 dagen |
| N6 | **/nis2-cyberbeveiligingswet** | `cyberbeveiligingswet mkb`, `cyberbeveiligingswet compliance` | NL-wet medio 2026 — timing-venster | Hoog | 1 dag |
| N7 | **/iso-27001-nederland** | `iso 27001 consultant nederland`, `iso 27001 implementatie nl` | Primaire markt geo-LP, hreflang nl-NL | Midden-hoog | 1 dag |
| N8 | **/iso-27001-implementatie** | `iso 27001 implementatie mkb`, `iso 27001 stappenplan` | Cluster-versterking | Midden | 1-2 dagen |
| N9 | **/iso-27001-beleid** | `iso 27001 beleid template`, `informatiebeveiligingsbeleid mkb` | Product-LP voor beleidspakket | Midden | 1 dag |
| N10 | **/iso-27001-audit** (pre-audit) | `iso 27001 interne audit`, `iso 27001 pre audit review` | Product-LP voor pre-audit €1.495 | Midden | 1 dag |
| N11 | **/iso-27001-belgie** | `iso 27001 consultant belgie`, `iso 27001 implementatie vlaanderen` | Secundaire markt | Midden | 1 dag |
| N12 | **/nis2-supply-chain** | `nis2 leveranciersbeleid`, `nis2 supply chain mkb` | Differentiatie; bestaande blog is top-tier linkable | Laag-midden | 1 dag |

**⚠️ Bouw géén sector-/branche-pagina's (zorg/advocaten/etc.) tot je er daadwerkelijk audit-ervaring in hebt om dunne content te vermijden (Helpful Content risk).**

### Per-pagina template voor nieuwe LPs

1. H1 met kern-keyword + USP-modifier ("voor het MKB" / "vanaf €795")
2. Intro-paragraaf: 2-3 zinnen directe beantwoording (AI Overview-extraction-ready, 100-150 woorden)
3. Sectie "Wat houdt [topic] in?" — 200-400 woorden
4. Sectie "Hoe Annex27 [topic] aanpakt" — 200-400 woorden + CTA naar /gap-analyse
5. Sectie "Wat kost dit?" — link naar /prijzen of /bestellen, geen dubbele prijs-tabel
6. Sectie "Vergelijking [topic] vs. alternatieven" — SEO-asset én E-E-A-T
7. FAQ-sectie — minimaal 5 Q&A's, voeg FAQPage schema toe
8. Trust-blok — link naar /trust, /rapport-voorbeeld, /over
9. CTA-herhaling
10. Volledige schema: Service/Product + Offer + FAQPage + BreadcrumbList

---

## 6. E-E-A-T & autoriteit

### 6.1 On-site E-E-A-T
| Actie | Prio | Effort |
|---|---|---|
| `/over` pagina met: verhaal, missie, oprichter Raoul + certificeringen (LA-nummer + uitgevende instantie), team-foto's indien van toepassing, KBO + BTW + IBAN | 1 | 1 dag |
| `/raoul-haas` (of `/auteur/raoul`): bio, kwalificaties, ISO 27001 LA-cert, publicaties, LinkedIn, alle blog-posts als "recent work" | 1 | 0.5 dag |
| Update `author.Person.url` in alle Article-schema → `/raoul-haas` | 1 | 15 min |
| Alle blog-posts: zichtbaar "Door Raoul Haas, ISO 27001 Lead Auditor" met foto + datum + "Laatst bijgewerkt" | 1 | 1-2 uur |
| `/trust` uitbreiden met zichtbare auditor-credentials + uitgifte-bevestiging | 2 | 2 uur |
| Cases / testimonials — 2-3 met toestemming (initialen + branche mag, full name beter) | 2 | Afhankelijk van klanten |
| Klant-logo-strip op homepage + /trust | 2 | Afhankelijk |
| "In de media" / "Gesproken op" blok voor Raoul (incl. speakerdecks, podcasts) | 3 | Continu |

### 6.2 Off-site autoriteit (link-building)

**Aanpak 2026 = authority > volume** (Fase 4 §6).

| # | Actie | Welke autoriteit | Wie | Impact | Effort |
|---|---|---|---|---|---|
| EEA1 | **Digital PR pitch**: "MKB NIS2-Readiness Barometer 2026" (geaggregeerde quickscan-data) | Computable, AG Connect, Emerce, Security.NL, BNR, FD | Content + PR | Hoog | 1-2 weken voorbereiding |
| EEA2 | **Gastblog** op Tweakers.net, Cyberveilig Nederland, PvIB | Hoog-DR NL-tech | Raoul | Midden-hoog | 1 dag per post |
| EEA3 | Speaker-opportunities ISF, PvIB, NOREA, DPO-netwerken | Autoriteit-signaal (niet altijd do-follow, wel mention) | Raoul | Midden | Continu |
| EEA4 | Partner-pagina's: Supabase, Hostnet (technologie-stack), certifiers (BSI, DEKRA, Kiwa, DNV, TÜV) | Partner-backlinks | Business | Midden | Afh. partner |
| EEA5 | HARO / Source of Sources voor Raoul als ISO-expert (journalisten-queries) | Pers-mentions | Raoul | Midden (langetermijn) | 15 min/dag |
| EEA6 | **Linkable assets publiceren**: NIS2-checklist PDF, ISO 27001 risk register template, controls-mapping Excel — achter lichte gate (email), maar free access | Organische referenties | Content | Hoog | 1-2 dagen per asset |
| EEA7 | Google Business Profile aanmaken (service-area: NL + BE) | Local SEO + brand-signal | Business | Laag-midden | 30 min |
| EEA8 | Bedrijfsprofiel op Clutch.co, Goodfirms, G2 (B2B software review sites) — alleen met echte reviews | Trust-mentions + backlinks | Business | Laag-midden | 1-2 uur |
| EEA9 | Podcast-ronde voor Raoul: "Security voor het MKB" — BNR Digitaal, Datenschutz-podcasts, Computable-audio | Autoriteit-opbouw | Raoul | Midden | Continu |

**Niet doen:** link-schemes, PBN's, betaalde directory-dumps, scaled outreach agencies.

---

## 7. Roadmap — 30 / 60 / 90 dagen

### 🎯 30 dagen — "Technische schoonmaak + E-E-A-T fundament"

**Doel:** foutloze basis, crawl-issues opgelost, E-E-A-T-signalen live, eerste commerciële LP's gebouwd.

| Week | Acties |
|---|---|
| **1** | QW1-QW10 (alle quick wins) + CI1-CI4 (GSC/Bing/sitemap) |
| **2** | TF1 (/bestellen Product-schema), TF2 (/nis2 Service-schema), TF5 (author.url fix), PF1 (self-host fonts), PF2 (lazy Supabase), PF3 (Brotli) |
| **3** | N2 (/over + /raoul-haas live — inclusief bijwerken blog-posts met auteur-attributie), TF3 (/trust credential-schema), SH1-SH2 (semantic HTML) |
| **4** | N1 (/iso-27001 cluster-hub live), N4 (/prijzen of upgrade /bestellen met 500+ extra woorden + Product-schema verification) |

**Deliverables einde 30 dagen:**
- ✅ Alle crawl-errors weg, GSC groen
- ✅ Alle indexeerbare pagina's consistent meta-robots, hreflang, canonical
- ✅ `/over`, `/raoul-haas`, `/iso-27001` live
- ✅ Structured data op alle commerciële pagina's (Product/Service/Offer)
- ✅ Performance: defer/lazy-load geïmplementeerd, self-hosted fonts

### 🎯 60 dagen — "Topical authority bouwen + NIS2-venster grijpen"

**Doel:** 4-6 commerciële LP's toegevoegd; NIS2-hub dominant; eerste link-building campagne loopt.

| Week | Acties |
|---|---|
| **5** | N5 (/nis2-audit), N6 (/nis2-cyberbeveiligingswet) — timing voor NL-wet medio 2026 |
| **6** | N3 (/iso-27001-voor-saas) — gevalideerd sectorprofiel; N7 (/iso-27001-nederland) |
| **7** | N8 (/iso-27001-implementatie), N9 (/iso-27001-beleid), N10 (/iso-27001-audit) — cluster-versterking |
| **8** | EEA1 (MKB NIS2-Barometer research), EEA6 (publiceer eerste linkable asset: NIS2-checklist PDF), EEA7 (GBP live), content-verrijking /gap-analyse (800 → 1200 woorden) |

**Deliverables einde 60 dagen:**
- ✅ ISO 27001-cluster (/iso-27001 + 3-4 spokes) volledig
- ✅ NIS2-cluster (/nis2 + /nis2-audit + /nis2-cyberbeveiligingswet + /nis2-supply-chain) volledig
- ✅ 1 linkable asset live (NIS2-checklist)
- ✅ GBP (Google Business Profile) live
- ✅ Eerste outreach-pitch verstuurd

### 🎯 90 dagen — "Link-building + optimalisatie-cycle"

**Doel:** organisch verkeer meetbaar stijgend; 5-10 autoritaire backlinks verworven; data-gedreven optimalisatie-cycle opzet.

| Week | Acties |
|---|---|
| **9-10** | EEA1 (NIS2-Barometer publicatie + pitch aan 10 redacties), EEA2 (2 gastblogs geschreven + geplaatst), EEA5 (HARO dagelijks) |
| **11** | N11 (/iso-27001-belgie), N12 (/nis2-supply-chain), TF4 (rapport-voorbeeld schema), SH/PF resterende perf-opt |
| **12** | **Review-maand**: GSC data-analyse, CrUX CWV-field-data review, Lighthouse per pagina, top-10 pagina's A/B op meta-title (CTR-test), update oude blog-posts met interne links naar nieuwe LP's |

**Deliverables einde 90 dagen:**
- ✅ 10-12 nieuwe commerciële pagina's live
- ✅ 5+ autoritaire backlinks (streefdoel, afhankelijk van PR-succes)
- ✅ Lighthouse: alle core-pagina's 90+ mobile
- ✅ GSC: zichtbaarheid voor top-20 keywords uit Fase 5
- ✅ E-E-A-T: /over, /raoul-haas, credentials zichtbaar, Person-schema werkend

### Maandelijkse monitoring (vanaf dag 90)

- **GSC**: zoektermen-rapport, pagina-rapport, CWV-ervaringsrapport
- **Bing Webmaster**: dup-signaal
- **Lighthouse CI**: op deploy (via GitHub Actions / Hostnet-hook)
- **Keyword rank tracking**: gratis via Serprobot / SE Ranking free tier voor top-20 keywords, 1× per week
- **Backlink-profile**: Ahrefs free backlink checker maandelijks; Google Alerts op "annex27" voor mentions
- **CrUX dashboard**: INP/LCP/CLS-trends, mobile+desktop

---

## 8. Ranking-verwachting (onderbouwd)

Dit is een **gefundeerde inschatting**, niet een belofte — SEO-resultaten hangen af van concurrenten en Google-updates.

| Timeline | Doel | Waarom realistisch |
|---|---|---|
| **30 dagen** | Index-coverage 100%, CWV groen, 5-8 nieuwe pagina's geïndexeerd | Technisch werk + schone site = snelle GSC-groeicurve |
| **60 dagen** | Posities 20-50 op 50% van Fase 5 long-tails; Top 10 op 2-3 niche-longtails (bv. "iso 27001 gap analyse gratis", "nis2 cyberbeveiligingswet mkb") | Nieuwe content + lage concurrentie in deze specifieke longtails |
| **90 dagen** | Posities 10-30 op 15-20 Fase 5 keywords; Top 10 op "nis2 mkb", "iso 27001 quickscan", "iso 27001 mkb prijs" | Topical authority + backlinks + Annex27's USP-zichtbaarheid |
| **6 maanden** | Top 5 op "iso 27001 mkb", "nis2 mkb", "iso 27001 gap analyse" — 2-5× huidig organisch verkeer | Compounding van content + backlinks; concurrentie is traag (MaasISO/KAM minder actief) |
| **12 maanden** | Top 3 op 5-10 kern-keywords, AI Overview citaties op 5+ queries, steady inbound leads via organisch | Na vol 90-dagen inhaalslag + continue optimalisatie |

---

## 9. Risico's & wat niet te doen

- ❌ **Geen lokale stadpagina's bouwen** (/iso-27001-amsterdam, enz.) zonder unieke substantie — doorway-page-risk.
- ❌ **Geen massa-content-productie** met AI zonder redactie — Helpful Content-afstraffing sinds dec 2025/maart 2026 updates.
- ❌ **Geen link-purchasing** — Google spam-updates 2025/2026 detecteren dit aggressief.
- ❌ **Niet Raoul's naam op juridische pagina's zetten** — staat expliciet in memory. Raoul's E-E-A-T-signalen komen op `/over`, `/raoul-haas`, blog-auteur-bylines.
- ❌ **Geen "over-optimization"**: exacte match anchors overal + stuffed keywords in titels → onnatuurlijk signaal.
- ❌ **Geen tijdelijke sitemap met 50+ dunne pagina's** om sitemap-grootte te pochen — kwaliteit > kwantiteit.
- ⚠️ **NIS2-venster is tijdelijk**: na medio 2026 (NL-wet in werking) vlakt "wat is NIS2" queries af. Grijp het nu, verleg focus daarna naar "NIS2 audit", "NIS2 compliance-onderhoud".
- ⚠️ **www vs non-www**: als je de 301-redirect niet instelt, blijf alert op indexering van `www.` via GSC.

---

## 10. Bijlagen / referentiebestanden

Alle bevindingen onderbouwd in de ondersteunende bestanden in `./seo-audit/`:

- **`01-inventory.md`** — volledige sitemap + robots.txt analyse + orphan-check
- **`02-pages.md`** — per-pagina HTML-analyse (titels, meta, schema, links, etc.)
- **`02-pages.csv`** — machine-leesbare per-pagina data
- **`02-pages-findings.md`** — prioriteit-gelabelde issue-lijst
- **`03-performance.md`** — CWV-inschatting, TTFB, render-blocking, optimalisatie-prio
- **`04-recent-developments.md`** — 2025/2026 ranking-factoren met bronverwijzing
- **`05-competition-keywords.md`** — concurrentie-analyse + 20 commerciële long-tails
- **`/pages/` subdirectory** — ruwe HTML + headers per pagina (audit-trail)

---

## 11. Direct-volgende-actie (als ik jou was)

Volgorde voor de eerste werkdag — maximaal bang-for-buck:

1. **Fix `/blog` → 403** (30 min) — grootste crawl-fout
2. **QW2 + QW3**: defer alle scripts, 301 www→non-www (45 min)
3. **GSC property claimen + sitemap submitten** (30 min)
4. **TF1 + TF5**: Product-schema `/bestellen` + author.url fix (2 uur)
5. **Start `/over` + `/raoul-haas` outline** (rest van de dag, publish binnen week 1)

Als je dit in de eerste 48 uur doorzet, heb je binnen 2 weken meetbare beweging in GSC-impressions.
