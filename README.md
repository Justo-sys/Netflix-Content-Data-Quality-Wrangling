# Netflix Content Data Quality & Wrangling

## 1. The Problem

Raw datasets are rarely ready for analysis.

Even when a dataset appears structured, missing values, inconsistent formats, incompatible field relationships, duplicated information, and logically impossible records can introduce errors into downstream analysis.

This project addresses a practical data-quality problem:

> **How can a raw content catalogue be systematically transformed into a consistent, analysis-ready dataset while making data-quality decisions explicit and validating that the resulting data remains logically coherent?**

Using the Netflix Movies and TV Shows dataset, the project focuses on the data-preparation stage that sits between **raw data collection and reliable analysis**.

The objective was not simply to clean a dataset. It was to establish a repeatable workflow for:

**Inspect → Structure → Clean → Validate → Export**

---

## 2. Data Quality Questions

The wrangling process was guided by the following questions:

### Data structure

- What does the dataset contain?
- What are the data types and structural characteristics of each field?
- Are multi-value fields represented in a form that can support analysis?

### Completeness

- Which variables contain missing information?
- Which missing values can be handled using available relationships in the data?
- Which records should be retained, labelled, or removed?

### Consistency

- Are dates logically consistent with release years?
- Are content types consistent with their duration units?
- Are categorical and numerical fields represented consistently?

### Analytical readiness

- Can multi-value fields such as cast and genre be represented in a more useful relational structure?
- Can the resulting dataset be validated and exported for downstream analysis?

---

## 3. Dataset

The project uses the **Netflix Movies and TV Shows dataset** from Kaggle.

- **Records:** 8,807
- **Features:** 12 original columns

The original dataset contains information including:

- title;
- director;
- cast;
- country;
- release year;
- rating;
- duration;
- date added;
- content type; and
- other catalogue attributes.

---

## 4. Project Workflow

The project followed a structured data-wrangling workflow:

**Data Exploration → Data Structuring → Missing Value Treatment → Consistency Checks → Validation → Export**

---

## 5. Data Exploration

The first stage established the structure and initial quality of the dataset.

The dataset was loaded using `pandas` and inspected using:

- `.head()`
- `.info()`
- `.shape`
- `.dtypes`

The exploration also included:

- identifying missing values by column;
- checking for duplicate records;
- examining the structure of individual variables; and
- identifying fields requiring transformation before analysis.

No duplicate records were identified during the initial duplicate check.

---

## 6. Data Structuring

The raw data contained fields that required transformation before they could be used efficiently in analysis.

### Date standardisation

`date_added` was converted into a datetime format to support temporal analysis and validation.

### Duration decomposition

The original `duration` field combined both numerical values and different units.

It was therefore separated into:

- `duration_value` — numeric duration;
- `duration_unit` — minutes or seasons.

This allowed duration to be analysed consistently while accounting for the difference between movies and TV shows.

### Multi-value field normalization

Several variables contained multiple values within a single cell.

Rather than leaving these as comma-separated strings, relational structures were created.

#### Cast table

A separate **cast table** was created to represent the relationship between shows and actors.

#### Genre table

A separate **genre table** was created to represent the relationship between shows and genres.

This transforms the original multi-value fields into structures that are more suitable for downstream analysis and relational modelling.

---

## 7. Missing Value Treatment

Missing values were handled according to the structure and available relationships in the data rather than applying one universal rule.

### Director

Missing director values were first addressed using frequent cast–director relationships where the available data supported such an inference.

Remaining missing values were labelled as:

`Not Given`

### Country

Missing country values were addressed using the most frequent country associated with the available director information.

Remaining missing values were labelled as:

`Not Given`

### Cast

Missing cast values were replaced with:

`Not Given`

### Records removed

Records with missing values in:

- `date_added`;
- `rating`; and
- `duration`

were removed.

These decisions were made to produce a complete dataset for the intended downstream analysis while keeping the assumptions explicit.

---

## 8. Data Quality & Error Handling

Cleaning was followed by validation rather than assuming that successful transformations automatically produced valid data.

### Date consistency

Records were checked for cases where:

`date_added year < release_year`

This represents a logical inconsistency because a title cannot normally be added to the catalogue before its recorded release year under the interpretation used for this analysis.

**14 inconsistent records** were identified and corrected by aligning `release_year` with `date_added`.

### Content type vs duration validation

The relationship between content type and duration unit was also checked.

The expected structure was:

- **Movies → minutes**
- **TV Shows → seasons**

This check was used to identify and validate inconsistent duration representations.

---

## 9. Validation

The cleaned dataset was subjected to post-cleaning validation.

### Completeness

All columns were confirmed to contain **zero missing values** after cleaning.

### Logical integrity

The final dataset was checked to confirm:

- no remaining date inconsistencies;
- duration units were consistent with content type;
- transformed fields remained usable for analysis.

Validation was treated as a separate stage because a dataset can be complete while still containing logically inconsistent records.

---

## 10. Output

The cleaned and validated dataset was exported as:

`netflix_titles_cleaned.csv`

The resulting file is intended to provide a more consistent starting point for downstream analysis.

---

## 11. What This Project Demonstrates

This project demonstrates an end-to-end data-wrangling workflow focused on **data quality and analytical readiness**.

The main capabilities demonstrated include:

- data exploration and profiling;
- handling missing data using relational logic;
- feature engineering;
- data normalization;
- working with many-to-many relationships;
- consistency and validation checks;
- Pandas-based data transformation;
- producing an analysis-ready output dataset.

More importantly, the project demonstrates that data preparation is not simply about removing nulls. It requires understanding **what the data represents, what assumptions are justified, and whether the resulting data remains logically coherent**.

---

## 12. Limitations

Some cleaning decisions rely on relationships observed within the dataset.

For example, imputing missing director or country information using other variables introduces assumptions that may not represent the true underlying value.

Similarly, correcting the 14 inconsistent `release_year` records by aligning them with `date_added` is an analytical treatment rather than independent confirmation of the original release year.

These decisions are therefore documented explicitly rather than presented as undisputed facts.

For production use, such records would ideally be validated against an authoritative external source.

---

## 13. Requirements

```text
pandas
