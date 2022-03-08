Gram matrices compute the correlation between the outputs of every kernel in a CNN layer. Gram matrices are used by Gatys et. al to develop a measure of stylistic loss for the training of style transfer networks.

Computationally, gram matrices are calculated by flattening the convolutional output into a 2D array, with columns for each kernel and rows for every activation, A. The dot product of this array with its transpose is taken and output as the gram matrix, G. Here G(i,j) gives the correlation between the activations of kernels i and j. _Note that the term “correlation” is used rather loosely as this is not the statistically true correlation. Potentially the term cumulative co-activation would be more accurate._

Kernel combinations that show high correlation in their outputs are taken to be descriptive of the style of the image.

Sources: 
- [_A Neural Algorithm of Artistic Style_](https://arxiv.org/abs/1508.06576) by Gatys et. al
- [Developing Art Style Embeddings for Visual Similarity Comparison of Artworks](https://medium.com/towards-data-science/developing-art-style-embeddings-for-visual-similarity-comparison-of-artworks-7a9d4ade2045) by Grant Holtes

#computervision 