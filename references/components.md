# Componenti Noiza PDF — Catalogo

Tutti i componenti sono già inclusi nel CSS di `template.html`. Non aggiungere mai CSS inline o classi extra nei documenti.

---

## Badge (inline, nell'header subtitle)

```html
<span class="badge">Testo</span>           <!-- blu — data, versione, categoria -->
<span class="badge-orange">Testo</span>    <!-- arancione — warning, "non condividere" -->
<span class="badge-green">Testo</span>     <!-- verde — approvato, completato -->
```

---

## Callout box

### key-point — nota informativa (blu, angoli arrotondati)
```html
<div class="key-point">
  <p><strong>Titolo opzionale:</strong> testo della nota.</p>
</div>
```

### warning — attenzione / rischio (arancione)
```html
<div class="warning">
  <p><strong>Da verificare:</strong> testo dell'avviso.</p>
</div>
```

Con lista dentro:
```html
<div class="warning">
  <p><strong>Attenzione:</strong></p>
  <ul>
    <li>Primo punto</li>
    <li>Secondo punto</li>
  </ul>
</div>
```

---

## Price box — riquadro preventivo / riepilogo costi

Box compatto (hug content, non tutta la larghezza) con angoli arrotondati. Da usare per singoli importi evidenziati.

```html
<div class="price-box">
  <p><strong>Costo:</strong> 800 &euro; per lingua (IVA esclusa)</p>
</div>
```

---

## Team / card grid — riga di card grige

Per il team di progetto o liste di profili: una riga di card grige (riusa `.price-box`), allineate a sinistra con lo spazio libero a destra, distanza fra le card pari al border-radius (12px), senza righe divisorie. Niente immagini.

```html
<div class="team-grid">
  <div class="price-box"><p><strong>Nome Cognome</strong><br>Ruolo<br>email@noiza.com</p></div>
  <div class="price-box"><p><strong>Nome Cognome</strong><br>Ruolo<br>email@noiza.com</p></div>
  <div class="price-box"><p><strong>Nome Cognome</strong><br>Ruolo<br>email@noiza.com</p></div>
  <div class="price-box"><p><strong>Nome Cognome</strong><br>Ruolo<br>email@noiza.com</p></div>
</div>
```

Il testo nelle card del team è 7.5pt con `white-space: nowrap`, così quattro card stanno su una riga sola. Per molte card (oltre 4-5) valutare un font ancora più piccolo o lasciar andare a capo. Preferire `.team-grid` alla vecchia tabella 2 colonne, che mostrava bordi di cella deboli.

---

## Tabella standard

```html
<table>
  <tr><th>Colonna 1</th><th>Colonna 2</th><th>Colonna 3</th></tr>
  <tr><td>Valore</td><td>Valore</td><td>Valore</td></tr>
</table>
```

Header in slate chiaro, niente zebratura, angoli arrotondati in alto.

---

## Tabella voci (titolo + descrizione in cella unica)

Per liste di voci di intervento, listini, preventivi. Preferire questa forma a 3 colonne (titolo | descrizione | prezzo) quando titolo e descrizione sono entrambi testuali — evita righe troppo alte con descrizioni che vanno a capo molte volte.

```html
<table>
  <tr><th>Voce</th><th class="num">Prezzo</th></tr>
  <tr>
    <td><strong>01. Titolo voce</strong><br>Descrizione paragrafata della voce, anche lunga su più righe.</td>
    <td class="num">2.500 &euro;</td>
  </tr>
</table>
```

---

## Tabella spec — convenzione "colonna valore"

Per tabelle di specifiche o "cosa è incluso": i numeri vanno **fuori** dall'etichetta e dentro la colonna valore a destra. Es. "Page templates | 8" invece di "8 page templates | ✓". La colonna valore porta un numero se esiste, altrimenti `&check;`, e può contenere anche valori testuali ("4 filters", "2 hours", "included").

```html
<table>
  <tr><th>Item</th><th class="num">Included</th></tr>
  <tr><td>Page templates</td><td class="num">8</td></tr>
  <tr><td>Custom Post Types</td><td class="num">2</td></tr>
  <tr><td>Faceted archive</td><td class="num">4 filters</td></tr>
  <tr><td>GA4 and GTM setup</td><td class="num">&check;</td></tr>
  <tr><td><strong>Total</strong></td><td class="num"><strong>6.500 &euro;</strong></td></tr>
</table>
```

---

## Tabella comparativa — confronto pacchetti/tier

