# 📞 Customer Service Request Analysis (NYC 311 Data)

A data wrangling and exploratory data analysis (EDA) project on New York City's 311 service request data — cleaning a messy real-world dataset, uncovering complaint patterns by city, and statistically testing whether response time differs across complaint types.

---

## 📌 Problem Statement

You've been asked to analyze data on service request (311) calls from New York City, using data wrangling techniques to understand patterns in the data and visualize the major types of complaints.

## 🎯 Objectives

- Assess the data and prepare a clean dataset for training and prediction
- Plot a bar graph to identify the relationship between two variables
- Visualize the major types of complaints in each city

## 🧰 Prerequisites / Skills Applied

- Python fundamentals
- Application of Python data science libraries (pandas, numpy, seaborn, matplotlib, scipy)
- Exploratory data analysis on a real dataset
- DataFrame manipulation
- Statistical hypothesis testing (ANOVA, Kruskal-Wallis)

---

## 🗂️ Dataset

**File:** `Service_request.csv`
**Original shape:** 364,558 rows × 53 columns
**Final cleaned shape:** 360,431 rows × 18 columns

Key columns retained after cleaning:

| Column | Description |
|---|---|
| `Unique Key` | Unique identifier for each request |
| `Created Date` / `Closed Date` | Timestamps when the request was created/closed |
| `Agency` | Agency that handled the case |
| `Complaint Type` | Type of complaint received |
| `Descriptor` | Description of the complaint |
| `Location Type` | Type of location where the incident occurred |
| `Incident Zip` | Zip code of the incident |
| `Address Type` | Type of address (ADDRESS, INTERSECTION, etc.) |
| `City` | City where the incident occurred |
| `Facility Type` | Type of facility |
| `Status` | Status of the complaint |
| `Resolution Description` | Resolution provided |
| `Community Board` | Community board location |
| `Latitude` / `Longitude` | Geolocation of the incident |
| `Request Closing Time` | *Engineered feature:* time elapsed (in seconds) between Created Date and Closed Date |

Many low-value / mostly-null columns (School*, Taxi*, Bridge/Highway*, Ferry*, Vehicle Type, Landmark, Due Date, X/Y State Plane coordinates, etc.) were dropped during cleaning since they were either near-100% missing or not relevant for this analysis.

---

## 🧹 Data Cleaning & Wrangling Steps

1. **Loaded & explored** the dataset — shape, columns, dtypes, null counts, duplicates
2. **Dropped irrelevant/high-null columns**: `Agency Name`, `Landmark`, `Due Date`, school/taxi/bridge/ferry-related fields, redundant address fields, `Location`, coordinate columns
3. **Missing value treatment:**
   - Removed records with null `Closed Date`
   - Imputed `Descriptor` with `"No Description"`
   - Imputed `Location Type` with `"Unknown"`
   - Imputed `Incident Zip` with the mode
   - Imputed `Address Type` with `"Unknown"`
   - Imputed `City` with `"Unknown City"`
   - Imputed `Facility Type` with `"Unknown"`
   - Dropped rows with null `Latitude`/`Longitude`
4. **Date handling:**
   - Converted `Created Date` and `Closed Date` to datetime
   - Removed records with an incorrect timeline (`Closed Date` earlier than `Created Date`)
   - Engineered `Request Closing Time` = `Closed Date` − `Created Date`, converted to seconds
5. **Final check:** verified zero nulls across all retained columns, saved the cleaned dataset to `Customer Service Analysis.csv`

---

## 📊 Exploratory Data Analysis & Visualizations

- **Null value frequency plot** — visualized missing data before and after cleaning
- **Incident Zip distribution** — histogram/KDE and distplot
- **City-wise complaint frequency** — bar chart of complaint volume per city (Brooklyn, New York, Bronx, Staten Island, etc. are the top contributors)
- **Brooklyn complaint concentration** — scatter plot and hexbin plot of complaint density by latitude/longitude
- **Top complaint types (city-wide)** — bar chart; **Blocked Driveway**, **Illegal Parking**, and **Noise – Street/Sidewalk** are the top 3 complaint types overall
- **Top 10 complaint types** ranked by frequency
- **Complaint types by city** — cross-tab pivot table (`df_new`) and stacked bar chart showing complaint-type distribution across all cities
- **Average `Request Closing Time` by complaint type** — sorted bar chart; complaints like *Posting Advertisement* and *Illegal Fireworks* close fastest, while *Derelict Vehicle* and *Graffiti* take the longest to resolve

---

## 📈 Statistical Analysis

To test whether the average response/closing time is significantly different across complaint types:

- **One-way ANOVA (`scipy.stats.f_oneway`)** → p-value = 0.0
- **Kruskal-Wallis H Test (`scipy.stats.kruskal`)** → p-value = 0.0

**Conclusion:** Since p < 0.05 in both tests, we **reject the null hypothesis (H0)** — the average `Request Closing Time` is **not equal** across complaint types, i.e., response time significantly varies by complaint type.

---

## 💡 Key Observations

- Missing data was handled successfully across all critical columns
- The majority of complaints originate from a small set of cities (Brooklyn, New York, Bronx, Staten Island)
- Certain complaint types (e.g., Derelict Vehicle, Graffiti, Animal Abuse) take considerably longer to resolve than others (e.g., Posting Advertisement, Illegal Fireworks)
- Response/closing time varies significantly across complaint types, confirmed via ANOVA and Kruskal-Wallis tests

---

## 🛠️ Tools & Libraries Used

- **Python** (Jupyter Notebook)
- `pandas`, `numpy` — data wrangling
- `seaborn`, `matplotlib` — data visualization
- `scipy.stats` — ANOVA (`f_oneway`) and Kruskal-Wallis (`kruskal`) hypothesis testing

## 📁 Repository Structure

```
├── README.md
├── Service_request.csv                        # Raw source dataset
├── Customer_Service_Request_Analysis.ipynb     # Jupyter notebook with full analysis
└── Customer Service Analysis.csv               # Cleaned dataset (output)
```

## ▶️ How to Run

1. Clone or download this repository.
2. Ensure Python and the required libraries are installed:
   ```
   pip install pandas numpy seaborn matplotlib scipy
   ```
3. Place `Service_request.csv` in the same directory as the notebook.
4. Open `Customer_Service_Request_Analysis.ipynb` in Jupyter Notebook / JupyterLab and run all cells sequentially.
5. The cleaned dataset will be saved as `Customer Service Analysis.csv` in the same directory.

## 🎯 Project Outcome

This project demonstrates the full data wrangling and EDA workflow: importing and inspecting a messy real-world dataset, treating missing values and inconsistent timelines, engineering a derived time-based feature, visualizing complaint patterns geographically and by category, and validating findings with formal statistical tests.
