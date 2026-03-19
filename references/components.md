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

### key-point — nota informativa (blu)
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

```html
<div class="price-box">
  <p><strong>Costo:</strong> 800 &euro; per lingua (IVA esclusa)</p>
  <p>Note aggiuntive se necessario.</p>
</div>
```

---

## Section label — etichetta di sezione con page break

Usare prima di un `<h2>` quando si vuole forzare una nuova pagina e aggiungere un'etichetta sopra il titolo (es. "Proposta S", "Appendice").

```html
<span class="section-label">Proposta S</span>
<h2>Titolo della sezione</h2>
```

---

## Tabella

```html
<table>
  <tr><th>Colonna 1</th><th>Colonna 2</th><th>Colonna 3</th></tr>
  <tr><td>Valore</td><td>Valore</td><td>Valore</td></tr>
</table>
```

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
<p class="logo"><img src="data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iODAiIGhlaWdodD0iMjQiIHZpZXdCb3g9IjAgMCA4MCAyNCIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48dGV4dCB4PSIwIiB5PSIyMCIgZm9udC1mYW1pbHk9Ik1vbnRzZXJyYXQsIHNhbnMtc2VyaWYiIGZvbnQtd2VpZ2h0PSI4MDAiIGZvbnQtc2l6ZT0iMjQiIGZpbGw9IiMyNTYzZWIiPk5vaXphPC90ZXh0Pjwvc3ZnPg==" alt="Noiza" style="height: 24px;"></p>
<hr>
<p class="subtitle">Cliente &middot; <span class="badge">Mese Anno</span></p>
<h1>Titolo documento</h1>
<hr>
<p><strong>Preparato da:</strong> Noiza (Web Development Area)<br>
<strong>Per:</strong> Cliente<br>
<strong>Scopo:</strong> Descrizione breve</p>
<hr>
```

Footer:
```html
<hr>
<p><em>Preparato da Noiza — Web Development Area</em><br>
<em>Mese Anno</em></p>
```
