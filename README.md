# AI Travel Analyst — Flight Price Analysis
**MIC AIML Department Recruitment Challenge — Data Science & Visualization Track (Part 1)**

## Project Overview
This project explores a flight pricing dataset to understand what drives airfare in India and internationally. Starting from raw, inconsistently formatted data, the notebook cleans the key fields and builds seven visualizations that surface how distance, number of stops, booking lead time, booking channel, route length, and travel class relate to price.

## Problem Statement
Flight prices vary widely and travelers rarely know *why* one ticket costs more than another. The goal of Part 1 was to clean a messy real-world-style flight dataset and use exploratory visualization to identify the major factors that affect flight price, so that a traveler could use those patterns to book smarter.

## Dataset Used
- **File:** `flight_pricing_dataset.csv`
- **Size:** 100,000 rows × 18 columns
- **Source:** dataset provided by MIC for the challenge ([Google Drive link](https://drive.google.com/file/d/1tNUDxjXHzbRXe8CQdIoyJWh8OweGW0rR/view?usp=sharing))
- **Key columns used:** `Price`, `Distance_km`, `Total_Stops`, `Days_Before_Departure`, `Airline`, `Booking_Channel`, `Travel_Class`
- **Other columns available but not yet analyzed:** `Aircraft_Type`, `Season`, `Weekday`, `Duration`, `Passenger_Count`

The raw data is intentionally messy: roughly 5% missing values in every column, ~2,000 duplicate rows, inconsistent units (`"910.5"` vs `"910.5 km"`), inconsistent currency formatting (`"5181.56"` vs `"Rs. 5181.56"`), and inconsistent text casing (`"Air India"` vs `"AIR INDIA"` vs `"air india"`).

## Methodology
1. **Deduplication** — dropped exact duplicate rows.
2. **Distance cleaning** — stripped the `" km"` suffix and converted to numeric.
3. **Price cleaning** — stripped the `"Rs. "` prefix and converted to numeric.
4. **Stops cleaning** — normalized `"non-stop"` → `0` and `"1 stop"` / `"2 stops"` → numeric.
5. **Airline normalization** — upper-cased airline names so variants like `Air India` / `AIR INDIA` are merged.
6. **Days-before-departure cleaning** — stripped the `"days"` label and converted to numeric.
7. **Missing values** — rather than imputing, rows missing the specific fields needed for a given chart were dropped for that chart only (`dropna(subset=[...])`), so each visualization uses all the valid data available for that comparison.
8. Each chart is followed by a short written interpretation of what it shows.

## Technologies Used
- Python 3
- pandas — data cleaning and aggregation
- matplotlib — visualization
- Jupyter Notebook — analysis environment

## Results

### 1. Price vs. Distance
![Price vs Distance]
Price rises with distance up to ~2,000 km, then flattens into a hard ceiling around ₹2,00,000 regardless of how much farther the flight goes — suggesting the dataset caps or bands prices at long range rather than scaling them linearly forever.

### 2. Average Price by Airline
![Avg Price by Airline]
Average price varies noticeably by airline once naming inconsistencies are merged.

### 3. Price by Number of Stops
![Price by Stops]
Median and mean price both increase with the number of stops. The mean is consistently pulled above the median by high-price outliers, and that gap narrows as stops increase.

### 4. Median Price vs. Days Before Departure
![Price vs Days Before Departure]
Prices are highest for last-minute bookings and drop as the booking is made further in advance, leveling off after roughly 50 days out.

### 5. Median Price by Booking Channel
![Price by Booking Channel]
Booking channel makes only a small difference overall; the website tends to show slightly lower prices than other channels.

### 6. Median Price by Distance Category
![Price by Distance Category]
Grouping routes into short/medium/long/ultra-long-haul bands makes the distance effect from chart 1 clearer and easier to compare at a glance.

### 7. Median Price by Travel Class
![Price by Travel Class]
Travel class shows the largest gap of any factor plotted: median price is roughly ₹39,800 for Economy, ₹61,300 for Premium Economy, ₹116,400 for Business, and ₹183,800 for First — about a 4.6x spread from Economy to First.

## Challenges Faced
- The same underlying value showed up in multiple formats across columns (e.g. stops as `"non-stop"` vs `"0"`, distance as `"910.5"` vs `"910.5 km"`), so each column needed its own custom cleaning rule rather than one generic function.
- Airline names had three different casings for the same airline, which would have split single airlines into multiple bars in the bar chart if left uncleaned.
- Around 5% of values in every column were missing, which meant deciding, per chart, whether to drop or keep rows — dropping per-chart (rather than dropping any row with *any* missing value) kept more data usable overall.
- The price ceiling around ₹2,00,000 in the distance scatter plot doesn't have an obvious explanation yet — it isn't clear if it's a natural fare cap, a first/business-class limit, or a data-generation artifact.

## Future Improvements
- Investigate the ~₹2,00,000 price ceiling directly (e.g. by breaking the scatter plot down by travel class or airline) instead of just noting it — now that travel class is plotted, this is a natural next step since First/Business fares are the most likely explanation for that ceiling.
- Add a correlation heatmap across the numeric fields to back up "major factors" with a number, not just visual impression.
- Explore `Season`, `Weekday`, and `Aircraft_Type` as additional price factors.
- Move into Part 2 (feature engineering + a price prediction model) for the next academic-year tier of the challenge.


## Installation Instructions
```bash
# 1. Clone the repository
git clone <your-repo-url>
cd <your-repo-folder>

# 2. (Optional but recommended) create a virtual environment
python -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Launch the notebook
jupyter notebook Data_science_and_visualisation.ipynb
```
**Note:** the boxplot in this notebook uses matplotlib's `tick_labels` argument, which requires **matplotlib ≥ 3.9**. Older versions will raise an error on that cell.