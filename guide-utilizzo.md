---
tipo: guida-utilizzo
versione: 1.0
---

## Prerequisiti

Prima di iniziare, verifica di avere:

- **Claude Code** installato (`claude` da terminale)
- **Playwright MCP** abilitato nelle impostazioni di Claude Code
- **La skill installata** in `~/.claude/skills/noiza-tech-pdf/`

Installazione skill:
```bash
git clone git@github.com:NoizaDev/noiza-pdf-skill.git ~/.claude/skills/noiza-tech-pdf
```

Aggiornamento:
```bash
cd ~/.claude/skills/noiza-tech-pdf && git pull
```

---

## Generare un documento

### Opzione A — da Markdown (consigliata per progetti condivisi)

1. Crea o apri il file `.md` sorgente nella cartella del progetto (locale o su Drive)
2. Scrivi il contenuto in Markdown standard
3. Per i componenti speciali (box, badge, riquadri), usa HTML inline — gli esempi sono in `references/components.md` nella cartella della skill
4. Chiedi a Claude:

> "Genera il PDF da `nome-file.md` in stile Noiza"

Claude converte il Markdown in HTML con il template Noiza e produce il PDF nella stessa cartella.

### Opzione B — da zero

Descrivi il documento a Claude direttamente:

> "Crea un documento tecnico Noiza: proposta per [cliente] su [argomento]. Includi: [sezioni principali]"

Claude genera HTML e PDF senza che tu debba scrivere nulla.

---

## Componenti disponibili

Tutti i componenti sono già inclusi nel CSS. Non serve aggiungere stili.

| Componente | Come si scrive nel `.md` | Effetto |
|---|---|---|
| Nota informativa | `<div class="key-point"><p>testo</p></div>` | Box blu chiaro |
| Avviso / rischio | `<div class="warning"><p>testo</p></div>` | Box arancione |
| Riquadro costi | `<div class="price-box"><p>testo</p></div>` | Box grigio con bordo blu |
| Nuova sezione | `<span class="section-label">Etichetta</span>` | Page break + label blu |
| Badge blu | `<span class="badge">Testo</span>` | Etichetta blu |
| Badge arancione | `<span class="badge-orange">Testo</span>` | Etichetta arancione |
| Badge verde | `<span class="badge-green">Testo</span>` | Etichetta verde |

Esempi completi: apri `~/.claude/skills/noiza-tech-pdf/references/components.md`

---

## Progetti condivisi su Google Drive

Se il progetto ha `shared:` impostato nel `CLAUDE.md`, Claude legge e salva i file direttamente su Drive. Non devi fare nulla di diverso: il PDF finale apparirà nella cartella Drive condivisa.

Per impostare un progetto come condiviso, dì a Claude:

> "Questo progetto è condiviso, la cartella Drive è Clienti > NomeCliente"

Claude aggiorna il `CLAUDE.md` del progetto.

---

## Regola d'oro

**Non modificare mai il CSS nei documenti generati.** Se un componente grafico non esiste, chiedi a chi gestisce la skill di aggiungerlo — non aggiungerlo inline nel documento. Questo garantisce che tutti i documenti restino visivamente coerenti nel tempo.

---

## Aggiornare un documento esistente

Se il `.md` sorgente esiste già, modificalo e poi chiedi:

> "Rigenera il PDF da `nome-file.md`"

Claude sovrascrive `.html` e `.pdf`. Il `.md` resta invariato — è sempre la sorgente di verità.
