# NIS2-Addendum — Productspecificatie (Model B)

**Beslissing:** 2026-04-22 — Model B (addendum-upsell) gekozen, prijs €295.
**Context:** NIS2-addendum is alleen koopbaar i.c.m. een gap-analyse (of na afloop daarvan). Combi-prijs €1.090.
**Doel:** losse NIS2 Readiness (€995) verdwijnt; gap blijft €795 standalone; NIS2 wordt een optionele verrijking.

Dit document is de **single source of truth** voor wat het addendum bevat, hoe het wordt verkocht, en welke code-/content-wijzigingen nodig zijn in de Annex27-repo.

---

## 1. Inhoud van het NIS2-addendum (de 5 secties)

Dit is wat de klant krijgt bovenop de gap. De secties komen in het rapport na het standaard gap-gedeelte, onder een duidelijke DEEL 2-scheiding.

### Sectie 1 — Scope-oordeel

**Wat:** Per klant vastgesteld of ze onder NIS2 vallen, en zo ja in welke categorie.

**Inputbronnen:**
- Sector van de klant (bestel-flow vraagt dit al uit)
- Aantal medewerkers + jaaromzet + balanstotaal (toevoegen aan bestel-flow of via dashboard-vraag bij addendum-activering)
- Land (NL / BE / beide)

**Logica:**
- Match sector tegen NIS2-Annex I (essential sectors: energie, transport, bankwezen, financiële markten, gezondheidszorg, drinkwater, afvalwater, digitale infrastructuur, ICT-servicebeheer, overheid, ruimtevaart) en NIS2-Annex II (important sectors: post & koerier, afvalbeheer, chemie, voedsel, industrie, digitale aanbieders, onderzoek)
- Grootte-criterium (Commission Recommendation 2003/361/EC):
  - Medium = 50–249 fte **of** omzet >€10m / balans >€10m
  - Large = 250+ fte **of** omzet >€50m / balans >€43m
- Default: alleen medium/large vallen eronder (uitzonderingen: DNS-providers, TLD-registries, aanbieders van openbare elektronische communicatie — ongeacht grootte)
- Output: "U valt onder NIS2 als [essential/important] entiteit" of "U valt niet onder NIS2 — de Cyberbeveiligingswet is niet op u van toepassing"

**Rapport-output:**
- Uitkomst in één zin bovenaan
- Onderbouwing (welke annex, welke sector, welke grootte-criteria)
- Consequenties per uitkomst (essential = zwaarder toezicht + boetes tot €10m/2% omzet; important = registratieplicht + boetes tot €7m/1,4% omzet)
- Disclaimer: juridische eindbeoordeling blijft verantwoordelijkheid van klant

### Sectie 2 — Bestuursaansprakelijkheid (NIS2 art. 20)

**Wat:** Paragraaf over directie-verantwoordelijkheid onder NIS2.

**Vaste content (template — geldt voor alle klanten die onder NIS2 vallen):**
- Artikel 20 NIS2: bestuursleden moeten cybersecurity-maatregelen goedkeuren, toezicht houden op uitvoering, en kunnen persoonlijk aansprakelijk worden gesteld
- Kennis-eis (art. 20 lid 2): bestuursleden moeten regelmatig training volgen om cybersecurity-risico's te begrijpen
- Handhaving NL: NCTV + sectorale toezichthouders (DNB voor financieel, ACM voor telecom, IGJ voor zorg, enz.)
- Handhaving BE: CCB als nationaal toezicht + sectorale autoriteiten

**Meegeleverd template "Board-brief":**
- Ondertekenbare brief (1 A4) waarin de directie verklaart:
  - Kennis te hebben genomen van de NIS2-verplichtingen
  - De cybersecurity-maatregelen (zoals beschreven in het bijgevoegde gap-rapport) goed te keuren
  - Jaarlijks een NIS2-update te ontvangen en te beoordelen
- Klant kan dit direct laten ondertekenen — bewijst bestuurlijke betrokkenheid naar toezichthouder

### Sectie 3 — Incident-notification-playbook

**Wat:** Concreet proces dat de klant kan activeren bij een significant incident.

