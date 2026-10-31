# Daylight Saving Time and Fatal Traffic Crashes (SDS 4135, Project 4)

This project looks at whether Daylight Saving Time (DST) is related to fatal traffic
crashes in the United States, using 25 years of crash data (2000–2024).

## Data

The data comes from the National Highway Traffic Safety Administration's
**Fatality Analysis Reporting System (FARS)**, a census of fatal motor vehicle crashes
on U.S. public roads.

- **2015–2024:** the provided `data/fars.RData` file
- **2000–2014:** national FARS CSV files downloaded from
  [NHTSA](https://static.nhtsa.gov/nhtsa/downloads/FARS/) (accident-level data only)

The two sources are combined into one file: **`data/fars_2000_2024.parquet`**

| | |
|---|---|
| Rows | 881,056 |
| Columns | 104 |
| Years | 2000–2024 |
| Duplicate crashes (`year`, `state`, `st_case`) | 0 |

> **Each row is one fatal crash, not one person killed.** A single crash can involve
> more than one fatality.

A few things to know about the columns:

- Column names were lowercased so both sources line up.
- Some columns exist only for part of the period. For example, the `...name` label
  columns (`statename`, `monthname`, …) exist only for 2015–2024, and older
  variables such as `sp_limit` and `sur_cond` exist only for 2000–2014. These are
  blank (`NaN`) for the other years.
- FARS uses special codes for unknown values, and these are still in the data.
  For example, **`hour == 99` means the crash hour is unknown** (6,918 crashes).
  Filter these out before any analysis by time of day.
- The checks done so far are row counts, years covered, and duplicate crash IDs.
  The data has **not** been otherwise cleaned.

## Notebooks

All notebooks are in `notebooks/`. Run them from that folder.

| Notebook | What it does |
|---|---|
| `00_data_preparation.ipynb` | Loads `fars.RData`, downloads and reads the 2000–2014 files, combines all 25 years, checks the result, and saves the Parquet file. **You do not need to run this.** |
| `01_exploratory_analysis.ipynb` | Annual crash trends, average daily crashes by month, and monthly comparisons across 2000–2008, 2009–2018, and 2019–2024. |
| `02_statistical_analysis.ipynb` | Starting point for our DST analysis (in progress). |

## Getting Started

1. Clone the repository:

   ```bash
   git clone https://github.com/adipstha11/Project4_Daylight.git
   cd Project4_Daylight
   ```

2. Install the Python packages:

   ```bash
   pip install -r requirements.txt
   ```

3. Open any notebook in `notebooks/` and load the data:

   ```python
   import pandas as pd

   fars = pd.read_parquet("../data/fars_2000_2024.parquet")
   ```

You do not need R, the raw ZIP files, or the preparation notebook to start analyzing.

## Working Together

- **Pull before you start** (`git pull`) and **commit often** with clear messages.
- **Avoid editing the same notebook at the same time.** Notebook merge conflicts are
  hard to fix. Make your own notebook or work on your own branch, then combine the
  work later.
- **Don't change `fars_2000_2024.parquet`.** If you need a new column, create it
  inside your notebook.
- Put research papers and assignment PDFs in `references/`.

## Project Structure

```text
Project4_Daylight/
├── data/
│   ├── fars.RData                  # original 2015–2024 data
│   ├── FARS20XXNationalCSV.zip     # raw 2000–2014 downloads (not on GitHub)
│   └── fars_2000_2024.parquet      # combined dataset, start here
├── notebooks/
│   ├── 00_data_preparation.ipynb
│   ├── 01_exploratory_analysis.ipynb
│   └── 02_statistical_analysis.ipynb
├── references/
├── README.md
├── requirements.txt
└── .gitignore
```
