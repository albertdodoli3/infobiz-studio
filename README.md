# Infobusiness Sartoriale Studio

La piattaforma del team delivery di Infobusiness Sartoriale per creare, lanciare e misurare i funnel dei clienti, reparto per reparto.

## Principio guida

**Lo Studio non rifà quello che i tool esistenti fanno già bene.** È un livello sottile che:

1. **Organizza**: un unico posto dove ogni cliente, lancio e deliverable ha uno stato.
2. **Orchestra**: lancia le skill AI e i tool di reparto con il contesto giusto del cliente.
3. **Misura**: registra produzione, tempi, costi, consumi e performance.

Il "cervello" vive in Google Drive + skill Claude/GPT. Il codice serve solo per tenere insieme i pezzi.

## Architettura a 4 livelli

```
┌──────────────────────────────────────────────────────────────┐
│ 4. INTERFACCIA  Studio (hub) → InfoCopy, InfoAds, InfoEdit,  │
│                 InfoGraphic, InfoFunnel, InfoAuto, InfoClient │
├──────────────────────────────────────────────────────────────┤
│ 3. MISURA       Registro deliverable (1 riga = 1 output)      │
│                 → dashboard produzione/tempi/costi/perf       │
├──────────────────────────────────────────────────────────────┤
│ 2. INTELLIGENZA Skill Claude per reparto, GPT, tool AI        │
│                 verticali (Descript, Canva, Meta API…)        │
├──────────────────────────────────────────────────────────────┤
│ 1. DATI         Google Drive = media base + Brain cliente     │
│                 (struttura cartelle standard)                 │
└──────────────────────────────────────────────────────────────┘
```

### 1. Dati: Google Drive come media base

Struttura **identica per ogni cliente**: è questo che rende le skill riutilizzabili.

```
/CLIENTI/<nome-cliente>/
  00_BRAIN/            ← tutto ciò che rende l'AI "consapevole"
    profilo-cliente.md   (sintesi viva, aggiornata dall'AI)
    call/                (trascrizioni call, automatiche)
    chat/                (export WhatsApp/Slack/email)
    materiali/           (corsi, PDF, vecchi funnel, testimonianze)
  01_RICERCA/          ricerca di mercato, avatar
  02_LANCI/<AAAA-MM-nome-lancio>/
    brief.md
    copy/  ads/  video/  grafiche/  funnel/  automazioni/
  03_PERFORMANCE/      export ads, KPI funnel
```

Più un `/SISTEMA/` condiviso con: database dei blocchi vincenti, template, brand guidelines, prompt e versioni delle skill.

### 2. Intelligenza: un motore per reparto