**Template-content:**
- **24u "early warning"** naar CSIRT (NL: NCSC / NCTV) of CCB (BE):
  - Wat: vermoeden van significant incident, aard en impact indicatie, vermoeden van kwaadwillig karakter
  - Template-formulier met alle velden en voorbeeld-wording
- **72u "incident notification"** (volledig rapport):
  - Wat: bevestiging/update van het incident, mate van ernst + impact, indicatoren van compromittering
  - Template met verplichte velden
- **1-maand "final report"**:
  - Wat: gedetailleerde beschrijving incident + oorzaak + getroffen maatregelen + impact-mitigatie
  - Template
- **Beslisboom:** wanneer is een incident "significant"? Drempelwaarden op basis van:
  - Duur dienstonderbreking
  - Geografische spreiding
  - Aantal getroffen gebruikers
  - Financiële impact
- **Contactgegevens:**
  - NL: NCSC-NL (ncsc.nl/melden), sectorale CSIRT's
  - BE: CCB (ccb.belgium.be/cert-be)

### Sectie 4 — Supply-chain-sectie (NIS2 art. 21 §2.d)

**Wat:** Leveranciers-eisen onder NIS2, strenger dan ISO Annex A.5.19-23.

**Rapport-content:**
- Uitleg NIS2-eis: risico-gebaseerde leveranciersbeoordeling + contractuele maatregelen + supplier-kwetsbaarheidsmonitoring
- Verschil met ISO A.5.19-23 (die vraagt beleid; NIS2 vraagt bewezen uitvoering + gelaagde due diligence)
- **Meegeleverde supplier-clausules (template in NL + BE):**
  1. Security-baseline clausule (leverancier moet minimale maatregelen aantoonbaar hebben)
  2. Incident-notificatie clausule (leverancier meldt incidenten binnen X uur)
  3. Audit-right clausule
  4. Sub-processor transparantie-clausule
  5. Recht-op-exit bij structurele non-compliance
- **Supplier-inventarisatie-template** (Excel/CSV): lijst met criteria en scoring-model

### Sectie 5 — Registratiestatus per land

**Wat:** Concrete stappen voor registratie bij de toezichthouder.

**Per land:**
- **Nederland (Cyberbeveiligingswet, verwachte inwerkingtreding medio 2026):**
  - Registratie bij NCTV (exacte portaal + deadline afhankelijk van definitieve wet)
  - Sectorale registratie waar van toepassing
  - Link naar concept-regelgeving + update-status
  - Status per peildatum van rapport-generatie
- **België (NIS2 omgezet, deadline 18 april 2026 essential entities):**
  - CCB-registratie via ccb.belgium.be
  - Aanleveren: naam, sector, contactpersoon, geografische scope, dienstbeschrijving
  - Termijn: binnen 5 maanden na "relevantie-vaststelling"
- **Cross-border:** als klant in beide landen actief is, welke autoriteit is primair (hoofdvestiging)

**Rapport-output:** concrete to-do-lijst met deadlines per land dat op klant van toepassing is.

---

## 2. Wijzigingen in bestel-flow

### Extra vraag in `/bestellen` (of in quickscan voordat klant bij bestellen komt)

Nodig voor Sectie 1 (scope-oordeel). Pas toe bij aanschaf addendum:

- Aantal medewerkers (bands: <10 / 10–49 / 50–249 / 250+)
- Jaaromzet (bands: <€2m / €2–10m / €10–50m / >€50m)
- Land van hoofdvestiging (NL / BE / anders)
- Geografische scope (alleen NL / alleen BE / beide / EU-breed)

Deze 4 velden bepalen of klant überhaupt onder NIS2 valt — zonder deze input is de scope-check niet te maken.

### UI op `/bestellen`

De gap-kaart krijgt een checkbox-sectie eronder:

```
┌───────────────────────────────────────────────┐
│  Gap-analyse                          €795    │
│  ─────────────                                │
│  Complete ISO 27001 Annex A scan.             │
│  Compliance-score, 93 controls gemapt,        │
│  prioriteitenrapport. Persoonlijke Lead       │
│  Auditor review.                              │
│                                               │
│  ☐ + NIS2-addendum (€295)                     │
│      Juridische scope-check, bestuurs-        │
│      aansprakelijkheid, incident-notification │
│      playbook, supply-chain-clausules,        │
│      registratiestatus per land. Alleen       │
│      beschikbaar i.c.m. gap-analyse.          │
│                                               │
│  [ Bestellen ]                                │
└───────────────────────────────────────────────┘
```

