# Fase 4 — Relevante ranking-ontwikkelingen 2025/2026

**Datum onderzoek:** 2026-04-22
**Scope:** alleen wat direct van toepassing is op Annex27 (B2B dienstverlener, ISO 27001 + NIS2 compliance voor MKB, NL + BE).

## 1. Core Web Vitals — INP is de nieuwe scherprechter

**Harde feiten (2025/2026):**
- INP (Interaction to Next Paint) verving FID als Core Web Vital op **12 maart 2024**.
- De officiële drempel is **<200 ms = "goed"**, **200–500 ms = needs improvement**, **>500 ms = poor**.
- Recente ranking-analyses wijzen op strengere bands voor **ranking-stabiliteit** (<150 ms).
- **43% van alle sites** faalt de 200-ms drempel — INP is in 2026 de meest-gefaalde CWV.
- Google's **maart 2026 core update** tilde site-speed expliciet op als directe ranking-factor.

**Implicatie voor Annex27:**
- TTFB en HTML-payload zijn in orde (Fase 3). INP-risico zit bij **`/bestellen`** (Supabase SDK sync in head) en pagina's met veel DOM-nodes.
- Aanbeveling: breek grote JS-taken op, lazy-load Supabase SDK tot bij user-interactie, defer `analytics.js`.

