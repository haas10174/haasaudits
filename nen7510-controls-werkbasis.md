# NEN 7510 — Zorg-specifieke controls (werkbasis)

**Status document:** 2026-04-22 — samengesteld uit publieke bronnen (Nictiz, IGJ, Z-CERT, Kiwa, TÜV, Microsoft Compliance, consultancy-publicaties). **Geen vervanging** van de normatieve tekst van NEN 7510-2:2024 — die is auteursrechtelijk beschermd en bij NEN.nl te bestellen. Deze werkbasis is geschikt om:

- Sector-vragenboom voor zorg te drafteren
- Rapport-template-structuur te definiëren
- Productbeschrijvingen en copy te schrijven
- Addendum/sector-scope-afbakening te maken

**Maar niet voor:** letterlijke citaten in een auditrapport of compliance-bevinding. Voor die laag moet je NEN 7510-2:2024 in huis hebben.

**Cijferbasis (publiek bevestigd):**
- 36 van 114 ISO 27002:2013-controls zijn in NEN 7510-2:2017 uitgebreid met zorg-specifieke maatregelen
- 14 ISO-controls zijn aangepast voor zorg-context
- 8 volledig nieuwe zorg-specifieke maatregelen
- 42 zorg-specifieke implementatie-guidelines (waarvan 25 als aanvulling zonder eigen control)
- **Totaal: ~58 unieke zorg-specifieke controls/guidelines**

In de 2024-editie zijn de maatregelen geconsolideerd naar 101 (van 117), met behoud van de zorg-specifieke toevoegingen in de nieuwe 4-thema-structuur van ISO 27002:2022.

---

## Domein A — Toegangsbeheer & autorisatie

Zorg-specifiek: autorisatie gekoppeld aan behandelrelatie, niet aan functie alleen.

| # | Onderwerp | Zorg-specifieke eis | ISO-basis |
|---|---|---|---|
| A1 | Patient context / behandelrelatie | Toegang tot medisch dossier is afhankelijk van een actieve behandelrelatie tussen zorgverlener en patiënt. Systemen moeten dit technisch afdwingen (patient-context in HiX, Epic, Nexus, MicroHIS). | A.5.15, A.5.18 (ISO:2022) / A.9.1, A.9.2 (ISO:2013) |
| A2 | Break-glass / noodprocedure | In acute situaties mag een zorgverlener dossier inzien zonder vooraf gedefinieerde relatie; alle break-glass-activiteit wordt gelogd, achteraf beoordeeld en gerapporteerd aan CISO/privacy-officer. | Uitbreiding op A.5.15 |
| A3 | Role-based access conform zorgberoepen | Autorisatie-rollen volgen zorgberoepenprofielen (arts, verpleegkundige, POH, doktersassistent, secretarieel). Geen "alles mogen zien"-rollen voor administratief personeel. | A.5.15, A.8.3 |
| A4 | Privacyscheiding binnen zelfde instelling | GGZ-afdeling niet zichtbaar voor somatische afdeling tenzij relatie aantoonbaar; jeugddossier niet inzichtelijk zonder legitimatie. | Zorg-specifieke uitbreiding |
| A5 | Patiënt-inzage in eigen dossier | Proces voor inzagerecht patiënt (WGBO art. 7:456): binnen 4 weken, kosteloos, gedocumenteerd. Technisch via PGO/MedMij waar mogelijk. | Zorg-specifiek (WGBO-koppeling) |

## Domein B — Logging & monitoring van medische data

Zorg-specifiek: elke inzage in medisch dossier moet herleidbaar zijn.

| # | Onderwerp | Zorg-specifieke eis | ISO-basis |
|---|---|---|---|
| B1 | Volledige inzage-logging | Elke read, write, print, export op het medisch dossier wordt gelogd met: timestamp, gebruiker, patiënt-ID, reden, apparaat. | A.8.15 (ISO:2022) / A.12.4 (ISO:2013) |
| B2 | Logreview-verplichting | Periodieke (bv. maandelijks) review van dossier-inzagen op anomalieën; VIP-patiënt-logging (bekende Nederlanders, eigen familie) extra scherp. | A.8.15 + A.8.16 |
| B3 | Bewaartermijn logs | Toegangslogs op medisch dossier: minimaal 5 jaar bewaard (praktijkrichtlijn; verifieer tegen NEN 7510:2024). | Zorg-specifieke uitbreiding |
| B4 | Logging van break-glass + escalatie | Break-glass-gebruik triggert automatische melding bij CISO/privacy-officer binnen X uur. | A.8.15 |
| B5 | Onveranderbaarheid van logs | Medische toegangslogs moeten tamper-evident zijn (append-only, WORM of hash-chain). | A.8.15 + A.8.11 |

