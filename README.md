# 🇮🇹 Italy Travel Agile Map

> Database agile e versionato per percorsi di viaggio in Italia con validazione multi-fonte

## 🎯 Obiettivo

Creare un database affidabile e versionato dei percorsi di viaggio in Italia, utilizzando un **metodo agile iterativo** che:

1. **Raccoglie dati da fonti multiple** (10-15+ per percorso)
2. **Valida per confronto incrociato** per eliminare dati errati
3. **Versiona ogni aggiornamento** con Git per tracciabilità completa
4. **Permette rollback rapidi** se nuovi dati si rivelano incorretti
5. **Mantiene storico verificabile** di tutte le modifiche

## 📊 Metodo di Validazione

### Processo di Raccolta Dati

```
┌──────────────────┐
│  Query Percorso  │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│  Ricerca Multi-  │
│  Fonte (15+)     │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│  Confronto e     │
│  Validazione     │
└────────┬─────────┘
         │
         ├─────► Dati Consistenti → Database ✓
         │
         └─────► Dati Conflittuali → Flag per Review ⚠️
```

### Livelli di Confidenza

| Livello | Fonti | Descrizione |
|---------|-------|-------------|
| 🟢 **Very High** | 5+ | Conferma ufficiale + multiple fonti consistenti |
| 🟡 **High** | 3-4 | Multiple fonti consistenti |
| 🟠 **Medium** | 2 | Due fonti o piccole discrepanze |
| 🔴 **Low** | 1 | Fonte singola o dati conflittuali |

## 🗺️ Percorsi Disponibili

### 1. Marsala → Giffoni Sei Casali

**Versione:** 1.0.0 | **Ultimo aggiornamento:** 2025-11-29 | **Affidabilità:** 92%

- **Destinazione:** Studio Dentistico Dott. Sante Vassallo, Via Serroni 115
- **Durata totale:** 15-18 ore
- **Segmenti:** 5 (treno + traghetto + bus)
- **Fonti validate:** 15+
- **Dettaglio:** [routes/marsala-giffoni/](routes/marsala-giffoni/)

#### Segmenti del percorso:

1. **Marsala → Trapani** (Treno Regionale)
   - 13 treni/giorno, 28-37 min
   - [Dettaglio fermate](routes/marsala-giffoni/segment-1-marsala-trapani.json)

2. **Palermo → Napoli/Salerno** (Intercity Notte 1958)
   - Partenza 19:25, arrivo 05:17/06:22
   - Include traghetto Stretto di Messina
   - [Dettaglio fermate](routes/marsala-giffoni/segment-3-palermo-napoli-ic-notte.json)

3. **Napoli → Salerno** (Regionale)
   - 200+ treni/giorno, 34-90 min
   - [Dettaglio fermate](routes/marsala-giffoni/segment-4-napoli-salerno.json)

4. **Salerno → Giffoni Sei Casali** (Bus SITA 5170)
   - 6-8 bus/giorno, 30-45 min
   - ⚠️ Orari limitati - verificare prima
   - [Dettaglio fermate](routes/marsala-giffoni/segment-5-salerno-giffoni.json)

## 📁 Struttura Database

```
italy-travel-agile-map/
│
├── README.md                          # Questo file
├── CONTRIBUTING.md                    # Guida per contribuire
│
└── routes/
    └── [route-name]/
        ├── route-metadata.json        # Info generali percorso
        ├── segment-1-[name].json      # Dettagli segmento 1
        ├── segment-2-[name].json      # Dettagli segmento 2
        ├── segment-N-[name].json      # Dettagli segmento N
        ├── CHANGELOG.md               # Storico modifiche
        └── validation-sources.md      # Elenco fonti validate
```

## 🔄 Workflow Agile

### Aggiornamento Mensile

1. **Ricerca nuovi dati** (query multi-fonte)
2. **Confronto con versione corrente**
3. **Identificazione modifiche**:
   - ✅ Modifiche consistenti → Update database
   - ❌ Modifiche conflittuali → Flag per indagine
   - 🔍 Dati sospetti → Mantieni versione precedente
4. **Commit con messaggio dettagliato**
5. **Tag versione** (semantic versioning)

### Gestione Errori

```bash
# Se nuovi dati si rivelano errati
git revert [commit-hash]

# Ripristino versione precedente
git checkout v1.0.0 -- routes/marsala-giffoni/

# Confronto versioni
git diff v1.0.0 v1.1.0 -- routes/marsala-giffoni/
```

## 🛠️ Come Usare

### Per AI/Chatbot

```python
import json

# Carica metadati percorso
with open('routes/marsala-giffoni/route-metadata.json') as f:
    route = json.load(f)

print(f"Percorso: {route['route_name']}")
print(f"Affidabilità: {route['reliability_score']*100}%")
print(f"Durata: {route['total_duration_hours']} ore")

# Carica singolo segmento
with open('routes/marsala-giffoni/segment-1-marsala-trapani.json') as f:
    segment = json.load(f)
    
for stop in segment['stops']:
    print(f"{stop['order']}. {stop['station']}")
```

### Per Utenti

1. Naviga la cartella `routes/[nome-percorso]/`
2. Consulta `route-metadata.json` per panoramica
3. Leggi i file segment per dettagli specifici
4. Verifica `CHANGELOG.md` per aggiornamenti recenti

## 📅 Frequenza Aggiornamenti

- **Mensile:** Verifica routine di tutti i percorsi
- **On-demand:** Se segnalate modifiche agli orari
- **Stagionale:** Prima di periodi ad alta variabilità (estate/inverno)

## 🤝 Come Contribuire

### Segnalare Errori

1. Apri una Issue su GitHub
2. Specifica:
   - Percorso interessato
   - Segmento con dati errati
   - Fonte corretta (link)
   - Data della verifica

### Aggiungere Nuovi Percorsi

1. Fork del repository
2. Crea nuova cartella in `routes/[nuovo-percorso]/`
3. Segui la struttura esistente
4. Valida con almeno 5 fonti
5. Pull request con descrizione dettagliata

## 📊 Statistiche Database

- **Percorsi attivi:** 1
- **Fonti validate:** 15+
- **Segmenti mappati:** 5
- **Fermate totali:** 60+
- **Ultimo aggiornamento:** 2025-11-29

## 📞 Contatti

**Maintainer:** [@XxShinjiroxX](https://github.com/XxShinjiroxX)

## 📄 Licenza

MIT License - Dati liberi per uso personale e commerciale.

## 🙏 Credits

Dati aggregati da:
- Trenitalia
- TheTrainLine
- Omio.it
- Rome2Rio
- SITA Sud Trasporti
- Moovit
- E656.net
- E molti altri...

---

**⚡ Database agile per viaggi sicuri e verificati**

*Ultima build: v1.0.0 | 2025-11-29*
