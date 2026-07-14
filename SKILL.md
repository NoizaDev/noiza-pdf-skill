---
name: noiza-docs
description: Scrive e formatta documenti Noiza applicando le regole di scrittura su qualsiasi MD o documento testuale. Genera HTML e PDF solo su richiesta esplicita. Trigger su "documento Noiza", "proposta", "nota tecnica", "md Noiza", "pdf Noiza", "genera pdf".
user_invocable: true
---

# Noiza Documents

Questa skill ha due modalità:

- **Sempre attiva**: regole di scrittura e struttura editoriale. Si applicano su qualsiasi documento Noiza, anche se l'output è solo un file `.md`.
- **Solo su richiesta esplicita di PDF**: creazione del file HTML e export. Non generare HTML se non è stato chiesto un PDF.

---

## Parte 1: Regole di scrittura

Applicare sempre, prima di finalizzare qualsiasi documento Noiza.

### Regole anti-slop

**Frasi da eliminare:**
- "È importante sottolineare che…" → taglia, vai al punto
- "Come indicato in precedenza…" / "Come vedremo…" → ridondante
- "Si tratta di una soluzione che permette di…" → forma diretta
- "In questo contesto…" / "In questa fase…" → taglia
- Liste di tre elementi con struttura identica → varia o accorpa

**Regole attive:**
- Soggetti reali, non astratti: "Noiza configura WPML" non "La configurazione di WPML viene eseguita"
- Frasi che iniziano con "Questo" come soggetto vago: nomina il soggetto reale
- Una frase, un concetto
- I numeri vanno nelle tabelle, non nel testo: "150 articoli, 75 esperienze, 65 operatori" → tabella
- I box `key-point` e `warning` contengono solo informazioni che non compaiono già nel testo normale
- **Niente em dash (—)**: sostituire con virgola, due punti o parentesi. Vale per titoli e testo inline. Esempi: "Cookie bar GDPR — recurring" → "Cookie bar GDPR (recurring)"; "300€ — incluso" → "300€, incluso"; "One-off costs — 30%" → "One-off costs: 30%"

---

## Parte 2: Struttura e linee guida editoriali

Applicare sempre quando si struttura un documento Noiza.

### Nomenclatura voci

Numeri (`01`, `02`, `03`...), non lettere (`A`, `B`...). Sotto-voci in notazione decimale (`01.1`, `01.2`). Le lettere con `+` o `++` sembrano classi energetiche: evitare.

### Servizi continuativi

Hosting, manutenzione, pacchetti di assistenza e opzioni strategiche vanno come sezioni numerate in continuità con le voci puntuali, non come righe dentro una tabella generica.

### Gerarchie e sotto-etichette

Le label che aprono un blocco ("Page templates", "GA4 configuration") vanno in `h3` (o `###` in Markdown), non come prima riga in grassetto di un paragrafo. Il grassetto inline resta solo per l'enfasi che continua nella frase.

### Colonna valore nelle tabelle spec/listino

I numeri vanno nella colonna valore a destra, non dentro l'etichetta. "Page templates | 8" invece di "8 page templates | ✓". La colonna valore porta un numero, un check, o un valore testuale breve ("4 filters", "included").

### Liste "label + descrizione"

Niente trattino fra label e descrizione. Label in grassetto, poi a capo, poi la descrizione.

### Footer

Testo senza italic e senza em dash. Virgola come separatore: "Preparato da Noiza, Web Development Area" + a capo + "Mese Anno".

### Colore brand

Blu ufficiale Noiza: `#2563EB`. Non sostituire con altre sfumature.

### Team di progetto (documenti per clienti)

Se il documento è destinato a un cliente esterno (proposta, preventivo, contratto, tender), chiedere: "Ti serve inserire un team di progetto? Indicami i nomi (e dimmi se vuoi anche bio e anno di ingresso per ciascuno, altrimenti resto su nome, ruolo e mail)."

Se l'utente indica solo il nome proprio (es. "Federica e Giulia"), completare automaticamente cognome e ruolo consultando https://noiza.com/team/, ed email consultando `references/team-emails.md` (lista storica per le eccezioni, altrimenti `nome.cognome@noiza.com`). Non chiedere conferma dei dati recuperati, salvo nomi ambigui o assenti dalla pagina (in quel caso segnalarlo e chiedere il dato mancante).

Bio e anno di ingresso non si ricavano dalla pagina team né si inventano: vanno sempre forniti dall'utente. Se richiesti ma mancanti per una o più persone, chiederli esplicitamente invece di generarli.

Layout di default: `.team-grid` (nome, ruolo, email; card ancorate a sinistra, vanno a capo su più righe se non entrano su una sola). Usare `.team-proposal-grid` (2 colonne, con bio e opzionale anno di ingresso) solo se l'utente ha chiesto esplicitamente bio/anno.

---

## Parte 3: Export PDF

