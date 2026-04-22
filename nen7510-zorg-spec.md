# NEN 7510 / Zorgsector-module — Productspecificatie

**Context:** 2026-04-22 — opvolger van `nis2-addendum-spec.md`. Zelfde architectuurpatroon (Model B addendum), toegepast op zorgnorm NEN 7510 + uitgebreid met sector-bewuste vragenboom.

**Doel:**
1. Zorgsector krijgt NEN 7510-verwerking (quickscan + gap + optioneel addendum).
2. Alle sectoren krijgen een sector-overlay: vragen en rapport-taal passen zich aan.
3. Klant ziet dat Annex27 de sector-context daadwerkelijk toepast (niet stil op de achtergrond).

---

## 1. Architectuur — drie lagen

```
┌─────────────────────────────────────────────────────────────┐
│  LAAG 1 — BASIS (gratis in quickscan, in gap-rapport)      │
│  ISO 27001 Annex A controls (93 stuks), ~30 kernvragen     │
│  in quickscan, ~80-120 in gap                              │
└─────────────────────────────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│  LAAG 2 — SECTOR-OVERLAY (gratis, altijd actief)           │
│  Sectorkeuze triggert:                                      │
│    • Extra vragen (5-15 per sector)                        │
│    • Sector-taal in vragen en rapport                       │
│    • Sector-specifieke scoring-context                      │
│    • Voorbeelden uit de sector                             │
└─────────────────────────────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│  LAAG 3 — SECTOR-ADDENDUM (€295, optioneel, patroon NIS2)  │
│  Alleen voor sectoren met eigen norm:                       │
│    • Zorg      → NEN 7510-addendum                          │
│    • IT/SaaS   → SOC 2 readiness-addendum                  │
│    • Financieel → DORA-addendum                            │
│    • Overheid  → BIO-mapping-addendum                       │
│  NIS2-addendum blijft parallel bestaan (juridische laag)    │
└─────────────────────────────────────────────────────────────┘
```

**Waarom deze drie lagen:**

- **Laag 1** is de kern — wat je verkoopt voor €795. Wijzigt niet.
- **Laag 2** is de **zichtbare differentiator**. Geen meerprijs, maar maakt instant duidelijk dat Annex27 de sector kent. Concurrenten hebben dit niet.
- **Laag 3** is de premium-uitbreiding — alleen voor klanten die in sectoren zitten met een eigen norm die diepgang vereist. Zelfde prijsstructuur als NIS2-addendum.

Een zorgklant kan dus kiezen uit:
- Gratis quickscan (Laag 1 + Laag 2-zorg-overlay) — ziet direct dat zorg-context wordt toegepast
- Gap-analyse €795 (Laag 1 + Laag 2)
- Gap + NEN 7510-addendum €1.090 (Laag 1 + 2 + 3)
- Gap + NIS2 + NEN 7510 €1.385 (sommige zorginstellingen vallen onder beide — essential entity in health-sector)

---

## 2. Sector-taxonomie

Eerste vraag in quickscan én in bestel-flow. Radio-keuze, verplicht.

