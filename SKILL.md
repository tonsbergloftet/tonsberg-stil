# SKILL.md — Tønsbergløftet presentasjonsskill

Bruk dette dokumentet som kontekst når du skal lage nye presentasjoner for
Tønsbergløftet. Les hele filen før du skriver kode.

---

## Hva er dette?

Et presentasjonsformat som fungerer som en-sides HTML-nettside med
scroll-snap-navigasjon. Brukes til workshops, ledelsespresentasjoner og
scenariopresentasjoner. Utvikles i Claude-artifakt, pushes til GitHub Pages.

Formatet erstatter PowerPoint. Det er ikke ment å «leve» som nettside etterpå
— det er et engangsdokument som brukes i rommet og sendes som lenke.

---

## Designtokens

### Farger — Tønsberg kommunes grafiske profil (2024)

```css
:root {
  /* Hovedfarger */
  --navy:       #013952;   /* Primær — mørkeblå */
  --navy-bg:    #e8eff3;   /* Lys variant av navy */
  --sage:       #c5daaf;   /* Sagegreen — kommunens sekundærfarge */
  --sage-dark:  #7a9e5f;
  --sage-bg:    #f0f5ea;

  /* Støttefarger */
  --rust:       #923c3d;
  --rust-bg:    #f3e4e4;
  --salmon:     #e69d7d;
  --salmon-bg:  #fdf0e8;
  --amber:      #f1be48;
  --amber-bg:   #fdf5dc;
  --sky:        #91cef2;
  --sky-bg:     #e8f5fd;
  --green:      #00714e;
  --green-bg:   #e0f2ec;
  --gray:       #4a4a49;
  --gray-light: #f5f5f5;

  /* Nøytraler */
  --white:      #ffffff;
  --bg:         #f7f8f9;
  --ink:        #1a1a1a;
  --ink-mid:    #3d3d3d;
  --ink-dim:    #666666;
  --border:     rgba(1,57,82,0.12);
}
```

**Fargebruk i praksis:**
- `--navy` er alltid primær for overskrifter, hero-bakgrunn og nav
- `--sage` er kommunens bølge/slør-farge og brukes på hero mot navy
- `--amber` er markørfarge (slide-tag dot, chips, oppmerksomhetssignal)
- `--rust` = negativ retning/røde flagg i bevegelsesmålinger
- `--green` = positiv retning/bærekraftig i bevegelsesmålinger
- `--amber` = nøytral/utsatt tilstand i bevegelsesmålinger

### Typografi

```css
:root {
  --serif: "Barlow", system-ui, sans-serif;  /* Kommunens profilfont */
  --hand:  "Square Peg", cursive;            /* Kommunens håndskriftfont */
}
```

**Google Fonts-import:**
```html
<link href="https://fonts.googleapis.com/css2?family=Barlow:ital,wght@0,300;0,400;0,600;0,700;0,800;1,400;1,600&family=Square+Peg&display=swap" rel="stylesheet">
```

**Typografisk hierarki:**
- Hero H1: Barlow 800, clamp(56px, 12vw, 104px)
- Slide H2: Barlow 800, clamp(28px, 4.5vw, 48px)
- Brødtekst: Barlow 300, 15px, linjehøyde 1.7
- Lead-tekst: Barlow 400, 17px, linjehøyde 1.65
- Etiketter/tags: Barlow 600, 9–11px, letter-spacing 0.12–0.16em, uppercase
- Håndskrift (`.hand`): Square Peg, brukes som innslag i headings eller som dekorative etiketter

**Bruk av Square Peg:**
Aldri som primærfont for lesbar tekst. Kun til:
- Innslag i H1/H2 via `<span class="hand">`
- Neste-knappens tekst
- Numre i spørsmålsblokker og veivalg-kort
- Dekorative etiketter der håndskriftuttrykk styrker kommunikasjonen

---

## Strukturprinsipp

### Grunnstruktur

```html
<!DOCTYPE html>
<html lang="no">
<head>
  <!-- meta, fonts, all CSS inline -->
</head>
<body>
  <nav class="nav-dots"><!-- én knapp per slide --></nav>
  <div class="slides" id="slides">
    <section class="slide" data-index="0">
      <div class="slide-inner"><!-- innhold --></div>
      <button class="next-btn" onclick="goNext(0)">↓ Neste</button>
    </section>
    <!-- flere slides... -->
  </div>
  <script><!-- scroll-logikk og toggleVV --></script>
</body>
</html>
```

