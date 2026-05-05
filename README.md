# Healthcare Equity and Hospital Capacity in Maryland

A quantitative epidemiological study examining hospital infrastructure disparities across all 24 Maryland jurisdictions (23 counties and Baltimore City). Using public licensing data from the Maryland Department of Health, this project applies a comprehensive suite of statistical methods to document and quantify geographic inequalities in healthcare capacity — with direct implications for public health planning, emergency preparedness, and healthcare equity policy.

**Co-authored by:** Stuti Upadhyay and Rajdeep Roy | UMBC DATA608 — Probability and Statistics for Data Science | December 2025

---

## Key Findings

| Finding | Result |
|---|---|
| Urban vs. Rural capacity gap | Urban mean 254.9 beds vs. Rural mean 121.9 beds |
| Statistical significance | p = 0.0078 (pooled t-test), p = 0.0111 (Welch's) |
| Effect size | Cohen's d = 0.76 (medium to large) |
| Urban location premium (MLR) | +206 beds holding other factors constant |
| Baltimore City dominance | 15 hospitals, 4,212 beds (32.5% of state total) |
| Rural healthcare deserts | 4 counties with zero high-capacity hospitals, 101 combined beds |
| Statewide disparity ratio | 1,404-fold (Baltimore City vs. Somerset County) |

---

## Research Questions

1. Is there a relationship between hospital ownership and the number of specialized services offered?
2. Are there regional disparities in the availability of acute vs. special hospitals?
3. Is there any correlation between hospital type and ownership?
4. What is the average bed-to-hospital ratio by county, and how does it compare across regions?
5. Which Maryland counties are most vulnerable to healthcare capacity crises?
6. Where do healthcare deserts exist spatially, and what is the average travel distance for residents?
7. What percentage of Maryland's population lives within a 15-minute drive of a hospital with more than 50 licensed beds?
8. Do federally designated Medically Underserved Areas (MUAs) have systematically lower hospital capacity per capita?

---

## Dataset

**Source:** Maryland Department of Health, Office of Health Care Quality — Acute, General, and Special Hospitals Licensing Registry (Public Open Data)

- 64 licensed hospital facility records
- All 23 counties and Baltimore City
- Data current as of August 2, 2025
- 99.2% complete for analyzed variables (3 missing License Capacity values)

**Key Variables:**
- `License Capacity` — licensed bed count (primary dependent variable)
- `Type` — Acute/General/Special, Psychiatric, Children's, Rehabilitation, Geriatric
- `County` — 24 geographic units
- `the_geom` — geographic coordinates for spatial analysis

**Derived Variables Created:**
- `RegionType` — Urban vs. Rural binary classification (based on Maryland Department of Planning designations)
- `is_high_capacity` — Binary: High (more than 50 beds) vs. Low (50 beds or fewer)
- `capacity_category` — Ordinal: Very Small (0-30), Small (31-100), Medium (101-200), Large (201-500), Very Large (500 plus)

---

## Statistical Methods Applied

### Inferential Tests
| Test | Research Question | Key Result |
|---|---|---|
| One-sample t-test | Does MD hospital capacity differ from 100-bed benchmark? | t(60)=4.13, p<0.0001, Cohen's d=0.53 |
| Two-sample t-test (pooled) | Urban vs. rural capacity difference? | t(59)=2.76, p=0.0078, Cohen's d=0.76 |
| Welch's t-test (robustness) | Same, unequal variances | t≈2.64, p=0.0111 |
| One-way ANOVA | Capacity differences by hospital type? | F(4,56)=0.545, p=0.704 (ns) |
| Kruskal-Wallis | Non-parametric alternative to ANOVA | H=5.23, p=0.264 (ns) |
| Chi-square | Hospital type vs. urban/rural location | χ²(4)=5.273, p=0.260 (ns) |
| Fisher's Exact Test | Same, appropriate for small expected counts | p=0.196 |
| Multiple Linear Regression | Predictive model of hospital capacity | F(6,54)=2.39, p=0.040, R²=0.210 |

### Assumption Diagnostics
- **Normality:** Shapiro-Wilk test (W=0.85, p<0.001 — violated; CLT invoked for t-tests with n>20)
- **Homoscedasticity:** Levene's test (p=0.142 for two-group comparison — satisfied); Breusch-Pagan (χ²=1.30, p=0.767 for MLR — satisfied)
- **Autocorrelation:** Durbin-Watson statistic = 1.92, p=0.42 — no significant autocorrelation
- **Multicollinearity:** All VIF values < 2.0

### Effect Sizes Reported
- Cohen's d for t-tests
- Cramer's V for chi-square (V=0.287, medium association)
- R² and Adjusted R² for regression

---

## Repository Structure

```
Maryland-Healthcare-Equity/
├── DATA_608_Notebook.ipynb       # Full analysis notebook (EDA, statistical tests, geospatial)
├── DATA608_FinalReport.pdf       # 32-page manuscript-style research report
├── DATA608_Presentation.pdf      # Slide deck presented to class
├── README.md
└── data/
    └── maryland_hospitals.csv    # Maryland Department of Health licensing data
```

---

## Setup

```bash
git clone https://github.com/stutiupadhyay03/Maryland-Healthcare-Equity.git
cd Maryland-Healthcare-Equity
pip install pandas numpy matplotlib seaborn scipy statsmodels geopandas folium
jupyter notebook DATA_608_Notebook.ipynb
```

---

## Analysis Structure (Notebook)

**Part 1 — Exploratory Data Analysis**
- Univariate analysis: license capacity distribution, hospital size categories
- Bivariate analysis: capacity by hospital type, capacity by county
- County-level aggregation: hospital counts, total beds, average capacity
- Geospatial mapping: hospital locations by capacity level and type

**Part 2 — Statistical Modeling and Inference**
- One-sample t-test against 100-bed national benchmark
- Two-sample t-test (urban vs. rural) with Levene's variance test and Welch's robustness check
- One-way ANOVA across hospital types with Kruskal-Wallis non-parametric alternative
- Multiple linear regression with full assumption diagnostics
- Contingency table analysis (chi-square and Fisher's Exact)

**Part 3 — Geospatial Analysis**
- Interactive Folium map of hospital locations by capacity level
- Healthcare desert identification using spatial clustering
- Drive-time buffer analysis (15-minute threshold)

---

## Key Visualizations

- Distribution of hospital capacity (histogram, box plot, bar chart)
- Urban vs. rural capacity comparison (box plot with outliers)
- Hospital capacity by facility type (grouped bar chart)
- Number of hospitals and total licensed beds by county (horizontal bar charts)
- Average beds per hospital by county
- Geospatial map of Maryland hospitals by capacity level and type
- Heatmap of hospital density across the state
- MLR residual histogram, residuals vs. fitted, and Q-Q plot

---

## Conclusions

**Primary finding:** Urban hospitals in Maryland have significantly higher licensed bed capacity than rural hospitals (254.9 vs. 121.9 beds, p=0.0078, Cohen's d=0.76). Urban location predicts approximately 206 additional beds in multivariate analysis.

**Secondary finding:** No statistically significant differences in capacity across hospital types (ANOVA p=0.704; Kruskal-Wallis p=0.264), likely reflecting low statistical power rather than true equality.

**Tertiary finding:** Available administrative variables explain only 21% of the variance in hospital capacity (R²=0.210), suggesting important missing predictors, including population served, teaching hospital status, and ownership type.

**Policy implication:** Geographic factors should be weighted more heavily than facility type in resource allocation. The 1,404-fold capacity disparity between Baltimore City and Somerset County represents a systemic inequality requiring targeted intervention in rural and underserved regions.

---

## Limitations

- Cross-sectional single-timepoint analysis (cannot assess trends or causality)
- Binary urban-rural classification obscures within-category gradients
- Three hospital types have n=2 observations, severely limiting ANOVA power
- Normality assumptions violated throughout; parametric tests invoked CLT robustness
- 15-minute drive-time service areas use Euclidean buffers, not road-network isochrones

---

## Future Extensions

- Merge with Census population and income data for socioeconomic stratification
- Road-network isochrone analysis (OpenRouteService) for true drive-time coverage
- Longitudinal analysis tracking capacity changes over time
- Incorporation of ED visit volumes, occupancy rates, and health outcomes
- Vulnerability index combining capacity, distance, and population need

---

## Stack

`Python` · `Pandas` · `NumPy` · `Matplotlib` · `Seaborn` · `SciPy` · `Statsmodels` · `GeoPandas` · `Folium` · `Jupyter`
