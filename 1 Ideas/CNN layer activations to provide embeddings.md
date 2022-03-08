Intermediate layer activations in CNNs can be used to provide image similarity embeddings. This is a simple method with a number of key limitations.

1. The embeddings can be extremely large
2. CNN layer activations preserve spacial information, so the same image that is offset spatially or resized will produce a very different embedding.
 
These limitations are reduced by using [[Gram matricies]] which use correlations between kernel activations to provide style embeddings that are less sensitive to spacial chances. 

#computervision 