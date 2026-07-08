---
tipo: guida-utilizzo
versione: 1.3
---

## Come funziona

La skill lavora in due fasi.

**Fase 1, sempre attiva: scrittura.** Quando chiedi un documento Noiza, Claude applica le regole di scrittura (niente em dash, soggetti reali, numeri in tabella, struttura editoriale) e produce o rivede un file `.md`. Nessun HTML, nessun PDF, finché non lo chiedi.

**Fase 2, solo su richiesta: PDF.** Quando chiedi esplicitamente il PDF, Claude converte il `.md` in HTML con il template Noiza, incorpora il font e genera il file. Non devi toccare nessun file a mano.

Il `.md` resta sempre la sorgente di verità: l'HTML e il PDF si rigenerano da lì.

---

## Setup iniziale

Fallo una volta sola.

### 1. Claude Code
Installa l'estensione **Claude Code** in VS Code o Antrigravity. Serve un account Anthropic con accesso Claude Code.

### 2. Playwright MCP (consigliato)
Apri le impostazioni di Claude Code → sezione **MCP servers** → abilita **Playwright**. È la via preferita per la generazione PDF. Se non è disponibile, Claude usa un fallback via Chrome headless che richiede solo Google Chrome installato: la fase 2 funziona comunque.

### 3. Google Drive Desktop
Installa **Google Drive per Desktop** e accedi con il tuo account Noiza. La cartella Drive viene montata in locale e Claude può leggerla e scriverci come una cartella normale sul tuo computer.

### 4. La skill
Apri Claude Code in qualsiasi progetto e chiedi:

> "Installa la skill clonando https://github.com/NoizaDev/noiza-pdf-skill in ~/.claude/skills/noiza-docs"

Claude clona il repo nella cartella `noiza-docs`: il nome della cartella è quello con cui si invoca la skill (`/noiza-docs`), indipendente dal nome del repo. La skill è subito disponibile in tutte le sessioni.

---

## Scrivere un documento

Apri il progetto in Claude Code e descrivi quello che ti serve:

> "Crea un documento tecnico Noiza: proposta per [cliente] su [argomento]. Includi: introduzione, architettura proposta, costi e prossimi passi."

Claude produce il file `.md` applicando le regole Noiza. A questo punto hai il contenuto pronto, senza PDF.

## Generare il PDF

Quando ti serve il file da inviare, chiedilo esplicitamente:

> "Genera il PDF da `nome-file.md` in stile Noiza"

Claude converte il `.md` in HTML con il template Noiza e produce il PDF.

---

## Aggiornare un documento

Chiedi a Claude di modificare il contenuto:

> "Aggiorna la proposta per [cliente]: cambia i costi nella sezione 3 e aggiungi una nota sui tempi di consegna"

Claude aggiorna il `.md` sorgente, rigenera l'HTML e il PDF. **Non modificare mai l'HTML direttamente** — il `.md` è la sorgente di verità.

---

## Progetti condivisi su Google Drive

Se vuoi che i documenti vengano salvati su Drive (visibili a tutto il team), chiedi a Claude di configurare il progetto:

> "Questo progetto è condiviso, la cartella Drive è Clienti > NomeCliente"

Claude aggiorna il file di configurazione del progetto. Da quel momento, ogni volta che generi o aggiorni un documento, Claude lo salva automaticamente nella cartella Drive corrispondente.

Per verificare se un progetto è già configurato come condiviso:

> "Questo progetto è locale o condiviso su Drive?"

---

## Componenti disponibili

Puoi chiedere a Claude di usare componenti grafici specifici descrivendo cosa vuoi:

> "Aggiungi un box informativo che dice: [testo]"
> "Metti un avviso arancione per segnalare che [rischio]"
> "Inserisci un riquadro costi con [dettagli]"
> "Inizia una nuova sezione intitolata [titolo]"

Per vedere tutti i componenti disponibili con anteprima:

> "Mostrami i componenti disponibili nella skill noiza-docs"

---

## Regola d'oro

<div class="warning">
  <p><strong>Non modificare mai il CSS nei documenti generati.</strong> Se un componente grafico non esiste, segnalalo a chi gestisce la skill. Aggiungere stili inline rompe la coerenza visiva di tutti i documenti.</p>
</div>
