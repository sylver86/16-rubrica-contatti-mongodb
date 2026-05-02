# DigitalConnect — MongoDB Contacts Directory

![MongoDB](https://img.shields.io/badge/MongoDB-6.x-47A248?logo=mongodb&logoColor=white)
![NoSQL](https://img.shields.io/badge/Database-NoSQL%20Document-green)
![Shell](https://img.shields.io/badge/Shell-mongosh-yellow)

## Overview

Contacts directory built on MongoDB to demonstrate NoSQL document modelling for heterogeneous data: contacts with variable phone numbers (string or array), embedded sub-documents for addresses and social profiles, boolean flags, and array-based tags.

Covers the full CRUD lifecycle plus aggregation pipelines — from schema design and bulk import to update strategies, field-type migration, and analytical queries.

---

## Document Schema

```json
{
  "Nome": "Mario",
  "Cognome": "Rossi",
  "Numero_di_cellulare": ["333111222", "344555666"],
  "Società": "WebCorp",
  "Data_di_nascita": "1990-05-14",
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

---

## Setup

```bash
# 1. Start MongoDB
mongod --dbpath /data/db

# 2. Import the dataset
mongoimport --db Rubrica_Utenti \
            --collection Rubrica \
            --file contatti.json \
            --jsonArray

# 3. Open the shell
mongosh
use Rubrica_Utenti
```

---

## Query Examples

| Query | Technique |
|-------|-----------|
| Contacts from "WebCorp" | `find({ Società: "WebCorp" })` |
| Contacts with multiple phones | Array index check: `"Numero_di_cellulare.1": { $exists: true }` |
| Phone numbers of "lavoro" contacts | `find` with field projection, `_id: 0` |
| Contacts without social profile | `$exists: false` on nested field |
| Count close friends vs others | `aggregate` → `$group` by boolean field |
| Average calls for close friends | `$match` + `$avg` aggregation |
| Add phone to existing contact | `updateMany` with `$push` + type conversion |
| Insert new contact | `insertOne` with partial schema |

Full query implementations with inline explanation in `queries.js`.

---

## Design Highlights

- **Flexible schema**: phone field accepts both `string` and `array` — update strategy demonstrates in-place type conversion via `updateMany` without schema migration
- **Embedded sub-documents**: `Altri_contatti` nests email, address, and social link in a single document, avoiding relational joins
- **Array tags**: multi-value `Tag` field enables AND/OR filtering without a join table
- **Aggregation pipeline**: `$group` + `$avg` demonstrates MongoDB's analytics capability beyond simple CRUD

---

## Technologies

`MongoDB 6.x` · `mongosh` · `mongoimport` · `Aggregation Pipeline` · `Document Modelling` · `NoSQL`
