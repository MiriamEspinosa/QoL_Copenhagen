# GeoSpatial_DS
Source code for Geospatial Data Science - Exam Project, Spring 2026

## Contributors 
Caroline Sofie Skovby (cssk@itu.dk) & Miriam Espinosa Solana (miri@itu.dk)

## Environment 
We are using a pixi environment. 

## Data folder structure

The `data/` directory contains raw, intermediate, and cleaned geospatial and tabular datasets used in the project. The data will not be added to the final repository. 

    data/
    ├── buildings_copenhagen/
    │   ├── buildings.*
    │   └── buildings_within_city.*
    │
    ├── buildings_denmark/ # extracted from geoFabrik
    │   └── gis_osm_buildings_a_free_1.*
    │
    ├── clean_data/
    │   ├── accommodation.gpkg
    │   ├── air_quality.gpkg
    │   ├── buildings.gpkg
    │   ├── bus_stops.gpkg
    │   ├── clinic.gpkg
    │   ├── green_areas.gpkg
    │   ├── hospital.gpkg
    │   ├── qol_index.gpkg
    │   ├── schools.gpkg
    │   ├── train_stations.gpkg
    │   ├── transport.gpkg
    │   └── voting_areas.gpkg
    │
    ├── copenhagen_boundaries/
    │   └── boundaries.*
    │
    └── raw_data/
        ├── CAV_25May2021.geojson
        ├── employment.xlsx
        ├── population_district.xlsx
        ├── unemployment.xlsx
        └── voting_areas.gpkg


## Results 
### QoL Index 
![alt text](QoL.png)

![alt text](3.png)

The three different choropleth maps in Figure 2, show the QoL index from different granularity levels. The fist one (a), plots the results obtained looking at each building. The second one (b), shows the mean values after aggregating based on the hexagonal grid. The last one (c), shows the aggregation based on voting areas from Copenhagen and Frederiksberg municipalities. This index is ranged from 1 to 5, where 5 represents a higher QoL index. 

We observe higher scores in the central areas. Despite being spatially close, there is a notable difference between Frederiksberg and the western area of Copenhagen. We can also observe a high value in the center of Amager. We see that aggregating based on voting area obfuscates the pattern of gradual change between areas that we observe in the other two aggregation levels.

In order to understand better how this values were obtained, we have analyzed the score for each of the distance metrics separately (see Figure \ref{fig_indicators}). Based on the combination of both analysis, we can see a correlation between areas close to a hospital and a high QoL score. It is also relevant to notice that the north west area of Copenhagen, not only does not have a hospital close, but also is far from clinics, train stations and schools. This explains the results obtained and the difference between this area and the city center. 

### Global Spatial Autocorrelation 
![alt text](4.png)

We use Moran's I to investigate whether there is a significant spatial pattern in the QoL indices.

In order to do so, we first compute the spatial weights of each hexagon, which is an indicator of every adjacent hexagon.
We used rook contiguity to define neighbors, although with the hexagonal grid, queen contiguity would have had the same effect, as there can never be an instance of two hexagons sharing only one point. 

The spatial weights were standardized before 
calculating the spatial lag of the hexagons. We also standardized the QoL indices, and calculated a standardized spatial lag. Based on these calculations, we computed Moran's I and plotted the relation between the standardized QoL indices and their spatial lag (see Figure 4).

Moran's I summarizes the distribution for a spatial dataset, indicating if the spatial autocorrelation is positive, negative or random, and it also corresponds to the slope of the linear fit on the Moran plot. With a Moran's I of 0.83, we see an indication of positive autocorrelation, which means that areas with similar QoL indices tend to appear next to each other. With a p-value of 0.001, we can also assume that the pattern we see is not due to random chance.


### Local Spatial Autocorrelation 

![alt text](5.png)

Based on the results from the global spatial autocorrelation, where we observed a pattern in how QoL indices are clustered in space,
we look into clusters' locations and whether they are significant on a local level.

