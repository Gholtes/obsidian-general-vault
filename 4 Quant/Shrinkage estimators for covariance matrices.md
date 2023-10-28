## Summary

Shrinkage estimators aim to produce more useful covariance matrices by combining 2 estimates:
1. A unbiased but high error estimate, F, usually the [[sample covariance matrix]]
2. A biased but low error estimate, S

These are combined as a weighted sum, with the final matrix being given by:
`dS + (1-d)F`

## Choices for input matrices 

### Unbiased, high error input (F)
1. [[sample covariance matrix]]: This is the usual choice
2. [[Factor Models for Covariance Matrix Estimation]]: using a factor model can reduce the error in this matrix

### Biased, low error input (S):
1. [[Diagonal (Homoscedastic) Covariance Matrix]]: Zero correlation matrix.
2. [[Constant correlation covariance matrix]]: Single correlation between every pair of variables.
3. Factor Model: A factor model decomposes each asset into a smaller number of factors, with the weights on each factor and the factor covariance matrix being used to form the asset-level covariance matrix. See [[Factor Models for Covariance Matrix Estimation]].

## Choosing how to weight the inputs

## Specific named methods:

- [[Ledoit-Wolf Shrinkage Estimator]]: This estimator shrinks the [[sample covariance matrix]] towards a [[Diagonal (Homoscedastic) Covariance Matrix]], assuming that most of the correlations between variables are zero. It is particularly useful in high-dimensional settings.
- [[Factor Model Shrinkage]]: In this method, the [[Factor Models for Covariance Matrix Estimation]] is shrunk towards another matrix, with the [[Constant correlation covariance matrix]] showing good results.


## Papers

