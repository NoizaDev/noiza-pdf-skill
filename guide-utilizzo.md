---
tipo: guida-utilizzo
versione: 1.1
---

## Prerequisiti

Prima di iniziare, assicurati di avere:

- **Claude Code** installato — come estensione in VS Code o Antrigravity
- **Playwright MCP** abilitato nelle impostazioni di Claude Code
- **Google Drive Desktop** installato e sincronizzato con il tuo account Noiza — i documenti condivisi si trovano in Drive, non in locale
- **La skill installata** — chiedi a chi gestisce la skill il comando di installazione, oppure segui la guida contribuzione

---

## Generare un documento

### Opzione A — da Markdown (consigliata per progetti condivisi)

1. Crea o apri il file `.md` sorgente nella cartella del progetto su Google Drive
2. Scrivi il contenuto in Markdown standard
3. Per i componenti speciali (box, badge, riquadri costi), usa HTML inline — gli esempi sono in `references/components.md` nella cartella della skill
4. Apri il progetto in Claude Code e chiedi:

> "Genera il PDF da `nome-file.md` in stile Noiza"

Claude converte il Markdown in HTML con il template Noiza e produce il PDF nella stessa cartella Drive.

### Opzione B — da zero

Apri il progetto in Claude Code e descrivi il documento:

> "Crea un documento tecnico Noiza: proposta per [cliente] su [argomento]. Includi: [sezioni principali]"

Claude genera HTML e PDF direttamente.

---

## Componenti disponibili

Tutti i componenti sono già inclusi nel CSS. Non serve aggiungere stili.

| Componente | HTML nel .md | Effetto |
|---|---|---|
| Nota informativa | `<div class="key-point"><p>testo</p></div>` | Box blu chiaro |
| Avviso / rischio | `<div class="warning"><p>testo</p></div>` | Box arancione |
| Riquadro costi | `<div class="price-box"><p>testo</p></div>` | Box grigio bordo blu |
| Nuova sezione | `<span class="section-label">Label</span>` | Page break + label |
| Badge blu | `<span class="badge">Testo</span>` | Etichetta blu |
| Badge arancione | `<span class="badge-orange">Testo</span>` | Etichetta arancione |
| Badge verde | `<span class="badge-green">Testo</span>` | Etichetta verde |

Esempi completi: apri `references/components.md` nella cartella della skill e chiedi a Claude di mostrarteli.

---

## Progetti condivisi su Google Drive

Se il progetto ha il campo `shared:` impostato nel suo `CLAUDE.md`, Claude legge e salva i file automaticamente su Drive. Il PDF finale appare nella cartella condivisa senza passi aggiuntivi.

Per impostare un progetto come condiviso, dì a Claude:

> "Questo progetto è condiviso, la cartella Drive è Clienti > NomeCliente"

Claude aggiorna il `CLAUDE.md` del progetto.

---

## Aggiornare un documento esistente

Modifica il `.md` sorgente su Drive, poi chiedi a Claude:

> "Rigenera il PDF da `nome-file.md`"

Claude sovrascrive `.html` e `.pdf`. Il `.md` è sempre la sorgente di verità — non modificare mai direttamente l'HTML generato.

---

## Regola d'oro

<div class="warning">
  <p><strong>Non modificare mai il CSS nei documenti generati.</strong> Se un componente grafico non esiste, segnalalo a chi gestisce la skill. Aggiungere stili inline rompe la coerenza visiva di tutti i documenti.</p>
</div>
