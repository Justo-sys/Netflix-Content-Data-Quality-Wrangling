# Netflix Data Wrangling Project

## Overview

This project demonstrates practical data wrangling and data quality techniques using the **Netflix Movies and TV Shows dataset** from Kaggle.

The goal was to prepare raw data for analysis by performing:

* Data exploration
* Structuring and transformation
* Missing value treatment
* Consistency and validation checks
* Exporting a clean, analysis-ready dataset

**Tools used:**
Python, Pandas, VS Code

---

## Dataset

Source: Netflix Movies and TV Shows dataset (Kaggle)
Records: 8,807
Features: 12 original columns including title, director, cast, country, release year, rating, and duration.

---

## Project Workflow

### 1. Data Loading

* Imported dataset using `pandas`
* Reviewed structure using:

  * `.head()`
  * `.info()`
  * `.shape()`
  * `.dtypes()`

---

### 2. Data Exploration

* Identified missing values per column
* Checked for duplicate records (none found)
* Examined data types and column structure

---

### 3. Data Structuring

* Converted `date_added` to datetime format
* Split `duration` into:

  * `duration_value` (numeric)
  * `duration_unit` (minutes or seasons)
* Normalized multi-value fields:

  * Created **cast table** (show–actor relationship)
  * Created **genre table** (show–genre relationship)

---

### 4. Data Cleaning

#### Missing Value Treatment

* **Director**

  * Imputed using frequent cast–director relationships
  * Remaining values labeled as *Not Given*

* **Country**

  * Imputed using the most frequent country per director
  * Remaining values labeled as *Not Given*

* **Cast**

  * Missing values replaced with *Not Given*

* Rows with missing values in:

  * `date_added`
  * `rating`
  * `duration`
    were removed.

---

### 5. Data Quality & Error Handling

#### Consistency Checks

* Identified records where:

  `date_added year < release_year`

* Corrected 14 inconsistent records by aligning `release_year` with `date_added`.

#### Type vs Duration Validation

* Verified:

  * Movies → minutes
  * TV Shows → seasons

---

### 6. Validation

#### Completeness

All columns confirmed to have **zero missing values** after cleaning.

#### Logical Integrity

* No remaining date inconsistencies
* Duration units consistent with content type

---

### 7. Output

Cleaned dataset exported as:

`netflix_titles_cleaned.csv`

---

## Key Skills Demonstrated

* Data exploration and profiling
* Handling missing data with relational logic
* Feature engineering
* Data normalization (many-to-many relationships)
* Data quality validation (completeness, consistency)
* Pandas data transformation techniques

---

## Requirements

```
pandas
```

---

## Project Structure

```
netflix-data-wrangling/
│
├── netflix_wrangling.ipynb
├── netflix_titles_cleaned.csv
├── requirements.txt
└── README.md
```

---

## Author

Justus Onyango
Data Wrangling Portfolio Project
