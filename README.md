# 🎬 Producer Challenge — ITS Academy Roberto Rossellini

> Open Day 2026 — Un gioco interattivo per scoprire il mondo della produzione cinematografica.

---

## 📖 Descrizione

Producer Challenge è un gioco browser-based in cui l'utente assume il ruolo di produttore cinematografico con un budget di **3.000.000 €**. Attraverso 6 decisioni strategiche, deve assemblare il proprio film (genere, regia, cast, location, post-produzione, distribuzione) cercando di bilanciare qualità e costi. Il risultato finale determina se il film arriverà al Festival di Cannes, finirà come Flop qualitativo o causerà la Bancarotta della produzione.

---

## 🎮 Come si gioca

1. Scansiona il QR code allo stand Produzione dell'ITS Open Day
2. Clicca **"Inizia ora"** dalla schermata introduttiva
3. Prendi 6 decisioni consecutive, ciascuna con un costo in euro
4. Il gioco calcola automaticamente il risultato finale

---

## 🏗️ Architettura

```
ProdGame.html          ← File unico (HTML + CSS + JS inline)
├── <head>
│   ├── Google Fonts (Barlow + Barlow Condensed)
│   └── CSS inline (~400 righe)
├── <body>
│   ├── HUD fisso (titolo + indicatori di fase)
│   ├── Progress bar
│   └── <div id="app"> ← contenitore dinamico
└── <script>
    ├── DATA: array STAGES[]
    ├── STATE: oggetto state{}
    ├── HELPERS: formattazione, calcoli
    ├── ACTIONS: startGame, makeChoice, confirmVfx, goBack
    └── RENDER: 4 schermate (intro, stage, vfx, end)
```

---

## 📊 Fasi di gioco

| Fase | Titolo | Tipo | Scelte disponibili | Costi |
|------|--------|------|--------------------|-------|
| 1 | Il Genere | choice | Drama Indie / Horror / Sci-Fi | 150K / 300K / 600K |
| 2 | La Regia | choice | Il Maestro / L'Esordiente | 600K / 100K |
| 3A | Cast Femminile | cast | Star / Talents / Indie | 1.2M / 600K / 300K |
| 3B | Cast Maschile | cast | Star / Talents / Indie | 1.2M / 600K / 300K |
| 4 | La Location | choice | Roma / Studio / Virtual Production | 100K / 250K / 500K |
| 5 | VFX & Montaggio | vfx | Auto-deduzione basata sul genere | 100K (std) / 600K (sci-fi) |
| 6 | La Distribuzione | choice | Festival / Social Viral | 100K / 40K |

---

## 🎯 Condizioni di vittoria

| Risultato | Condizione | Emoji |
|-----------|------------|-------|
| 🏆 Cannes | Spesa totale ≥ 1.800.000 € e budget ≥ 0 | cannes |
| 📉 Flop | Spesa totale < 1.800.000 € | flop |
| 💸 Bancarotta | Budget < 0 (sforato il limite) | bankrupt |

---

## 🎨 Design System

### Palette colori

```css
--blue:       #1a6bbf;   /* Primario */
--blue-dark:  #0d3d6e;   /* Background */
--blue-mid:   #1e5fa0;   /* Accento */
--yellow:     #f5c518;   /* CTA / highlight */
--white:      #f0ece4;   /* Testo */
--green:      #2ecc71;   /* Successo */
--orange:     #e67e22;   /* Warning / flop */
--red:        #e74c3c;   /* Errore / bancarotta */
```

### Tipografia

- Titoli: **Barlow Condensed** (weight 800–900)
- Body: **Barlow** (weight 300–500)

### Effetti visivi

- Grain overlay SVG (noise fractale, opacity 0.04)
- Glassmorphism HUD (`backdrop-filter: blur(12px)`)
- Animazioni CSS `fadeInUp` con stagger
- Hover states con gradienti e transform

---

## 🔧 Tecnologie

| Tecnologia | Uso |
|------------|-----|
| HTML5 | Struttura semantica |
| CSS3 | Styling, animazioni, layout responsive |
| Vanilla JS | Logica di gioco, state management, rendering |
| Google Fonts | Barlow + Barlow Condensed |
| SVG inline | Grain texture overlay |

