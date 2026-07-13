# Tegnestuen — bureausite (design-spec)

Dato: 2026-07-13 · Status: godkendt af bruger (plan præsenteret og accepteret i session)

## Formål

Hjemmeside for **Tegnestuen** — leverer hjemmesider og interne systemer (lager, booking,
ordrestyring) til alle brancher. Skal slå alt tidligere arbejde på fire fronter samtidig:
visuelt design, performance, konvertering og features. Showcaser tidligere arbejde.

## Beslutninger (bekræftet af bruger)

- **Stack:** Vanilla statisk HTML/CSS/JS, nul dependencies, ingen build-step
- **Hosting:** Nyt repo `tegnestuen` → GitHub Pages (nickguidesyou-jpg.github.io/tegnestuen). Eget domæne senere
- **Kontakt:** Formular → nyt Google Apps Script → mail til nickguidesyou@gmail.com
- **Features:** Prisberegner, live case-previews, proces-tidslinje, AI-chat ("Spørg Tegnestuen" via GAS Claude-proxy med kvote-værn — i stedet for tredjeparts livechat)
- **Navn:** "Tegnestuen" er endeligt

## Research-konklusioner (DEPT, Novicell, Hello Monday)

Mønstre vi overtager: tal/beviser før påstande; cases vist stort; én tydelig CTA-rute.
Hul vi udfylder: ingen viser *levende* produkter, ingen har prisgennemsigtighed.

## Visuel identitet: "Fra streg til system"

- Mørk "blæk"-baggrund (næsten sort, blå tone) med subtilt millimeterpapir-grid; lyse "papir"-sektioner som rytme
- Én accentfarve: byggeplads-orange ("blyantstregen") til CTA'er/markeringer
- Signatur: SVG-linjer der tegner sig selv ved scroll (stroke-dashoffset); hero-wireframe der morpher til færdigt design
- Typografi: Space Grotesk (self-hosted WOFF2-subset) til display, systemfont til brødtekst
- Mikrointeraktioner: magnetiske knapper, count-up, scroll-reveals — alle bag `prefers-reduced-motion`

## Informationsarkitektur (one-pager)

1. **Hero** — USP på 5 sek. + primær CTA "Få et uforpligtende bud"
2. **Bevis-strip** — konkrete tal (leverancer, Lighthouse, leveringstid)
3. **Services** — to spor: Hjemmesider / Interne systemer, konkret indhold pr. pakke
4. **Cases** — King Pizza + Stenlille Herrefrisør som live iframes (indlæses ved klik) i browser-mockups; SESU-lagersystem som anonymiseret klikbar demo-mockup (bag login). Resultat-tal pr. case
5. **Proces-tidslinje** — animeret Skitse → Design → Byg → Live med dage
6. **Prisberegner** — type + tilvalg → vejledende interval → pre-udfyldt kontaktformular
7. **Udtalelser** — udkast, bruger godkender/erstætter
8. **FAQ** — indvendinger + SEO
9. **Kontakt** — minimal formular (navn, kontakt, besked) → GAS-mail
10. **Footer** — gentaget CTA + stamdata

AI-chat-widget flyder over siden (lazy-init ved klik).

## Teknik

- `index.html` + `assets/` (WebP, WOFF2). Ingen eksterne requests ved load
- Live case-iframes lazy: indlæses først ved brugerklik (performance)
- Semantisk HTML, ARIA, kontrast AA, JSON-LD `ProfessionalService`, OG-tags, sitemap.xml, robots.txt, manifest/theme-color
- Mål: Lighthouse 95+ på performance/accessibility/SEO — verificeres inden aflevering
- GAS-backend (`gas/Code.js`): `action=contact` (MailApp → bruger), `action=chat` (Claude-proxy, dagskvote, system-prompt om services/priser, fallback: lead-capture hvis API-nøgle mangler). Frontend virker fuldt med mailto-fallback indtil GAS er autoriseret

## Åbne punkter (kræver brugerens godkendelse/handling efter byg)

- Al copy er udkast — især prisintervaller i beregneren og tal i bevis-strip
- Udtalelser er pladsholder-udkast til erstatning med ægte citater
- GAS: engangs-autorisation af scopes + `ANTHROPIC_API_KEY` i Script Properties for AI-chat
- Eget domæne: DNS senere

## Efter byg (fra brief)

Kritisk selv-gennemgang som skeptisk besøgende + rettelser; punktliste over hvad der
konkret slår en gennemsnitlig bureauside.
