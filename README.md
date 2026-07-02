# Chicago Crime Time Series Analysis (2001–2022)

Portfolio Project 3, Part 1. Time series analysis of Chicago crime data, built to answer stakeholder questions for a local newspaper reporter.

## Data

Source: Chicago Data Portal, Crimes 2001 to Present. Provided as a zip of per-year CSVs, loaded and concatenated into one DataFrame indexed by datetime. Two forms of the data are kept: the original per-event data (one row = one crime) and a daily resampled version (one row = one day) used for the seasonality analysis.

## Questions Answered

1. Which police district has the most/least crime (2022)
2. Is total crime increasing or decreasing over time, and which crime types buck the trend
3. AM vs PM rush hour crime comparison
4. Which months have the most/least crime, and which crime types don't follow that pattern
5. Which holidays see the most crime
6. What cycles (seasonality) exist in the data, weekly and annual

## Key Findings

- Crime fell over 55% from 2001 to 2021, then rose again in 2022. The decline reversed.
- District 8 has the most crime in 2022. District 31 is not a real patrol district (administrative code), District 20 is the real lowest.
- PM rush hour has more crime than AM rush hour across every major category, including motor vehicle theft.
- July is the peak month, February the lowest. Gambling and Offense Involving Children don't follow the summer-peak pattern.
- New Year's Day is the highest-crime holiday, ahead of Independence Day and Labor Day.
- Crime repeats on two reliable cycles: 7 days (weekend peak, midweek trough) and 365 days (summer peak, winter trough).

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
