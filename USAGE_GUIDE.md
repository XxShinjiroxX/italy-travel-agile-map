# 📚 Guida all'Uso - Italy Travel Agile Map

## 🤖 Per Chatbot AI (Perplexity, ChatGPT, Claude, ecc.)

### Setup Iniziale

1. **Clona o referenzia il repository:**
```
https://github.com/XxShinjiroxX/italy-travel-agile-map
```

2. **Accedi ai dati via raw GitHub:**
```
https://raw.githubusercontent.com/XxShinjiroxX/italy-travel-agile-map/main/routes/[percorso]/[file].json
```

### Query di Esempio per AI

#### Esempio 1: Ottenere panoramica percorso

**Prompt utente:**
> "Come arrivo da Marsala a Giffoni Sei Casali?"

**AI dovrebbe:**
1. Leggere `routes/marsala-giffoni/route-metadata.json`
2. Presentare:
   - Durata totale
   - Numero segmenti
   - Affidabilità
   - Modalità trasporto

**Esempio output AI:**
```
Percorso Marsala → Giffoni Sei Casali:
- Durata: 15-18 ore
- Segmenti: 5 (treno + traghetto + bus)
- Affidabilità: 92%
- Validato da 15+ fonti

Segmenti principali:
1. Marsala → Trapani (treno)
2. Palermo → Salerno (Intercity Notte + traghetto)
3. Salerno → Giffoni (bus SITA)
```

#### Esempio 2: Dettaglio orari specifici

**Prompt utente:**
> "A che ora partono i treni da Marsala a Trapani?"

**AI dovrebbe:**
1. Leggere `routes/marsala-giffoni/segment-1-marsala-trapani.json`
2. Estrarre array `schedule`
3. Presentare orari formattati

**Esempio output AI:**
```
Treni Marsala → Trapani (Trenitalia Regionale):

• 04:05 → 04:38 (33min) - R 21851
• 04:55 → 05:38 (43min) - R 21853
• 05:25 → 05:58 (33min) - R 21857
• 06:21 → 06:55 (34min) - R 21861
• 07:13 → 07:41 (28min) - R 21859 ⭐ PIÙ VELOCE
...

Prezzo: €3,00 - €3,40
Frequenza: ogni 2 ore circa
```

#### Esempio 3: Itinerario completo con fermate

**Prompt utente:**
> "Mostrami tutte le fermate dell'Intercity Notte da Palermo"

**AI dovrebbe:**
1. Leggere `segment-3-palermo-napoli-ic-notte.json`
2. Iterare array `stops`
3. Evidenziare fermate speciali (traghetto)

**Esempio output AI:**
```
Intercity Notte 1958 - Palermo → Napoli/Salerno:

Partenza: 19:25 da Palermo Centrale

1. Palermo Centrale (19:25)
2. Termini Imerese
3. Cefalù
4. S. Agata di Militello
5. Capo d'Orlando
6. Barcellona
7. Patti
8. Milazzo
9. Messina Centrale
10. Messina Marittima 🚢 IMBARCO TRAGHETTO
11. [TRAGHETTO - 20 minuti]
12. Villa San Giovanni Marittima 🚢 SBARCO
13. Villa San Giovanni
14. Lamezia Terme Centrale
15. Paola
16. Sapri
17. Salerno (05:17) ⭐
18. Napoli Centrale (06:22)

Prezzo da: €28,00 (include traghetto)
```

### Schema Dati Strutturati

#### Route Metadata
```json
{
  "route_id": "string",
  "route_name": "string",
  "version": "semver",
  "last_updated": "YYYY-MM-DD",
  "validation_status": "verified|under_review|outdated",
  "sources_count": number,
  "destination": {
    "name": "string",
    "address": "string",
    "coordinates": {"latitude": number, "longitude": number}
  },
  "total_duration_hours": "string",
  "journey_segments": number,
  "transport_modes": ["string"],
  "complexity": "low|medium|high",
  "reliability_score": 0.0-1.0
}
```

#### Segment Data
```json
{
  "segment_id": "string",
  "segment_name": "string",
  "transport_type": "train|bus|ferry|taxi",
  "operator": "string",
  "validation": {
    "sources": ["string"],
    "confidence": "very_high|high|medium|low",
    "last_verified": "YYYY-MM-DD"
  },
  "route_details": {
    "distance_km": number,
    "duration_min": "string",
    "frequency": "string",
    "direct": boolean
  },
  "stops": [{
    "order": number,
    "station": "string",
    "type": "departure|intermediate|arrival|ferry_boarding|...",
    "coordinates": {"lat": number, "lon": number}
  }],
  "schedule": [{
    "departure": "HH:MM",
    "arrival": "HH:MM",
    "duration": "string",
    "train_number": "string"
  }],
  "pricing": {
    "currency": "EUR",
    "price_from": number,
    "price_avg": number
  }
}
```

## 👨‍💻 Per Sviluppatori

### Installazione

```bash
# Clona repository
git clone https://github.com/XxShinjiroxX/italy-travel-agile-map.git
cd italy-travel-agile-map
```

### Uso in Python

