# DigitalConnect — Rubrica Contatti su MongoDB

![MongoDB](https://img.shields.io/badge/MongoDB-6.x-47A248?logo=mongodb&logoColor=white)
![NoSQL](https://img.shields.io/badge/Database-NoSQL%20Document-green)
![Shell](https://img.shields.io/badge/Shell-mongosh-yellow)

## Panoramica

Rubrica contatti su MongoDB per dimostrare la modellazione di dati NoSQL eterogenei: contatti con numeri di telefono variabili (string o array), sub-documenti annidati per indirizzi e profili social, tag multi-valore e flag booleani. Copre il ciclo CRUD completo più aggregation pipeline — dalla progettazione dello schema all'importazione bulk, aggiornamento con migrazione di tipo e query analitiche.

Competenze NoSQL rilevanti per architetture microservizi, sistemi enterprise con dati eterogenei e qualsiasi contesto in cui la rigidità dello schema relazionale è un vincolo operativo.

## Valore Enterprise

| Settore / Azienda | Rilevanza |
|-------------------|-----------|
| IT Consulting (NTT Data, Accenture) | NoSQL in architetture cloud-native e microservizi |
| Telco & Media | Gestione profili utente con attributi variabili |
| Engineering Informatica | Database documentali in applicazioni enterprise |
| Data Reply | MongoDB come componente di data platform moderna |

## Schema Documento

```json
{
  "Nome": "Mario",
  "Cognome": "Rossi",
  "Numero_di_cellulare": ["333111222", "344555666"],
  "Società": "WebCorp",
  "Tag": ["lavoro", "cliente"],
  "Altri_contatti": {
    "email": "mario@webcorp.it",
    "Indirizzo": "Via Roma 1, Milano",
    "Profilo_social": "linkedin.com/in/mariorossi"
  },
  "Chiamate_ultimo_mese": 12,
  "Amici_stretti": false
}
```

## Setup

```bash
# 1. Avvia MongoDB
mongod --dbpath /data/db

# 2. Importa il dataset
mongoimport --db Rubrica_Utenti \
            --collection Rubrica \
            --file contatti.json \
            --jsonArray

# 3. Apri la shell
mongosh
use Rubrica_Utenti
```

## Query Implementate

| Query | Tecnica |
|-------|---------|
| Contatti di "WebCorp" | `find({ Società: "WebCorp" })` |
| Contatti con più telefoni | Check indice array: `"Numero_di_cellulare.1": { $exists: true }` |
| Numeri contatti tag "lavoro" | `find` con field projection, `_id: 0` |
| Contatti senza profilo social | `$exists: false` su campo annidato |
| Count amici stretti vs altri | `aggregate` → `$group` su campo booleano |
| Media chiamate amici stretti | `$match` + `$avg` in aggregation |
| Aggiunta telefono a contatto | `updateMany` con `$push` + conversione tipo |
| Inserimento nuovo contatto | `insertOne` con schema parziale |

## Design Highlights

- **Schema flessibile**: campo telefono accetta `string` e `array` — aggiornamento in-place senza schema migration
- **Sub-documenti annidati**: `Altri_contatti` raggruppa email, indirizzo e social in un singolo documento, evitando JOIN
- **Tag array**: filtri AND/OR su `Tag` senza tabella pivot
- **Aggregation pipeline**: `$group` + `$avg` dimostra capacità analitiche oltre il CRUD

## Stack Tecnologico

`MongoDB 6.x` · `mongosh` · `mongoimport` · `Aggregation Pipeline` · `Document Modelling`

---

---

# DigitalConnect — MongoDB Contacts Directory 🇬🇧

![MongoDB](https://img.shields.io/badge/MongoDB-6.x-47A248?logo=mongodb&logoColor=white)

## Overview

Contacts directory on MongoDB demonstrating NoSQL document modelling for heterogeneous data: variable phone numbers (string or array), embedded sub-documents for addresses and social profiles, multi-value array tags, and boolean flags. Covers full CRUD lifecycle plus aggregation pipelines — from schema design and bulk import to field-type migration and analytical queries.

## Document Schema

```json
{
  "Nome": "Mario", "Cognome": "Rossi",
  "Numero_di_cellulare": ["333111222", "344555666"],
  "Tag": ["lavoro", "cliente"],
  "Altri_contatti": { "email": "...", "Indirizzo": "...", "Profilo_social": "..." },
  "Chiamate_ultimo_mese": 12, "Amici_stretti": false
}
```

## Setup

```bash
mongod --dbpath /data/db
mongoimport --db Rubrica_Utenti --collection Rubrica --file contatti.json --jsonArray
mongosh && use Rubrica_Utenti
```

## Queries Implemented

| Query | Technique |
|-------|-----------|
| Contacts from "WebCorp" | `find({ Società: "WebCorp" })` |
| Multiple phone numbers | Array index check: `"Numero_di_cellulare.1": { $exists: true }` |
| Phones of "lavoro" contacts | `find` with field projection |
| Without social profile | `$exists: false` on nested field |
| Count close friends | `aggregate` → `$group` on boolean |
| Avg calls close friends | `$match` + `$avg` |
| Add phone to contact | `updateMany` + `$push` with type conversion |
| Insert new contact | `insertOne` with partial schema |

## Design Highlights

- **Flexible schema**: phone field in-place type migration (string→array) without schema migration
- **Embedded sub-documents**: avoids JOINs for contact details
- **Array tags**: multi-value filtering without pivot tables
- **Aggregation pipeline**: analytics capability beyond CRUD

## Technologies

`MongoDB 6.x` · `mongosh` · `mongoimport` · `Aggregation Pipeline` · `Document Modelling`
