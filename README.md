# noiza-docs

Skill per Claude Code che scrive e formatta documenti Noiza: proposte, note tecniche, reference, recap per clienti. Le regole di scrittura si applicano su qualsiasi documento o file `.md`. L'export PDF è disponibile su richiesta esplicita.

---

## Come si usa

### Scrivere o rivedere un MD

Chiedi semplicemente di creare o rivedere un documento seguendo le linee guida Noiza:

> "Crea la proposta per [cliente] su [argomento] seguendo le linee guida Noiza"

> "Rivedi questo documento applicando le regole di scrittura Noiza"

Claude applica automaticamente le regole anti-slop, l'assenza di em dash, i soggetti reali e le convenzioni di struttura. Nessun HTML, nessun PDF generato a meno che non lo chieda.

### Generare un PDF

Solo quando hai bisogno del file PDF:

> "Genera il PDF da `proposta-cliente.md` in stile Noiza"

> "Crea un PDF tecnico Noiza: proposta per [cliente] su [argomento]"

Claude converte il contenuto in HTML usando il template Noiza e produce il PDF via Playwright.

---

## Regole sempre attive

- Niente em dash (—): virgola, due punti o parentesi a seconda del contesto
- Soggetti reali e concreti, non astratti
- I numeri vanno nelle tabelle, non nel testo
- Zero frasi di riscaldamento ("È importante sottolineare", "Come vedremo", "In questo contesto")
- Una frase, un concetto

---

## Componenti PDF disponibili

| Componente | Uso |
|---|---|
| `<div class="key-point">` | Nota informativa, raccomandazione (sfondo blu chiaro) |
| `<div class="warning">` | Avviso, punto critico, rischio (sfondo arancione chiaro) |
| `<div class="price-box">` | Preventivo, riepilogo costi |
| `<div class="team-grid">` | Griglia card team |
| `<span class="section-label">` | Etichetta sezione + page break |
| `<span class="badge">` | Badge blu (data, versione) |
| `<span class="badge-orange">` | Badge arancione (uso interno, warning) |
| `<span class="badge-green">` | Badge verde (approvato, completato) |
| `<table>` | Tabelle con header, righe alternate |
| `<pre>` | Flow diagram ASCII, blocchi codice |
| `<code>` | Codice inline nel testo |

Esempi HTML pronti in `references/components.md`.

---

## Regola d'oro sul CSS

**Non aggiungere mai CSS ai documenti.** Tutto lo stile è in `references/template.html`. Se un componente non esiste, aggiungilo al template e aggiorna `components.md`.

---

## Struttura del repo

```
noiza-docs/
├── SKILL.md                  <- istruzioni per Claude
├── README.md                 <- questo file
└── references/
    ├── template.html         <- CSS Noiza completo + struttura base
    └── components.md         <- catalogo componenti con snippet HTML
```

---

## Maintainer

This project is maintained by [Martino Stenta](https://github.com/martinostenta).