Figure 5 shows the kernel density distribution of the local Moran's I for all the hexagons. 
We see that there is a high peak around 0.5, a smaller peak around 3.75 and a long right tail.
Therefore, the majority of hexagons have a low local Moran's I, meaning they do not have a strong spatial autocorrelation with their neighbors. The second peak shows that there is a group of hexagons with higher local Moran's I, implying that, while there are many uncorrelated hexagons, there is also a considerable amount of hexagons that are strongly correlated.

To further investigate this, we used Local Indicators of Spatial Association (LISA), which delegates the hexagons into one of four quadrants: high values surrounded by high values (HH), low values surrounded by low values (LL), high values surrounded by low values (HL), and low values that are surrounded by high values (LH). 

![alt text](6.png)

## Discussion and Conclusion

The results from the QoL Index visual inspection and the Local Spatial Autocorrelation show a pattern in which areas in the center of the the city are ranked higher, while peripheral zones tend to have lower values. These high ranked areas included Frederiksberg and northern part of Amager. We attribute this to the proximity of these areas to hospitals, an amenity that has a high weight in our model and a low number of samples. It is noteworthy that, despite limiting the coast line, Hellerup is also classified as high QoL. This area is ranked in the top total income in Copenhagen. This idea links to the papers discussed in the Section 2 and suggest a possible link between economic factors and access to amenities. 

Based on the visual inspection of the residential buildings on the map, we can see that some areas might be missing buildings, creating unjustified holes in the hexagonal grid. This pattern can be observed in Frederiksberg, with several buildings that appear to be in residential areas and are not tagged as such, which explains why Frederiksberg looks so sparse on the figures. This highlights the limitation of using open data and the impact that it can have in predictions and modelling. 

Ideally and based on the concept of QoL, we would have liked to included socio-economic data, but we were not able to find a dataset with spatial units that were small enough to fit with the rest of the data. The danish government publishes socio-economic data updated every quarter. However, the lower granularity is based on districts, including a total of 23 for Copenhagen municipality. Using this would lead to a mismatch between them and the scale of the 839 hexagons, which would cause a modifiable areal unit problem (MAUP). There is already a risk of this happening for any type of aggregation. Either way, given that the buildings are irregular polygons that do not lie on the network graph and they are separated by streets with different widths, defining a grid was necessary to be able to define the neighbors. We have chosen hexagons due to their geometric properties, defining two hexagons as neighbors if they share an edge. 

We considered basing the LISA analysis on voting area aggregation. However, that resulted in an uneven distribution of surface area per voting area and Amager becoming a disconnected component. On the other hand, the hexagonal grid allows areas that are close while separated by water to still be considered neighbors. We also acknowledge that hexagons that have buildings on opposing sides of canals or lakes can have more variance in their shortest distances to amenities, since there are fewer direct paths between them. This would mean that the hexagons do not have a uniform distribution among themselves. This is a challenge when working with a city like Copenhagen, where the amount of canals and water influences mobility. A better approach could have been taking into account the location of bridges between the different areas.

The proxy selected for QoL index was heavily influenced by Salvati et al. (2025). However, given the lack of data on some of the indicators, we had to adapt the weights that they propose. This modification lead to a model that prioritizes proximity to hospitals and air quality over other indicators. Understanding that these weights were designed for a different society and the impact of the reduced indicators, lead us to conclude that maybe the proxy selected was not suitable. Among other methods, we explore the option of computing our own weights, but giving the lack of expertise and the scope of the project, we decided to use the classification table that they propose.

Although our approach is focused in Copenhagen and Frederiksberg, the methodology of using hexagonal grids and spatial proxies for QoL is transferable to other cities with similar open data availability. Nevertheless, the model’s performance depend heavily on local context and data completeness, and the weights should be recalculated for different cities.


## References 
Seyedeh Mahsa Salavati, Milad Janalipour, and Nadia Abbaszadeh Tehrani. Measuring urban quality of life through spatial analytics and machine learning: A data-driven framework
for sustainable urban planning and development. Sustainability, 17(11):4863, 2025.