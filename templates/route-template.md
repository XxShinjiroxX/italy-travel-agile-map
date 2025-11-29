# Template per Nuovo Percorso

## Struttura Cartelle

```
routes/
└── [nome-percorso]/
    ├── route-metadata.json
    ├── segment-1-[nome].json
    ├── segment-2-[nome].json
    ├── segment-N-[nome].json
    ├── CHANGELOG.md
    └── validation-sources.md
```

## Template: route-metadata.json

```json
{
  "route_id": "[ORIGIN-DEST-###]",
  "route_name": "[Città Origine] → [Città Destinazione]",
  "version": "1.0.0",
  "last_updated": "YYYY-MM-DD",
  "validation_status": "verified",
  "sources_count": 0,
  "destination": {
    "name": "[Nome Destinazione]",
    "address": "[Indirizzo Completo]",
    "phone": ["+39 XXX XXX XXXX"],
    "coordinates": {
      "latitude": 0.0,
      "longitude": 0.0
    }
  },
  "total_duration_hours": "X-Y",
  "journey_segments": 0,
  "transport_modes": ["train", "bus", "ferry"],
  "complexity": "low|medium|high",
  "reliability_score": 0.0
}
```

## Template: segment-X.json

```json
{
  "segment_id": "SEG-00X",
  "segment_name": "[Origine] → [Destinazione]",
  "transport_type": "train|bus|ferry|taxi",
  "operator": "[Nome Operatore]",
  "validation": {
    "sources": [
      "fonte1.com",
      "fonte2.com",
      "fonte3.com"
    ],
    "confidence": "very_high|high|medium|low",
    "last_verified": "YYYY-MM-DD",
    "note": "[Note opzionali]"
  },
  "route_details": {
    "distance_km": 0,
    "duration_min": "X-Y",
    "frequency": "[Descrizione frequenza]",
    "trains_per_day": 0,
    "direct": true
  },
  "stops": [
    {
      "order": 1,
      "station": "[Nome Stazione]",
      "type": "departure|intermediate|arrival",
      "coordinates": {"lat": 0.0, "lon": 0.0}
    }
  ],
  "schedule": [
    {
      "departure": "HH:MM",
      "arrival": "HH:MM",
      "duration": "XXmin",
      "train_number": "[Codice Treno]"
    }
  ],
  "pricing": {
    "currency": "EUR",
    "price_from": 0.00,
    "price_avg": 0.00,
    "class": "2nd class"
  }
}
```

## Template: CHANGELOG.md

```markdown
# Changelog - Route [Nome] → [Destinazione]

## [1.0.0] - YYYY-MM-DD

### Added
- Initial database structure
- Complete route data
- X segments with detailed stops
- Schedule for [segmenti]
- GPS coordinates for key locations
- Pricing information

### Validated Sources
- Fonte 1
- Fonte 2
- Fonte 3

### Notes
- Reliability score: X.XX/1.00
- Total journey time: X-Y hours
- Confidence level: High/Medium/Low

---

*Last verified: YYYY-MM-DD*  
*Next review: YYYY-MM-DD*
```

## Template: validation-sources.md

```markdown
# Fonti di Validazione - [Percorso]

## Fonti Ufficiali

### Operatori Trasporto
- [ ] [Nome Operatore](link) - Verificato YYYY-MM-DD
- [ ] [Nome Operatore 2](link) - Verificato YYYY-MM-DD

## Aggregatori

### Multi-Modale
- [ ] Rome2Rio.com - Verificato YYYY-MM-DD
- [ ] Omio.it - Verificato YYYY-MM-DD

### Specifici Treno
- [ ] TheTrainline.com - Verificato YYYY-MM-DD
- [ ] Trenitalia.com - Verificato YYYY-MM-DD
- [ ] E656.net - Verificato YYYY-MM-DD

### Specifici Bus
- [ ] Moovit.com - Verificato YYYY-MM-DD
- [ ] [Operatore locale] - Verificato YYYY-MM-DD

## Validazione Incrociata

| Dato | Fonte 1 | Fonte 2 | Fonte 3 | Consistente |
|------|---------|---------|---------|-------------|
| Durata segmento 1 | X min | X min | X min | ✅ |
| Prezzo medio | €X | €Y | €X | ⚠️ |
| Numero fermate | X | X | X | ✅ |

## Note di Validazione

### Dati Consistenti
- [Elenco dati verificati su multiple fonti]

### Dati Conflittuali
- [Elenco discrepanze con risoluzione]

### Dati da Verificare
- [ ] [Dato 1] - Solo 1 fonte
- [ ] [Dato 2] - Fonti in conflitto

---

*Ultima validazione: YYYY-MM-DD*
```

## Checklist Creazione Percorso

### Fase 1: Ricerca
- [ ] Identificato percorso e destinazione
- [ ] Raccolte almeno 5 fonti per ogni segmento
- [ ] Verificata consistenza orari
- [ ] Confermati prezzi attuali
- [ ] Ottenute coordinate GPS destinazione

### Fase 2: Strutturazione
- [ ] Creata cartella `routes/[nome-percorso]/`
- [ ] Compilato `route-metadata.json`
- [ ] Creati file segment per ogni tratta
- [ ] Documentati tutti gli orari disponibili
- [ ] Aggiunte tutte le fermate intermedie

### Fase 3: Validazione
- [ ] Confrontate almeno 3 fonti per ogni dato
- [ ] Risolte discrepanze
- [ ] Assegnato livello di confidenza
- [ ] Calcolato reliability score
- [ ] Compilato `validation-sources.md`

### Fase 4: Documentazione
- [ ] Scritto CHANGELOG.md
- [ ] Aggiunte note e warning importanti
- [ ] Documentate alternative di viaggio
- [ ] Indicati punti critici (orari limitati, ecc.)

### Fase 5: Pubblicazione
- [ ] Commit con messaggio descrittivo
- [ ] Tag versione (v1.0.0)
- [ ] Aggiornato README.md principale
- [ ] Testato accesso via raw GitHub

## Linee Guida Naming

### Nomi File
- `route-metadata.json` (sempre uguale)
- `segment-1-[origine]-[destinazione].json`
- `segment-2-[origine]-[destinazione].json`

### Nomi Cartelle
- Tutto minuscolo
- Usa trattini `-` al posto di spazi
- Formato: `[città-origine]-[città-destinazione]`
- Esempio: `roma-firenze`, `milano-venezia`

### Route IDs
- Formato: `[ORIG]-[DEST]-###`
- Usa codici città a 4 lettere quando possibile
- Esempio: `ROMA-FIRE-001`, `MILA-VENE-001`

---

*Template version: 1.0*  
*Last updated: 2025-11-29*
