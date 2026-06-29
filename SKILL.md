---
name: noiza-tech-pdf
description: Genera documenti tecnici PDF in stile Noiza. Usa quando si deve creare un PDF tecnico, un documento di architettura, una reference, o un recap per clienti. Garantisce coerenza visiva tra tutti i documenti. Trigger su "pdf tecnico", "documento tecnico", "genera pdf", "pdf Noiza", "tech document".
user_invocable: true
---

# Noiza Technical PDF Generator

## Aggiornamento automatico

Prima di qualsiasi altra operazione, aggiorna la skill silenziosamente:

```bash
git -C ~/.claude/skills/noiza-tech-pdf pull --ff-only --quiet 2>/dev/null || true
```

Se fallisce (nessuna connessione, conflitti), prosegui con la versione locale senza segnalare errori.

---

## Workflow

### Se hai un file Markdown come sorgente
1. Leggi il file `.md`
2. Converti il contenuto in HTML usando il template da `references/template.html`
3. Genera il PDF (vedi procedura sotto)

### Se stai creando da zero o da un brief
Valuta l'input prima di procedere:

**Input strutturato** (sezioni chiare, file `.md`, lista ordinata) → genera direttamente senza interrompere.

**Input grezzo** (note di riunione, email, brief informale, bullet sparsi) → prima di generare:
1. Estrai automaticamente: scope, deliverables, pricing/budget (se presenti), timeline, milestone
2. Mostra un riepilogo compatto con i campi estratti — un campo per riga, valori mancanti segnalati esplicitamente
3. Chiedi: *"Procedo con il PDF su questa base, o vuoi modificare qualcosa?"*
4. Aspetta conferma, poi genera

Non chiedere "vuoi che faccia l'analisi?" — falla e mostra il risultato. Un solo checkpoint, non un dialogo.

---

## Regole obbligatorie

- **Mai CSS inline o classi extra** — usa solo le classi in `references/template.html`
- **Mai modificare il CSS del template** nei file di output
- **Genera il PDF con Playwright MCP** (non headless Chrome diretto):
  1. Avvia un server HTTP locale nella directory del progetto:
     ```bash
     python3 -m http.server 8877 &>/tmp/pdf-server.log &
     ```
  2. Usa `mcp__playwright__browser_run_code` per esportare:
     ```js
     async (page) => {
       await page.goto('http://localhost:8877/NOMEFILE.html', { waitUntil: 'networkidle' });
       await page.pdf({
         path: '/path/assoluto/NOMEFILE.pdf',
         format: 'A4',
         displayHeaderFooter: false,
         printBackground: true,
         margin: { top: 0, bottom: 0, left: 0, right: 0 }
       });
       return 'ok';
     }
     ```
  3. Termina il server:
     ```bash
     kill $(lsof -ti:8877) 2>/dev/null
     ```
- Playwright MCP non accetta `file://` — serve sempre il server HTTP locale
- **Layout**: `@page { size: A4; margin: 20mm 22mm; }` — i margini di pagina controllano lo spazio, non il body
- **Font**: Montserrat via Google Fonts (400, 500, 700, 800)
- **Pre blocks**: max ~80 caratteri per riga

---

## Componenti disponibili

| Componente | Classe / Tag | Quando usarlo |
|---|---|---|
| Tabella | `<table>` con `<th>` | Dati strutturati, comparazioni, stime |
| Tabella voci | `<table>` con titolo+`<br>`+descr in cella | Listini, voci di preventivo (vedi components.md) |
| Tabella comparativa | `<table>` con righe-feature × N colonne-opzione | Confronto pacchetti/tier (es. 10/20/40/80h, trimestrale/mensile/settimanale), `&check;` nelle celle |
| Colonna valore | `<th class="num">` / `<td class="num">` | Colonna a destra di tabelle spec/listino — numeri o anche valori testuali ("4 filters", "2 hours", "included"); right-align + tabular-nums + nowrap |
| Codice / flow | `<pre>` | Flussi ASCII, snippet, sequenze |
| Codice inline | `<code>` | Nomi di funzioni, hook, parametri nel testo |
| Nota informativa | `<div class="key-point">` | Raccomandazioni, punti chiave (blu) |
| Avviso / rischio | `<div class="warning">` | Warning, punti critici, cose da verificare (arancione) |
| Riquadro costi | `<div class="price-box">` | Singoli importi evidenziati (hug content, angoli arrotondati) |
| Griglia card | `<div class="team-grid">` con `.price-box` dentro | Riga di card grige (team, profili), allineate a sinistra, senza righe divisorie (vedi components.md) |
| Page break | `<h2 class="page-break">` | Forzare nuova pagina prima di una sezione |
| Etichetta sezione | `<span class="section-label">` | Etichetta uppercase + page break (solo se non ripete il titolo sotto) |
| Badge blu | `<span class="badge">` | Data, versione, categoria |
| Badge arancione | `<span class="badge-orange">` | Uso interno, non condividere |
| Badge verde | `<span class="badge-green">` | Approvato, completato |
| Footer | `<p class="doc-footer">` | Footer documento (anteporre `<hr>`, font ridotto centrato) |
| Separatore | `<hr>` | Fine sezione |

