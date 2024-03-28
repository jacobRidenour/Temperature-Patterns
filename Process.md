* Dataset
  * Features:
    * `dt` - format year, month
    * `AverageTemperature` - average recorded temperature in degrees Celsius
    * `AverageTemperatureUncertainty` - uncertainty of corresponding `AverageTemperature` in degrees Celsius
    * `City` - name of the city
    * `Country` - country in which the corresponding `City` is located
    * `Latitude` - latitude of the corresponding `City` in format 0.00{N/S}
    * `Longitude` - longitude of the corresponding `City` in format 0.00{E/W}
* Exploratory Data analysis (EDA)
  * Missing Data
    * `AverageTemperature` and corresponding `AverageTemperatureUncertainty` - 11002 values
      * Get list of missing values:
        * Grouped by `City`, `Year`
        * Grouped by `City`
        * Grouped by `Year`
  * Unique values for `City`: 100
  * Unique values for `Country`: 49
  * Number of rows: 239177
  * Plot: graph `AverageTemperature` vs. `Year` for 5 random countries & include global `AverageTemperature` for each `Year` for comparison.
  * Plot: scatter plots of `AverageTemperature` vs. `{Numeric|Absolute} {Latitude|Longitude}`
    * Determine correlation coefficients:
      * `AbsLatitude`: -0.88376
      * `AbsLongitude`: -0.79830
      * `NumericLatitude`: -0.482300
      * `NumericLongitude`: -0.33504
  * Plot: bar graph of number of unique cities by year 
* Data preprocessing
  * Convert `dt` to python's DateTime format
  * Only include data from year 1880 and beyond
    * Side effect: missing data eliminated
  * Average the temperature for each year
    * Number of rows: 13380
  * Filter cities with < 100 data points
    * No change
  * Add `NumericLatitude` & `NumericLongitude` columns
    * For correlation analysis
  * Add `AbsLatitude` & `AbsLongitude` columns
    * For correlation analysis
* Dataset Enhancements
  * Add new features:
    * `Continent` - continent on which the corresponding `City` is located. Possible values: Africa, Asia, Europe, North America, Oceania, South America
      * Determine relative abundance of each continent
        * Asia: 7236
        * Africa: 2539
        * South America: 1327
        * Europe: 1072
        * North America: 938
        * Oceania: 268
      * Determine correlations between each unique continent and `AverageTemperature`
        * Asia: 0.83150
        * Africa: 0.867592
        * South America: 0.80614
        * Europe: 0.62185
        * North America: 0.70954
        * Oceania: 0.62906
    * `kcc` - Köppen climate classification of the corresponding `City` based on `NumericLatitude` and `NumericLongitude`.
      * Determine relative abundance of each climate classification:
        * Aw: 2003
        * BSh: 1742
        * Cfa: 1206
        * BWh: 1200
        * Ocean: 1199
        * BSk: 938
        * Cfb: 804
        * Dfb: 670
        * Csa: 536
        * Cwa: 536
        * Af: 402
        * Dwa: 402
        * Am: 402
        * Cwb: 268
        * As: 268
        * Dwb: 268
        * Dfa: 134
        * Bwk: 134
        * Csb: 134
        * ET: 134
      * Determine correlations between each unique kcc and `AverageTemperature`
        * Aw: 0.88020
        * BSh: 0.72504
        * Cfa: 0.77563
        * BWh: 0.62706
        * Ocean: 0.88366
        * BSk: 0.79700
        * Cfb: 0.70737
        * Dfb: 0.59481
        * Csa: 0.69059
        * Cwa: 0.80396
        * Af: 0.877053
        * Dwa: 0.68221
        * Am: 0.834221
        * Cwb: 0.70178
        * As: 0.76097
        * Dwb: 0.70900
        * Dfa: 0.48392
        * Bwk: 0.63488
        * Csb: 0.586208
        * ET: 0.68555
  * Preliminary Analysis
    * Find global correlation between `Year` and `AverageTemperature`.
      * Result: 0.877052
    * Plot: results of global correlation between `Year` and `AverageTemperature`.
* Data Mining
  * Create standardized (z-score) city profiles based on `Year` and `AverageTemperature`
  * Create Euclidean distance matrix and Dynamic Time Warping (DTW) distance matrix
    * Perform Multidimensional Scaling (MDS) on the DTW distance matrix
    * Plot: heatmap with the Euclidean distance matrix
    * Plot: heatmap with the DTW distance matrix
    * Find strongly correlated city pairs (2.5th percentile) by creating a Set out of the strongly correlated pairs from both distance matrices
    * Plot: pyvis network graph with the Set of strongly correlated city pairs
    * Cluster Mining
      * Plot: SSE vs. Number of Clusters (elbow method) and Silhouette Score vs. Number of Clusters (k-means)
        * Perform k-means clustering with the determined optimal number of clusters (k=3)
        * Plot: Visualize the clusters and their center points
      * For each cluster:
        * Plot: relative abundance of each Continent (compare with original non-clustered data)
          * Plot: heatmap with relative abundance of each Continent in each cluster relative to the overall dataset
        * Plot: relative abundance of each kcc (compare with original non-clustered data)
          * Plot: heatmap with relative abundance of each kcc in each cluster relative to the overall dataset
        * Plot: `Average Temperature` vs. `Year`
          * Correlation coefficients for each cluster:
            * Cluster 1: 0.79
            * Cluster 2: 0.68
            * Cluster 3: 0.66
        * Plot: Box and whiskers plots
          * Visualize geographical spread by latitude (N/S)
          * Visualize geographical spread by longitude (E/W)
    * 