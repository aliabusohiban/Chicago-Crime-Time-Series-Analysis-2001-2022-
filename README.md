# Chicago Crime Time Series Analysis (2001-2022)

Portfolio Project 3, Part 1. Time series analysis of Chicago crime data, built to answer stakeholder questions for a local newspaper reporter.

## Data

Source: Chicago Data Portal, Crimes 2001 to Present. Provided as a zip of per-year CSVs, loaded and concatenated into one DataFrame indexed by datetime. Two forms of the data are kept: the original per-event data (one row = one crime) and a daily resampled version (one row = one day) used for the seasonality analysis.

## Methods

- `pandas` for datetime indexing, resampling, groupby aggregation
- `statsmodels.tsa.seasonal_decompose` for trend/seasonal decomposition (period=7 and period=365)
- `holidays` package for US holiday mapping
- Linear regression slope (`np.polyfit`) to detect crime types moving against the overall trend
- Correlation of normalized monthly distributions to detect crime types with a different seasonal shape

## Stakeholder Q&A

**Q1: Which district had the most crimes in 2022? Which had the least?**

District 8 had the most, 14,800 incidents. District 20 had the least at ~4,900 (District 31 excluded, it's an administrative code, not a patrol district).

![District Crime 2022](plots/district_crime_2022.png)

**Q2: Is total crime increasing or decreasing across the years?**

Decreasing overall, down 55%+ from 2001 to 2021. Reversed in 2022, ticked back up to ~240K.

![Total Crime Trend 2001-2022](plots/total_crime_trend_2001_2022.png)

**Q3: Are crimes more common during AM or PM rush hour?**

PM. Every top crime category is higher in the evening window than the morning one.

![Rush Hour Top 5 Comparison](plots/rush_hour_top5_comparison.png)

**Q4: Is Motor Vehicle Theft more common AM or PM?**

PM: 53,716 vs 41,578 AM.

![Motor Vehicle Theft AM vs PM](plots/motor_vehicle_theft_am_vs_pm.png)

**Q5: What months have the most/least crime?**

Most: July (~715K). Least: February (~530K).

![Total Crime by Month](plots/total_crime_by_month.png)

**Q6: Any crime types that don't follow the monthly pattern?**

Yes. Gambling spikes hard May-Sep. Offense Involving Children spikes in January instead of summer.

![Monthly Distribution Heatmap](plots/monthly_distribution_heatmap.png)

**Q7: Top 3 holidays for crime?**

New Year's Day (32,725), Independence Day (22,672), Labor Day (22,164).

![Top 3 Holidays](plots/top3_holidays.png)

**Q8: Top 5 crimes on each of those holidays?**

New Year's: Theft, Battery, Criminal Damage, Deceptive Practice, Offense Involving Children.
Independence Day: Battery, Theft, Criminal Damage, Assault, Narcotics.
Labor Day: Battery, Theft, Criminal Damage, Narcotics, Assault.

![Top 5 Crimes New Year's Day](plots/top5_crimes_new_years.png)
![Top 5 Crimes Independence Day](plots/top5_crimes_independence_day.png)
![Top 5 Crimes Labor Day](plots/top5_crimes_labor_day.png)

**Q9: What cycles exist in the data? How long, what magnitude?**

Two cycles. Weekly (7 days): peak near weekend (+50), trough midweek (-48). Annual (365 days): peak summer, trough winter, swing of about ±140/day.

![Seasonal Decomposition Annual](plots/seasonal_decomposition_annual.png)
![Seasonal Decomposition Weekly](plots/seasonal_decomposition_weekly.png)

## Key Findings

- Crime fell over 55% from 2001 to 2021, then rose again in 2022. The decline reversed.
- District 8 has the most crime in 2022. District 20 is the real lowest.
- PM rush hour beats AM rush hour across every major category, including motor vehicle theft.
- July is the peak month, February the lowest. Gambling and Offense Involving Children don't follow the summer-peak pattern.
- New Year's Day is the highest-crime holiday, ahead of Independence Day and Labor Day.
- Crime repeats on two reliable cycles: 7 days and 365 days.

## Repo Structure

```
├── Chicago_Crime_Time_Series_Part1.ipynb
├── README.md
└── plots/
    ├── district_crime_2022.png
    ├── total_crime_trend_2001_2022.png
    ├── rush_hour_top5_comparison.png
    ├── motor_vehicle_theft_am_vs_pm.png
    ├── total_crime_by_month.png
    ├── monthly_distribution_heatmap.png
    ├── top3_holidays.png
    ├── top5_crimes_new_years.png
    ├── top5_crimes_independence_day.png
    ├── top5_crimes_labor_day.png
    ├── seasonal_decomposition_annual.png
    └── seasonal_decomposition_weekly.png
```

## Limitations

- District/Ward/Location fields have missing values, excluded only from analyses that need them.
- Primary Type reflects how a crime was classified, not confirmed outcome.
- Holiday matching uses calendar date only, not observed/shifted dates.