Nessuna dipendenza esterna oltre ai font. Zero framework. Zero build step.

---

## 🚀 Deployment

Il file è completamente statico e può essere deployato su:

- **GitHub Pages** (consigliato)
- Netlify / Vercel (drop & deploy)
- Qualsiasi hosting statico (Apache, Nginx, S3, etc.)

### GitHub Pages

1. Carica `ProdGame.html` in un repository
2. Vai su **Settings → Pages**
3. Seleziona branch `main` e folder `/` (root)
4. Il gioco sarà disponibile su `https://tuo-username.github.io/repository/`

---

## 🛡️ Sicurezza & Privacy

| Aspetto | Stato | Note |
|---------|-------|------|
| Input utente | ✅ Sicuro | Nessun form libero; solo click su card predefinite |
| XSS | ✅ Rischio zero | `innerHTML` usato esclusivamente con dati hardcoded nell'array `STAGES[]`; nessun input utente viene mai interpolato nel DOM |
| LocalStorage | ✅ Non usato | Nessuna persistenza lato client |
| Cookie | ✅ Nessuno | Zero tracking di default |
| HTTPS | ⚠️ Raccomandato | Forzare HTTPS a livello server |
| CSP | ⚠️ Opzionale | Aggiungere meta CSP per hardening |

### Note GDPR

Il gioco nella sua forma attuale **non raccoglie alcun dato personale** e non richiede informativa né consenso.

Se si intende aggiungere raccolta dati (anche anonima) tramite QR code — ad esempio timestamp, user-agent, dimensioni schermo — è necessario predisporre un'informativa ex Art. 13 GDPR prima del deployment. Il "legittimo interesse" ex Art. 6(1)(f) non è una scappatoia automatica: richiede un bilanciamento documentato degli interessi secondo le linee guida EDPB. Per un evento pubblico rivolto a minori (studenti delle superiori), la soglia di cautela è più alta. Consultare il DPO dell'istituto prima di attivare qualsiasi raccolta.

---

## 📝 Changelog

### v3.0 (2026-05-10)
- ✅ Fix critico: rimosso `<base target="_blank">` che rompeva tutti gli `onclick` su Firefox e Safari
- ✅ Fix critico: corretto smart apostrophe (U+2019) in `desc` dell'Esordiente — causava `SyntaxError: missing } after property list`
- ✅ `state.choices.vfx` ora salva il valore numerico del costo invece della stringa `'auto'` — il recap finale legge il valore direttamente senza ricalcolare
- ✅ Rimossa variabile `vfxCost` inutilizzata in `renderEnd`
- ✅ Rimossa soglia flop dall'intro (era stata reintrodotta nella v2)

### v2.0 (2026-05-10)
- ✅ Aggiunta schermata dedicata VFX & Montaggio (Fase 5)
- ✅ Messaggio contestuale VFX basato sul genere scelto (Sci-Fi = costo elevato)
- ✅ Fix numerazione fasi (ora 1→2→3A→3B→4→5→6)
- ✅ Rimossi riferimenti CSS al budget HUD (eliminato per design)
- ✅ Ricalibrazione costi: bancarotta ora raggiungibile su percorsi all-premium (max spend 4.7M > 3M budget)
- ✅ Rimossa visualizzazione budget rimanente dall'HUD
- ✅ Indicatori di fase sostituiti con pallini (countdown visivo, no testo)
- ✅ Rimossa soglia flop dalla schermata intro

### v1.0
- Release iniziale per ITS Open Day 2026

---

## 👥 Crediti

- **Concept & Design:** Corso Cinema & TV Production — ITS Academy Roberto Rossellini
- **Sviluppo:** Team ITS Open Day 2026
- **Font:** Barlow by Jeremy Tribby (OFL)

---

## 📬 Contatti

**ITS Academy Roberto Rossellini**
🌐 [itsrossellini.it](https://itsrossellini.it)
📍 Open Day 2026 — Stand Produzione
