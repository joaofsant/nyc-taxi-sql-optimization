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