---

## Scrittura — regole anti-slop

Prima di finalizzare il testo del documento, verifica questi punti. Il testo generato da AI tende ad essere verboso e formulaico — un documento Noiza deve suonare scritto da un professionista.

**Frasi da eliminare:**
- "È importante sottolineare che…" → taglia, vai dritto al punto
- "Come indicato in precedenza…" / "Come vedremo…" → ridondante
- "Si tratta di una soluzione che permette di…" → riscrivi in forma diretta
- "In questo contesto…" / "In questa fase…" → taglia
- Liste di tre elementi con struttura identica → variano la struttura o accorpano
- Frasi che iniziano con "Questo" come soggetto vago → nomina il soggetto reale

**Regole attive:**
- Soggetti reali, non astratti: "Noiza configura WPML" non "La configurazione di WPML viene eseguita"
- Una frase = un concetto
- I numeri vanno nelle tabelle, non nel testo (es. "150 articoli, 75 esperienze, 65 operatori" → tabella)
- I box `key-point` e `warning` devono contenere informazioni che non si trovano già nel testo normale
- **Niente em dash (—) in tutto il documento**, non solo nel footer: sostituire con virgola, due punti o parentesi a seconda del contesto. Vale per titoli e testo inline. Esempi: "Cookie bar GDPR — recurring" → "Cookie bar GDPR (recurring)"; "300€ — incluso" → "300€, incluso"; "One-off costs — 30%" → "One-off costs: 30%"

---

## Linee guida editoriali

**Nomenclatura voci numerica**: usare numeri (`01`, `02`, `03`...) e non lettere (`A`, `B`...). Per le sotto-voci usare la notazione decimale (`01.1`, `01.2`). Le lettere con `+` o `++` sembrano classi energetiche, da evitare.

**Servizi continuativi come sezioni standalone**: hosting, pacchetti di assistenza, manutenzione, opzioni strategiche e voci simili vanno trattate come sezioni `<h2>` numerate che continuano la numerazione delle voci puntuali, NON come righe dentro a una tabella generica. Esempio: dopo voci 01-09 della tabella interventi, le sezioni standalone diventano `10. Hosting`, `11. Pacchetto assistenza`, ecc.

**Prezzi nelle tabelle**: usare sempre `class="num"` sulla colonna prezzi per garantire right-align, cifre tabulari e nessun a-capo tra numero e simbolo `€`.

**Sotto-etichette come `h3`, non `<strong>` nel paragrafo**: le label che aprono un blocco ("Page templates", "Design and development", "GA4 configuration", ecc.) vanno in `<h3>` (14pt), non come prima riga in grassetto di un `<p>`. Lo `<strong>` inline resta solo per l'enfasi che continua nella frase (es. "**One-off costs:** 30% advance...").

**Colonna valore nelle tabelle spec/listino**: spostare i numeri fuori dall'etichetta e dentro la colonna valore a destra. Es. "Page templates | 8" invece di "8 page templates | ✓". La colonna valore porta un numero quando esiste, altrimenti `&check;`; può contenere anche valori testuali ("4 filters", "2 hours", "included"). Sempre `class="num"` sulla colonna valore.

**Liste "label + descrizione"**: niente trattino fra label e descrizione. Label in `<strong>`, poi `<br>`, poi la descrizione a capo. Es. `<li><strong>Homepage</strong><br>layout fully custom</li>`.

**Gerarchia titoli**: h1 22pt / h2 18pt / h3 14pt / p 12pt / li 11pt. Già nel CSS del template, non ridefinire inline.

**Tabelle voci**: preferire 2 colonne (titolo + descrizione nella stessa cella | prezzo) invece di 3 colonne (titolo | descrizione | prezzo) quando le due colonne testuali rischiano di andare a capo molte volte.

**Footer**: testo senza italic e senza em dash. Virgola come separatore: *"Preparato da Noiza, Web Development Area"* + a capo + *"Mese Anno"*. Usare il componente `<p class="doc-footer">` con un `<hr>` davanti come stacco.

**Colore brand**: blu ufficiale Noiza `#2563EB`. Già usato in logo, subtitle, badge, key-point. Non sostituire con altre sfumature di blu.

---

## Checklist pre-generazione

- [ ] CSS identico al template, nessuna modifica
- [ ] Solo classi presenti in `references/template.html`
- [ ] Righe nei `<pre>` max ~80 caratteri
- [ ] Header: logo, hr, subtitle+badge, h1, hr, metadata, hr
- [ ] Footer: `<hr>` + `<p class="doc-footer">` (no italic, no em dash, virgola separator)
- [ ] Nessun em dash (—) in tutto il documento (virgola / due punti / parentesi)
- [ ] Sotto-etichette in `<h3>`, non `<strong>` come prima riga di un `<p>`
- [ ] Numeri nella colonna valore (`class="num"`), non dentro l'etichetta
- [ ] Voci numerate `01`, `02`... (mai lettere)
- [ ] Colonne prezzi con `class="num"`
- [ ] Testo rivisto con le regole anti-slop
- [ ] Server HTTP avviato prima dell'export, terminato dopo
