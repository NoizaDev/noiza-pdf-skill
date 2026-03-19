---
tipo: guida-utilizzo
versione: 1.2
---

## Come funziona

Scrivi a Claude quello che vuoi. Claude crea il documento, lo converte in HTML con il template Noiza e genera il PDF. Non devi toccare nessun file a mano.

---

## Setup iniziale

Fallo una volta sola.

### 1. Claude Code
Installa l'estensione **Claude Code** in VS Code o Antrigravity. Serve un account Anthropic con accesso Claude Code.

### 2. Playwright MCP
Apri le impostazioni di Claude Code → sezione **MCP servers** → abilita **Playwright**. Serve per la generazione PDF.

### 3. Google Drive Desktop
Installa **Google Drive per Desktop** e accedi con il tuo account Noiza. La cartella Drive viene montata in locale e Claude può leggerla e scriverci come una cartella normale sul tuo computer.

### 4. La skill
Apri Claude Code in qualsiasi progetto e chiedi:

> "Installa la skill noiza-pdf-skill clonando https://github.com/NoizaDev/noiza-pdf-skill in ~/.claude/skills/"

Claude clona il repo. La skill è subito disponibile in tutte le sessioni.

---

## Generare un documento

Apri il progetto in Claude Code e descrivi quello che ti serve:

> "Crea un documento tecnico Noiza: proposta per [cliente] su [argomento]. Includi: introduzione, architettura proposta, costi e prossimi passi."

oppure, se hai già un file `.md` con il contenuto:

> "Genera il PDF da `nome-file.md` in stile Noiza"

Claude genera l'HTML con il template Noiza e produce il PDF.

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

> "Mostrami i componenti disponibili nella skill noiza-pdf"

---

## Regola d'oro

<div class="warning">
  <p><strong>Non modificare mai il CSS nei documenti generati.</strong> Se un componente grafico non esiste, segnalalo a chi gestisce la skill. Aggiungere stili inline rompe la coerenza visiva di tutti i documenti.</p>
</div>
