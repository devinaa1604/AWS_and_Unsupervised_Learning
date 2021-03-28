# AWS Cloud and Unsupervised Learning

## Clustering Crypto

![Cryptocurrencies coins](Images/cryptocurrencies-coins.jpg)
_[Cryptocurrencies coins by Worldspectrum](https://www.pexels.com/@worldspectrum?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) | [Free License](https://www.pexels.com/photo-license/)_

### Background

Generated analysis of cryptocurrencies are available on the trading market and how they can be grouped using classification. To do this I used unsupervivsed learning and Amazon SageMaker by clustering cryptocurrencies and creating plots to present results.

Main Tasks:

* **[Data Preprocessing](#Data-Preprocessing):** Prepare data for dimension reduction with PCA and clustering using K-Means.

* **[Reducing Data Dimensions Using PCA](#Reducing-Data-Dimensions-Using-PCA):** Reduce data dimension using the `PCA` algorithm from `sklearn`.

* **[Clustering Cryptocurrencies Using K-Means](#Clustering-Cryptocurrencies-Using-K-Means):** Predict clusters using the cryptocurrencies data using the `KMeans` algorithm from `sklearn`.

* **[Visualizing Results](#Visualizing-Results):** Create some plots and data tables to present your results.

* **[Optional Challenge](#Optional-Challenge):** Deploy notebook to Amazon SageMaker.

---
### Data Preprocessing

Load the information about cryptocurrencies and perform data preprocessing tasks.

1. Using the `CSV` file, created a `Path` object and read the file data directly into a DataFrame named `crypto_df` using `pd.read_csv()`.

2. Using the `requests` library, retreive the necessary data from the following API endpoint from _CryptoCompare_ - `https://min-api.cryptocompare.com/data/all/coinlist`.

With the data loaded into a Pandas DataFrame, continue with the following data preprocessing tasks.

3. Keep only the necessary columns: 'CoinName','Algorithm','IsTrading','ProofType','TotalCoinsMined','TotalCoinSupply'

4. Keep only the cryptocurrencies that are trading.

5. Keep only the cryptocurrencies with a working algorithm.

6. Remove the `IsTrading` column.

7. Remove all cryptocurrencies with at least one null value.

8. Remove all cryptocurrencies that have no coins mined.

9. Drop all rows where there are 'N/A' text values.

10. Store the names of all cryptocurrencies in a DataFrame named `coins_name`, use the `crypto_df.index` as the index for this new DataFrame.

11. Remove the `CoinName` column.

12. Create dummy variables for all the text features, and store the resulting data in a DataFrame named `X`.

13. Use the [`StandardScaler` from `sklearn`](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html) to standardize all the data of the `X` DataFrame.

### Reducing Data Dimensions Using PCA

Used the [`PCA` algorithm from `sklearn`](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html) to reduce the dimensions of the `X` DataFrame down to three principal components.

After reducing the data dimensions, created a DataFrame named `pcs_df` using as columns names `"PC 1", "PC 2"` and `"PC 3"`;  used the `crypto_df.index` as the index for this new DataFrame.

Final DataFrame looks like:

![pcs_df](Images/pcs_df.png)

### Clustering Cryptocurrencies Using K-Means

Used the [`KMeans` algorithm from `sklearn`](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html) to cluster the cryptocurrencies using the PCA data.

Performed the following tasks:

1. Created an Elbow Curve to find the best value for `k` using the `pcs_df` DataFrame.

2. After defining the best value for `k`, ran the `Kmeans` algorithm to predict the `k` clusters for the cryptocurrencies data. Used the `pcs_df` to run the `KMeans` algorithm.

3. Created a new DataFrame named `clustered_df`, that includes the following columns `"Algorithm", "ProofType", "TotalCoinsMined", "TotalCoinSupply", "PC 1", "PC 2", "PC 3", "CoinName", "Class"`. Maintained the index of the `crypto_df` DataFrames as is shown bellow.

    ![clustered_df](Images/clustered_df.png)

#### Visualizing Results

Created some data visualization to present the final results. Performed the following tasks:

1. Created a 3D-Scatter using Plotly Express to plot the clusters using the `clustered_df` DataFrame. Included the following parameters on the plot: `hover_name="CoinName"` and `hover_data=["Algorithm"]` to show this additional info on each data point.

2. Used `hvplot.table` to create a data table with all the current tradable cryptocurrencies. The table has the following columns: `"CoinName", "Algorithm", "ProofType", "TotalCoinSupply", "TotalCoinsMined", "Class"`

3. Created a scatter plot using `hvplot.scatter`, to present the clustered data about cryptocurrencies having `x="TotalCoinsMined"` and `y="TotalCoinSupply"` to contrast the number of available coins versus the total number of mined coins. Used the `hover_cols=["CoinName"]` parameter to include the cryptocurrency name on each data point.

### Additional Work

Uploaded the Jupyter notebook to Amazon SageMaker and deployed it.

The `hvplot` and Plotly Express libraries are not included in the built-in anaconda environments, used the `altair` library instead.

Performed the following tasks:

1. Uploaded the Jupyter notebook and renamed it as `crypto_clustering_sm.ipynb`

2. Selected the `conda_python3` environment.

3. Installed the `altair` library by running the following code before the initial imports.

  ```python
  !pip install -U altair
  ```

4. Useed the `altair` scatter plot to create the Elbow Curve.

5. Used the `altair` scatter plot, instead of the 3D-Scatter from Plotly Express, to visualize the clusters. Since this is a 2D-Scatter, use `x="PC 1"` and `y="PC 2"` for the axes, and added the following columns as tool tips: `"CoinName", "Algorithm", "TotalCoinsMined", "TotalCoinSupply"`.

6. Used the `altair` scatter plot to visualize the tradable cryptocurrencies using  `x="TotalCoinsMined"` and `y="TotalCoinSupply"` for the axes.

7. Showed the table of current tradable cryptocurrencies using the `display()` command.
