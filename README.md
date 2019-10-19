# poi_cluster
Extracting point of interest(POI) data from openstreetmap and then clustering them to find commercial markets/centers in a region.

## Tools and Libraries needed to run:
Python, matplotlib, numpy, pandas, geopandas, geopy, folium, shapely, smopy, fiona, sklearn, overpy

## Methodology:
### Data Extraction:
First, all the points of interests(POI) for commercial market/center are extracted. I assumed that a commercial market will only have shops as their POIs. I extracted all the nodes for the Delhi region with the key shop using the overpass api for python (overpy). Now there will be some commercial markets in the form of ways. I assumed that every commercial market has a way with the key shop=mall. Certain markets are left out due to the fact that all tags can't be searched through in a short time.
### Clustering the nodes:
I chose DBSCAN (Density based spatial clustering of application with noise) because density clustering algorithms use the concept of reachability i.e. how many neighbors has a point within a radius and also we didn't know how many clusters will be formed from the dataset. So I've chosen the minimum number of samples in a cluster to be 5 and the reachability to be 100 metres. I got 60 clusters for the area that I chose for Delhi. I decided that if a cluster has more than 10 nodes then it must be significant.
### Creating polygons:
Using shapely I created convex_hull polygons for each of the cluster. For some clusters, the polygon for the ways and the cluster interesected. So the cluster polygons were removed as they were a superset of the way polygons.
### Deciding labels for clusters:
For the way, there is a name tag available for each way in the OSM database which was used. For clusters, reverse geocoding using geopy was done of the centermost point of the cluster to get the address. From the address the 1st street name was extracted and that name was given to the markets. Labeling is only done for the significant clusters and is left blank for the clusters with less than 10 nodes and the ways with no name tag.
### Visualization and saving to shapefile:
The geometry and the name of the polygons (clusters) are stored in a shapefile 'polygon.shp' and geojson file 'polygon.shp' using geopandas. For interative visualization folium is used which uses OSM maps. Interaction feature of clicking on polygon and getting name of the market and cluster is added.
