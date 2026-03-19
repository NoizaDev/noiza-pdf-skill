---
name: noiza-tech-pdf
description: Genera documenti tecnici PDF in stile Noiza. Usa quando si deve creare un PDF tecnico, un documento di architettura, una reference, o un recap per clienti. Garantisce coerenza visiva tra tutti i documenti. Trigger su "pdf tecnico", "documento tecnico", "genera pdf", "pdf Noiza", "tech document".
user_invocable: true
---

# Noiza Technical PDF Generator

## Workflow

### Se hai un file Markdown come sorgente
1. Leggi il file `.md`
2. Converti il contenuto in HTML usando il template da `references/template.html`
3. Genera il PDF (vedi procedura sotto)

### Se stai creando da zero
1. Leggi `references/template.html` per il CSS completo
2. Leggi `references/components.md` per il catalogo dei componenti
3. Scrivi il contenuto nel template
4. Genera il PDF

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
| Codice / flow | `<pre>` | Flussi ASCII, snippet, sequenze |
| Codice inline | `<code>` | Nomi di funzioni, hook, parametri nel testo |
| Nota informativa | `<div class="key-point">` | Raccomandazioni, punti chiave (blu) |
| Avviso / rischio | `<div class="warning">` | Warning, punti critici, cose da verificare (arancione) |
| Riquadro costi | `<div class="price-box">` | Preventivi, riepiloghi economici |
| Etichetta sezione | `<span class="section-label">` | Label + page break prima di un h2 |
| Badge blu | `<span class="badge">` | Data, versione, categoria |
| Badge arancione | `<span class="badge-orange">` | Uso interno, non condividere |
| Badge verde | `<span class="badge-green">` | Approvato, completato |
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

---

## Checklist pre-generazione

- [ ] CSS identico al template, nessuna modifica
- [ ] Solo classi presenti in `references/template.html`
- [ ] Righe nei `<pre>` max ~80 caratteri
- [ ] Header: logo, hr, subtitle+badge, h1, hr, metadata, hr
- [ ] Footer: hr, prepared by, mese anno
- [ ] Testo rivisto con le regole anti-slop
- [ ] Server HTTP avviato prima dell'export, terminato dopo
