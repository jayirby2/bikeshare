# Clustering Chicago Bike-Share Stations to Understand Rider Behavior

### Jay Irby

This project analyzes Chicago’s bike-share system to uncover station-level usage patterns. Using K-Means clustering (silhouette = 0.81), I grouped stations based on features like trip frequency, ride length, bike type ratios, 
and member usage. Visualization (t-SNE and geospatial mapping) revealed clear suburban vs. downtown patterns, with hotspots in the city center averaging 90+ trips per day. 
Statistical tests and Random Forest feature importance confirmed that total trips and average trips per active day were the strongest differentiators across clusters.

Download Analysis.ipynb to view code and visualizations.

## Analysis and Insights

KMeans identified 4 distinct clusters (silhouette = 0.81). The silhouette score portrays how similar a station/object is to its own cluster compared to other clusters. The score ranges from -1 to 1, so a score of 0.81 suggests that the station features contain strong, clear grouping patterns.

Kruskal–Wallis tests confirmed that differences in avg_trips_per_active_day and total_trips were highly statistically significant (p < 1e-160), indicating these variables are the primary drivers of station differences

A Random Forest classifier confirmed that total trips and average trips per hour together explain ~74% of the clustering variance.

Since two variables were incredibly imporant, with the other variables playing minor roles, each cluster was laregely defined by the two variables: avg_trips_per_active_day and total_trips.

Cluster 0 had the least amount of traffic, with roughly 2 trips per day averaged across all of the stations within this cluster. This makes total sense, as by looking at the map, all of these stations are located in the suburbs, extremely far away from city centers. Mostly automobiles are used here, and e-bikes and scooters are not as useful. Also, this cluster had the largest percentage of electric bike rides at a staggering 78.5%. This is likely due to the need to travel long distances in the suburbs. Also, cluster 0 has the longest average ride length at 2.9km. This cluster clearly shows the suburban stations and subsequent suburban rider behavior. 

Cluster 1, 2, and 3 all have significantly more traffic. Interestingly, the clusters are somewhat defined by how close they are to Chicago's main city center and downtown area. Cluster 2 has the second least amount of rides per day, and the stations within this closer are all further away from downtown. 

Cluster 3 is extremely unique. This cluster has very few stations (11 total), but the average number of trips per day among these stations is 93, which is 50 rides greater than the second busiest cluster. These stations are the hotspots; these stations could be large hubs where the bike companies maintain and place a large quantity of vehicles. These stations are also in the heart of downtown. There is a 50/50 split between members and casual riders. 

Cluster 1 is a medium between cluster 3 and 2. Cluster 1 stations are all located near the city center and have a lot of traffic. 67% of riders are members, so these stations are likely used by Chicago residents and workers. These riders likely use the service to commute to work or to leisure activities.

Seasonality does not play a huge role. This is probably due to the majrority (64%) of riders being members, so regular residents use the bikes far more often than tourists. 

## Technical Details and Methodology

### Feature Engineering

- Used Pandas functions and created my own functions to calculate certain features, and used apply() to create new feature columns.

- For most of the features, I calculated them on a ride-by-ride basis, and then averaged them out to aggregate on the station level. Then, I had average measurements of all the rides for each given station.

### KMeans

- I used the elbow method to determine the most optimal k value -- the number of clusters.

### tSNE

- I used tSNE to reduce the dimensionality of my df so I could visiualize my clusters in 2D. I saw very clear clustering and a variety in cluster sizes. 

- I had to standardize all of my variables since tSNE relies on euclidean distance measuremnts for embedding. Since all of my features had different scales, I needed to convert them all on to the same, standardized scale. I did this by using StandardScalar().

- I used a perplexity of 40, meaning t-SNE considered each station’s 40 nearest neighbors when constructing the low-dimensional embedding. In this process, t-SNE relies on Euclidean distance to measure similarity between stations, so stations with similar feature profiles are placed closer together in the 2D projection. Stations with more similar feature values (in the original high-dimensional space) are assigned a higher probability of being mapped close together in the 2D projection.

- Ultimately, I used tSNE because I had few enough rows/stations (~1400), and I wanted to capture non-linear relationships. 

### Random Forest

- I trained a Random Forest classifier using the cluster assignments as labels.

- The model was fit on the full dataset (no train/test split), since the goal was not prediction but understanding feature importance.

- By analyzing the trained model, I identified which features were most influential in determining the cluster assignments.

- This provides insight into which station characteristics contributed most strongly to the clustering results.
