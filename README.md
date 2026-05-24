# Local Data Sanitizer

> Sanitize PII in your browser using a 100% local LLM. Zero network calls.

**Live demo:** https://local-data-sanitizer.vercel.app

---

## English

### What it does

You paste text containing PII (names, addresses, phone numbers, emails, IBANs). The page replaces them with `[CENSURÉ]` using **Gemini Nano running on-device via Chrome's built-in Prompt API**. Nothing leaves your tab once the page is loaded.

### Architecture (and why "local" still applies despite Vercel)

- **Vercel** serves a **single static HTML file** (~26 KB). That's it — no backend, no API, no cookie, no logging.
- Once the file is loaded in your browser, the **Gemini Nano LLM runs entirely on your machine** (managed by Chrome).
- After the initial page load, you can **cut your internet connection** and the sanitizer keeps working.
- Open DevTools → Network tab to verify: zero outbound request during inference.

### Why it matters

The instinct of pasting client data into ChatGPT to "summarize it" leaks PII to remote servers. This POC is a 30-second pre-step before any cloud LLM call.

### How to use

The page guides you through 3 or 4 steps depending on your browser state:

1. **Browser check**
2. **Enable Prompt API flag** — Chrome 138-147 only
3. **Download Gemini Nano** (~1.5-4 GB, one-time)
4. **Activate the engine** — one click to load the model into your tab's RAM/VRAM
5. **UI unlocked**

### Requirements

- **Chrome 148+** (stable, no flag needed) or **Chrome 138-147** (flag required) — Edge 138+ may also work
- ~22 GB free disk space (Chrome verifies automatically)
- GPU with ≥ 4 GB VRAM

### Caveats

POC only. LLM-based PII detection is **not GDPR-compliant on its own** — entities may slip through. For production use, pair with deterministic regex/NER rules (e.g. [Microsoft Presidio](https://github.com/microsoft/presidio)).

---

## Français

### À quoi ça sert

Vous collez un texte contenant des données personnelles. La page remplace tout par `[CENSURÉ]` via **Gemini Nano qui tourne sur votre machine via l'API Prompt native de Chrome**. Rien ne quitte votre onglet une fois la page chargée.

### Architecture (et pourquoi « local » reste vrai malgré Vercel)

- **Vercel** sert un **fichier HTML statique unique** (~26 Ko). Point. Aucun backend, aucune API, aucun cookie, aucun log.
- Une fois le fichier chargé dans votre navigateur, le **LLM Gemini Nano tourne entièrement sur votre machine** (géré par Chrome).
- Après le chargement initial de la page, vous pouvez **couper votre connexion internet** et le sanitizer continue de fonctionner.
- Ouvrez DevTools → onglet Network pour vérifier : zéro requête sortante pendant l'inférence.

### Pourquoi c'est utile

Le réflexe de coller des données client dans ChatGPT pour « le résumer » exfiltre des données vers des serveurs distants. Ce POC est une pré-étape de 30 secondes avant tout appel LLM cloud.

### Comment l'utiliser

La page vous guide en 3 ou 4 étapes selon votre navigateur :

1. **Vérification du navigateur**
2. **Activation du flag** — Chrome 138-147 uniquement
3. **Téléchargement de Gemini Nano** (~1,5 à 4 Go, une fois)
4. **Activation du moteur** — un clic pour charger le modèle en RAM
5. **Interface débloquée**

### Prérequis

- **Chrome 148+** (stable, sans flag) ou **Chrome 138-147** (flag requis) — Edge 138+ peut fonctionner aussi
- ~22 Go d'espace disque libre (vérification automatique par Chrome)
- GPU avec ≥ 4 Go de VRAM

### Limites

POC uniquement. La détection PII par LLM seul **n'est pas conforme RGPD** — des entités peuvent passer à travers. Pour la prod, coupler avec des règles regex/NER déterministes (ex. [Microsoft Presidio](https://github.com/microsoft/presidio)).

---

## Tech

- Vanilla JavaScript, single HTML file, **0 dependencies, 0 build step**
- Chrome's [`LanguageModel.create()`](https://developer.chrome.com/docs/ai/prompt-api) global from the Prompt API
- Gemini Nano on-device model managed by Chrome
- Vercel hosting = static file delivery only (no server-side processing)

## License

MIT

---

## Built by

[@jpp75-cc](https://github.com/jpp75-cc) — founder of [HLBRT Consulting](https://github.com/hlbrt-consulting).

This POC was reviewed using the [10-Pass Protocol](https://github.com/hlbrt-consulting/10-pass-protocol) — an adversarial code review protocol developed in production for AI-assisted development.