**Regler:**
- Alt CSS inline i `<style>`-taggen — ingen eksterne stilark
- Alt JS inline i `<script>`-taggen nederst
- Én HTML-fil, ingen dependencies utover Google Fonts
- `scroll-snap-type: y mandatory` på `.slides`
- `scroll-snap-align: start` på hvert `.slide`
- Hero er alltid slide 0, avslutningsspørsmål alltid siste slide

### Bølgedekorasjon (kommunens signaturelement)

Alle slides har:
- **Bunnbølge** (`::after`): Mørkeblå SVG-bølge, kommunens profil
- **Slør** (`::before`): Gjennomskinnelig sage-grønn bølge øverst til høyre

Hero-slide overstyrer disse med lysere varianter (sage mot navy-bakgrunn).

---

## Slide-typer og komponenter

### 1. Hero-slide

Mørkeblå bakgrunn (`--navy`). Brukes kun som første slide.

```html
<section class="slide slide-hero" data-index="0">
  <div class="slide-inner">
    <div class="hero-kicker">Kontekst · Gruppe · Årstall</div>
    <h1>Tittel <span class="hand">innslag</span></h1>
    <p class="hero-sub">Én setning som setter scenen.</p>
    <div class="hero-facts">
      <div class="fact"><div class="fact-label">Dato</div><div class="fact-value">DD.MM.ÅÅÅÅ</div></div>
      <div class="fact"><div class="fact-label">Tid</div><div class="fact-value">TT:MM</div></div>
      <div class="fact"><div class="fact-label">Sted</div><div class="fact-value">Stedsnavn</div></div>
    </div>
  </div>
  <button class="next-btn" onclick="goNext(0)">↓ Begynn</button>
</section>
```

### 2. Innholds-slide (standardmal)

```html
<section class="slide" data-index="N">
  <div class="slide-inner">
    <div class="slide-tag">Emne</div>
    <h2>Overskrift med eventuelt <span class="hand">håndskrift-innslag</span></h2>
    <p class="lead">Én ingress-setning som bærer poenget.</p>
    <!-- komponent her -->
    <div class="callout">Kursiv sitatstyling for nøkkelpoeng.</div>
  </div>
  <button class="next-btn" onclick="goNext(N)">↓ Neste</button>
</section>
```

**slide-tag:** Alltid med gul dot (via CSS `::before`). Kort, beskrivende.
**H2 + .hand:** Håndskrift på deler av overskriften der det styrker budskapet.
**Callout:** Sage-grønn venstrelinje. Brukes til sitater, nøkkelpoeng, oppsummeringer.

### 3. Tre-kolonne-kort

Brukes til: liste av aktører, perspektiver, nivåer, kategorier.

```html
<div class="three-col">
  <div class="col-card">
    <div class="col-icon">emoji</div>
    <div class="col-title">Tittel</div>
    <ul class="col-list">
      <li>Punkt</li>
    </ul>
  </div>
  <!-- × 3 -->
</div>
```

### 4. Mål-grid (2×2)

Brukes til: fire strategiske mål, fire prinsipper, fire innsatsområder.

```html
<div class="goals-grid">
  <div class="goal-card g1"><!-- g1–g4 gir ulike ikonbakgrunner (sky/sage/amber/rust) -->
    <div class="goal-icon">emoji</div>
    <div>
      <div class="goal-num">01</div>
      <div class="goal-title">Mål</div>
      <div class="goal-meta">Indikatorer · måleparameter</div>
    </div>
  </div>
</div>
```

### 5. Systemflyt

Brukes til: årsak → signal → resultat, input → prosess → output.

```html
<div class="system-flow">
  <div class="sf-block">
    <div class="sf-label">Steg 1 · Beskrivelse</div>
    <div class="sf-title">Navn</div>
    <div class="sf-chips">
      <span class="sf-chip">Nøytral</span>
      <span class="sf-chip rust">Negativ</span>
      <span class="sf-chip green">Positiv</span>
      <span class="sf-chip amber">Mellom</span>
    </div>
  </div>
  <div class="sf-arrow">→</div>
  <!-- flere blokker -->
</div>
```