**Eseguire questa sezione solo se è stato richiesto esplicitamente un PDF.**

Se l'output richiesto è solo un file `.md`, fermarsi alla Parte 2.

### Fonte del documento

**Da un file Markdown esistente:**
1. Leggi il file `.md`
2. Converti in HTML usando il template da `references/template.html`
3. Incorpora il font (vedi sotto) e genera il PDF

**Da zero o da un brief:**
- **Input strutturato** (sezioni chiare, file `.md`, lista ordinata): genera direttamente.
- **Input grezzo** (note di riunione, email, brief informale): estrai automaticamente scope, deliverables, pricing, timeline e milestone; mostra un riepilogo compatto con i campi estratti e i valori mancanti segnalati; chiedi conferma prima di generare. Un solo checkpoint, non un dialogo.

### Regole HTML obbligatorie

- Solo le classi presenti in `references/template.html`: niente CSS inline, niente classi extra
- Non modificare il CSS del template nei file di output
- Layout: `@page { size: A4; margin: 20mm 22mm; }` — i margini di pagina controllano lo spazio, non il body
- Pre blocks: max ~80 caratteri per riga

### Font: embed base64 obbligatorio

`@import url(https://fonts.googleapis.com/...)` non funziona nell'export PDF: Chrome headless non carica risorse esterne. Montserrat va incorporata come data URI nel CSS dell'HTML, prima di esportare.

**Procedura:**

```bash
# Scarica i due subset (variable font: un file per tutti i pesi)
curl -s "https://fonts.gstatic.com/s/montserrat/v31/JTUSjIg1_i6t8kCHKm459WlhyyTh89Y.woff2" -o montserrat-latin.woff2
curl -s "https://fonts.gstatic.com/s/montserrat/v31/JTUSjIg1_i6t8kCHKm459WdhyyTh89ZNpQ.woff2" -o montserrat-latin-ext.woff2
```

```python
import base64

latin     = base64.b64encode(open("montserrat-latin.woff2",     "rb").read()).decode()
latin_ext = base64.b64encode(open("montserrat-latin-ext.woff2", "rb").read()).decode()

# Unicode ranges
LATIN_EXT_RANGE = "U+0100-02BA, U+02BD-02C5, U+02C7-02CC, U+02CE-02D7, U+02DD-02FF, U+0304, U+0308, U+0329, U+1D00-1DBF, U+1E00-1E9F, U+1EF2-1EFF, U+2020, U+20A0-20AB, U+20AD-20C0, U+2113, U+2C60-2C7F, U+A720-A7FF"
LATIN_RANGE     = "U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD"

font_face_block = ""
for weight in [400, 500, 700, 800]:
    font_face_block += f"@font-face {{ font-family: 'Montserrat'; font-style: normal; font-weight: {weight}; font-display: swap; src: url('data:font/woff2;base64,{latin_ext}') format('woff2'); unicode-range: {LATIN_EXT_RANGE}; }}\n"
    font_face_block += f"@font-face {{ font-family: 'Montserrat'; font-style: normal; font-weight: {weight}; font-display: swap; src: url('data:font/woff2;base64,{latin}') format('woff2'); unicode-range: {LATIN_RANGE}; }}\n"

with open("NOMEFILE.html") as f:
    html = f.read()

html = html.replace(
    "  @import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500;700;800&display=swap');",
    font_face_block
)

with open("NOMEFILE.html", "w") as f:
    f.write(html)
```

### Procedura di export

**Opzione A: Playwright MCP** (se disponibile, preferita)

1. Avvia il server HTTP nella directory del progetto:
   ```bash
   python3 -m http.server 8877 &>/tmp/pdf-server.log &
   ```
2. Esporta con `mcp__playwright__browser_run_code`:
   ```js
   async (page) => {
     await page.goto('http://localhost:8877/NOMEFILE.html', { waitUntil: 'networkidle' });
     await page.pdf({
       path: '/path/assoluto/NOMEFILE.pdf',
       format: 'A4',
       displayHeaderFooter: false,
       printBackground: true,
       preferCSSPageSize: true,
       margin: { top: 0, bottom: 0, left: 0, right: 0 }
     });
     return 'ok';
   }
   ```
3. Termina il server: `kill $(lsof -ti:8877) 2>/dev/null`

**Opzione B: CDP via Python** (se Playwright MCP non è disponibile)

Il flag CLI `--print-to-pdf-no-header` non rimuove in modo affidabile le intestazioni predefinite di Chrome. Usare il Chrome DevTools Protocol, che permette di impostare `displayHeaderFooter: false` direttamente.

Prerequisito: `pip3 install websockets`

