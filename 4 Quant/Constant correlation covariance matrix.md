A covariance matrix where every off-diagonal component is scales so that the correlation matrix computed from this covariance matrix has a constant correlation between every pair of variables.

Calculated by:
1. Compute the [[sample covariance matrix]] and correlation matrix
2. Calculate the average correlation
3. Scale each off-diagonal component of the original covariance matrix so that every **correlation** is equal to the average computed in step 2.