## Domein C — Data-uitwisseling in de zorgketen

Zorg-specifiek: LSP, Mitz, VIPP, KIK-V, verwijsindex, etc.

| # | Onderwerp | Zorg-specifieke eis | ISO-basis |
|---|---|---|---|
| C1 | LSP-aansluiting | Als aangesloten op Landelijk Schakelpunt: voldoen aan VZVZ-aansluitvoorwaarden inclusief NEN 7510-certificering, UZI-authenticatie, patiënttoestemming via LSP-toestemmingsregister. | Zorg-specifiek (VZVZ-framework) |
| C2 | Mitz-toestemmingsbeheer | Vervanger van OPT-IN voor toestemmingsbeheer. Zorgaanbieder moet Mitz-integratie ondersteunen als keten dit vereist. | Zorg-specifiek |
| C3 | VIPP-programma-koppeling | Bij deelname aan VIPP (PGO, eiPM, medicatieoverdracht, OPEN): voldoen aan informatiestandaarden MedMij, Nictiz-BGZ, Zorgaanbieder Adresboek (ZAB). | Zorg-specifiek |
| C4 | Verwijsindex Risicojongeren (VIR) | Alleen relevant voor jeugd-/kinderzorg: aansluiting en gebruik conform Wet verplichte meldcode huiselijk geweld. | Zorg-specifiek |
| C5 | KIK-V (langdurige zorg) | VVT/thuiszorg met Wlz-contract: deelname aan KIK-V-keteninformatiesysteem. | Zorg-specifiek |
| C6 | Zorgaanbieder Adresboek (ZAB) | Correct geregistreerde AGB- en URA-gegevens voor keten-identificatie. | Zorg-specifiek |
| C7 | FHIR/HL7-berichtstandaarden | Bij geautomatiseerde uitwisseling: gebruik informatie-standaarden Nictiz (BGZ, BGN, BGLZ) en veilige transport (TLS 1.3, signed JWT). | A.5.14 (ISO:2022) / A.13.2 |

## Domein D — Identificatie & authenticatie

Zorg-specifiek: UZI, BSN, DigiD, UZI-register.

| # | Onderwerp | Zorg-specifieke eis | ISO-basis |
|---|---|---|---|
| D1 | UZI-pas voor zorgverleners | Gebruik UZI-pas (persoonsgebonden smartcard) of UZI-servercertificaat voor systeem-tot-systeem. Alternatieven: Idensys (hoog AL3) of eIDAS-equivalent. | A.5.17, A.8.5 |
| D2 | BSN-verwerking onder WBSN-Z | BSN mag alleen worden verwerkt als organisatie zorgaanbieder is in de zin van de Wet; BSN-verificatie via SBV-Z of EPD. | Zorg-specifiek (juridisch) |
| D3 | Patiënt-authenticatie (DigiD / MedMij) | Als patiënt online toegang krijgt: DigiD Midden of Substantieel voor medische gegevens; MedMij-authenticatie-pattern. | A.5.17 |
| D4 | Multi-factor voor zorgverlener | Toegang tot medisch dossier vereist MFA of UZI-pas; password-only is niet toegestaan voor dossier-toegang. | A.5.17, A.8.5 |
| D5 | Sessie-timeout klinische werkplek | Sessies op werkplekken in klinische omgeving beperkt (bv. 15 min inactiviteit); snel re-authenticeren (badge-tap) om workflow niet te blokkeren. | A.8.5, A.8.7 |

## Domein E — Fysieke beveiliging in de zorg

Zorg-specifiek: patiëntomgeving, mobiele apparatuur, home-visits.

