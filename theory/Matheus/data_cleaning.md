# Data Cleaning
- outliers detection and removal (boxplots, manual removal with scatter plot, z-score, IQR, isolation forest, Local Outlier Factor (LOF))

## IQR

The IQR (interquartile range) method identifies outliers as the points that relies outside an interval. This interval is calculated as

<center>[Q25 - 1.5 * IQR, Q75 + 1.5 * IQR]</center>

where Qx is the x percentile and IQR is Q75 - Q25.

## Z-Score

The Z-Score method identifies outliers as the points that are far enough from the mean. To calculate this distance, it converts the data into a normal distribution. Then, commonly, the points below -3 or above 3 are removed.

Note that: in a normal distributino, the standard deviation is 1. $[-\sigma,\sigma]$, $[-2\sigma,2\sigma]$ and $[-3\sigma,3\sigma]$ encompass, respectively, 68.27%, 95.45% and 99.73% of the sample.

## Isolation Forest

Isolation forest is a machine learning algorithms that is used for anomaly detection. The main idea is to create a tree, similar to a decision tree, and identify, by the tree, points that are easily separable. How is this done?

The tree is partitioned by selecting a random feature and a random split value (between the minimum and maximum values). This is done until every point is divided (or until a predefined max depth). Since outliers are less frequent than normal observations, they should be closer to the root of the tree. In other words, outliers are points that are isolated from the rest of the data, meaning that they can be separated from the other points by only a small number of splits in the feature space. This is done multiple times in order to have an ensemble of trees.

After the forest is computed, an anomaly score is computed for each observation. A possible formula is

$$
\begin{aligned}
s(x,n) = 2^\frac{-E(h(x))}{c(n)}
\end{aligned}
$$

where $h(x)$ is the path length of observation x and $c(n)$ is the average path length of unsuccessful searches (this is quite complicate, check the paper if doubt) which can be computed as

$$
\begin{aligned}
c(n) = 2H(n-1) -(2(n-1)/n)
\end{aligned}
$$

where $H(i)$ is the harmonic number and can be estimated by $ln(i) + 0.577$ (Euler constant). $c(n)$ is used to normalise $h(x)$.
For an anomalous point, $E(h(x)) -> 0, s -> 1$. For a common point, $E(h(x)) -> n-1, s -> 0$. For a sample without clear anomalies, $E(h(x)) -> c(n), s -> 0.5$. 


Some problems with isolation forest are: it may not perform as well on highly dimensional data or data with highly correlated features, may require some parameter tuning to achieve optimal results and may misunderstand outliers as normal points in datasets with multiple clusters.

Links:
https://towardsdatascience.com/outlier-detection-with-isolation-forest-3d190448d45e

https://www.analyticsvidhya.com/blog/2021/07/anomaly-detection-using-isolation-forest-a-complete-guide/

https://cs.nju.edu.cn/zhouzh/zhouzh.files/publication/icdm08b.pdf

## Local Outlier Factor (LOF)

LOF is a machine learning algorithm for outlier detection. The main idea is that outlier points are characterised as points with low local density compared to its neighbors. To associate an outlier score to a point, some formulas are needed.

K-distance: the distance between a point and its Kth nearest neighbor.

K-neighbors: $N_k(A)$, a set with point that lies in the circle with radius K-distance.

Reachability density:
$$RD(X_i,X_j) = max(K-distance(K_j),distance(K_i,K_j))$$
is the K-distance if $X_i$ is a K-neighbor of $X_j$, else is their distance.

Local Reachability Density:
$$LRD_k(A) = \frac{1}{\sum_{X_j\in N_k(A)}\frac{RD(A,X_j)}{||N_k(A)||}}$$
is the inverse of the aerage RD of A from its neighbors. It can tell how far a point is from its nearest cluster.

Local Outlier Factor:
$$LOF_k(A) = \frac{\sum_{X_j \in N_k(A)}LRD_k(X_j)}{||N_k(A)||} \times \frac{1}{LRD_k(A)}$$
compares own LRD with the neighbors LRD. If a point is not an outlier, the value should be close to one.


In summary, the score is based on the ratio of the average density of the k-nearest neighbors of the point to its own density. Points that have a low density compared to their neighbors will have a high LOF score, indicating that they are likely to be outliers.

The LOF algorithm can be applied to high-dimensional data, and is relatively robust to the choice of distance metric and the number of nearest neighbors used to compute the score. However, it may not perform as well on datasets with highly skewed distributions or heavily overlapping clusters. Also, the threshold is dependent on the dataset.

https://towardsdatascience.com/local-outlier-factor-lof-algorithm-for-outlier-identification-8efb887d9843