| Hoofdsector | Subsector-opties | Laag 3-addendum beschikbaar |
|---|---|---|
| **Zorg & welzijn** | Ziekenhuis · Huisartsen/tandarts/paramedisch · GGZ · VVT/thuiszorg · Medisch specialist/zelfstandige kliniek · Zorgsoftware/leverancier · Apothekers · Kinderopvang/jeugdzorg · Anders zorg | **NEN 7510** (€295) |
| **IT & digital** | SaaS/cloudsoftware · MSP/IT-dienstverlener · Webbureau/development · Hosting/infra · Cybersecurity-dienstverlener · Data/analytics · Anders IT | **SOC 2** (€295) + NIS2 (€295) |
| **Financieel & advies** | Accountantskantoor · Administratiekantoor · Belastingadviseur · Financieel planner · Verzekeraar/tussenpersoon · Betaaldienstverlener · Anders financieel | **DORA** (€295) als klant onder DORA valt |
| **Juridisch** | Advocatuur · Notariaat · Deurwaarder · Mediator · Juridisch adviesbureau | (geen specifieke norm — wel strengere vertrouwelijkheid-overlay) |
| **Bouw & techniek** | Bouwbedrijf · Installateur · Architect/ingenieursbureau · Projectontwikkelaar · Infra/aannemer | (geen specifieke norm) |
| **Productie & industrie** | Maakindustrie · Voedsel/agro · Chemie · Logistiek · Groothandel | NIS2-relevant afh. subsector |
| **Retail & e-commerce** | Webshop · Fysieke retail · Marktplaats/platform | PCI-DSS niet apart; wel zwaar AVG-accent |
| **Onderwijs** | PO/VO · MBO · HBO/WO · Particulier onderwijs · Opleidingsbureau | SURFaudit-mapping-addendum (€295) |
| **Overheid & publieke dienst** | Gemeente · Waterschap/provincie · ZBO · Semi-publiek | **BIO** (€295) |
| **Anders** | Vrije tekstveld | (generieke gap zonder overlay) |

**Voor elke subsector** is een vragenboom-profiel gedefinieerd (zie Sectie 3). De subsector bepaalt ook welk addendum relevant is.

---

## 3. Sector-vragenboom — hoe de overlay werkt

### 3.1 Technisch model

Elke quickscan-/gap-vraag heeft metadata:

```yaml
question_id: Q-AC-01
control_ref: A.5.15   # ISO 27001 Annex A
base_question: "Is er een beleid voor toegangsbeheer tot bedrijfsgegevens?"
scoring: weight=3
sectors:
  zorg:
    phrasing: "Is er een beleid dat autorisatie tot het medisch dossier koppelt aan de behandelrelatie van de gebruiker?"
    extra_examples: ["EPD-toegang", "break-glass-procedure", "patient-context-based access"]
    additional_questions: [Q-ZORG-AC-01, Q-ZORG-AC-02]
    scoring_boost: +1   # zwaarder gewicht in zorg
    sector_control_refs: [NEN 7510 §9.4.1, NEN 7510 §9.4.2]
  saas:
    phrasing: "Is er een beleid dat least-privilege-toegang tot klantdata en productieomgevingen borgt?"
    extra_examples: ["RBAC", "just-in-time access", "tenant-isolation"]
    sector_control_refs: [SOC 2 CC6.1, CC6.2, CC6.3]
  bouw:
    phrasing: "Is er een beleid dat toegang tot bouwtekeningen, calculaties en projectdocumenten beheert?"
    extra_examples: ["BIM-modellen", "aannemers-toegang", "projectarchief"]
    additional_questions: [Q-BOUW-AC-01]
```

Bij renderen van de vragenboom kiest de engine per vraag de juiste variant op basis van de gekozen sector. Geen variant? Dan gebruik `base_question`.

### 3.2 Zorg-overlay — extra vragen voor de basisgap

Bovenop de ~30 quickscan-vragen en ~80-120 gap-vragen komen deze er voor zorg **standaard** bij (gratis, in Laag 2):

**Identiteit & patiënt-context:**
1. Koppelt uw autorisatiemodel toegang tot medische gegevens aan de behandelrelatie van de gebruiker?
2. Heeft u een noodprocedure ("break-glass") waarbij een zorgverlener in acute situatie medisch dossier kan inzien buiten zijn normale autorisatie, met volledige logging achteraf?
3. Hoe authenticeert u zorgverleners (UZI-pas, Idensys, eIDAS, ORCID)?