| # | Onderwerp | Zorg-specifieke eis | ISO-basis |
|---|---|---|---|
| E1 | Afschermen schermen in patiëntbereik | Werkplekken in zicht van patiënten (balie, verpleegpost) moeten schermen afschermen of privacy-filter gebruiken. | A.7.7 |
| E2 | Retourneren bedrijfsmiddelen + medische data-wipe | Bij einde dienstverband alle persoonlijke gezondheidsinformatie gewist van devices; fysieke dossiers en geheugendragers retour. | A.5.11 (uitbreiding) |
| E3 | Fysieke dossiers | Papieren patiëntendossiers in afgesloten kasten; transport tussen locaties via verzegeld pakket. | A.7.2, A.7.3 |
| E4 | Thuiszorg / mobiel werken | Tablets/laptops bij wijkverpleegkundigen: device-encryptie, offline-synchronisatie met selectief kopiëren, remote wipe bij verlies. | A.6.7, A.8.1 |

## Domein F — HR, personeel & beroepsgeheim

Zorg-specifiek: medisch beroepsgeheim, geheimhoudingsverklaring, BIG-registratie.

| # | Onderwerp | Zorg-specifieke eis | ISO-basis |
|---|---|---|---|
| F1 | Beroepsgeheim & geheimhoudingsverklaring | Medewerkers tekenen geheimhoudingsverklaring die verwijst naar WGBO art. 7:457 (beroepsgeheim); geldt ook voor niet-BIG-medewerkers. | A.6.2, A.6.6 |
| F2 | BIG-registratie-verificatie | Voor BIG-plichtige functies (arts, verpleegkundige, fysio, etc.): periodieke controle of registratie nog geldig is. | A.6.1 (uitbreiding) |
| F3 | Awareness training zorg-context | Jaarlijkse training inclusief casuïstiek rond VIP-patiënten, sociale media, foto's van patiënten, whatsapp-gebruik. | A.6.3 |
| F4 | Disciplinaire procedures bij dossier-misbruik | Duidelijke procedure bij ongeautoriseerde dossier-inzage (bv. kijken in dossier eigen familielid); koppeling met IGJ-meldplicht waar relevant. | A.6.4 |

## Domein G — Incident-management & melding

Zorg-specifiek: dubbele meldplicht AP + IGJ, Z-CERT, patiëntveiligheid.

| # | Onderwerp | Zorg-specifieke eis | ISO-basis |
|---|---|---|---|
| G1 | Datalek AP-melding | 72u melding aan Autoriteit Persoonsgegevens bij datalek met (kans op) risico voor betrokkenen. Medische data = hoog risico default. | A.5.24–A.5.27 |
| G2 | IGJ-melding bij patiëntveiligheid-impact | Parallel aan AP-melding: IGJ-melding via Wkkgz als het incident patiëntveiligheid raakt (bv. EPD down tijdens OK, verkeerde medicatie door data-issue). | Zorg-specifiek (Wkkgz) |
| G3 | Z-CERT-betrokkenheid | Zorgaanbieders: lidmaatschap Z-CERT aanbevolen; incident-uitwisseling via TLP-protocol. | Zorg-specifiek |
| G4 | Interne calamiteitenregistratie | Alle incidenten geregistreerd in VIM-systeem (Veilig Incident Melden); privacy-incidenten in aparte register. | A.5.24 |
| G5 | Communicatie naar patiënt | Bij datalek met hoog risico: informeren van betrokken patiënten in heldere taal binnen redelijke termijn. | A.5.26 |

## Domein H — Bewaartermijnen & data-retentie

Zorg-specifiek: WGBO, WBP, kinderdossier, erfelijkheidsdata.

| # | Onderwerp | Zorg-specifieke eis | ISO-basis |
|---|---|---|---|
| H1 | WGBO-bewaartermijn medisch dossier | 20 jaar vanaf laatste wijziging of zoveel langer als goed hulpverlenerschap vereist; voor kinderen: tot 20 jaar na 18e verjaardag. | Zorg-specifiek (WGBO 7:454) |
| H2 | Erfelijkheidsonderzoek | Gegevens uit erfelijkheidsonderzoek: onbeperkt bewaren in het belang van familieleden. | Zorg-specifiek |
| H3 | Dwangbehandelingen (WvGGZ/Wzd) | Dossiers rond dwangbehandeling: 20 jaar + aparte logging. | Zorg-specifiek |
| H4 | Vernietigingsverzoek patiënt | Verzoek tot vernietiging (WGBO 7:455) moet binnen 3 maanden uitgevoerd tenzij wettelijke uitzondering; vernietiging gedocumenteerd. | A.5.34, A.8.10 |
| H5 | Back-up-retentie conform bewaartermijn | Back-ups mogen retentie niet omzeilen: bij vernietigingsverzoek ook back-ups aanpakken of een gedocumenteerd proces hebben. | A.8.13 |

