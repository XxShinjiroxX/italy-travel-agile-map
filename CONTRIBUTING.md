# 🤝 Guida per Contribuire

Grazie per il tuo interesse nel contribuire a **Italy Travel Agile Map**! 

Questo progetto si basa sulla **validazione collettiva multi-fonte** per garantire dati di viaggio affidabili e sempre aggiornati.

## 🎯 Obiettivo del Progetto

Creare un database di percorsi di viaggio in Italia:
- ✅ **Verificato** da fonti multiple
- 🔄 **Versionato** con Git per tracciabilità
- 📅 **Aggiornato** mensilmente o on-demand
- 🔙 **Rollback-ready** se nuovi dati errati

## 👥 Come Puoi Contribuire

### 1️⃣ Segnalare Errori nei Dati

Se trovi dati obsoleti o incorretti:

1. **Apri una Issue** su GitHub
2. **Usa il template "Data Error Report"**
3. **Fornisci:**
   - Percorso e segmento interessato
   - Dato errato attuale
   - Dato corretto + fonte verificabile (link)
   - Data della tua verifica

**Esempio Issue:**
```markdown
Titolo: [marsala-giffoni] Orario bus SITA errato

Percorso: Marsala → Giffoni Sei Casali
Segmento: segment-5-salerno-giffoni.json

Dato errato:
- Orario partenza bus: 06:40

Dato corretto:
- Orario partenza bus: 07:00 (verificato su sitasudtrasporti.it il 2025-11-29)

Fonte: https://sitasudtrasporti.it/campania/orari/5170/

Note: Cambio orario invernale dal 01/12/2025
```

### 2️⃣ Aggiornare Dati Esistenti

**Processo:**

1. **Fork del repository**
2. **Clona il tuo fork**
   ```bash
   git clone https://github.com/[TUO-USERNAME]/italy-travel-agile-map.git
   cd italy-travel-agile-map
   ```

3. **Crea branch per aggiornamento**
   ```bash
   git checkout -b update-[percorso]-[data]
   ```

4. **Aggiorna i file JSON** con nuovi dati
   - Modifica solo i dati cambiati
   - Mantieni la struttura esistente
   - Aggiorna `last_verified` in validation

5. **Aggiorna CHANGELOG.md**
   ```markdown
   ## [1.1.0] - 2025-12-01
   
   ### Changed
   - Aggiornato orario partenza bus SITA da 06:40 a 07:00
   - Fonte: sitasudtrasporti.it (verificato 2025-11-29)
   
   ### Sources Added
   - sitasudtrasporti.it orario invernale 2025/2026
   ```

6. **Aggiorna version in route-metadata.json**
   ```json
   {
     "version": "1.1.0",
     "last_updated": "2025-12-01"
   }
   ```

7. **Commit con messaggio descrittivo**
   ```bash
   git add .
   git commit -m "Update SITA bus schedule for winter 2025/2026 - marsala-giffoni route"
   ```

8. **Push e crea Pull Request**
   ```bash
   git push origin update-[percorso]-[data]
   ```

9. **Nella PR descrivi:**
   - Cosa hai aggiornato
   - Perché (cambio orario, errore precedente, ecc.)
   - Fonti utilizzate (link)
   - Data verifica

### 3️⃣ Aggiungere Nuovo Percorso

**Processo completo:**

#### Step 1: Ricerca Multi-Fonte

**Minimo 5 fonti per segmento:**
- Sito ufficiale operatore
- Aggregatori (Rome2Rio, Omio, TheTrainline)
- Database ferroviari (E656.net per Italia)
- App trasporto pubblico (Moovit, Citymapper)
- Fonti locali

**Checklist raccolta dati:**
- [ ] Orari verificati su almeno 3 fonti
- [ ] Prezzi confermati da 2+ fonti
- [ ] Fermate verificate su fonti ufficiali
- [ ] Coordinate GPS da Google Maps o OpenStreetMap
- [ ] Contatti destinazione verificati

#### Step 2: Strutturazione

1. **Crea nuova cartella**
   ```bash
   mkdir -p routes/[origine]-[destinazione]
   ```

2. **Usa i template** da `templates/`
   - `route-metadata.json`
   - `segment-X.json` (uno per segmento)
   - `CHANGELOG.md`
   - `validation-sources.md`

3. **Compila tutti i file** seguendo lo schema

#### Step 3: Validazione

**Confronto incrociato:**

| Dato | Fonte 1 | Fonte 2 | Fonte 3 | Status |
|------|---------|---------|---------|--------|
| Durata | 45min | 45min | 50min | ⚠️ Verificare |
| Prezzo | €5 | €5 | €5 | ✅ OK |
| Fermate | 12 | 12 | 12 | ✅ OK |

**Calcolo Reliability Score:**
```
Reliability = (Dati Consistenti / Dati Totali) * (Numero Fonti / 10)

Esempio:
- 50 dati raccolti
- 46 consistenti su multiple fonti
- 8 fonti totali

Reliability = (46/50) * (8/10) = 0.92 * 0.8 = 0.736 → 74%
```

#### Step 4: Documentazione

**CHANGELOG.md completo:**
```markdown
## [1.0.0] - 2025-11-29

### Added
- Percorso completo [Origine] → [Destinazione]
- 4 segmenti: treno + bus + metro + piedi
- 45 fermate totali documentate
- Orari completi per tutti i segmenti
- Coordinate GPS destinazione
- 8 fonti validate

### Validated Sources
- Trenitalia.com (ufficiale)
- TheTrainline.com
- Rome2Rio.com
- Omio.it
- Moovit.com
- [Operatore locale]
- E656.net
- Google Maps

### Notes
- Reliability: 0.87 (87%)
- Journey time: 3-4 ore
- Confidence: High
- Critical: Bus con orari limitati (verificare prima)

### Known Issues
- Weekend: servizio bus ridotto
- Festivi: verificare disponibilità
```

