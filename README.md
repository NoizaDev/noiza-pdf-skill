# noiza-pdf-skill

Skill per Claude Code che genera documenti tecnici PDF in stile Noiza: proposte, reference, note interne, recap per clienti.

**Stack:** HTML + CSS Noiza → Playwright MCP → PDF A4

---

## Installazione

```bash
git clone git@github.com:NoizaDev/noiza-pdf-skill.git ~/.claude/skills/noiza-tech-pdf
```

Prerequisiti: Claude Code con Playwright MCP abilitato (nessuna dipendenza npm).

Aggiornamento:
```bash
cd ~/.claude/skills/noiza-tech-pdf && git pull
```

---

## Come si usa

### Opzione A — da Markdown (consigliata)

Scrivi il contenuto del documento in un file `.md` nella cartella del progetto, poi chiedi a Claude:

> "Genera il PDF da `proposta-cliente.md` in stile Noiza"

Claude legge il Markdown, lo converte in HTML con il template Noiza e produce il PDF.

**Nel file `.md` puoi usare:**
- Markdown standard per testo, titoli, liste, tabelle, codice
- HTML inline per i componenti speciali (box, badge, section label) — vedi sotto

### Opzione B — da zero

> "Crea un PDF tecnico Noiza: proposta per [cliente] su [argomento]"

Claude genera l'HTML e il PDF direttamente.

---

## Componenti disponibili

| Componente | Uso |
|---|---|
| `<div class="key-point">` | Nota informativa, raccomandazione (sfondo blu chiaro) |
| `<div class="warning">` | Avviso, punto critico, rischio (sfondo arancione chiaro) |
| `<div class="price-box">` | Preventivo, riepilogo costi |
| `<span class="section-label">` | Etichetta sezione + page break (prima di un h2) |
| `<span class="badge">` | Badge blu (data, versione) |
| `<span class="badge-orange">` | Badge arancione (uso interno, warning) |
| `<span class="badge-green">` | Badge verde (approvato, completato) |
| `<table>` | Tabelle con header blu, righe alternate |
| `<pre>` | Flow diagram ASCII, blocchi codice |
| `<code>` | Codice inline nel testo |

Esempi HTML pronti in `references/components.md`.

---

## Regola d'oro sul CSS

**Non aggiungere mai CSS ai documenti.** Tutto lo stile è in `references/template.html`. Se un componente non esiste, aggiungilo al template e aggiorna `components.md` — non metterlo inline nel documento.

---

## Qualità del testo

I documenti Noiza non devono sembrare scritti da un'AI. Prima di generare il PDF, Claude applica automaticamente queste regole:

- Soggetti reali e concreti, non astratti
- Fatti prima, spiegazioni dopo
- I numeri vanno nelle tabelle, non nel testo
- Zero frasi di riscaldamento ("È importante sottolineare", "Come vedremo", "In questo contesto")
- I box `key-point` e `warning` contengono solo informazioni che non compaiono già nel testo normale

---

## Struttura del repo

```
noiza-pdf-skill/
├── SKILL.md                  ← istruzioni per Claude (non modificare se non sai cosa fai)
├── README.md                 ← questo file
└── references/
    ├── template.html         ← CSS Noiza completo + struttura base
    └── components.md         ← catalogo componenti con snippet HTML
```

---

## Contribuire

Per aggiungere un componente o correggere un bug:
1. Modifica `references/template.html` (CSS) e `references/components.md` (esempio)
2. Aggiorna la tabella componenti in `SKILL.md`
3. PR su `main` con una riga di descrizione nel commit

---

## Maintainer

This project is maintained by [Martino Stenta](https://github.com/martinostenta).