**EPD & systeemintegratie:**
4. Welk elektronisch patiëntendossier (EPD) gebruikt u? (HiX, Epic, Nexus, MicroHIS, Medicom, etc.)
5. Is uw EPD aangesloten op het Landelijk Schakelpunt (LSP), Mitz of VIPP-programma's?
6. Hoe worden medische apparaten (diagnostiek, monitoring) beveiligd en gesegmenteerd op het netwerk?

**Patient-data specifiek:**
7. Wordt elk inzagemoment in het medisch dossier gelogd inclusief reden van inzage?
8. Is er een proces voor pseudonimisering van patiëntdata bij onderzoeksdoeleinden?
9. Hoe gaat u om met BSN-verwerking (WBSN-Z)?
10. Is de WGBO-bewaartermijn (standaard 20 jaar) in uw data-lifecycle verankerd?

**Toezicht & incidenten:**
11. Is uw datalek-meldprocedure dubbel ingericht (Autoriteit Persoonsgegevens + IGJ bij patiëntveiligheid-impact)?
12. Hoe borgt u continuïteit van kritieke zorg-IT (EPD, medische apparatuur) bij een cyberincident? (klinische downtime-procedure)

**Samenwerking & keten:**
13. Hoe sluit u verwerkersovereenkomsten met software-leveranciers, laboratoria, en verwijscentra?
14. Bent u aangesloten bij Z-CERT? Zo ja, hoe gebruikt u hun dreigingsinformatie?

### 3.3 SaaS-overlay (ter vergelijking — patroon voor andere sectoren)

Extra vragen voor SaaS/softwareleverancier-klanten:

1. Hoe scheidt uw architectuur klantgegevens (single-tenant / multi-tenant / isolated schemas)?
2. Documenteert u welke data-residency-regio elke klant heeft (EU-only, US-allowed)?
3. Heeft u een geformaliseerd SDLC met security gates (threat modelling, dependency-scanning, SAST/DAST)?
4. Hoe gaat u om met customer-managed encryption keys (BYOK/CMK)?
5. Wordt audit-logging per tenant beschikbaar gesteld aan de klant?
6. Hoe zijn sub-processors geïnventariseerd en doorgegeven (DPA-transparantie)?
7. Is er een publieke status-page met incident-communicatie?
8. Wordt production-access door engineers gelogd en gelimiteerd (JIT)?

### 3.4 Bouw-overlay (kort voorbeeld)

1. Hoe beheert u toegang tot bouwtekeningen en BIM-modellen tussen onderaannemers?
2. Zijn projectarchieven beveiligd tegen ongeautoriseerde wijziging (integriteit vs. opzettelijke fraude)?
3. Welke controls zijn er op mobiele apparatuur op bouwlocaties (laptops, tablets van uitvoerders)?

### 3.5 Rapport-taal verschilt per sector

Zelfde bevinding, andere verwoording:

| Sector | Bevindings-formulering voor dezelfde ISO-control A.5.15 |
|---|---|
| Generiek | "Er is geen gestructureerd toegangsbeheer-beleid voor bedrijfsgegevens." |
| Zorg | "Er is geen beleid dat EPD-autorisatie koppelt aan de behandelrelatie. NEN 7510 §9.4.2 en WGBO vereisen dat elke raadpleging herleidbaar is naar een legitieme behandelrelatie." |
| SaaS | "Er is geen gedocumenteerd least-privilege-model voor productieomgevingen. Voor SOC 2 Type II is dit een harde vereiste (CC6.1). Enterprise-klanten gaan hier standaard naar vragen in hun DPA-review." |
| Bouw | "Toegang tot bouwtekeningen en projectdocumenten is niet geformaliseerd. Bij onderaanneming is dit een contractueel risico." |

De klant ziet dus niet alleen een bevinding, maar **een bevinding in zijn taal**.

---

## 4. Zichtbaarheid — hoe de klant het "ziet gebeuren"

Drie momenten waarop sector-aanwezigheid expliciet getoond wordt.

### 4.1 Bij start quickscan

