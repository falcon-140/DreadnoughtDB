# DreadnoughtDB: Naval Fleet Analytics Engine

A PostgreSQL-driven relational database project focused on historical naval fleet compositions, warship specifications, and battle outcomes from the early-to-mid 20th century. This project implements advanced SQL querying, schema creation, data aggregation, and analytical profiling using Python (`psycopg2`) and PostgreSQL (hosted via Neon Serverless Postgres).

## 📊 Database Architecture

The database models naval power dynamics using four interrelated entities:

* **`Classes`**: Tactical specifications of a ship model (country, type, guns, bore, displacement).
* **`Ships`**: Individual physical vessels, mapping out their launch year and designated structural class.
* **`Battles`**: Historical naval engagements and their exact dates.
* **`Outcomes`**: The operational status (`sunk`, `damaged`, `ok`) of specific ships post-engagement.

---

## 🛠️ Tech Stack & Dependencies

* **Language:** Python 3.x
* **Database System:** PostgreSQL (Neon Tech Serverless Engine)
* **Drivers & Libraries:**
    * `psycopg2-binary` (PostgreSQL database adapter)
    * `pandas` (Data manipulation and analytics)

To install the environment dependencies, run:
```bash
pip install psycopg2-binary SQLAlchemy pandas