**Bronnen:**
- [Interaction to Next Paint becomes a Core Web Vital on March 12 — web.dev](https://web.dev/blog/inp-cwv-march-12)
- [Core Web Vitals 2026: Fix LCP, CLS & INP Fast — W3Era](https://www.w3era.com/blog/seo/core-web-vitals-guide/)
- [Site Speed and Rankings 2026 — Digital Applied](https://www.digitalapplied.com/blog/site-speed-rankings-2026-march-core-update-performance)

## 2. Google AI Overviews & AI Mode — wat echt werkt in 2026

**Wat er veranderde:**
- AI Overviews groeiden **+58% YoY** over 9 industrieën (2026 data).
- Voor B2B SaaS en compliance-diensten verschijnen AI Overviews steeds vaker bovenaan de SERP — traditionele rankings verliezen CTR-share.

**Rankingsignalen die AI Overviews bepalen (onderbouwd met correlaties uit industry-research):**

| Signaal | Effect op AI-selectie |
|---|---|
| **Semantic completeness** (volledigheid antwoord) | r = 0.87 (sterkste voorspeller). Content >8.5/10 = 4.2× meer gecit. |
| **Vector embedding alignment** (cosine similarity >0.88) | 7.3× meer citaties |
| **E-E-A-T signals** (auteur, kwalificaties, author-page) | 96% van citaties komt van E-E-A-T-sterke bronnen |
| **Entity density** (≥15 herkende entities per pagina) | 4.8× meer selectie |
| **Structured data** (FAQPage, Article, HowTo, Product) | +30–40% AI-zichtbaarheid |
| **Extraction-ready passages** (100–300 woorden, vraag+direct antwoord) | 62% van featured content zit in deze range |
| **Topical authority** (cluster van 8–12 onderling gelinkte pagina's) | Minimum voor betrouwbare citaties in een topic |

**Implicatie voor Annex27:**
- FAQ op /nis2, /faq en blog-posts zijn goed — dit zijn AI-citation-prime formats.
- **Mankement:** geen zichtbare **auteurspagina** voor Raoul (Lead Auditor). Geen `/over-ons` / `/team` / `/auteur/raoul-haas`. Blogs hebben wel JSON-LD Person-markup, maar géén zichtbare bio-pagina waar Google/AI naartoe kan linken ter verificatie. Dit is een groot gemis voor E-E-A-T én AI-Overview-citatie.
- **Entity-density** rond ISO 27001 is OK op homepage (A-controls, clauses, NIS2, AVG, ENISA…) maar kan systematischer: voeg entities als "Annex A 5.19-23", "ISO 27002:2022", "CCB", "Cyberbeveiligingswet", "ENISA", "CISA", "Raad van Europa", etc. toe.
- **Clustering:** je hebt 5 blog-posts. Voor topical authority rond ISO 27001 + NIS2 zijn dit er eerder 8–12 nodig — maar het moeten **commercial-intent pagina's** zijn, geen fluff.

**Bronnen:**
- [Google AI Overviews Ranking Factors: 2026 Guide — Wellows](https://wellows.com/blog/google-ai-overviews-ranking-factors/)
- [Google AI Overview: New Ranking Signals That Matter in 2026 — Mike Khorev](https://mikekhorev.com/google-ai-overview)
- [Google AI Overviews Surge 58% Across 9 Industries — ALM Corp](https://almcorp.com/blog/google-ai-overviews-surge-9-industries/)
- [Generative Engine Optimization (GEO): Complete 2026 Guide — Enrich Labs](https://www.enrichlabs.ai/blog/generative-engine-optimization-geo-complete-guide-2026)

## 3. Helpful Content System — niet meer een update, nu het algoritme

**Status 2026:**
- HCS is geen losse update meer; het is **volledig geïntegreerd** in Google's core ranking.
- **December 2025** update + **maart 2026** core update waren de meest volatiele in jaren.
- Feb 2026: Discover-only update.
- E-E-A-T reikwijdte **uitgebreid buiten YMYL** (health/finance) naar alle sectoren — **B2B compliance is nu YMYL-achtig** (beslist over bedrijfsveiligheid).
- Kernconcept 2026: **"Information Gain"** — unieke waarde boven alternatieven.

**Implicatie voor Annex27:**
- Als compliance-adviseur val je nu feitelijk onder het verzwaarde E-E-A-T-regime.
- **Experience** bewijzen is essentieel: cases, klantlogo's, specifieke audit-ervaring van Raoul (KvK-aantallen, branches, jaren als Lead Auditor).
- **Information Gain** = iets bieden wat concurrent-pagina's niet hebben. Dit zit al in blog-posts (bv. "A.5.19-23" in supply-chain post, prijsranges in kosten-post). Voor dienstpagina's is het minder duidelijk — `/gap-analyse` heeft 244 woorden, amper differentiatie vs. concurrent-scans.

**Bronnen:**
- [Google Core Updates 2026: Timeline — Dataslayer](https://www.dataslayer.ai/blog/google-core-update-december-2025-what-changed-and-how-to-fix-your-rankings)
- [Google Helpful Content Update And Its Relevance in 2026 — Hobo Web](https://www.hobo-web.co.uk/the-google-helpful-content-update-and-its-relevance-in-2026/)
- [Google Algorithm Updates Dec 2025–2026 — Medium](https://medium.com/@frothose46/google-algorithm-updates-dec-2025-2026-what-really-changed-why-rankings-drop-and-how-to-stay-354b5b772e0b)

## 4. Structured data — wat echt ranking-waarde heeft in 2026

**Veranderingen sinds 2023:**
- **FAQPage rich results op SERP** zijn grotendeels teruggeschroefd — alleen nog zichtbaar voor government / well-known health. **Maar:** FAQPage schema blijft belangrijk voor **featured snippet eligibility** en **AI Overview citation probability**.
- **HowTo rich results** zijn eveneens ingeperkt (mobile-only, beperkte eligibility). Schema zelf telt nog wel mee.
- **Product / Offer schema met prijsranges** levert nog steeds rijke price-snippets op.
- **Organization + ProfessionalService** (combinatie) op homepage is een sterke E-E-A-T + Knowledge Graph signal.
- **Article met `author.Person` + `author.url`** is kritiek — en die `author.url` moet naar een **bestaande auteurspagina** linken.

**Implicatie voor Annex27:**
- ✅ Homepage-schema is uitstekend (ProfessionalService, Organization, OfferCatalog, ContactPoint).
- ✅ Blog-schema is correct (Article, Person).
- ⚠️ Blog `author.Person` linkt (hopelijk) naar een `/over-raoul-haas` of `/auteur/raoul` die niet bestaat → broken author reference. **Verifieer in JSON-LD en bouw de auteurspagina.**
- ❌ `/bestellen` mist Product+Offer schema (zie Fase 2 K4).
- ❌ `/nis2` mist Service+Offer schema (alleen FAQPage).
- ❌ `/trust` mist Organization-schema met aanvullende trust-signals (ISO 27001 Lead Auditor-certificering van Raoul kan in `knowsAbout`/`hasCredential`/`award`).

**Bronnen:**
- [Schema Markup in 2026: Why It's Now Critical — ALM Corp](https://almcorp.com/blog/schema-markup-detailed-guide-2026-serp-visibility/)
- [Schema Markup Guide 2026 — Kerkar Media](https://kerkarmedia.com/schema-markup-guide/)
- [Stand out in Search with FAQ Rich Results — Schema App](https://www.schemaapp.com/schema-markup/stand-out-in-search-with-faq-rich-results/)

## 5. E-E-A-T voor B2B dienstverleners — concrete signalen

Samengevat uit de bovenstaande bronnen + Google Quality Rater Guidelines (publiek):

| Signaal | Status Annex27 | Actie |
|---|---|---|
| **Over-ons / Team pagina** met gezichten, namen, functies | ❌ Bestaat niet | **Maak `/over` of `/team`** |
| **Auteurs-bio pagina** met kwalificaties (Raoul = ISO 27001 Lead Auditor) | ❌ Bestaat niet | **Maak `/raoul-haas` of `/auteur/raoul`** — link vanuit elke blog-post Person-schema |
| **Gepubliceerde credentials / certificaten** (zichtbaar, verifieerbaar) | ⚠️ Niet zichtbaar | Toon Lead Auditor-certificaat-nummer + uitgevende instantie |
| **Cases / klant-testimonials** met echte namen/bedrijven | ❌ Niet zichtbaar | Overweeg (met toestemming) 2–3 case snippets op /trust |
| **NAP-consistentie** (Naam-Adres-Telefoon) | ⚠️ deels | KBO/BTW staat op /algemene-voorwaarden, niet op schema of /over |
| **External mentions** (branche-pers, podcasts, speaker-profielen) | Onbekend | Off-site strategie in Fase 5 |
| **Trust-pagina met technische security maatregelen** | ✅ `/trust` bestaat | Uitbreiden met auditor-kwalificaties |
| **HTTPS + security headers** | ✅ | Al uitstekend |
| **Privacy/AV up-to-date** | ✅ | |

**Let op memory-feedback:** Raoul's naam staat **bewust niet op juridische pagina's** (uit `reference_annex27_business.md`). Dit is een **spanning** met E-E-A-T: voor Google wil je juist een zichtbare, aantoonbare expert. Advies: bouw een `/over` en `/auteur`-pagina met Raoul's kwalificaties en link daar vanuit blog-posts, *zonder* de naam op algemene-voorwaarden/privacy te zetten (houd die juridisch "Annex27"-only).

## 6. Link-building 2026 — authority > volume

**Wat werkt niet meer:**
- PBN's, betaalde directory-listings, link-exchanges, scaled outreach → aggressief afgestraft sinds de 2025 spam-updates.

**Wat wel werkt voor B2B compliance:**
1. **Digital PR** — nieuwswaardige stukken over NIS2-deadlines, cyberincident-analyses (Annex27 doet dit al: `/blog/supply-chain-attacks-mkb` is een uitstekende linkable asset).
2. **Linkable assets** — `/rapport-voorbeeld` is precies dit. Kan worden uitgebreid met: NIS2-checklist PDF, ISO 27001 gap-analyse template (public), kosten-calculator.
3. **Guest posts** op compliance-autoriteiten: CIO Magazine, Computable, AG Connect, Emerce, Tweakers (zorg), Automatisering Gids.
4. **Mentions/unlinked** → achterhaal en converteer (`site:nl -annex27.nl "annex27"` zoekopdracht + HARO/Sourcebottle).
5. **Strategische partnerships** — certificatie-instellingen (DEKRA, BSI, DNV, Kiwa), IT-MSP's, bookkeepers die ISO 27001-klanten hebben.

**Niet doen:**
- Brandverzadigde gastposts op generieke marketing-blogs
- Directory-submissions in massa
- Link-purchasing

**Bronnen:**
- [Definitive Guide to Link Building in 2026 — ALM Corp](https://almcorp.com/blog/definitive-guide-link-building-2026/)
- [Why Backlink Services Now Focus On Authority, Not Volume — ITMunch](https://itmunch.com/backlink-services-b2b-authority/)
- [15 Proven B2B SaaS Link Building Strategies for 2026 — Jeenam](https://jeenaminfotech.com/blog/b2b-saas-link-building-made-simple/)

## 7. Lokale SEO — is dit relevant voor Annex27?

Uit memory: hoofdmarkt NL (volume), BE secundair. Service wordt digitaal geleverd. Dus:

- **Google Business Profile:** zinnig, ja — maakt Annex27 zichtbaar in local pack voor queries zoals "iso 27001 consultant [stad]". Zelfs als je remote werkt kun je "Service area business" instellen.
- **NAP-consistentie:** KBO + BTW staan op juridische pagina. Voor Local SEO wil je in GBP + homepage structured data `address` (land-gericht, niet straatadres nodig).
- **Dutch-only second GBP?** Aangezien Annex27 BE-geregistreerd is (KBO) maar NL hoofdmarkt bedient, kun je óf:
  - Eén GBP in BE-locatie, service-area NL+BE (transparant).
  - Een aparte NL-entiteit (niet aanbevolen — complex, niet nodig).
- **Lokale landingspagina's** zoals `/iso-27001-amsterdam`, `/iso-27001-rotterdam` zijn **risicovol** als ze dun of gekopieerd zijn (Google straft "doorway pages" af). Alleen bouwen als er substantie is (lokale cases, events, etc.).

**Aanbeveling voor Annex27:** GBP opzetten met service-area (Nederland + België), maar **geen dunne lokale landingspagina's**. Investeer in één sterke `/iso-27001-nederland` en één `/iso-27001-belgie` met land-specifieke content (Cyberbeveiligingswet NL, CCB BE, etc.).

## 8. Branchespecifieke rankingfactoren

**Directe concurrenten (uit search):**
- **MaasISO** (maasiso.nl) — zichtbaar voor "ISO consultant MKB"
- **KAM Groep** (kam-groep.nl) — generieke KAM + ISO positionering
- **CertificeringsAdvies Nederland** — generieke certificeringen
- **DigiTrust** (digitrust.nl) — ISO 27001-specifiek, lijkt sterk
- **Vanta, Conformio** — internationale SaaS, concurreren op "iso 27001 compliance software"

**Belangrijkste observatie:** Annex27's positionering (digitaal platform + menselijke Lead Auditor review) is een **hybride** die noch de pure consultancies (MaasISO, KAM) noch de pure SaaS (Vanta, Conformio) uitspeelt. Dit is een **differentiatie-asset** dat bijna nergens op de site wordt uitgebuit. Op homepage "voor fractie van de prijs van een traditionele consultant" staat er wel — maar nergens een *side-by-side comparison* of dedicated pagina.

**Actie:** Maak een `/waarom-annex27` (of integreer in `/`) met directe vergelijking: consultant vs. SaaS vs. Annex27 — zichtbaar en Google-crawlable.

**Bronnen:**
- [ISO 27001 consulting MKB Nederland 2026 — DigiTrust](https://www.digitrust.nl/en/articles/welke-bedrijven-hebben-iso-27001-certificering-nodig/)
- [MaasISO homepage](https://www.maasiso.nl/)
- [De 4 beste ISO 27001 compliance software voor 2026 — Vanta](https://www.vanta.com/resources/best-iso-27001-compliance-software)

## 9. NIS2-timing — een kortstondig ranking-venster

- **NL Cyberbeveiligingswet:** verwacht in werking halverwege 2026 (ingeschat).
- **BE:** NIS2 omgezet, deadline voor essential entities: **18 april 2026**.
- Traffic naar `nis2`-queries piekt nu en de komende 12 maanden. **Daarna vlakt de nieuwheid af.**

**Actie:** Annex27's NIS2-content (`/nis2`, blog `/nis2-deadline-2026`) is goed getimed. Meer diepgang en schema (Service + Offer) + interne linking tussen beide NIS2-pagina's kan kortetermijn ranking winnen. Na medio-2026 verschuift de focus naar "NIS2 audit", "NIS2 compliance check" i.p.v. "wat is NIS2".

**Bronnen:**
- [EU NIS2 Compliance — Google Cloud](https://cloud.google.com/security/compliance/eu-nis2)
- [NIS2: 18 April 2026 deadline — CCB Belgium](https://ccb.belgium.be/news/nis2-18-april-2026-deadline-what-essential-entities-must-have-place)

## Samenvatting Fase 4 — wat is meegenomen in het eindplan

1. **INP < 200 ms** als kritieke CWV → behandeld in Fase 3 performance-acties.
2. **AI Overview optimalisatie** → author-pagina, entity-density, extraction-ready passages, Product/Service schema.
3. **E-E-A-T voor B2B compliance** → `/over`, `/raoul-haas`, zichtbare credentials.
4. **Structured data**: Product/Offer op /bestellen, Service op /nis2, author-linking op blog.
5. **Topical authority cluster** → 3–5 nieuwe commerciële pagina's nodig (niet blogs).
6. **Link-building via digital PR** → linkable assets + authoritative outreach.
7. **Lokale SEO** → GBP + 2 land-specifieke pagina's (NL + BE), geen dunne stad-pagina's.
8. **NIS2-venster** → nu aangrijpen met diepere NIS2-hub.