Na sector-keuze, vóór vragen beginnen: **sector-badge-scherm** (overgangspagina, 5 seconden):

```
┌────────────────────────────────────────────────────┐
│  U heeft "Huisartsenpraktijk" geselecteerd.        │
│                                                     │
│  We passen uw scan aan op basis van:               │
│    ✓ NEN 7510 (zorg-specifieke informatie-         │
│      beveiligingsnorm)                             │
│    ✓ WGBO-bewaartermijnen en behandelrelatie       │
│    ✓ Typische EPD-systemen in uw subsector        │
│    ✓ LSP/Mitz-context                              │
│    ✓ IGJ-meldplicht bij incidenten                 │
│                                                     │
│  14 extra vragen en een aangepaste scoring-        │
│  context worden gebruikt.                          │
│                                                     │
│  [ Start scan →  ]                                 │
└────────────────────────────────────────────────────┘
```

Dit is één minuut werk om te bouwen maar enorme vertrouwens-impact. Concurrenten tonen nergens sector-logica.

### 4.2 Tijdens vragen — sector-badge per vraag

Elke sector-specifieke vraag krijgt een zichtbaar label:

```
┌──────────────────────────────────────────────────┐
│  [ZORG-SPECIFIEK · NEN 7510 §9.4.2]              │
│                                                   │
│  Koppelt uw autorisatiemodel toegang tot          │
│  medische gegevens aan de behandelrelatie         │
│  van de gebruiker?                                │
│                                                   │
│  💡 Waarom we dit vragen: NEN 7510 vereist dat   │
│  alleen zorgverleners met een actieve            │
│  behandelrelatie met de patiënt toegang hebben.  │
│  In HiX en Epic heet dit "patient context".       │
│                                                   │
│  ◯ Ja, technisch afgedwongen                     │
│  ◯ Ja, procedureel maar niet afgedwongen         │
│  ◯ Nee                                            │
│  ◯ Weet ik niet                                  │
└──────────────────────────────────────────────────┘
```

Het `[ZORG-SPECIFIEK · NEN 7510 §...]`-label én het `💡 Waarom we dit vragen`-blokje maakt direct voelbaar dat dit geen generieke scan is.

### 4.3 In de rapportage

Drie plaatsen:

- **Voorpagina rapport:** "Gap-analyse ISO 27001 — met zorg-overlay (huisartsenpraktijk)" i.p.v. generieke titel.
- **Score-overzicht:** twee scores naast elkaar: "ISO 27001 Annex A: 73%" en "Zorg-overlay (NEN 7510 basis): 61%". Verschil legt Annex27 direct uit.
- **Aparte sectie "Zorg-specifieke bevindingen":** 5-10 punten die alleen voor zorg relevant zijn (WGBO-bewaartermijn, LSP-koppeling, break-glass, enz.).

---

## 5. Laag 3 — NEN 7510-addendum (€295)

Zelfde structuur als NIS2-addendum. Alleen koopbaar i.c.m. gap. Content-delta die **niet** in Laag 2-overlay zit:

### Sectie 1 — Volledige NEN 7510 Annex A-mapping

- Alle ~30 zorg-specifieke controls uit NEN 7510-2:2017 één voor één doorgelopen met klant-specifieke scoring
- Per control: status (compliant / partial / gap / n.v.t.) + onderbouwing + advies
- Mapping-tabel NEN 7510 ↔ ISO 27001 Annex A (laat klant zien welke controls dubbel tellen)

### Sectie 2 — WGBO + Wkkgz + WvGGZ compliance-check

- Medisch dossier bewaartermijn (20 jaar vanaf laatste wijziging — langer voor kinderen en bij erfelijkheidsonderzoek)
- Inzagerecht patiënt (binnen 4 weken, kosteloos)
- Correctie- en verwijderrecht
- Wkkgz: melden van calamiteiten aan IGJ (incl. digitale datalek met patiëntveiligheidsimpact)
- WvGGZ (voor GGZ): aparte regels voor dwangmaatregelen-dossiers
- Template: Wkkgz-meldformulier + IGJ-communicatiebrief

