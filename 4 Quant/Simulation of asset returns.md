The generation of asset returns is a subset of [[Monte Carlo simulation]], which has uses in:
1. [[Portfolio Optimisation]]
2. [[Portfolio performance forecasts]]
3. [[Asset allocation forecasts]]
4. [[Liquidity stress tests]]

Types of returns simulations:
- Normally distributed, arithmetic returns
- Normally distributed, geometric returns
- Log-normal returns
- Bespoke distributions - higher moments, arbitrary PDFs.
#### Adjustments to generated returns
The randomly generated returns may need to be adjusted in the following ways:
1. Adjust each year's returns so that the mean of the sample is exactly the requested mean, by adding a small delta to every observation.
2. If returns assumptions are geometric but the generated returns are arithmetic, adding a small amount to the means of years 2 onward to compensate for [[volatility drag]].

#### Inputs:
- [[Capital Market Assumptions]] or individual asset return assumptions
- Usually a covariance matrix is required - [[Covariance matrix estimation]]