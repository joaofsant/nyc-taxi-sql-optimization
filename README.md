# NYC Yellow Taxi — SQL Optimization with DuckDB

**Goal:** analyze a large real-world dataset (NYC Yellow Taxi, Jan 2015) using pure SQL, and demonstrate optimization strategies for time-based queries.  
**Why DuckDB?** It's an in-process analytical database that allows querying CSVs directly — perfect for reproducible portfolio projects without heavy setup.

---

## Dataset
- **Source:** [NYC TLC Yellow Taxi Trip Records](https://www.nyc.gov/assets/tlc/downloads/pdf/trip_record_data_user_guide.pdf)  
- **File used:** `yellow_tripdata_2015-01.csv` (~12.7M rows, ~1.99GB)

---

## Quickstart

Clone and install requirements:

```bash
git clone https://github.com/joaofsant/nyc-taxi-sql-optimization.git
cd nyc-taxi-sql-optimization

python -m venv .venv
source .venv/bin/activate   # (Windows: .venv\Scripts\activate)

pip install -r requirements.txt

Run the notebook:
jupyter notebook analysis.ipynb
👉 Or just open analysis.ipynb directly on GitHub — all queries and results are already rendered.

Highlights (Jan 2015)
	•	Rows scanned: ~12.7 million
	•	Avg trip distance: 13.46 miles
	•	Time span: 2015-01-01 00:00:00 → 2015-02-02 16:30:52
	•	Top 10 busiest days: queries show clear weekday peaks vs weekend dips
	•	Revenue per day: strong seasonality, weekday vs weekend
	•	Payment mix: distribution by payment_type shows dominance of card vs cash
	•	Tips: median tip ≈ 1 USD, 75th percentile ≈ 2.5 USD

SQL Examples

Top 10 busiest days:
SELECT DATE_TRUNC('day', pickup_ts) AS day, COUNT(*) AS trips
FROM trips
WHERE pickup_ts >= TIMESTAMP '2015-01-01'
  AND pickup_ts <  TIMESTAMP '2015-02-01'
GROUP BY 1
ORDER BY trips DESC
LIMIT 10;

Revenue per day:
SELECT DATE_TRUNC('day', pickup_ts) AS day,
       ROUND(SUM(total_amount), 2) AS revenue
FROM trips
WHERE pickup_ts >= TIMESTAMP '2015-01-01'
  AND pickup_ts <  TIMESTAMP '2015-02-01'
GROUP BY 1
ORDER BY day;

Payment mix:
WITH t AS (
  SELECT payment_type, COUNT(*) AS trips
  FROM trips
  WHERE pickup_ts >= TIMESTAMP '2015-01-01'
    AND pickup_ts <  TIMESTAMP '2015-02-01'
  GROUP BY 1
)
SELECT payment_type, trips,
       ROUND(100.0 * trips / SUM(trips) OVER (), 2) AS pct
FROM t
ORDER BY trips DESC;

Project Structure

├── analysis.ipynb          # Main notebook (queries + outputs)
├── requirements.txt        # Dependencies
├── artifacts/              # Optional exports (CSV with daily revenue etc.)
└── README.md               # Project overview

Notes
	•	The notebook was executed and saved with outputs — GitHub renders the results without any setup.
	•	Heavy CSV files are not committed (.gitignore) to keep the repo lightweight.
	•	This project demonstrates:
	•	handling large CSVs with SQL only,
	•	optimizing queries (indexes in Postgres, BRIN vs B-Tree, etc.),
	•	exploring real-world data in a reproducible way.

✦ Built as part of a Data Engineering learning journey.
Portfolio-friendly: simple to reproduce, results visible at a glance.