### Sectie 3 — Keten & uitwisseling (LSP / Mitz / VIPP / KIK-V)

- Status aansluiting LSP (Landelijk Schakelpunt) met aandachtspunten
- Mitz-toestemmingsbeheer (vervanger van OPT-IN)
- VIPP-programma-koppeling (ziekenhuizen + GGZ-subsidies)
- Verwijsindex Risicojongeren (VIR) indien kinder-/jeugdzorg
- KIK-V (Keteninformatiesysteem Kwaliteit Verpleeghuiszorg) indien VVT
- Template: verwerkersovereenkomst met zorg-leverancier (EPD-vendor, lab, apotheek)

### Sectie 4 — BSN & identificatie

- WBSN-Z: voorwaarden voor BSN-verwerking in zorg
- UZI-pas (zorgverlener) en DigiD/eIDAS (patiënt) — authenticatie-eisen
- Proces voor BSN-verificatie bij intake
- Datalek met BSN = automatisch AP-meldplicht

### Sectie 5 — Klinische continuïteit & cybercrisis

- Downtime-procedure EPD (papieren terugval, SOP's)
- Klinische impact-inschatting bij incident (patiëntveiligheid vs. privacy)
- Z-CERT-lidmaatschap en threat-intel-gebruik
- Template: cybercrisisplan met klinische rol (MT + hoofdbehandelaar + CISO)
- IGJ-meldplicht bij significant incident met patiëntveiligheids-risico

### Sectie 6 (bonus als ruimte) — Medical device security

**Alleen activeren** als klant in subsector "zorgsoftware/leverancier" of "ziekenhuis met veel MRI/CT/IC" zit:

- MDR-implicaties (Medical Device Regulation) voor software als medisch hulpmiddel
- Netwerksegmentatie medisch versus kantoorautomatisering
- Patch-management-dilemma (vendor-locked firmware)
- CE-markering-relevantie voor in-huis ontwikkelde tools

---

## 6. Wijzigingen in bestel-flow & UI

### 6.1 `/bestellen` — derde addon-checkbox

Naast de NIS2-addendum-checkbox (zie `nis2-addendum-spec.md`) komt er nu een sector-afhankelijke tweede checkbox:

```
Gap-analyse                                    €795
──────────────────────────────────────────────────
[...beschrijving gap...]

Optionele addenda:
☐ + NIS2-addendum (€295)
    Juridische NIS2-scope, bestuur, incident-
    notification, supply-chain, registratie.

[conditioneel zichtbaar op basis van sectorkeuze:]

☐ + NEN 7510-addendum (€295)   ← alleen voor zorg
    Volledige NEN 7510-2 mapping, WGBO/Wkkgz-
    check, LSP/Mitz-status, BSN-proces,
    klinische continuïteit.

☐ + SOC 2-addendum (€295)      ← alleen voor IT/SaaS
    [...]

☐ + BIO-addendum (€295)        ← alleen voor overheid
    [...]

☐ + DORA-addendum (€295)       ← alleen voor financieel
    [...]

Totaal: €795 + aangevinkte addenda
```

De addon-checkbox is alleen zichtbaar als de sectorkeuze in de gap-flow het relevante addendum triggert. Zo blijft de pagina overzichtelijk en krijgt elke klant de addenda-opties die echt voor hem zinvol zijn.

### 6.2 Pricing voor een zorgklant

| Combinatie | Prijs |
|---|---|
| Alleen gap (zorg-overlay gratis meegenomen) | €795 |
| Gap + NEN 7510 | €1.090 |
| Gap + NIS2 (zorg valt soms onder NIS2 als essential entity) | €1.090 |
| Gap + NEN 7510 + NIS2 | €1.385 |
| Full stack (gap + NIS2 + NEN 7510 + Beleidspakket) | €2.180 |

### 6.3 Supabase data-model-uitbreidingen

```sql
-- Bestaande order-model wordt verrijkt met sector + subsector
ALTER TABLE orders ADD COLUMN sector TEXT;          -- bv. "zorg"
ALTER TABLE orders ADD COLUMN subsector TEXT;       -- bv. "huisartsen"
ALTER TABLE orders ADD COLUMN addons TEXT[];        -- bv. ["nis2","nen7510"]

-- Nieuwe tabellen voor sector-vragen
CREATE TABLE sectors (
  id TEXT PRIMARY KEY,          -- 'zorg','it_saas','bouw',...
  display_name TEXT,
  subsectors JSONB,             -- array of subsector objects
  addon_available TEXT[],       -- ['nen7510'] of ['soc2']
  overlay_intro TEXT            -- tekst voor badge-scherm §4.1
);

CREATE TABLE questions (
  id TEXT PRIMARY KEY,
  control_ref TEXT,             -- 'A.5.15'
  base_question TEXT,
  scoring JSONB,                -- weight etc.
  sector_variants JSONB         -- zie §3.1 YAML-model
);

CREATE TABLE sector_specific_questions (
  id TEXT PRIMARY KEY,
  sector_id TEXT REFERENCES sectors(id),
  subsector_filter TEXT[],      -- bv. ['ggz'] of leeg = alle zorg
  question_text TEXT,
  control_refs TEXT[],          -- bv. ['NEN 7510 §9.4.2','ISO A.5.15']
  why_we_ask TEXT               -- voor 💡-tooltip
);
```

### 6.4 Quickscan-flow aanpassingen

Huidige flow (aanname):
1. Landingspagina `/gap-analyse`
2. ~30 ISO-vragen
3. Score-resultaat

Nieuwe flow:
1. Landingspagina `/gap-analyse` — voegt sectorkeuze toe als **eerste stap**
2. Sector-keuze (verplicht) + subsector (verplicht waar relevant)
3. **Sector-badge-overgangsscherm** (§4.1) — 5 seconden, dismissable
4. Basis-vragen mét sector-phrasing per vraag
5. Sector-specifieke vragen (met `[ZORG-SPECIFIEK]`-label)
6. Score-resultaat met dubbele balk: ISO-score + sector-overlay-score
7. CTA: "Bekijk uw volledige gap-analyse met NEN 7510-addendum voor €1.090" (dynamisch per sector)

---

## 7. Bestaande pagina's — wijzigingen voor zorg

### Nieuwe sector-LP `/iso-27001-zorg` (of `/nen-7510`)

Tier 1-pagina uit SEO-plan (§5, was "Tier 3" in keyword-gap). Nu promoveren omdat we concrete sectormodule hebben.

**Structuur:**
1. H1: "ISO 27001 + NEN 7510 voor zorginstellingen en zorgsoftware"
2. Probleem-framing: WGBO + NEN 7510 + NIS2 + AVG art. 9 stapelen zich op voor MKB-zorg
3. Hoe Annex27 het aanpakt: zorg-overlay in gap (gratis) + NEN 7510-addendum (€295)
4. Wat de addendum toevoegt (de 5 secties uit §5)
5. Per-subsector blok (huisartsen / GGZ / VVT / medisch specialist / zorgsoftware)
6. FAQ met zorg-specifieke vragen
7. CTA: "Start gratis zorg-quickscan" → `/gap-analyse` met pre-geselecteerde sector=zorg

**Schema:** Service + Offer + areaServed NL/BE + audience=MedicalBusiness.

### Homepage

- Voeg een sector-strip toe: 6-8 sector-icoontjes met "Werkt voor: zorg · SaaS · accountants · bouw · onderwijs · overheid · ..."
- Link elk icoon naar zijn sector-LP (als bestaand) of naar `/gap-analyse?sector=X`

### `/faq`

Drie nieuwe zorg-specifieke vragen:

**Q: Dekt uw gap-analyse ook NEN 7510?**
A: Ja — op twee niveaus. In de standaard gap-analyse (€795) passen we de sector-overlay voor zorg toe: vragen en bevindingen gebruiken NEN 7510-context, WGBO-bewaartermijnen en EPD-specifieke autorisatie. Voor een volledige NEN 7510-2 mapping op alle ~30 zorg-specifieke controls, inclusief LSP/Mitz, BSN-proces en klinische continuïteit, is het optionele NEN 7510-addendum (€295) beschikbaar.

**Q: Moet ik als huisartsenpraktijk NEN 7510-gecertificeerd zijn?**
A: NEN 7510-certificering is niet voor elke zorgaanbieder wettelijk verplicht, maar de norm is de facto standaard onder Wkkgz en contractueel via zorgverzekeraars. IGJ gebruikt NEN 7510 als referentiekader bij toezicht. Voor LSP-aansluiting en sommige VIPP-subsidies is aantoonbare NEN 7510-naleving verplicht.

**Q: Hoe zit het met BSN-verwerking en de AVG?**
A: Verwerking van het BSN is in de zorg wettelijk toegestaan onder de WBSN-Z, mits u een zorgaanbieder bent in de zin van de Wet. Het addendum bevat een proces-check voor BSN-verificatie bij intake, logging van BSN-gebruik en datalek-meldprocedure specifiek voor BSN-incidenten.

### `/gap-analyse` landingspagina

Voeg een sector-voorbeeld-sectie toe: "Bekijk hoe de gap eruit ziet voor uw sector — kies uw sector:" met 6-8 sector-tegels. Klik toont een klein example-rapport-fragment (teaser) voor die sector.

---

## 8. Content-taken: wat moet daadwerkelijk geschreven

Dit document is de spec. De feitelijke content-productie:

| Content-item | Bron nodig |
|---|---|
| 14 zorg-specifieke vragen (§3.2) met waarom-teksten | NEN 7510-1/2:2017 + eigen auditor-ervaring Raoul |
| NEN 7510 ↔ ISO 27001 mapping-tabel | NEN 7510-2:2017 bijlage + NEN.nl norm-aankoop |
| WGBO/Wkkgz/WvGGZ compliance-templates (Sectie 2) | Wetgevingstekst + IGJ-guidance |
| LSP/Mitz/VIPP-addendum-content (Sectie 3) | VZVZ, Nictiz, Ministerie van VWS publicaties |
| BSN & WBSN-Z-proces (Sectie 4) | WBSN-Z + AP-guidance |
| Klinische continuïteit-template (Sectie 5) | Z-CERT-publicaties + ENISA health-guidance |
| Medical device-sectie (bonus) | MDR + NIS2 (health subsector) |
| SaaS-overlay content (voor andere sectoren) | SOC 2 Trust Services Criteria + ENISA Cloud security |
| Per-sector taxonomie-teksten (§2) | Eigen productpositionering |

**Belangrijk:** NEN 7510-1:2017 en NEN 7510-2:2017 zijn niet publiek — je moet ze bij NEN.nl kopen (~€150-€250 per norm). Dit is een eenmalige investering die inhoudelijk onmisbaar is. Raoul's ISO 27001 Lead Auditor-kennis is relevant maar niet voldoende — NEN 7510 heeft expliciete zorg-specifieke taal die je moet citeren.

Zonder de formele normtekst in huis is het niet verantwoord om een NEN 7510-addendum te verkopen waarin je op §9.4.2 wijst.

---

## 9. Uitvoering-volgorde

1. **NEN 7510-1:2017 + NEN 7510-2:2017 aanschaffen bij NEN.nl** (blokker 1)
2. **Sector-taxonomie & subsector-lijst vastleggen** (§2) — productbeslissing, geen code
3. **Vragenboom-metadata-model implementeren** (§3.1) — Supabase-schema uitbreiden
4. **Zorg-overlay content schrijven**: 14 sector-vragen + waarom-teksten + rapport-phrasings
5. **Sector-badge-overgangsscherm bouwen** (§4.1) — visuele differentiator eerst
6. **Per-vraag sector-badge + waarom-tooltip in UI** (§4.2)
7. **Rapport-template uitbreiden** met sector-overlay-sectie + dubbele score-balk
8. **NEN 7510-addendum content schrijven** (Sectie 1-5, bonus 6 optioneel)
9. **`/bestellen` UI: conditionele addon-checkboxes per sector**
10. **`/iso-27001-zorg` SEO-LP bouwen**
11. **Homepage sector-strip toevoegen**
12. **FAQ-vragen toevoegen**
13. **SaaS/bouw/overige sectoren** — herhaal §3 voor elke andere sector (kan parallel aan zorg)
14. **Addenda SOC 2 / BIO / DORA** — alleen bouwen als er commerciële vraag is (start met NEN 7510 + NIS2)

---

## 10. Meetbaarheid

- **Sector-keuze-distributie** in quickscan (wat is het %-aandeel zorg versus SaaS versus anders?)
- **Attach-rate gap → NEN 7510-addendum** (target 30-45% binnen zorg-subsector; hoger dan NIS2 omdat NEN 7510 harder gevraagd is in de zorg)
- **Completion-rate sector-overlay quickscan** (verval na sector-keuze — als >20% hier afhaakt is de sectorbadge-pagina te lang)
- **SEO-impressions `/iso-27001-zorg`** in GSC
- **Klantfeedback: "de scan voelt aan of jullie onze sector kennen"** — kwalitatieve vraag in post-rapport-enquête

---

## 11. Risico's

- **NEN 7510-norm niet in huis** = juridisch en inhoudelijk risico bij verkoop addendum. Blokkeerend.
- **Te veel sectoren tegelijk lanceren** = verdunning. Start met 2-3 sectoren waar je aantoonbare ervaring hebt (zorg, SaaS, + optioneel één die Raoul kent).
- **Over-belovende sector-expertise** = als klant merkt dat de overlay oppervlakkig is, schaadt dit brand. Kwaliteit > volume van sectoren.
- **NEN 7510:2024-update** — NEN werkt aan update; houd de norm-status in de gaten zodat addendum niet verouderd raakt.
- **Medical device / MDR-overlap** = juridisch complex terrein. Begin conservatief (alleen als klant specifiek aangeeft MD-software/-hardware te leveren).

---

## Samenvatting

**Wat gebouwd wordt:**
1. Sector-keuze als verplichte eerste stap in quickscan en bestel-flow.
2. Vragenboom met per-sector overlay — gratis, zichtbaar via badges + waarom-tooltips.
3. NEN 7510-addendum (€295) voor zorg als premium-uitbreiding, zelfde patroon als NIS2.
4. Parallel addenda-lijn voor andere sectoren met eigen norm (SOC 2 / BIO / DORA) — later uit te rollen.
5. Aparte SEO-LP per sector waar dit commercieel rendeert (eerst `/iso-27001-zorg`).

**Kosten-plaatje voor zorgklant (huisartspraktijk):**
- Gratis quickscan met zorg-overlay
- €795 gap-analyse met zorg-overlay + gewone ISO-mapping
- €1.090 gap + NEN 7510-addendum = compleet zorg-pakket MKB
- €1.385 gap + NEN 7510 + NIS2 als klant onder NIS2 valt (sommige zorginstellingen wel)

**Eerste concrete actie:** NEN 7510-1 en NEN 7510-2 aanschaffen bij NEN.nl. Zonder die norm in huis kun je het addendum niet verantwoord leveren en is alle andere werk een stap voor de muziek uit.