Checkbox:
- Standaard uit
- Bij aanvinken: totaalprijs bovenaan toont "€795 + €295 = €1.090"
- Bij aanvinken: de 4 extra scope-vragen verschijnen inline

### Checkout / Mollie

- Eén Mollie-payment, twee line-items: `gap_analyse` en `addon_nis2`
- Order-record in Supabase krijgt `addons: ["nis2"]` array
- BTW-regime volgt bestaande logica (BE 21% standaard, NL reverse-charge B2B bij geldig VAT-ID — zie reference_annex27_business memory)

### Post-purchase / rapport-generatie

- Rapport-template controleert `order.addons.includes("nis2")`
- Zo ja: render DEEL 2 met de 5 secties
- Addendum-secties gebruiken dezelfde controls-scores als de gap, maar voegen juridische/bestuurlijke analyse toe
- Aparte deelscore "NIS2-readiness %" bovenaan DEEL 2

---

## 3. Content-wijzigingen op bestaande pagina's

### Homepage

**Huidige meta-description (201 chars — ook te lang, zie SEO-audit):**
> Betaalbare ISO 27001 compliance voor het MKB in Nederland & België. Gap-analyse vanaf €795, beleidspakket en persoonlijke review door gecertificeerde Lead Auditor. NIS2 en AVG standaard meegenomen.

**Nieuwe meta-description (~155 chars):**
> ISO 27001 compliance voor het MKB in NL & BE. Gap-analyse vanaf €795, Lead Auditor review, optioneel NIS2-addendum. AVG standaard meegenomen.

**Pakketten-sectie:** haal "NIS2 Readiness €995" eruit. Laat vier kaarten staan: Gap (€795), Beleidspakket (€795), Pre-audit (€1.495), en een "Addon: NIS2-addendum (€295)" kaart met uitleg dat het alleen i.c.m. gap beschikbaar is.

### `/nis2`

Deze pagina wordt nu een **informatieve SEO-pagina**, geen losse productpagina.

**Structuur:**
1. H1: "NIS2 voor het MKB — wat moet u nu doen?" (blijft)
2. Intro: wat is NIS2 + wie valt eronder + deadline NL+BE
3. Secties over de 5 kernverplichtingen (analoog aan de 5 addendum-secties maar op publiek informatieniveau — niet de volledige templates weggeven)
4. **Nieuwe CTA-sectie** onderaan: "Weten of u onder NIS2 valt en wat u moet doen? Bestel de gap-analyse + NIS2-addendum" met link naar `/bestellen` met voor-geselecteerd addendum
5. FAQ blijft (FAQPage schema blijft)
6. **Nieuw:** voeg Service-schema toe (zie Sectie 5 hieronder)

**Belangrijk:** haal alle suggestie weg dat NIS2 een losse prijs heeft (geen "€995" meer op deze pagina).

### `/faq`

Voeg drie nieuwe vragen toe:

**Q: Wat is het verschil tussen de gap-analyse en het NIS2-addendum?**
A: De gap-analyse meet uw naleving van ISO 27001 Annex A — de technische en organisatorische controls. Het NIS2-addendum voegt daar de juridische en bestuurlijke NIS2-laag aan toe: scope-oordeel (valt u onder NIS2?), bestuursaansprakelijkheid, incident-notification-proces richting CSIRT of CCB, supply-chain-eisen onder artikel 21, en registratiestatus per land. NIS2 is voor 70–80% dezelfde controls als ISO 27001, maar heeft juridische verplichtingen die ISO niet kent.

**Q: Kan ik alleen het NIS2-addendum afnemen, zonder gap-analyse?**
A: Nee. Het NIS2-addendum bouwt voort op de scores en bevindingen uit de gap-analyse. Zonder de onderliggende controls-analyse is de NIS2-beoordeling onvolledig. U kunt het addendum wel toevoegen na een eerder afgenomen gap-analyse.

