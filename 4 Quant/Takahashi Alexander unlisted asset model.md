## Summary

The TA model is a simple and deterministic model to predict cashflows from unlisted asset classes such as private equity funds.

The model has 3 main inputs:
1. Asset growth rate - How quickly the value of invested funds grows
2. Contribution rate - The % of remaining committed capital that is invested each time period, usually varies by year of fund life
3. Distribution rate - The % of the investment value that is distributed back to investors each time period, usually varies by year of fund life

While growth rate and contribution rate are given inputs, distribution rate is determined by a formula:

D = MAX(yield, (Age / fund lifespan)^Bow).

Where yield and bow are selected based on fund characteristics. 

## Extensions

This model is extended in work by PGIM in [[Modeling Private Investment Cash Flows with Market-Sensitive Periodic Growth (2021)]] to incorporate variable growth rates depending on listed market condition.

In my work at JANA, I allowed both contribution rate and distribution rate to vary with time, being based on historical averages by asset class, which provided a more explainable set of results. 