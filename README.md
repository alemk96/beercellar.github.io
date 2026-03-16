<div align="center">
 
# 🍺 Beer Cellar
 
### La tua cantina digitale per birre artigianali
 
[![PWA](https://img.shields.io/badge/PWA-ready-F07828?style=for-the-badge&logo=pwa&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps)
[![HTML5](https://img.shields.io/badge/HTML5-single--file-E34F26?style=for-the-badge&logo=html5&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/HTML)
[![JavaScript](https://img.shields.io/badge/JavaScript-vanilla-F4C840?style=for-the-badge&logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![License](https://img.shields.io/badge/license-MIT-8ABF40?style=for-the-badge)](LICENSE)
 
<br/>
 
> Traccia, valuta e scopri le birre artigianali del birrificio **50&50 Craft Brewery** —  
> tutto offline, tutto nel browser, zero backend.
 
<br/>
 
![Beer Cellar Preview](https://via.placeholder.com/720x400/130F08/F07828?text=Beer+Cellar+Preview)
<!-- Sostituisci con uno screenshot reale dell'app -->
 
</div>
 
---
 
## ✨ Funzionalità
 
### 🗂 Gestione Cantina
- **Catalogo completo** — tutte le birre 50&50 precaricate con schede dettagliate; in futuro potrei aggiornare con altri Birrifici Artigianali o altre entries estemporanee maaaaa... puoi farlo anche te ;)
- **Aggiungi birre personalizzate** — nome, birrificio, origine, ABV, IBU, colore
- **Modifica ed elimina** — aggiornamento completo di ogni scheda
- **Categorie** — 12 stili (Lager, IPA, Stout, Weizen, Sour, Trappista, e altri)
 
### ⭐ Valutazione
- **Rating a mezze stelle** — da 0 a 5 con granularità di 0.5
- **Etichette dinamiche** — da *"Non è per me"* a *"Capolavoro assoluto!"*
- **Profilo organolettico** — note su aroma, sapore, corpo, finale
- **Storia e curiosità** — background di ogni birra
- **Note personali** — abbinamenti, occasioni, ricordi
 
### 🔍 Ricerca & Filtri
- **Ricerca full-text** — per nome, birrificio, origine
- **Filtro per categoria** — chip selezionabili in scrolling orizzontale
- **Filtri rapidi** — Tutte / Provate / Preferite
- **Ordinamento** — per data, voto, nome A–Z, birrificio
 
### 📊 Statistiche
- **KPI dashboard** — birre provate, preferite, voto medio, stili scoperti
- **Top 5 birre** — classifica con medaglie per le più votate
- **Grafico per stile** — distribuzione visiva del catalogo
 
### 💾 Import / Export
- **Export JSON** — salvataggio della cantina completa con File System Access API + fallback download
- **Import JSON** — con due strategie: *Unisci* (mantieni esistenti) o *Sostituisci tutto*
- **Validazione robusta** — sanitizzazione campi, deduplicazione ID, compatibilità formati
 
### 📱 Progressive Web App
- **Installabile** — su Android, iOS (Safari), desktop Chrome/Edge
- **Offline-first** — funziona completamente senza connessione dopo il primo carico
- **Service Worker** — cache-first per l'app shell, stale-while-revalidate per i font
- **Shortcuts** — pressione lunga sull'icona per accesso rapido ad "Aggiungi" e "Statistiche"
- **Update toast** — notifica in-app quando è disponibile una nuova versione
- **Barra offline** — indicatore visivo quando la connessione è assente
 
---
 
## 🏗 Architettura
 
```
beer-cellar/
├── index.html              # App shell completa (single-file SPA)
├── manifest.json           # Web App Manifest
├── sw.js                   # Service Worker
├── _headers                # Cloudflare Pages — header HTTP
└── icons/
    ├── icon.svg            # Icona vettoriale universale
    ├── icon-72/96/128/192/512.png
    ├── icon-maskable-192/512.png
    ├── shortcut-add.png
    └── shortcut-stats.png
```
 
### Stack tecnico
 
| Layer | Scelta | Motivazione |
|---|---|---|
| Frontend | Vanilla JS + HTML/CSS | Zero dipendenze, carico istantaneo |
| Persistenza | `localStorage` | Nessun backend, privacy totale |
| Font | Bebas Neue + Barlow (Google Fonts) | Identità visiva taproom industrial |
| Deploy | Cloudflare Pages | Edge CDN globale, HTTPS automatico |
| Icone | Pillow (generazione) + SVG | Controllate al pixel, brand-consistent |
 
### Strategie di caching (Service Worker)
 
```
Richiesta locale (app shell)  →  Cache-First
                                 └─ Miss? → Fetch + aggiorna cache
                                 └─ Offline? → Fallback su index.html
 
Font Google                   →  Stale-While-Revalidate
                                 └─ Risposta immediata dalla cache
                                 └─ Aggiornamento silenzioso in background
 
Risorse esterne               →  Network-First
                                 └─ Fallback su cache se offline
```
 
---
 
## 🚀 Deploy su Cloudflare Pages
 
### 1. Fork e connetti il repository
 
```bash
# Clona il repository
git clone https://github.com/tuo-username/beer-cellar.git
cd beer-cellar
```
 
Nel dashboard Cloudflare Pages:
1. **Create a project** → Connetti il repository GitHub
2. **Build settings**: lascia tutto vuoto — nessun build command, nessun framework
3. **Root directory**: `/` (root del repository)
4. Deploy
 
### 2. Verifica il file `_headers`
 
Il file `_headers` nella root è **critico** per il funzionamento della PWA:
 
```
/sw.js
  Cache-Control: no-cache, no-store, must-revalidate
  Content-Type: application/javascript; charset=utf-8
 
/manifest.json
  Cache-Control: public, max-age=3600
  Content-Type: application/manifest+json; charset=utf-8
```
 
> ⚠️ Senza questo file il Service Worker non si registra e l'app non è installabile.
 
### 3. Installa la PWA
 
| Piattaforma | Procedura |
|---|---|
| **Android** (Chrome) | Banner automatico dopo qualche secondo → *Installa* |
| **iOS** (Safari) | Condividi `⎙` → *Aggiungi a schermata Home* |
| **Desktop** (Chrome/Edge) | Icona `⊕` nella barra indirizzi → *Installa* |
 
---
 
## 🛠 Sviluppo locale
 
Non è necessario nessun bundler. Apri semplicemente con un server HTTP locale per evitare restrizioni CORS sui Service Worker:
 
```bash
# Python 3
python3 -m http.server 8080
 
# Node.js (npx)
npx serve .
 
# oppure VS Code → Live Server extension
```
 
Poi apri `http://localhost:8080` — il SW si registra automaticamente su `localhost`.
 
### Debug PWA
 
```
Chrome DevTools → Application
├── Manifest      → verifica icone e campi senza errori rossi
├── Service Workers → stato: "activated and running"
└── Storage → Local Storage → beer-cellar-5050-v4 → dati cantina
```
 
---
 
## 📦 Dati & Privacy
 
- Tutti i dati sono salvati **esclusivamente in `localStorage`** nel dispositivo dell'utente
- Nessun dato viene trasmesso a server esterni
- L'export JSON è un backup locale, mai caricato automaticamente
- Nessun tracker, nessuna analytics, nessun cookie
 
---
 
## 📄 Licenza
 
Distribuito sotto licenza **MIT**. Vedi [`LICENSE`](LICENSE) per i dettagli.
 
---
 
<div align="center">
 
Fatto con ❤️ e 🍺 &nbsp;·&nbsp; 
 
</div>