Righe-feature × N colonne-opzione, con `&check;` nelle celle e la riga prezzo in fondo. Per confronti orizzontali tipo 10/20/40/80h o trimestrale/mensile/settimanale.

```html
<table>
  <tr><th>Feature</th><th class="num">10 h</th><th class="num">20 h</th><th class="num">40 h</th><th class="num">80 h</th></tr>
  <tr><td>Interventi evolutivi</td><td class="num">&check;</td><td class="num">&check;</td><td class="num">&check;</td><td class="num">&check;</td></tr>
  <tr><td>Gestione a scalare</td><td class="num">&check;</td><td class="num">&check;</td><td class="num">&check;</td><td class="num">&check;</td></tr>
  <tr><td><strong>Prezzo</strong></td><td class="num"><strong>1.000 &euro;</strong></td><td class="num"><strong>1.800 &euro;</strong></td><td class="num"><strong>3.200 &euro;</strong></td><td class="num"><strong>6.000 &euro;</strong></td></tr>
</table>
```

La tabella è una tabella normale: niente wrapper grigio attorno (il `.price-box` è solo per singoli importi, non per avvolgere tabelle).

---

## Liste "label + descrizione"

Niente trattino fra label e descrizione: label in `<strong>`, poi `<br>`, poi la descrizione a capo.

```html
<ol>
  <li><strong>Homepage</strong><br>layout fully custom, disegnato singolarmente</li>
  <li><strong>Contact page</strong></li>
</ol>
```

---

## Colonna numerica — classe `.num`

Applicare a `<th>` e `<td>` di qualsiasi colonna di prezzi o numeri. Si usa anche per la colonna valore con contenuti testuali brevi ("4 filters", "2 hours", "included"). Effetti combinati:
- allineamento a destra
- cifre tabulari (larghezza fissa)
- `white-space: nowrap` per evitare a-capo tra numero e simbolo `€`

```html
<tr>
  <th>Voce</th>
  <th class="num">Prezzo</th>
</tr>
<tr>
  <td>Sviluppo modulo X</td>
  <td class="num">1.200 &euro;</td>
</tr>
```

Tutto il documento usa già `font-variant-numeric: tabular-nums` a livello `body`, quindi anche i numeri nei titoli e nei paragrafi sono a larghezza fissa.

---

## Page break — forzare nuova pagina

Per spezzare il documento in pagine logiche (es. appendici, sintesi finali, sezioni standalone). Applicare a un `<h2>`.

```html
<h2 class="page-break">Titolo sezione su nuova pagina</h2>
```

Preferire questa classe alla `.section-label` quando non serve l'etichetta uppercase sopra il titolo.

---

## Section label — etichetta di sezione con page break

Da usare quando si vuole un'etichetta uppercase sopra il titolo (es. "Appendice", "Proposta A"). Include `page-break-before: always`.

```html
<span class="section-label">Appendice A</span>
<h2>Titolo della sezione</h2>
```

Se l'etichetta sarebbe semplicemente una ripetizione del titolo, usare `.page-break` sull'`<h2>` invece.

---

## Blocco codice / flow diagram

Max ~80 caratteri per riga per evitare overflow nel PDF.

```html
<pre>
[Step 1] → [Step 2] → [Step 3]
               |
               v
           [Step 4]
</pre>
```

---

## Codice inline

```html
Il hook <code>gform_post_process_16</code> gestisce la transizione.
```

---

## Separatore sezione

```html
<hr>
```

---

## Struttura header completa

```html
<p class="logo"><svg xmlns="http://www.w3.org/2000/svg" viewBox="..." style="height:32px;width:auto">...</svg></p>
<hr>
<p class="subtitle">Cliente &middot; <span class="badge">Mese Anno</span></p>
<h1>Titolo documento</h1>
<hr>
<p><strong>Preparato da:</strong> Noiza (Web Development Area)<br>
<strong>Per:</strong> Cliente<br>
<strong>Scopo:</strong> Descrizione breve</p>
<hr>
```

Il logo è il logo ufficiale Noiza (file in `references/logo-noiza_blu.svg`, già embeddato inline nel template).

---

## Footer

```html
<hr>
<p class="doc-footer">Preparato da Noiza, Web Development Area<br>
Mese Anno</p>
```

Niente italic, niente em dash. Virgola come separatore tra "Noiza" e "Web Development Area". Il footer ha font ridotto (9pt), grigio chiaro, allineato al centro. Anteporre un `<hr>` come stacco visivo.