```python
import asyncio, json, base64, subprocess, time, urllib.request, websockets

HTML_URL = "http://localhost:8877/NOMEFILE.html"
PDF_PATH = "/path/assoluto/NOMEFILE.pdf"

async def export():
    proc = subprocess.Popen([
        "/Applications/Google Chrome.app/Contents/MacOS/Google Chrome",
        "--headless=new", "--remote-debugging-port=9222",
        "--disable-gpu", "--no-sandbox", "--no-first-run",
        "--user-data-dir=/tmp/chrome-pdf-tmp"
    ])
    time.sleep(2)

    tabs = json.loads(urllib.request.urlopen("http://localhost:9222/json").read())
    ws_url = tabs[0]["webSocketDebuggerUrl"]

    async with websockets.connect(ws_url, max_size=200*1024*1024) as ws:
        async def send(method, params={}):
            await ws.send(json.dumps({"id": 1, "method": method, "params": params}))
            while True:
                msg = json.loads(await ws.recv())
                if msg.get("id") == 1:
                    return msg.get("result", {})

        await send("Page.enable")
        await send("Page.navigate", {"url": HTML_URL})
        await asyncio.sleep(3)

        result = await send("Page.printToPDF", {
            "printBackground": True,
            "displayHeaderFooter": False,
            "paperWidth": 8.27,
            "paperHeight": 11.69,
            "marginTop": 0, "marginBottom": 0,
            "marginLeft": 0, "marginRight": 0,
            "preferCSSPageSize": True,
        })

        with open(PDF_PATH, "wb") as f:
            f.write(base64.b64decode(result["data"]))

    proc.terminate()

asyncio.run(export())
```

Salva lo script, sostituisci `HTML_URL` e `PDF_PATH`, poi:
```bash
python3 -m http.server 8877 &>/tmp/pdf-server.log &
python3 export_pdf.py
kill $(lsof -ti:8877) 2>/dev/null
```

### Componenti disponibili

| Componente | Classe / Tag | Quando usarlo |
|---|---|---|
| Tabella | `<table>` con `<th>` | Dati strutturati, comparazioni, stime |
| Tabella voci | `<table>` titolo+`<br>`+descr in cella | Listini, voci di preventivo |
| Tabella comparativa | `<table>` righe-feature × N colonne-opzione | Confronto pacchetti/tier, `&check;` nelle celle |
| Colonna valore | `<th class="num">` / `<td class="num">` | Prezzi, numeri, valori brevi nelle tabelle |
| Codice / flow | `<pre>` | Flussi ASCII, snippet, sequenze |
| Codice inline | `<code>` | Nomi di funzioni, hook, parametri nel testo |
| Nota informativa | `<div class="key-point">` | Raccomandazioni, punti chiave (blu) |
| Avviso / rischio | `<div class="warning">` | Warning, punti critici (arancione) |
| Riquadro costi | `<div class="price-box">` | Singoli importi evidenziati |
| Griglia card (default team) | `<div class="team-grid">` con `.price-box` | Team di progetto: nome, ruolo, email. Ancorata a sx, va a capo su più righe se necessario |
| Griglia card (2 col, con bio) | `<div class="team-proposal-grid">` con `.team-proposal-card` | Solo su richiesta esplicita di bio/descrizione estesa, anno opzionale |
| Anno card | `<span class="card-year">` | Badge anno in alto a destra nella card |
| Tavola dei contenuti | `<div class="toc-box">` con `.toc-row` e `.toc-num` | Indice con anchor link, sfondo grigio |
| Page break | `<h2 class="page-break">` | Nuova pagina prima di una sezione |
| Etichetta sezione | `<span class="section-label">` | Etichetta uppercase + page break |
| Badge blu | `<span class="badge">` | Data, versione, categoria |
| Badge arancione | `<span class="badge-orange">` | Uso interno, non condividere |
| Badge verde | `<span class="badge-green">` | Approvato, completato |
| Footer | `<p class="doc-footer">` | Footer documento (anteporre `<hr>`) |
| Separatore | `<hr>` | Fine sezione |

Esempi HTML pronti in `references/components.md`.

### Checklist pre-export

- [ ] Font Montserrat incorporata come base64 (nessun `@import` Google Fonts)
- [ ] CSS identico al template, nessuna modifica
- [ ] Solo classi presenti in `references/template.html`
- [ ] Righe nei `<pre>` max ~80 caratteri
- [ ] Header: logo, hr, subtitle+badge, h1, hr, metadata, hr
- [ ] Footer: `<hr>` + `<p class="doc-footer">` (no italic, no em dash, virgola separator)
- [ ] Nessun em dash (—) in tutto il documento
- [ ] Sotto-etichette in `<h3>`, non `<strong>` come prima riga di un `<p>`
- [ ] Numeri nella colonna valore (`class="num"`), non dentro l'etichetta
- [ ] Voci numerate `01`, `02`... (mai lettere)
- [ ] Colonne prezzi con `class="num"`
- [ ] Testo rivisto con le regole anti-slop (Parte 1)
- [ ] Server HTTP avviato prima dell'export, terminato dopo
