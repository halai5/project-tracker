# Sea2Sustain Tracker

Applicazione web per il monitoraggio di budget, spese, output e attività formative del progetto **Sea2Sustain: Learning sustainable tourism through practice** (Erasmus+ KA210-ADU, periodo 01/11/2026 – 31/10/2027).

L'app è costruita su [Base44](https://base44.com) e serve ai partner del progetto per seguire in un unico posto l'avanzamento dei fondi a lump sum.

> **Nota sulla riservatezza.** L'app gestisce importi di progetto, documenti e dati dei partner. Tieni il repository **privato**.

## Cosa fa

| Sezione | Contenuto |
|---|---|
| **Dashboard** | Budget totale, speso, impegnato e residuo; monitoraggio fondi per partner (budget preventivo, bonificato, speso); grafici budget vs speso per attività e spesa per categoria; giornate/uomo preventivate vs effettive. |
| **Attività** | Le 4 macro-attività del progetto con date, capofila, stato, avanzamento del budget e sottovoci di budget previste dal Grant Agreement. |
| **Movimenti di spesa** | Registro delle voci di spesa con filtri per attività, categoria, stato e partner (Preventivato / Pagato / Da pagare). |
| **Output & Deliverable** | Controllo degli output previsti: nei progetti a lump sum il saldo finale dipende dal loro raggiungimento, non dalla spesa. |
| **Corso & Presenze** | Iscritti, sessioni e presenze per classe (italiana e spagnola), con stampa del registro firme e del registro presenze. |
| **Documenti** | Archivio di Grant Agreement, Accession Form, giustificativi di spesa e report interni, con anteprima e download. |
| **Partner** | Informazioni di progetto, referenti, riferimenti dell'Agenzia Nazionale e indicazioni sull'identità visiva EU. |

L'interfaccia è disponibile in **italiano e spagnolo**. Sono previsti login, registrazione, recupero password e autenticazione a più fattori (MFA). L'accesso ai dati è regolato per ruolo (coordinatore/admin) e per partner di appartenenza.

## Screenshot

| | |
|---|---|
| ![Dashboard](screenshots/01-dashboard.png) | ![Grafici](screenshots/02-dashboard-grafici.png) |
| ![Attività](screenshots/03-attivita.png) | ![Movimenti di spesa](screenshots/04-movimenti-di-spesa.png) |
| ![Output & Deliverable](screenshots/05-output-deliverable.png) | ![Corso & Presenze](screenshots/06-corso-presenze.png) |
| ![Documenti](screenshots/07-documenti.png) | |

Gli screenshot sono stati ripresi dall'anteprima dell'app. Nomi di persone e aziende e i codici identificativi sono stati oscurati; la pagina Partner non è inclusa perché mostra contatti personali.

## Struttura del progetto

```
base44/
  config.jsonc          configurazione del progetto Base44 (build, serve)
  entities/             schema dei dati (vedi sotto)
src/
  api/                  client Base44
  components/           componenti dell'app (corso/, ui/ ...)
  hooks/
  lib/                  budget, i18n, autenticazione, utilità
  pages/                Dashboard, Attivita, Movimenti, Outputs,
                        Corso, Documenti, Partners, Login, Register ...
  App.jsx               routing
```

In questo pacchetto trovi gli schemi dei dati in `base44/entities/`. Il codice sorgente completo dell'app (`src/` e file di configurazione) si ottiene da Base44 e si aggiunge al repository, come spiegato più sotto.

## Modello dati

| Entità | Scopo |
|---|---|
| `Attivita` | Macro-attività: nome, date, capofila, budget allocato, stato, giornate preventivate per partner. |
| `VoceSpesa` | Movimento di spesa: attività, partner, categoria, importo, data, giornate, persona, modalità di pagamento, documento, stato. |
| `Partner` | Organizzazione partecipante: paese, ruolo, referente, dati per bonifici, importo bonificato. |
| `Output` | Output/deliverable per attività: previsto, raggiunto, data di raggiungimento. |
| `Documento` | Documento di progetto: tipo, file, data, partner, note. |
| `Iscritto` | Partecipante al corso: contatti, classe (italiana/spagnola), data di iscrizione. |
| `Sessione` | Sessione formativa: data, classe, argomento, docente, luogo, orari, presenze. |
| `MobilitaPartecipante` | Mobilità transnazionale: voli, hotel, costi, ruolo (staff/corsista). |
| `User` | Estensione utente con il partner di appartenenza. |

Le regole di accesso (RLS) sono definite negli stessi file: le voci di spesa sono visibili e modificabili dal partner di appartenenza e dagli admin; i documenti anche da chi li ha caricati; le altre entità sono scrivibili solo dagli admin.

## Tecnologie

React 18 · Vite · Tailwind CSS · shadcn/ui (Radix UI) · TanStack Query · React Router · Recharts · jsPDF / html2canvas · Base44 SDK.

## Come ottenere il codice completo

Il codice vive in Base44. Due modi per portarlo in questo repository:

1. **Export manuale.** Nell'editor Base44, menu «…» in alto a destra → **Export project as ZIP**. Scompatta lo ZIP in questa cartella (mantieni `README.md` e `screenshots/`) ed esegui il commit.
2. **Sincronizzazione GitHub.** Con il piano Base44 che la include (Builder), menu «…» → **GitHub connection**: Base44 crea e sincronizza il repository, e le modifiche fatte su GitHub si riflettono nell'editor.

## Esecuzione in locale

Dopo aver aggiunto il codice completo:

```bash
npm install
npm install -g base44@latest   # CLI Base44
base44 dev                     # backend locale + frontend
```

Per lavorare solo sul frontend contro il backend ospitato, crea `.env.local`:

```bash
VITE_BASE44_APP_ID=<id_app>
VITE_BASE44_APP_BASE_URL=https://<nome-app>.base44.app
```

poi `npm run dev`. Non committare mai `.env.local`.

## Pubblicazione

Dopo il push su git, apri il pannello Base44 (`base44 dashboard open`) e pubblica l'app.

## Riferimenti

- Documentazione Base44: <https://docs.base44.com>
- App pubblicata: <https://s2smanagement.base44.app/>