## Domein I — Bedrijfscontinuïteit & klinische impact

Zorg-specifiek: downtime-procedures EPD, klinische SOP's.

| # | Onderwerp | Zorg-specifieke eis | ISO-basis |
|---|---|---|---|
| I1 | EPD-downtime-procedure | Gedocumenteerd proces voor werken zonder EPD: papieren dossiers, SOP's, tweede zorgverlener-check, catch-up na herstel. | A.5.29, A.5.30 |
| I2 | Klinische impact-analyse bij incident | Cybercrisis-plan bevat klinisch beslismoment (gaat OK door? kan IC opname? sluiting SEH?) met MT + hoofdbehandelaar. | A.5.29 |
| I3 | Medische apparatuur-fallback | Bij uitval netwerken: zelfstandig werkende medische apparaten (infuuspomp, monitor) blijven functioneel of hebben manueel-alternatief. | A.5.30, A.8.14 |
| I4 | Z-CERT + NCTV-coördinatie | Bij significant incident: coördinatie via Z-CERT; bij NIS2-scope ook NCTV. | Zorg-specifiek (ketens) |

## Domein J — Leveranciers & verwerkersovereenkomsten

Zorg-specifiek: EPD-vendors, labs, apotheken, medical device suppliers.

| # | Onderwerp | Zorg-specifieke eis | ISO-basis |
|---|---|---|---|
| J1 | Verwerkersovereenkomst zorg-specifiek | Alle softwareleveranciers die patiëntdata verwerken: VO met zorg-specifieke clausules (locatie EU, sub-processors inzichtelijk, audit-recht, incidentmelding 24u, retourgarantie data bij exit). | A.5.19, A.5.21 |
| J2 | EPD-vendor-onafhankelijkheid | Exit-strategie van EPD-leverancier gedocumenteerd; data-portabiliteit in standaard formaten (HL7, FHIR). | A.5.22 |
| J3 | Lab/diagnostiek-koppelingen | HL7-berichten met labs: integriteit via digital signing, TLS 1.3, patiënt-ID matching-proces. | A.5.14 |
| J4 | Apotheek-communicatie | Receptenverkeer via NICTIZ-standaarden (medicatieoverdracht); integratie met LSP. | A.5.14 |
| J5 | Medical device suppliers | Bij medische apparatuur met netwerkkoppeling: leverancier-verantwoordelijkheden voor patching, kwetsbaarheidsmeldingen, end-of-life. | A.5.20 |

## Domein K — Patiëntrechten (AVG art. 9 integratie)

Zorg-specifiek: bijzondere categorie persoonsgegevens.

| # | Onderwerp | Zorg-specifieke eis | ISO-basis |
|---|---|---|---|
| K1 | Grondslag verwerking (art. 9 AVG) | Verwerking medische gegevens alleen op basis van art. 9 lid 2: behandeling (h), volksgezondheid (i), arbeidsgeneeskunde (h), uitdrukkelijke toestemming (a). Vastlegging per verwerking. | A.5.34 |
| K2 | DPIA voor hoog-risico verwerking | Bij grootschalige verwerking gezondheidsgegevens: DPIA verplicht conform AP-beleid. | A.5.34 |
| K3 | Patiënt-inzagerecht gefaciliteerd | Proces voor WGBO+AVG-inzage: uit te leveren binnen 1 maand, kosteloos, in begrijpelijke vorm. | Zorg-specifiek |
| K4 | Correctieverzoek beperkt tot feitelijke onjuistheden | WGBO staat beperkte correctie toe (feitelijke fouten, niet arts-oordeel); proces voor aanvullende eigen verklaring. | Zorg-specifiek |

## Domein L — Onderzoek, pseudonimisering, secundair gebruik

