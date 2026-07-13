# Tegnestuen bureausite — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** One-page bureausite for Tegnestuen på GitHub Pages der showcaser tidligere arbejde live, med prisberegner, AI-chat og GAS-kontaktformular — Lighthouse 95+.

**Architecture:** Én statisk `index.html` (HTML+CSS+JS inline, samme mønster som King Pizza/frisøren) + `assets/` (WOFF2-font). Google Apps Script-backend (`gas/Code.js`) med `contact`- og `chat`-actions; frontend degraderer pænt (mailto/lead-capture) når GAS ikke er autoriseret. Live cases som klik-aktiverede iframes i browser-mockups.

**Tech Stack:** Vanilla HTML/CSS/JS, Google Apps Script (clasp), GitHub Pages.

## Global Constraints

- Nul eksterne requests ved sideload (fonte self-hostet, ingen CDN)
- Alle animationer bag `prefers-reduced-motion`
- Iframes indlæses KUN ved brugerklik
- Al copy på dansk; priser/tal er UDKAST markeret til brugergodkendelse i afleveringen
- Kontrast AA; semantiske landmarks; JSON-LD `ProfessionalService`
- Aldrig kalde Shipmondo-endpoints (irrelevant her, men global regel)

---

### Task 1: Scaffold + font
**Files:** Create `index.html` (skelet), `assets/space-grotesk-*.woff2`, `.gitignore`
- [ ] Hent Space Grotesk latin WOFF2 (vægt 400/500/700) via Google Fonts CSS-API med woff2-UA; gem i `assets/`
- [ ] Verificér filstørrelser (< 40 KB pr. fil): `ls -la assets/`
- [ ] Commit

### Task 2: Designsystem + skelet
**Files:** Modify `index.html`
- [ ] CSS-variabler (ink `#0d1117`-agtig, papir, orange accent), grid-baggrund, typografi-skala, knapper (primær/ghost, magnetisk hover), container/sektion-rytme, reveal-utility (`.rv`), reduced-motion-guard
- [ ] Alle 10 sektioner som tomme semantiske landmarks + sticky nav + skip-link
- [ ] Commit

### Task 3: Hero + bevis-strip
- [ ] Hero: H1-USP, subcopy, primær CTA "Få et uforpligtende bud" (→ #kontakt), sekundær "Se vores arbejde" (→ #cases); SVG-wireframe der tegner sig selv (stroke-dashoffset animation)
- [ ] Bevis-strip: 4 tal med count-up ved scroll (leverancer, Lighthouse-score, dage til live, support-svartid) — markeret UDKAST i aflevering
- [ ] Verificér i preview (read_page + screenshot)
- [ ] Commit

### Task 4: Services + proces-tidslinje
- [ ] To service-kort (Hjemmesider / Interne systemer) med konkrete leverance-lister og hver sin CTA
- [ ] Tidslinje Skitse→Design→Byg→Live med SVG-linje der tegnes ved scroll, dage pr. trin
- [ ] Commit

### Task 5: Cases med live previews
- [ ] Browser-mockup-komponent (chrome med prikker/URL-bar); King Pizza + frisør (shf-preview) som klik-til-load iframes; SESU som indbygget anonymiseret dashboard-demo (ren HTML/CSS, klikbare faner)
- [ ] Resultat-tal + "besøg siden"-links pr. case
- [ ] Verificér iframes loader ved klik i preview
- [ ] Commit

### Task 6: Prisberegner
- [ ] Trin: vælg type (hjemmeside/booking/internt system/webshop) → tilvalg (checkboxes) → interval-output med count-up + CTA der pre-udfylder kontaktformular med valgene
- [ ] Priser som `PRICING`-konstant øverst i JS med kommentar "UDKAST — justér"
- [ ] Commit

### Task 7: Udtalelser + FAQ + kontakt + footer
- [ ] 3 udtalelses-kort (markeret som udkast), FAQ som `<details>` (5-6 spørgsmål), kontaktformular (navn, mail/tlf, besked, honeypot) → `TEGNESTUEN_API` POST text/plain; fallback mailto hvis API tom/fejler; footer med gentaget CTA
- [ ] Commit

### Task 8: AI-chat-widget
- [ ] Flydende knap → panel; lazy init; kald GAS `action=chat`; hvis backend svarer fejl/mangler nøgle → lead-capture-formular i chatten (sender via `contact`)
- [ ] Commit

### Task 9: GAS-backend
**Files:** Create `gas/Code.js`, `gas/appsscript.json`, `gas/.clasp.json` (via clasp create)
- [ ] `doPost`: `contact` (validér felter, MailApp til nickguidesyou@gmail.com, rate-limit via CacheService), `chat` (Claude API-proxy, dagskvote 50/dag, system-prompt om Tegnestuens services/priser; mangler `ANTHROPIC_API_KEY` → `{error:'nokey'}`)
- [ ] `doGet`: `{ok:true}` ping
- [ ] clasp create/push/deploy; test GET-ping med curl; ved auth-fejl: dokumentér engangstrin til bruger
- [ ] Indsæt deploy-URL i `TEGNESTUEN_API` i index.html
- [ ] Commit

### Task 10: SEO + PWA-basics
**Files:** Create `robots.txt`, `sitemap.xml`; modify `index.html`
- [ ] Meta description, OG/Twitter-tags, canonical, theme-color, favicon (inline SVG data-URI), JSON-LD ProfessionalService
- [ ] Commit

### Task 11: Verifikation
- [ ] Preview-server (launch.json), gennemklik alle interaktioner via browser-tools; konsol/netværk rene; mobil (375px) + desktop + reduced-motion
- [ ] Lighthouse via `npx lighthouse` mod lokal server; mål 95+ perf/a11y/SEO; fix fund
- [ ] Kritisk skeptiker-gennemgang (brief §6) + rettelser
- [ ] Commit

### Task 12: Publish
- [ ] GitHub-repo `tegnestuen` via API (PAT fra keychain, som king-pizza), push main, enable Pages
- [ ] Verificér live URL i browser
- [ ] Aflevering: liste over hvad der slår gennemsnitssiden + alle UDKAST-punkter til godkendelse
