# Urban Mobility & Economic Productivity in Latin America

Exploratory data analysis combining real-time traffic data (TomTom Traffic Index) with urban economic indicators (OECD Cities) to understand how congestion relates to economic productivity in Latin American cities, and to flag where transport infrastructure investment may be most valuable.

> **Context:** Final project of Sprint 7 (Python for data analysis course). The scenario places me as a data analyst at the *Latin American Development Bank*, tasked with supporting infrastructure investment decisions.

---

## Business Questions

1. Which Latin American cities combine **high congestion** with **low economic productivity**?
2. Which cities show the best combined indicators (efficient mobility and a strong economy)?
3. Which variables seem most strongly related to urban development?

## Data Sources

| Dataset | Source | Description |
| --- | --- | --- |
| `tomtom_traffic.csv` | TomTom Traffic Index | ~1M real-time traffic records worldwide: jams delay, traffic index, jams length/count, travel time per 10 km, delay minutes |
| `oecd_city_economy.csv` | OECD Cities | Yearly city-level indicators: GDP per capita (USD), unemployment %, PM2.5, population |

> The raw datasets were provided by the course and are not included in this repository (see [How to run](#how-to-run)).

## Methodology

1. **Load and explore** both datasets (structure, dtypes, nulls).
2. **Clean and standardize**
   - Column names converted to `snake_case`.
   - Dates parsed with `pd.to_datetime`.
   - OECD numeric fields cleaned (thousand separators, decimal commas, `%` symbol) and converted to `float`.
   - New `population` column (`population_m` × 1,000,000).
3. **Extract the year** from the timestamps and **filter 2024**.
4. **Aggregate traffic** by city, country and year (mean of the traffic metrics).
5. **Merge** traffic and economy tables with an **inner join** on `city` and `year`.
6. **Visualize** distributions and relationships (boxplot, histogram, comparison bar chart) and compute correlations.
7. **Export** the final clean dataset: `ladb_mobility_economy_2024_clean.csv`.

## Coverage

- **Year:** 2024
- **Cities:** 15 (after the inner join)
- **Countries:** 7: Brazil (9 cities), Argentina, Chile, Colombia, Mexico, Peru and Uruguay

## Key Findings

- **No clear relationship between wealth and congestion.** The correlation between `jams_delay` and `city_gdp_capita` is **0.28** (weak and positive).
- Montevideo has the highest GDP per capita (~US$26k) and almost no congestion delay, while Bogotá and Lima have mid-range GDP and high delays.
- `jams_delay` is strongly right-skewed: mean (~630 min) is well above the median (~260 min). **Mexico City** is the main outlier (~2,800 min).
- The most congested cities (Mexico City, São Paulo, Bogotá, Lima) are also the largest, suggesting **city size may explain traffic better than GDP**.
- Santiago shows a GDP per capita far below the rest of the group (~US$2.3k), which looks suspicious and **needs to be verified at the source**.

## Recommendations

- **Bogotá** is the strongest candidate for priority investment: high congestion (~1,100 min) with GDP per capita below the group median. **Lima** follows with a similar profile.
- Before any investment decision:
  1. Validate Santiago's GDP value.
  2. Include `population` in the analysis to separate the effect of city size.
  3. Repeat the study with more years; 15 cities in a single year only supports preliminary conclusions.

## Repository Structure

```
.
├── README.md
├── notebook.ipynb                          # Full analysis (code + comments)
├── ladb_mobility_economy_2024_clean.csv    # Final clean dataset (15 cities, 2024)
└── data/                                   # (not tracked) raw CSVs go here
    ├── tomtom_traffic.csv
    └── oecd_city_economy.csv
```

## How to run

1. Clone the repository:
   ```bash
   git clone https://github.com/<your-username>/<your-repo>.git
   cd <your-repo>
   ```
2. Install the dependencies:
   ```bash
   pip install pandas numpy seaborn matplotlib jupyter
   ```
3. Place the raw CSVs in the `data/` folder.
4. In the notebook, update the file paths. The original notebook reads from `/datasets/...` (course platform path), so change them to `data/tomtom_traffic.csv` and `data/oecd_city_economy.csv`.
5. Launch Jupyter and run all cells:
   ```bash
   jupyter notebook
   ```

## Tech Stack

Python · pandas · NumPy · seaborn · matplotlib · Jupyter Notebook

## Limitations

- Only 15 cities and a single year, so results are exploratory and not statistically conclusive.
- Traffic data are averages of real-time snapshots, not a full-year official measure.
- The OECD data may contain inconsistencies (e.g., Santiago's GDP per capita).
- Correlation does not imply causation.

## Next Steps

- Add more years to analyze trends over time.
- Control for population (e.g., delay per capita) and other variables such as PM2.5 and unemployment.
- Build a combined mobility-economy index to rank cities.

## Author

**Camila Jorge**
[LinkedIn](https://linkedin.com/in/<your-profile>) · [GitHub](https://github.com/<your-username>)