Zorg-specifiek: medisch-wetenschappelijk onderzoek (WMO), Palga, CBS-leveringen.

| # | Onderwerp | Zorg-specifieke eis | ISO-basis |
|---|---|---|---|
| L1 | Pseudonimisering voor onderzoek | Verstrekking van data aan onderzoek: pseudonimisering met separate sleutelbeheer; re-identificatie-risico-analyse. | A.8.11 |
| L2 | Geen-bezwaar-register | Patiënten kunnen bezwaar maken tegen secundair gebruik (Code Goed Gedrag / WGBO); register beheerd. | Zorg-specifiek |
| L3 | Data-levering aan CBS/RIVM/WKO | Alleen op wettelijke grondslag en in afgesproken format; aantoonbare routering. | Zorg-specifiek |
| L4 | Palga-aanleveringen | Voor pathologie: aanlevering conform Palga-standaard, met afgesproken scope en pseudonimisering. | Zorg-specifiek |

## Domein M — Asset management & data-dragers

Zorg-specifiek: medische apparatuur, data-dragers met gezondheidsinformatie.

| # | Onderwerp | Zorg-specifieke eis | ISO-basis |
|---|---|---|---|
| M1 | Asset-register medische apparatuur | Aparte registratie medische apparatuur met: CE-markering, MDR-classificatie, software-versies, patch-status, eigenaar. | A.5.9 |
| M2 | Verwijdering data-dragers met PHI | USB-sticks, externe schijven, oude PC's uit klinische omgeving: cryptografisch wipe of fysiek vernietigen, procesmatig vastgelegd. | A.7.14 |
| M3 | Legacy medische systemen | Niet-patchbare apparatuur (vendor locked): gesegmenteerd netwerk + compensating controls + end-of-life plan. | A.5.9, A.8.8 |

## Domein N — Netwerk & medical device security

Zorg-specifiek: segmentatie medisch/kantoor, IoMT, OT.

| # | Onderwerp | Zorg-specifieke eis | ISO-basis |
|---|---|---|---|
| N1 | Netwerksegmentatie medisch ↔ kantoor | Klinische systemen (EPD, medische apparatuur, modaliteiten) op apart netwerk met firewall/allow-list; kantoor-IT niet mag bij medische OT. | A.8.22 |
| N2 | IoMT (Internet of Medical Things) | Specifieke monitoring op medische IoT-apparatuur: netwerk-baseline, anomalie-detectie, rogue-device-detectie. | A.8.16, A.8.22 |
| N3 | Gastennetwerk gescheiden | Patiënten-wifi volledig gescheiden van klinisch netwerk; geen shared infrastructure. | A.8.22 |
| N4 | Remote-support door vendor | Toegang van leveranciers op medische apparatuur via sprong-host, gelogd, tijdsgebonden, MFA. | A.8.21 |

---

## Totaal en dekking

| Domein | Aantal zorg-specifieke controls | Vragen voor quickscan (selectie) | Vragen voor gap (volledig) |
|---|---|---|---|
| A. Toegangsbeheer | 5 | 2 | 5 |
| B. Logging medisch dossier | 5 | 1 | 5 |
| C. Ketenuitwisseling (LSP etc.) | 7 | 2 | 6 |
| D. Identificatie/auth | 5 | 2 | 5 |
| E. Fysieke beveiliging zorg | 4 | 1 | 4 |
| F. HR & beroepsgeheim | 4 | 1 | 4 |
| G. Incident AP+IGJ | 5 | 1 | 5 |
| H. Bewaartermijnen | 5 | 1 | 4 |
| I. Continuïteit + klinisch | 4 | 1 | 4 |
| J. Leveranciers zorg | 5 | 1 | 4 |
| K. Patiëntrechten | 4 | 1 | 3 |
| L. Onderzoek/pseudonimisering | 4 | 0 | 3 |
| M. Asset mgmt medisch | 3 | 0 | 3 |
| N. Netwerk/IoMT | 4 | 1 | 4 |
| **Totaal** | **64 controls** | **~15 vragen** | **~59 vragen** |

**Voor de quickscan** (gratis, sector-overlay): selectie van 10-15 zorg-specifieke vragen bovenop de basis-ISO-vragen. Focus op de meest onderscheidende items: behandelrelatie-autorisatie, break-glass, inzage-logging, LSP/Mitz-status, datalek-dubbel (AP+IGJ), EPD-downtime-procedure, WGBO-bewaartermijn.

