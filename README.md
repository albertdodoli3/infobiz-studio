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

| Reparto | Sub-studio | Motore consigliato (no/low-code) | Stato |
|---|---|---|---|
| Copywriting | **InfoCopy Studio** | Skill Claude già esistenti (`costruttore-brief`, `pagine-funnel`, `script-ads`) + connettore Drive | ✅ già avviato |
| Client Mgmt | **InfoClient Studio** | "Brain": Claude Project per cliente sulla cartella `00_BRAIN` + trascrizioni call automatiche (Wispr Flow / Fathom / Fireflies → Drive) | da costruire, **priorità alta** |
| Grafica | **InfoGraphic Studio** | Canva (brand kit per cliente + brand template compilati via AI) | connettore Canva già disponibile |
| Media Buying | **InfoAds Studio** | Fase 1: lettura dati Meta Ads → registro + analisi AI. Fase 2: creazione/lancio campagne via Meta Marketing API | da costruire |
| Editing | **InfoEdit Studio** | Descript (editing da testo/prompt), OpusClip/Submagic (clip e sottotitoli), HeyGen (avatar). Lo Studio passa script e asset, il tool edita | da scegliere |
| Funnel Building | **InfoFunnel Studio** | Dipende dalla piattaforma (GoHighLevel, Systeme, ClickFunnels…): copy dalla skill → template della piattaforma | da definire |
| Automazioni | **InfoAuto Studio** | n8n o Make come "colla" tra tutti i tool; workflow standard per tipo di funnel | da costruire |

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

## Domande aperte

1. Piattaforma funnel usata (GHL, Systeme, ClickFunnels, altro)?
2. Piattaforme ads oltre Meta (Google, TikTok, YouTube)?
3. Quante persone nel team delivery e chi per reparto?
4. Piano Claude in uso (Team/Enterprise permettono skill e Project condivisi col team)?
5. Dove avvengono oggi le call con i clienti (Zoom, Meet) e le chat (WhatsApp, Slack)?
6. Tool già pagati per editing e grafica?
