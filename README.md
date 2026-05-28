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
🚀 Key Queries & Analytical Profiles
The engine handles structured relational calculus and textbook-level aggregation problem solving.

1. Naval Development Timeline
Tracks the industrial longevity of naval production by measuring the exact span of years between a country's oldest and newest launched vessels.

SQL
SELECT c.country,
       MIN(s.launched) as oldest_ship_year,
       MAX(s.launched) as newest_ship_year,
       MAX(s.launched) - MIN(s.launched) as years_span
FROM Ships s
JOIN Classes c ON s.class = c.class
GROUP BY c.country
ORDER BY years_span DESC;
2. Sister Ship Identification
Performs a relational self-join to locate sister ships sharing an identical class blueprint whose build completions occurred within a tight 2-year window.

SQL
SELECT s1.name, s2.name, s1.class, s1.launched, s2.launched
FROM Ships s1
JOIN Ships s2 ON s1.class = s2.class
WHERE s1.name < s2.name
  AND ABS(s1.launched - s2.launched) <= 2
ORDER BY s1.class;
3. Fleet Hegemony Index
Aggregates total combat utility metrics (gross ship counts, total artillery pieces deployed, and gross tonnage/displacement) segmented by superpower state.

SQL
SELECT c.country,
       COUNT(s.name) as total_ships,
       SUM(c.numGuns) as total_guns,
       SUM(c.displacement) as total_tonnage
FROM Ships s
JOIN Classes c ON s.class = c.class
GROUP BY c.country
ORDER BY total_tonnage DESC;
💡 Code Verification & Fixes
During development, the following textbook execution checkpoints were analyzed:

Battle Progression Evaluation (Exercise 6.2.3):
A query checking for ships damaged in one battle that later fought in another returned an empty set [] on this dataset. This correctly reflects the current state of the database, as no ships meet this temporal criteria based on the inserted rows.

String Pattern-Matching Fix:
The prompt query for identifying "ships whose name consists of three or more words" used the constraint WHERE LENGTH(name)>= 2;. This evaluates character lengths, which is why it outputted every vessel. To properly target 3+ words separated by spaces, use word-boundary wildcard filtering:

SQL
SELECT name FROM Ships
WHERE name LIKE '% % %';
Cascade Resets:
Transactional integrity is preserved across database resets by attaching RESTART IDENTITY CASCADE to the truncation queries, mitigating foreign-key constraint deadlocks.
"""