**Q: Val ik onder NIS2?**
A: Als uw organisatie 50+ medewerkers heeft of een omzet boven €10m én actief is in een sector uit NIS2-Annex I (essential) of Annex II (important), valt u er waarschijnlijk onder. De zekere uitkomst krijgt u in het NIS2-addendum — daar wordt uw specifieke situatie getoetst met onderbouwd oordeel. Een aantal diensten (DNS-providers, TLD-registries, openbare elektronische communicatie) valt er altijd onder, ongeacht grootte.

### Blog-posts

- `/blog/nis2-deadline-2026`: onderaan CTA aanpassen van "NIS2 Readiness bestellen" → "Bestel de gap-analyse + NIS2-addendum"
- Andere NIS2-verwijzingen in blog-posts (cyberverzekering-mkb, supply-chain-attacks-mkb) laten linken naar `/nis2` info-pagina (niet naar `/bestellen` direct)

---

## 4. Interne migratie bestaande klanten

Als er al klanten zijn met een oude "NIS2 Readiness €995" factuur of lopende order:

- Zij krijgen wat beloofd is (volledige NIS2-rapport) — geen wijziging aan lopende orders
- Voor toekomstige orders: oude SKU `nis2_readiness` blijft in DB bestaan als `deprecated: true` zodat oude rapporten reproduceerbaar blijven
- Nieuwe orders: alleen `gap_analyse` + optioneel `addon_nis2`

---

## 5. SEO: Product-schema op `/bestellen`

Dit vervangt/vervolledigt TF1 uit het SEO-plan (`seo-audit/00-PLAN.md` sectie 3.1).

```json
{
  "@context": "https://schema.org",
  "@type": "ItemList",
  "name": "Annex27 compliance-pakketten",
  "itemListElement": [
    {
      "@type": "Product",
      "position": 1,
      "name": "Gap-analyse ISO 27001",
      "description": "Volledige gap-analyse van uw informatiebeveiliging tegen ISO 27001 Annex A (93 controls). Inclusief compliance-score, prioriteiten-rapport en persoonlijke review door een gecertificeerde Lead Auditor.",
      "brand": {"@type": "Brand", "name": "Annex27"},
      "offers": {
        "@type": "Offer",
        "price": "795",
        "priceCurrency": "EUR",
        "availability": "https://schema.org/InStock",
        "url": "https://annex27.nl/bestellen",
        "priceValidUntil": "2026-12-31",
        "seller": {"@type": "Organization", "name": "Annex27"}
      }
    },
    {
      "@type": "Product",
      "position": 2,
      "name": "NIS2-addendum",
      "description": "Uitbreiding op de gap-analyse met juridische NIS2-scope-check, bestuursaansprakelijkheid (art. 20), incident-notification-playbook (24u/72u/1mnd), supply-chain-clausules (art. 21) en registratiestatus per land (NL Cyberbeveiligingswet + BE CCB). Alleen beschikbaar in combinatie met de gap-analyse.",
      "brand": {"@type": "Brand", "name": "Annex27"},
      "isAccessoryOrSparePartFor": {
        "@type": "Product",
        "name": "Gap-analyse ISO 27001"
      },
      "offers": {
        "@type": "Offer",
        "price": "295",
        "priceCurrency": "EUR",
        "availability": "https://schema.org/InStock",
        "url": "https://annex27.nl/bestellen",
        "priceValidUntil": "2026-12-31",
        "seller": {"@type": "Organization", "name": "Annex27"}
      }
    },
    {
      "@type": "Product",
      "position": 3,
      "name": "Beleidspakket ISO 27001",
      "description": "Complete set beleidsdocumenten (informatiebeveiligingsbeleid, risicomanagement, incident-response, etc.) afgestemd op uw gap-analyse-resultaten.",
      "brand": {"@type": "Brand", "name": "Annex27"},
      "offers": {
        "@type": "Offer",
        "price": "795",
        "priceCurrency": "EUR",
        "availability": "https://schema.org/InStock",
        "url": "https://annex27.nl/bestellen",
        "priceValidUntil": "2026-12-31",
        "seller": {"@type": "Organization", "name": "Annex27"}
      }
    },
    {
      "@type": "Product",
      "position": 4,
      "name": "Pre-audit review",
      "description": "Persoonlijke pre-audit review door een gecertificeerde ISO 27001 Lead Auditor voordat u naar certificatie gaat. Inclusief mock-audit en bevindingen-rapport.",
      "brand": {"@type": "Brand", "name": "Annex27"},
      "offers": {
        "@type": "Offer",
        "price": "1495",
        "priceCurrency": "EUR",
        "availability": "https://schema.org/InStock",
        "url": "https://annex27.nl/bestellen",
        "priceValidUntil": "2026-12-31",
        "seller": {"@type": "Organization", "name": "Annex27"}
      }
    }
  ]
}
```