| Reparto | Sub-studio | Motore (no/low-code) | Stato |
|---|---|---|---|
| Copywriting | **InfoCopy Studio** | Skill Claude già esistenti (`costruttore-brief`, `pagine-funnel`, `script-ads`) + connettore Drive | ✅ già avviato |
| Client Mgmt | **InfoClient Studio** | "Brain": Claude Project per cliente sulla cartella `00_BRAIN` + un unico notetaker per Zoom e Meet che salva le trascrizioni su Drive + Slack collegato a Claude | da costruire, **priorità alta** |
| Grafica | **InfoGraphic Studio** | Canva (brand kit + brand template per cliente, compilati dall'AI) + GPT per le immagini | connettore Canva già disponibile |
| Media Buying | **InfoAds Studio** | Meta. Fase 1: dati Meta Ads → Sheet automatico + analisi AI settimanale. Fase 2: lancio campagne via Meta Marketing API. Google Ads dopo | da costruire |
| Editing | **InfoEdit Studio** | CapCut per il montaggio finale (non ha API). L'AI prepara il piano di montaggio e i tagli; per le ads brevi va testato un tool con editing da prompt (Descript o Submagic) | da testare |
| Funnel Building | **InfoFunnel Studio** | GHL: snapshot standard per tipo di funnel + copy nel formato dei blocchi. Systeme.io: template clonabili e copy pronto da incollare (le API non coprono le pagine) | da costruire |
| Automazioni | **InfoAuto Studio** | Workflow GHL dentro le snapshot; n8n o Make per collegare i tool tra loro | da costruire |

## Stack del team (decisioni)

**Team: 4 persone.** 2 funnel builder/automazioni, 1 copy + media buyer, 1 editor.
Il collo di bottiglia è la persona che segue sia copy sia ads: copy e ads si automatizzano per primi.

### Account Claude
Un account personale usato da più persone viola i termini d'uso, e mette i dati dei clienti tutti sotto un solo login. Va migrato al piano **Claude Team**, che offre:
- Project condivisi: il Brain di ogni cliente diventa accessibile a tutto il team;
- skill condivise nell'organizzazione: tutti usano la stessa versione del metodo;
- dati non usati per l'addestramento, gestione dei membri e delle revoche.

### Funnel: GHL + Systeme.io
- **GHL** è il motore della standardizzazione. Ci vuole una **snapshot master** per ogni tipo di funnel (lancio live, evergreen VSL), con pagine, workflow, pipeline, calendari ed email già impostati. Un nuovo cliente parte con: snapshot caricata → copy dalla skill → personalizzazione. GHL ha API e un server MCP, quindi in futuro lo Studio potrà creare contatti, tag e workflow in automatico.
- **Systeme.io** ha API limitate (contatti, tag, corsi, niente pagine). Si lavora con template clonabili e la skill `pagine-funnel` consegna il copy già diviso per sezione.
- Le due skill copy devono produrre output **nello stesso ordine dei blocchi del template**, così il funnel builder copia e incolla senza riadattare.

### Media Buying: Meta (poi Google Ads)
1. Report automatico: Meta Ads → Google Sheet ogni giorno, via Make/n8n o connettore dedicato.
2. Skill `analisi-ads`: legge lo Sheet e il brief e propone cosa spegnere, scalare e testare. Le creatività che funzionano finiscono nel database dei blocchi vincenti.
3. Più avanti: creazione delle campagne da template via Meta Marketing API.

### Editing: CapCut
CapCut non si collega via API, quindi l'AI lavora **prima** dell'editor:
- dalla trascrizione del girato, l'AI produce il **piano di montaggio**: tagli con timecode, hook, testi a schermo, b-roll e musica;
- l'editor esegue su CapCut, con i sottotitoli automatici e i template di progetto per cliente;
- test di 2 settimane di Descript o Submagic per le ads brevi (editing da prompt/trascrizione); si tiene solo se accorcia davvero i tempi.

### Grafica: Canva + GPT
- Un **brand kit Canva per cliente** (colori, font, logo), creato in onboarding.
- **Brand template** per i formati ricorrenti: statiche ads, copertine, slide webinar, mockup lead magnet. L'AI li compila con i testi della skill `script-ads` (connettore Canva).
- GPT per immagini e visual da inserire nei template.

### Client Brain: Zoom, Meet, Slack, WhatsApp
- **Call**: un solo notetaker che funziona sia su Zoom sia su Meet (es. Fathom, Fireflies, tl;dv) e salva le trascrizioni in `00_BRAIN/call/` del cliente.
- **Slack**: un canale per cliente (Slack Connect). Claude legge Slack con il connettore ufficiale. Diventa il canale operativo preferito.
- **WhatsApp**: niente API per gli account personali. Si fa un export settimanale della chat in `00_BRAIN/chat/`, e piano piano si spostano le comunicazioni operative su Slack.
- Una skill `aggiorna-brain` legge le novità e aggiorna `profilo-cliente.md`: decisioni, obiezioni, feedback, preferenze di tono.

Regola: **ogni skill legge dal Brain del cliente e scrive nella cartella del lancio.** Così l'output di una diventa input della successiva (brief → copy → script ads → video/grafiche → funnel).

### 3. Misura: il registro deliverable

Il cuore dei dati di delivery è una tabella (Google Sheet o Airtable all'inizio):

| campo | esempio |
|---|---|
| data | 2026-10-13 |
| cliente / lancio | Rossi / Lancio Nov |
| reparto | Copy |
| deliverable | Pagina opt-in |
| operatore | Marco |
| strumento | skill `pagine-funnel` |
| tempo (min) | 25 |
| costo (€) | token API + quota abbonamenti |
| revisioni | 1 |
| stato | approvato |
| performance | CR opt-in 38% (compilato dopo il lancio) |

Ogni skill, a fine lavoro, scrive una riga. Da qui nascono la dashboard dello Studio (tutti i reparti) e quella di ogni sub-studio (filtro per reparto). Prima versione: Looker Studio collegato allo Sheet, zero codice.

### 4. Interfaccia: lo Studio

L'hub web con login del team, che mostra:
- **Clienti e lanci** con stato per reparto (pipeline: brief → copy → creatività → funnel → live).
- **Pulsanti "esegui"** che lanciano la skill giusta con il contesto del cliente già caricato.
- **Dashboard** produzione / consumo / costi / tempi / performance.
- **Ingresso ai sub-studio brandizzati**, ognuno con la stessa struttura ma filtrato per reparto.

Si costruisce **solo dopo** aver validato le fasi 1–2, come app leggera (Next.js + Supabase, sviluppata con Claude Code) che chiama le stesse skill via Claude API.

## Roadmap

| Fase | Obiettivo | Codice | Durata indicativa |
|---|---|---|---|
| **0. Fondamenta** | Struttura Drive standard per tutti i clienti, registro deliverable, elenco tool per reparto | nessuno | 1 settimana |
| **1. Brain + Copy** | Brain cliente (Project + trascrizioni automatiche), skill copy che leggono il Brain e scrivono nel registro | nessuno | 2–3 settimane |
| **2. Reparti** | Grafica (Canva), Ads (report Meta automatici), Editing (tool scelto), automazioni n8n/Make | poco (workflow) | 4–6 settimane |
| **3. Studio** | Web app hub + sub-studio brandizzati + dashboard | sì, leggero | 4–8 settimane |
| **4. Esecuzione** | Lancio campagne e build funnel direttamente dallo Studio via API | sì | continuo |

## Prossimi passi (Fase 0)

1. Passare a Claude Team e spostare skill e Project nell'organizzazione.
2. Creare su Drive la struttura `/CLIENTI` e `/SISTEMA` e migrare i clienti attivi.
3. Creare il registro deliverable (Google Sheet) e aggiungere a ogni skill lo step finale "registra".
4. Scegliere il notetaker e collegarlo a Drive.
5. Costruire la snapshot GHL master del funnel più frequente.
