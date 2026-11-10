# A/B Test Significance Analysis

## Task
Check whether the differences in conversion between the control and test groups in four A/B tests of an online store are statistically significant. Calculate significance for 4 conversion metrics per session (`add_payment_info`, `add_shipping_info`, `begin_checkout`, `new_account`), in total and by device, channel and continent, and prepare the results for a Tableau dashboard.

## What the analysis does
- Loads the event data (800,996 rows, 1 November 2020 – 27 January 2021) and checks its structure and missing values
- Cleans the data: replaces hidden missing values `(not set)` with `Unknown`, renames `new account` to `new_account`, converts the date to a date format and the test columns to text
- Counts sessions and events for the control group (1) and the test group (2) of each test
- Calculates the conversion rate of each metric (events divided by sessions)
- Runs a two-proportion z-test for each test and metric
- Repeats the calculation for the total and for slices by device, channel and continent using loops
- Saves all results to one table (240 rows) with conversion rates, change in %, z-statistic, p-value and a significance flag (p < 0.05)

## Techniques used
Python (`pandas`: `groupby`, `replace`, `to_datetime`; `numpy`), two-proportion z-test with Statsmodels (`proportions_ztest`), a custom function and loops for repeated calculations, export to CSV for Tableau

## Key findings (total results)
- Test 1: the test group had higher conversion in `add_payment_info` (+12.5%), `add_shipping_info` (+6.6%) and `begin_checkout` (+6.7%), and all three differences are significant; the change in `new_account` (-3.4%) is not significant
- Test 2: no significant changes in any of the four metrics
- Test 3: only `begin_checkout` changed significantly (-3.4%)
- Test 4: `begin_checkout` (-2.4%) and `new_account` (-3.4%) were significantly lower in the test group
- Test 1 looks the most promising and could be checked again on a bigger group; versions from tests 3 and 4 seem to work worse than the control

## Dashboard
[A/B test results dashboard on Tableau Public](https://public.tableau.com/app/profile/nastasiia.nelina/viz/AB-testing_17935414014350/AB-testDashboard?publish=yes)