**Schema voor `/nis2` (naast bestaande FAQPage):**

```json
{
  "@context": "https://schema.org",
  "@type": "Service",
  "serviceType": "NIS2 compliance-assessment",
  "provider": {
    "@type": "Organization",
    "name": "Annex27",
    "url": "https://annex27.nl"
  },
  "areaServed": [
    {"@type": "Country", "name": "Nederland"},
    {"@type": "Country", "name": "België"}
  ],
  "offers": {
    "@type": "Offer",
    "price": "295",
    "priceCurrency": "EUR",
    "description": "NIS2-addendum op gap-analyse (totaal €1.090 met verplichte gap).",
    "url": "https://annex27.nl/bestellen"
  },
  "audience": {
    "@type": "BusinessAudience",
    "audienceType": "MKB"
  }
}
```

---

## 6. Post-purchase upsell-flow (voor klanten die gap zonder addendum kochten)

Automatisering, zodra gap-rapport is afgeleverd:

**Trigger:** X dagen na aflevering gap-rapport (suggestie: zelfde dag + herinnering na 7 dagen)

**Email-template onderwerp:** "Valt uw organisatie onder NIS2? Voeg het NIS2-addendum toe"

**Inhoud:**
- Opening: bedankt voor gap-analyse, uw score was X%
- Vraag: "Weet u al of NIS2 op uw organisatie van toepassing is?"
- 3 korte bullets over wat het addendum toevoegt (scope-check, bestuur, incident-process)
- CTA: één-klik upgrade-link (pre-authenticated) voor €295
- P.S. over NL-Cyberbeveiligingswet timing (medio 2026)

**Eligibility-filter:** stuur alleen als:
- Klant heeft addendum nog niet afgenomen
- Klant zit in een sector die potentieel onder NIS2 valt (matchen tegen Annex I/II)
- Klant heeft niet recent een unsubscribe getriggerd

---

## 7. Volgorde van uitvoering

1. **Content uitwerken:** de 5 addendum-secties schrijven (dit is de echte ontwerparbeid — hergebruik waar mogelijk ISO-corpus uit `reference_iso_norm` + NIS2 Directive EU 2022/2555 + CCB-guidance)
2. **Rapport-template uitbreiden** met DEEL 2 conditionele render
3. **Supabase product-model:** `addon_nis2` product toevoegen, order-model uitbreiden met `addons[]` array
4. **Bestel-UI:** checkbox + 4 extra scope-vragen + prijs-recalculatie
5. **Mollie line-items:** twee regels in één betaling
6. **Pagina-updates:** `/bestellen`, `/nis2`, homepage, `/faq`
7. **Schema-injectie:** Product + Service zoals boven
8. **Email-upsell-flow** inbouwen + eligibility-filter
9. **Analytics:** attach-rate event (gap → addendum) tracken
10. **Oude NIS2 Readiness deprecated markeren** (niet verwijderen, wel verbergen op publieke pagina's)

---

## 8. Success-criteria (om later te evalueren)

- **Attach-rate gap → addendum:** streefwaarde 25–40%
  - <15% = addendum-copy niet overtuigend, of prijs te hoog gepositioneerd → herzien
  - >50% = addendum te goedkoop of te waardevol los verkocht → overwegen te bundelen of prijs omhoog
- **Revenue-per-gap:** moet stijgen van €795 naar gemiddeld €900+ (bij attach-rate 35%+)
- **Klachten over overlap:** moet dalen (nu onbekend baseline, maar een opportuniteit om in support-data te peilen)
- **SEO `/nis2` traffic:** behoudt of stijgt, CTR op bestel-CTA meetbaar
