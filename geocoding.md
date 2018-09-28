 GeoPandas 
0.4.0 
 
Getting Started
Installation
Examples Gallery
User Guide
Data Structures
Reading and Writing Files
Indexing and Selecting Data
Making Maps
Managing Projections
Geometric Manipulations
Set Operations with overlay
Aggregation with dissolve
Merging Data
Geocoding
Reference Guide
Reference to All Attributes and Methods
Developer
Contributing to GeoPandas
Docs » Geocoding 
View page source 

Geocoding
geopandas supports geocoding (i.e., converting place names to location on Earth) through geopy, an optional dependency of geopandas. The following example shows how to use the Google geocoding API to get the locations of boroughs in New York City, and plots those locations along with the detailed borough boundary file included within geopandas.
In [1]: boros = geopandas.read_file(geopandas.datasets.get_path("nybb"))

In [2]: boros.BoroName
Out[2]: 
0    Staten Island
1           Queens
2         Brooklyn
3        Manhattan
4            Bronx
Name: BoroName, dtype: object

In [3]: boro_locations = geopandas.tools.geocode(boros.BoroName, provider="google")

In [4]: boro_locations
Out[4]: 
                                geometry                       address
0         POINT (-74.1502007 40.5795317)        Staten Island, NY, USA
1         POINT (-73.7948516 40.7282239)               Queens, NY, USA
2  POINT (-73.94415789999999 40.6781784)             Brooklyn, NY, USA
3         POINT (-73.9712488 40.7830603)  Manhattan, New York, NY, USA
4         POINT (-73.8648268 40.8447819)                Bronx, NY, USA

In [5]: import matplotlib.pyplot as plt

In [6]: fig, ax = plt.subplots()

In [7]: boros.to_crs({"init": "epsg:4326"}).plot(ax=ax, color="white", edgecolor="black");

In [8]: boro_locations.plot(ax=ax, color="red");
 
The argument to provider can either be a string referencing geocoding services, such as 'google', 'bing', 'yahoo', and 'openmapquest', or an instance of a Geocoder from geopy. See geopy.geocoders.SERVICE_TO_GEOCODER for the full list. For many providers, parameters such as API keys need to be passed as **kwargs in the geocode call.
Please consult the Terms of Service for the chosen provider.

Next 
 Previous 

© Copyright 2013–2018, GeoPandas developers. 
Built with Sphinx using a theme provided by Read the Docs. 
