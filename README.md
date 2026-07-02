# Chicago Crime Time Series Analysis (2001–2022)

Portfolio Project 3, Part 1. Time series analysis of Chicago crime data, built to answer stakeholder questions for a local newspaper reporter.

## Data

Source: Chicago Data Portal, Crimes 2001 to Present. Provided as a zip of per-year CSVs, loaded and concatenated into one DataFrame indexed by datetime. Two forms of the data are kept: the original per-event data (one row = one crime) and a daily resampled version (one row = one day) used for the seasonality analysis.

## Stakeholder Q&A

**Q1: Which district had the most crimes in 2022? Which had the least?**
District 8 had the most, 14,800 incidents. District 20 had the least at ~4,900 (District 31 excluded, it's an administrative code, not a patrol district).
Plot: `district_crime_2022.png`

**Q2: Is total crime increasing or decreasing across the years?**
Decreasing overall, down 55%+ from 2001 to 2021. Reversed in 2022, ticked back up to ~240K.
Plot: `total_crime_trend_2001_2022.png`

**Q3: Are crimes more common during AM or PM rush hour?**
PM. Every top crime category is higher in the evening window than the morning one.
Plot: `rush_hour_top5_comparison.png`

**Q4: Is Motor Vehicle Theft more common AM or PM?**
PM: 53,716 vs 41,578 AM.
Plot: `motor_vehicle_theft_am_vs_pm.png`

**Q5: What months have the most/least crime?**
Most: July (~715K). Least: February (~530K).
Plot: `total_crime_by_month.png`

**Q6: Any crime types that don't follow the monthly pattern?**
Yes. Gambling spikes hard May-Sep. Offense Involving Children spikes in January instead of summer.
Plot: `monthly_distribution_heatmap.png`

**Q7: Top 3 holidays for crime?**
New Year's Day (32,725), Independence Day (22,672), Labor Day (22,164).
Plot: `top3_holidays.png`

**Q8: Top 5 crimes on each of those holidays?**
New Year's: Theft, Battery, Criminal Damage, Deceptive Practice, Offense Involving Children.
Independence Day: Battery, Theft, Criminal Damage, Assault, Narcotics.
Labor Day: Battery, Theft, Criminal Damage, Narcotics, Assault.
Plots: `top5_crimes_new_years.png`, `top5_crimes_independence_day.png`, `top5_crimes_labor_day.png`

**Q9: What cycles exist in the data? How long, what magnitude?**
Two cycles. Weekly (7 days): peak near weekend (+50), trough midweek (-48). Annual (365 days): peak summer, trough winter, swing of about ±140/day.
Plots: `seasonal_decomposition_annual.png`, `seasonal_decomposition_weekly.png`
## Methods

- `pandas` for datetime indexing, resampling, groupby aggregation
- `statsmodels.tsa.seasonal_decompose` for trend/seasonal decomposition (period=7 and period=365)
- `holidays` package for US holiday mapping
- Linear regression slope (`np.polyfit`) to detect crime types moving against the overall trend
- Correlation of normalized monthly distributions to detect crime types with a different seasonal shape

## Top 5 Plots (rename these on your laptop)

These five best show the range of skills: business framing, trend detection, categorical comparison, and time series decomposition.

1. `district_crime_2022.png` — total crimes by police district, 2022
2. `total_crime_trend_2001_2022.png` — yearly total crime trend, 2001–2022
3. `rush_hour_top5_comparison.png` — top 5 crimes, AM vs PM rush hour
4. `seasonal_decomposition_annual.png` — observed/trend/seasonal decomposition, 365-day period
5. `seasonal_decomposition_weekly.png` — weekly seasonal component, 7-day period

## Repo Structure

```
├── Chicago_Crime_Time_Series_Part1.ipynb
├── README.md
└── plots/
    ├── district_crime_2022.png
    ├── total_crime_trend_2001_2022.png
    ├── rush_hour_top5_comparison.png
    ├── seasonal_decomposition_annual.png
    └── seasonal_decomposition_weekly.png
```

## Limitations

- District/Ward/Location fields have missing values, excluded only from analyses that need them.
- Primary Type reflects how a crime was classified, not confirmed outcome.
- Holiday matching uses calendar date only, not observed/shifted dates.
