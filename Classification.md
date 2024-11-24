## K Nearest Neighbor (KNN)
- Supervised learning
- "Lazy Learner" - Allows the model to have data added to it without having to recompute the entire model

Defines a distance between each object and every other object. Then it assigns the classification of the nearest neighbor to the sample. Points are labeled on the majority classification of the k nearest neighbors where k is a number

| Pros                            | Cons                                      |
| ------------------------------- | ----------------------------------------- |
| Allows On-Demand Classification | Need to store the entire training dataset |

## Distance Metrics
Provide a measure of distance from a data a point to its nearest neighbors

*Axioms*
1) $d \ge 0$
2) $d(x_1,x_2, x_3) > d(x_1, x_3)$
3) $d(x_1,x_1)$
4) 