**Voor de betaalde gap** (+ NEN 7510-scope): volledig doornemen van alle 64 controls met scoring per control.

---

## Bronnen (publiek, geconsulteerd 2026-04-22)

- [NEN 7510: informatiebeveiliging in de zorg — NEN.nl](https://www.nen.nl/zorg-welzijn/ict-in-de-zorg/informatiebeveiliging-in-de-zorg)
- [NEN 7510 - Z-CERT](https://z-cert.nl/nen-7510)
- [Vragen over NEN 7510 — IGJ](https://www.igj.nl/vraag-en-antwoord/vragen-over-nen-7510)
- [NEN 7510 for healthcare sector information security updated — Kiwa](https://www.kiwa.com/nl/en-nl/areas-of-expertise/cyber-security/news/nen-7510-for-healthcare-sector-information-security-updated/)
- [Nieuwe NEN 7510 voor informatiebeveiliging in de zorg — TÜV NORD](https://www.tuv.nl/nl/kennisbank/blogs/nieuwe-nen-7510-voor-informatiebeveiliging-in-de-zorg-wat-is-er-veranderd/)
- [NEN 7510 — Microsoft Compliance](https://learn.microsoft.com/en-us/compliance/regulatory/offering-nen-7510-netherlands)
- [NEN 7510 vs ISO 27001: verschillen — Risguard](https://risguard.com/verschil-nen-7510-vs-iso-27001/)
- [NEN 7510 een overzicht van de norm — ICT Institute](https://softwarezaken.nl/2020/01/nen-7510-een-overzicht/)
- [NEN 7510 checklist — CertificeringsAdvies Nederland](https://certificeringsadvies.nl/nen-7510-checklist-doorloop-dit-stappenplan-op-weg-naar-nen-7510-certificering/)
- [Nieuwe NEN 7510 norm: dit is wat je moet weten — CertificeringsAdvies](https://certificeringsadvies.nl/vernieuwing-nen-7510-norm/)
- [NEN 7510 — alles wat je moet weten in 2025 — Computable](https://www.computable.nl/topics/nen-7510/)
- [NIS2 and NEN 7510 in healthcare — Bureau Veritas](https://cybersecurity.bureauveritas.com/services/integrated-approach/nis2/nis2-and-nen-7510-in-healthcare)

Wkkgz, WGBO, WBSN-Z, WvGGZ, AVG art. 9, Wet verplichte meldcode — wetgevingsbronnen op wetten.overheid.nl.

---

## Volgende stappen

1. **NEN 7510:2024 bestellen bij NEN.nl** (niet 2017 — die is verouderd sinds de herziening). Verwacht ~€150–€300.
2. **Dit document als v0.1 aannemen**: goed genoeg voor productontwerp, vragenboom-drafting, copy-schrijfwerk.
3. **Na ontvangst van de norm**: elk van de 64 items mappen tegen de normatieve tekst (A1 → NEN 7510-2:2024 §x.y.z). Correcties en aanvullingen. Resultaat: v1.0 die voor rapportage bruikbaar is.
4. **Consistentie met Raoul's auditor-ervaring checken**: welke van deze 64 wordt in de praktijk het vaakst als gap vastgesteld? Die krijgen meer gewicht in scoring.
5. **Verankeren in productstructuur** (zie `nen7510-zorg-spec.md`): quickscan-vragen (~15), gap-vragen (~59), addendum-content (volledige 64 + templates).

---

## Juridische disclaimer

Dit document beschrijft de inhoud van NEN 7510-2 in functionele termen op basis van publieke bronnen. De normatieve tekst is eigendom van Stichting Koninklijk Nederlands Normalisatie Instituut (NEN) en wordt niet gereproduceerd in dit document. Voor auditrapportage, certificering of publicatie naar klanten is de normatieve tekst vereist als bron. Mapping tegen exacte paragraafnummers (§) in kolom "ISO-basis" is indicatief op basis van ISO 27002:2022-structuur; exacte NEN 7510-2:2024-§-nummers zijn te verifiëren tegen de aangekochte norm.
