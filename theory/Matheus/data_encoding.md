# Data Encoding



## Label Encoding

Label encoding involves converting each unique category in a categorical variable into an integer. The integers are assigned based on the order of appearance. For example, if you have a categorical variable like "Red," "Green," and "Blue," label encoding would convert them to 0, 1, and 2, respectively. Label encoding is appropriate for ordinal data, where the categories have a meaningful order.

Advantages:
- Simple and easy to implement.
- Preserves the ordinal relationship between categories if it exists in the data.

Disadvantages:
- Can introduce unintended ordinal relationships between categories, which may not be suitable for certain machine learning algorithms.
- Not suitable for nominal data (categories with no inherent order) because it may mislead the algorithm by assigning arbitrary numerical values.


## One-Hot Encoding

One-hot encoding creates binary columns for each category in the original variable. For each category, a new binary column (also known as a "dummy variable") is created, where a 1 indicates the presence of that category, and 0 indicates absence. Using the previous example with "Red," "Green," and "Blue," one-hot encoding would create three columns, one for each color.

Advantages:
- Preserves the independence of categories (no arbitrary ordering).
- Suitable for nominal data, where categories have no inherent order.
- Works well with most machine learning algorithms.

Disadvantages:
- Increases the dimensionality of the dataset, which can be a problem for large categorical variables with many unique categories.