### 6. Bevegelsesmåler (animert)

Brukes til: fremgang langs en skala, tilstandsoverganger.
Animerer inn ved scroll. Krever `.slide.in-view`-klasse satt av IntersectionObserver.

```html
<div class="movement-track">
  <div class="movement-spine"><div class="movement-fill"></div></div>
  <div class="movement-steps">
    <div class="m-step">
      <div class="m-dot m-dot-rust"></div>
      <div class="m-badge m-rust-b">Tilstand</div>
      <div class="m-title">Navn</div>
      <div class="m-desc">Kort beskrivelse</div>
    </div>
    <!-- × 4 -->
  </div>
</div>
```

Fargeklasser: `m-dot-rust` / `m-dot-amber` / `m-dot-mid` / `m-dot-green`
Badge-klasser: `m-rust-b` / `m-amber-b` / `m-mid-b` / `m-green-b`

### 7. Dashboard-tabell

Brukes til: statusoversikter med trafikklys, prioriteringslister.

```html
<div class="dash">
  <div class="dash-head"><div class="dash-head-title">Tittel</div></div>
  <div class="dash-row">
    <div class="dash-journey">Kategori</div>
    <div class="dash-note">Beskrivelse av situasjonen</div>
    <span class="status-tag tag-red">Status</span>
  </div>
</div>
```

Status-tags: `tag-red` (rust) / `tag-yellow` (amber) / `tag-green`

### 8. Situasjonspanel

Brukes til: konkrete caser, problembilder med data.

```html
<div class="sit-panel">
  <div class="sit-top">
    <div>
      <div class="sit-top-label">Etikett</div>
      <div class="sit-top-title">Tittel</div>
    </div>
    <span class="status-tag tag-red">Status</span>
  </div>
  <div class="sit-body">
    <div class="sit-col">
      <div class="sit-flags">
        <span class="flag-chip">Signal</span>
      </div>
      <p class="sit-human">Sitat eller menneskelig perspektiv.</p>
      <ul class="sit-list"><li>Punkt</li></ul>
    </div>
    <div class="sit-col">
      <!-- Bevegelsesmåling eller annen data -->
      <div class="mov-title">Tittel</div>
      <div class="mov-row">
        <span class="mov-label">Rad</span>
        <span class="mov-num">tall</span>
        <span class="mov-num up">tall</span><!-- up/down for retning -->
      </div>
    </div>
  </div>
</div>
```

### 9. Veivalg-akkordeon

Brukes til: beslutningsalternativer, dilemmaer, valgsituasjoner.
Klikk åpner/lukker via `toggleVV(this)`.

```html
<div class="vv-list">
  <div class="vv-card" onclick="toggleVV(this)">
    <div class="vv-summary">
      <div class="vv-left">
        <span class="vv-letter">1</span>
        <div>
          <div class="vv-name">Navn på veivalget</div>
          <div class="vv-tagline">Kort beskrivelse</div>
        </div>
      </div>
      <div class="vv-right">
        <span class="vv-tag vv-tag-red">Etikett</span>
        <span class="vv-chevron">›</span>
      </div>
    </div>
    <div class="vv-detail">
      <div class="vv-cols">
        <div>
          <div class="vv-section-label">Situasjonen</div>
          <p class="vv-text">Beskrivelse.</p>
          <div class="vv-section-label" style="margin-top:12px;">Men dashboardet viser</div>
          <p class="vv-text">Motargument eller kompliserende faktor.</p>
        </div>
        <div>
          <div class="vv-dilemma">
            <div class="vv-dilemma-label">Det egentlige veivalget</div>
            Dilemmaet formulert som et spørsmål.
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
```

Politisk markert variant: legg til klassen `vv-political` på `.vv-card`.

### 10. Spørsmålsblokk (avslutning)

Brukes alltid som avslutningselement på siste slide.

```html
<div class="q-block">
  <div class="q-eyebrow">Tre spørsmål å lande på</div>
  <div class="q-item">
    <span class="q-num">1</span>
    <span class="q-text">Spørsmål formulert som refleksjonspunkt.</span>
  </div>
  <!-- × 2–4 -->
</div>
```