```python
import json
import requests
from typing import Dict, List

class ItalyTravelDB:
    BASE_URL = "https://raw.githubusercontent.com/XxShinjiroxX/italy-travel-agile-map/main"
    
    def get_route_metadata(self, route_name: str) -> Dict:
        """Ottieni metadati percorso"""
        url = f"{self.BASE_URL}/routes/{route_name}/route-metadata.json"
        return requests.get(url).json()
    
    def get_segment(self, route_name: str, segment_file: str) -> Dict:
        """Ottieni dettagli segmento"""
        url = f"{self.BASE_URL}/routes/{route_name}/{segment_file}"
        return requests.get(url).json()
    
    def get_all_stops(self, route_name: str, segment_file: str) -> List[str]:
        """Estrai lista fermate"""
        segment = self.get_segment(route_name, segment_file)
        return [stop['station'] for stop in segment['stops']]
    
    def get_schedule(self, route_name: str, segment_file: str) -> List[Dict]:
        """Ottieni orari"""
        segment = self.get_segment(route_name, segment_file)
        return segment.get('schedule', [])

# Esempio uso
db = ItalyTravelDB()

# Ottieni metadati
route = db.get_route_metadata('marsala-giffoni')
print(f"Percorso: {route['route_name']}")
print(f"Affidabilità: {route['reliability_score']*100}%")

# Ottieni orari treni
trains = db.get_schedule('marsala-giffoni', 'segment-1-marsala-trapani.json')
for train in trains[:5]:
    print(f"{train['departure']} → {train['arrival']} ({train['duration']})")

# Lista fermate
stops = db.get_all_stops('marsala-giffoni', 'segment-3-palermo-napoli-ic-notte.json')
for i, stop in enumerate(stops, 1):
    print(f"{i}. {stop}")
```

### Uso in JavaScript/Node.js

```javascript
const axios = require('axios');

class ItalyTravelDB {
  constructor() {
    this.baseURL = 'https://raw.githubusercontent.com/XxShinjiroxX/italy-travel-agile-map/main';
  }

  async getRouteMetadata(routeName) {
    const url = `${this.baseURL}/routes/${routeName}/route-metadata.json`;
    const response = await axios.get(url);
    return response.data;
  }

  async getSegment(routeName, segmentFile) {
    const url = `${this.baseURL}/routes/${routeName}/${segmentFile}`;
    const response = await axios.get(url);
    return response.data;
  }

  async getAllStops(routeName, segmentFile) {
    const segment = await this.getSegment(routeName, segmentFile);
    return segment.stops.map(stop => stop.station);
  }
}

// Esempio uso
(async () => {
  const db = new ItalyTravelDB();
  
  // Metadati percorso
  const route = await db.getRouteMetadata('marsala-giffoni');
  console.log(`Percorso: ${route.route_name}`);
  console.log(`Affidabilità: ${route.reliability_score * 100}%`);
  
  // Fermate
  const stops = await db.getAllStops('marsala-giffoni', 'segment-1-marsala-trapani.json');
  stops.forEach((stop, i) => console.log(`${i + 1}. ${stop}`));
})();
```

## 📝 Query Pratiche

### 1. "Qual è il primo treno da Marsala?"

```python
db = ItalyTravelDB()
schedule = db.get_schedule('marsala-giffoni', 'segment-1-marsala-trapani.json')
first_train = schedule[0]
print(f"Primo treno: {first_train['departure']} - {first_train['train_number']}")
```

### 2. "A che ora arriva l'Intercity Notte a Salerno?"

```python
segment = db.get_segment('marsala-giffoni', 'segment-3-palermo-napoli-ic-notte.json')
salerno_stop = next(s for s in segment['stops'] if 'Salerno' in s['station'])
print(f"Arrivo Salerno: {salerno_stop['arrival']}")
```

### 3. "Qual è la fermata bus più vicina allo studio?"

```python
segment = db.get_segment('marsala-giffoni', 'segment-5-salerno-giffoni.json')
nearest = next(s for s in segment['stops'] if s['type'] == 'nearest_to_destination')
print(f"{nearest['stop']} - {nearest['distance_to_destination_m']}m - {nearest['walk_time_min']} min a piedi")
```

## ⚠️ Note Importanti

### Verifica Sempre Prima del Viaggio

1. **SITA Bus**: Orari limitati, verificare su sitasudtrasporti.it
2. **Intercity Notte**: Prenotazione consigliata (cuccette limitate)
3. **Traghetto**: Incluso nel biglietto treno IC Notte
4. **Weekend/Festivi**: Servizi ridotti

### Affidabilità Dati

- 🟢 **92%+** = Molto affidabile, fonti multiple confermate
- 🟡 **80-91%** = Affidabile, alcune fonti da verificare
- 🟠 **70-79%** = Usare con cautela, dati parziali
- 🔴 **<70%** = Non affidabile, aggiornamento necessario

### Quando Aggiornare

⚡ **Aggiornamento immediato se:**
- Cambio orari operatore
- Sospensione linee
- Nuovi servizi disponibili
- Errori segnalati da utenti

📅 **Aggiornamento mensile routine**

## 🔗 Link Utili

- [Repository GitHub](https://github.com/XxShinjiroxX/italy-travel-agile-map)
- [Segnala Errore](https://github.com/XxShinjiroxX/italy-travel-agile-map/issues)
- [Changelog](routes/marsala-giffoni/CHANGELOG.md)

---

*Guida aggiornata: 2025-11-29*
