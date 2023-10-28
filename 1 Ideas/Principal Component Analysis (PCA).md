Principal Component Analysis (PCA) is a widely used dimensionality reduction technique and a fundamental tool in multivariate statistics. 

Its primary purpose is to transform high-dimensional data into a lower-dimensional representation while retaining as much of the original data's variance as possible. Here's a summary of PCA:

1. **Dimensionality Reduction**: PCA is employed to reduce the number of features or dimensions in a dataset. It's particularly useful when dealing with high-dimensional data, as it simplifies the data while preserving essential information.
2. **Orthogonal Transformation**: PCA performs an orthogonal linear transformation, which means it creates new variables (principal components) that are uncorrelated with each other. These principal components are linear combinations of the original features.
3. **Variance Maximization**: The first principal component explains the most variance in the data, and each subsequent component explains the remaining variance. This ordering ensures that important information is retained in the lower-dimensional representation.
4. **Mathematical Formulation**: PCA finds the principal components by solving an eigenvalue problem on the data's covariance matrix. The principal components are the eigenvectors of this matrix. 
	1. This requires [[Covariance matrix estimation]].
5. **Dimension Reduction**: After calculating the principal components, you can select a subset of them to reduce the dimensionality of the data. Typically, most of the variance can be preserved by keeping only a fraction of the total number of components.
6. **Applications**: PCA is widely used in various fields, such as image processing, feature extraction, data compression, and pattern recognition. It's also employed in exploratory data analysis to reveal underlying structures in the data.
7. **Assumptions**: PCA assumes that the data is linearly related and that high-variance directions are more informative. It may not perform well if these assumptions are not met, so it's important to preprocess data and assess these assumptions beforehand.
8. **Interpretability**: While PCA reduces dimensionality, the resulting components may not be directly interpretable in terms of the original features. However, they capture patterns and relationships in the data.

## Estimation
Principal Component Analysis (PCA) is a mathematical technique used to reduce the dimensionality of data while preserving as much of the original data's variance as possible. PCA works by finding the principal components, which are linear combinations of the original features. Here's a mathematical explanation of how PCA works:

1. **Centering the Data**:
   First, the data is typically centered by subtracting the mean of each feature from each data point. This is done to ensure that the data is mean-centered, which is a common practice in PCA. 

2. **Covariance Matrix**:
   The next step is to calculate the covariance matrix of the centered data. The covariance matrix provides information about how features are correlated with each other. It is computed as the [[sample covariance matrix]].

3. **Eigendecomposition of Covariance Matrix**:
   PCA involves finding the eigenvectors and eigenvalues of the covariance matrix. The eigenvectors are the principal components, and the eigenvalues represent the variance explained by each principal component.

4. **Selecting Principal Components**:
   The principal components are typically ordered in descending order of their corresponding eigenvalues. This means that the first principal component explains the most variance, the second explains the second most, and so on. You can choose to keep a subset of these components to reduce the dimensionality of the data.

5. **Transforming Data**:
   To reduce the dimensionality of the data, you can project the original data onto the space defined by the selected principal components. The transformed data, denoted as \(Y\), is given by: Y = X * V_k

   where (V_k) is the matrix of the first (k) principal components (the ones you choose to retain) and X is the centred dataset.

PCA transforms the data into a new coordinate system defined by the principal components, and the amount of variance retained in the reduced-dimensional data depends on the number of principal components selected. Typically, a high percentage of the original data's variance is preserved by using only a subset of the principal components, making PCA a powerful tool for dimensionality reduction and data visualization.