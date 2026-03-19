---
tipo: guida-contribuzione
versione: 1.0
---

## Struttura del repo

```
noiza-pdf-skill/
├── SKILL.md                  ← istruzioni per Claude (il "cervello" della skill)
├── README.md                 ← guida utente
├── guide-utilizzo.md         ← questa guida, per chi usa la skill
├── guide-contribuzione.md    ← questa guida, per chi la migliora
└── references/
    ├── template.html         ← CSS Noiza completo (unica fonte di stile)
    └── components.md         ← catalogo componenti con snippet HTML
```

---

## Cosa NON modificare

- **`SKILL.md`** — solo chi conosce bene Claude Code e vuole cambiare il comportamento della skill
- **Il CSS in `template.html`** senza prima testare su un documento reale — una modifica CSS rompe tutti i documenti esistenti

---

## Aggiungere un nuovo componente

Un "componente" è una classe CSS riutilizzabile (es. `.warning`, `.price-box`). Per aggiungerlo:

**1. Aggiungi il CSS in `references/template.html`**

Trova la sezione `/* --- CALLOUT BOXES --- */` e aggiungi il nuovo stile seguendo lo stesso pattern:

```css
.nome-componente {
  background-color: #f0fdf4;
  border-left: 3px solid #16a34a;
  padding: 10px 14px;
  margin: 10pt 0;
}
.nome-componente p, .nome-componente li {
  font-size: 11pt;
  padding-top: 3pt;
  padding-bottom: 3pt;
}
```

**2. Aggiungi l'esempio in `references/components.md`**

Segui il formato degli altri componenti: titolo, descrizione breve, snippet HTML pronto all'uso.

**3. Aggiorna la tabella in `SKILL.md`**

Aggiungi una riga alla tabella "Componenti disponibili" con: nome, classe, quando usarlo.

**4. Testa**

Crea un documento di prova con il nuovo componente e verifica che il PDF sia corretto prima di fare commit.

---

## Modificare il workflow di generazione PDF

Il workflow (server HTTP + Playwright MCP) è definito in `SKILL.md`. Se cambi qualcosa:

<div class="warning">
  <p><strong>Attenzione:</strong> il workflow è lo stesso per tutti i colleghi. Una modifica sbagliata qui rompe la generazione PDF per chiunque usi la skill. Testa sempre su un documento reale prima di fare push.</p>
</div>

---

## Fare una PR

1. Crea un branch con un nome descrittivo: `add-success-box`, `fix-table-overflow`, ecc.
2. Fai le modifiche (CSS + components.md + SKILL.md se serve)
3. Testa generando almeno un PDF completo
4. Apri la PR su `main` con una riga di descrizione nel titolo

Commit message consigliato:
```
feat: aggiungi componente .success-box (verde)
fix: correggi overflow tabelle con testo lungo
```

---

## Colori di riferimento

| Nome | Hex | Uso |
|---|---|---|
| Blu primario | `#2563eb` | Bordi, header tabelle, badge, accent |
| Blu chiaro (key-point) | `#eff6ff` | Sfondo note informative |
| Arancione (warning) | `#ea580c` | Bordo avvisi |
| Arancione chiaro | `#fff7ed` | Sfondo avvisi |
| Verde (success) | `#16a34a` | Badge completato |
| Grigio testo | `#1e293b` | Testo principale |
| Grigio bordi | `#e2e8f0` | Separatori, bordi tabelle |
| Grigio sfondo | `#f8fafc` | Sfondo righe alternate, price-box |