#### Step 5: Pull Request

**Titolo PR:**
```
Add new route: [Origine] → [Destinazione] (v1.0.0)
```

**Descrizione PR:**
```markdown
## Nuovo Percorso

**Route:** [Origine] → [Destinazione]
**Segments:** 4
**Total Duration:** 3-4 ore
**Reliability Score:** 0.87 (87%)

## Fonti Validate

- [x] Trenitalia.com (verificato 2025-11-29)
- [x] TheTrainline.com (verificato 2025-11-29)
- [x] Rome2Rio.com (verificato 2025-11-29)
- [x] Omio.it (verificato 2025-11-29)
- [x] Moovit.com (verificato 2025-11-29)
- [x] E656.net (verificato 2025-11-29)
- [x] Google Maps (coordinate GPS)
- [x] [Operatore bus locale] (orari ufficiali)

## Validazione Incrociata

Tutti i dati critici verificati su almeno 3 fonti.

## Checklist

- [x] Tutti i file creati secondo template
- [x] CHANGELOG.md compilato
- [x] validation-sources.md compilato
- [x] Coordinate GPS verificate
- [x] Prezzi attuali confermati
- [x] Orari verificati per data corrente
- [x] Note e warning aggiunti dove necessario

## Note Aggiuntive

- Bus: orari limitati weekend, verificare sempre prima
- Destinazione: 5 min a piedi da ultima fermata
```

### 4️⃣ Migliorare Documentazione

Contribuisci anche a:
- README.md (chiarezza, esempi)
- USAGE_GUIDE.md (nuovi use case)
- Templates (miglioramenti struttura)
- Questo file (CONTRIBUTING.md)

## 📝 Standard di Qualità

### Validazione Dati

✅ **Accettabile:**
- 5+ fonti per percorso
- Dati consistenti su almeno 3 fonti
- Verificato negli ultimi 30 giorni
- Reliability score ≥ 0.70

❌ **Non Accettabile:**
- Singola fonte
- Dati in conflitto non risolti
- Verifica oltre 90 giorni fa
- Reliability score < 0.60

### Formattazione JSON

```json
{
  "key": "value",
  "nested": {
    "subkey": "value"
  },
  "array": [
    {"item": 1},
    {"item": 2}
  ]
}
```

- Indentazione: 2 spazi
- UTF-8 encoding
- Nessun trailing comma
- Chiavi in snake_case o camelCase consistente

### Commit Messages

**Formato:**
```
[tipo] Breve descrizione (max 50 caratteri)

Descrizione dettagliata se necessario.

Fonti:
- fonte1.com (verificato YYYY-MM-DD)
- fonte2.com (verificato YYYY-MM-DD)
```

**Tipi:**
- `add` - Nuovo percorso o segmento
- `update` - Aggiornamento dati esistenti
- `fix` - Correzione errore
- `docs` - Modifica documentazione
- `refactor` - Ristrutturazione senza cambio dati

**Esempi:**
```
add: New route Rome to Florence (v1.0.0)

update: Winter 2025 schedule for SITA bus line 5170

fix: Correct ICN 1958 arrival time at Salerno (05:17 not 05:30)

docs: Add Python usage examples to USAGE_GUIDE.md
```

## ⚖️ Processo di Review

### Cosa Verifichiamo

1. **Completezza dati**
   - Tutti i campi richiesti compilati
   - CHANGELOG aggiornato
   - Fonti documentate

2. **Validazione incrociata**
   - Almeno 3 fonti per dati critici
   - Discrepanze risolte o documentate
   - Reliability score calcolato correttamente

3. **Qualità tecnica**
   - JSON valido
   - Struttura consistente
   - Nomi file corretti

4. **Documentazione**
   - CHANGELOG chiaro
   - Note e warning presenti
   - Fonti linkate

### Tempi di Review

- **Aggiornamenti minori**: 1-2 giorni
- **Nuovi percorsi**: 3-7 giorni
- **Modifiche documentazione**: 1-3 giorni

## 🛡️ Gestione Conflitti

Se due fonti forniscono dati diversi:

1. **Verifica fonte più autorevole**
   - Ufficiale > Aggregatore > Comunità

2. **Cerca terza fonte** per tiebreaker

3. **Documenta la discrepanza**
   ```json
   {
     "note": "Fonte A indica 45min, Fonte B 50min. Usato 45min da fonte ufficiale."
   }
   ```

4. **Abbassa confidence** se irrisolto
   ```json
   {
     "validation": {
       "confidence": "medium",
       "note": "Discrepanza durata tra fonti, da verificare"
     }
   }
   ```

## 📞 Supporto

**Hai domande?**

- Apri una **Discussion** su GitHub
- Chiedi nel **PR/Issue thread**
- Contatta maintainer: [@XxShinjiroxX](https://github.com/XxShinjiroxX)

## 🎉 Riconoscimenti

Tutti i contributori saranno:
- Listati in CONTRIBUTORS.md
- Menzionati nei CHANGELOG delle route modificate
- Creditati nei commit

Grazie per aiutare a migliorare questo progetto! 🚀

---

*Ultima modifica: 2025-11-29*
