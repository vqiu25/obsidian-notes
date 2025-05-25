> [!QUOTE] Quick Notes
> Common graph terminology

# Overview

>[!Note]- What is a Tree?
> <!-- Multiline -->
> A tree is an undirected graph with:
> * $n$ vertices
> * $n - 1$ edges
> 
> With no cycles.

>[!Note]- What is a Forest?
> <!-- Multiline -->
> Graph with multiple trees. As such, the total number of edges in a tree is:
> * If there are $C$ connected components
> * Each component has $n_i$ vertices
> * The total number of vertices is $n = n_1 + n_2 + n_3 + n_C$
> 
> Thus, the total number of edges in the forest is:
> $$(n_1 - 1) + (n_2 - 1) + (n_3 - 1) + ... + (n_C - 1)$$
> $$(n_1 + n_2 + n_3 + ... + n_C) - C = n - C$$

>[!Note]- How to find Connected Components
> <!-- Multiline -->

#flashcards/dsa/patterns/graphterms