### 11. Tre-kolonne nivåkort

Brukes til: hvem-gjør-hva, aktøroversikter, ansvarsfordeling.

```html
<div class="levels-grid">
  <div class="level-card">
    <div class="level-who">Aktør / nivå</div>
    <div class="level-point">Punkt</div>
    <div class="level-point">Punkt</div>
  </div>
</div>
```

### 12. 2×2-eierskap-grid

Brukes til: ansvarspunkter, fire prinsipper, fire elementer uten hierarki.

```html
<div class="own-grid">
  <div class="own-card">
    <div class="own-title">Tittel</div>
    <div class="own-desc">Beskrivelse.</div>
  </div>
</div>
```

---

## JavaScript (må alltid med)

```html
<script>
const dots = document.querySelectorAll(".nav-dot");
const slideEls = document.querySelectorAll(".slide");

dots.forEach(dot => {
  dot.addEventListener("click", () => {
    const idx = parseInt(dot.dataset.slide);
    slideEls[idx]?.scrollIntoView({ behavior: "smooth" });
  });
});

function goNext(current) {
  slideEls[current + 1]?.scrollIntoView({ behavior: "smooth" });
}

const observer = new IntersectionObserver(entries => {
  entries.forEach(entry => {
    if (entry.isIntersecting && entry.intersectionRatio > 0.5) {
      const idx = parseInt(entry.target.dataset.index);
      dots.forEach((d, i) => d.classList.toggle("active", i === idx));
      entry.target.classList.add("in-view");
    }
  });
}, { threshold: 0.5 });

slideEls.forEach(s => observer.observe(s));

function toggleVV(card) {
  const isOpen = card.classList.contains("open");
  document.querySelectorAll(".vv-card.open").forEach(c => c.classList.remove("open"));
  if (!isOpen) card.classList.add("open");
}
</script>
```

---

## Designprinsipper

### Innhold
- **Én overskrift per slide** som bærer ett poeng alene
- **Alltid en visuell komponent** — aldri bare tekst og overskrift
- **Callout brukes til nøkkelsetningen** som deltakerne skal huske
- **Spørsmålsblokk avslutter** alltid — presentasjonen ender ikke med konklusjon men med refleksjon

### Visuelle valg
- Bruk `.hand` (Square Peg) til innslag i overskriften som markerer det menneskelige, uformelle eller fremoverlente
- Bruk aldri emoji som primær illustrasjon — kun som ikon i kortkomponenter
- Trafikklysfarger (rust/amber/green) brukes konsekvent: rust = problem/negativ, amber = mellom/utsatt, green = bærekraftig/positiv
- `--navy` er autoritær og trygg — bruk den til alt strukturelt
- `--sage` er varm og åpen — bruk den til bølger, callout og sekundære elementer

### Tekst
- Norsk bokmål
- Kort og direkte — overskrifter er påstander, ikke temaoverskrifter
- Brødtekst aldri mer enn 3 setninger per slide utenfor komponenter
- `.lead` brukes til én åpningssetning som setter rammen

### Illustrasjoner
- Foretrekk illustrasjoner fra bildebanken fremfor emoji
- Bildebank-URL: [legg til når illustrasjoner er publisert]
- Katalog: [legg til README fra illustrations/-mappen]
- Hvis riktig illustrasjon ikke finnes: bruk emoji-ikon i kortkomponent, aldri løs i layout

---

## Referanseimplementasjon

Eksempelfil: `examples/veien-inn-tønsberg.html`

Denne filen demonstrerer alle komponentene i bruk og er Claude-kontekst for
«slik ser det riktige resultatet ut». Les den hvis noe er uklart i SKILL.md.

---

## Slik bruker du denne skillet

**Ny presentasjon:**
> «Les SKILL.md fra [repo-URL]. Lag en presentasjon om [tema] for [målgruppe].
> Den skal ha [N] slides og handle om [formål]. Bruk komponentene fra skillet.»

**Endre eksisterende:**
> «Les SKILL.md fra [repo-URL]. Her er en eksisterende presentasjon: [lim inn HTML].
> Legg til en slide om [tema] etter slide N.»

**Legg til illustrasjon:**
> «Bruk illustrasjonen [URL] på slide N i stedet for emoji-ikonet.»
