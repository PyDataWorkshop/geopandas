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
Attribute Joins
Spatial Joins
Geocoding
Reference Guide
Reference to All Attributes and Methods
Developer
Contributing to GeoPandas
Docs » Merging Data 
View page source 

Merging Data
There are two ways to combine datasets in geopandas – attribute joins and spatial joins.
In an attribute join, a GeoSeries or GeoDataFrame is combined with a regular pandas Series or DataFrame based on a common variable. This is analogous to normal merging or joining in pandas.
In a Spatial Join, observations from to GeoSeries or GeoDataFrames are combined based on their spatial relationship to one another.
In the following examples, we use these datasets:
In [1]: world = geopandas.read_file(geopandas.datasets.get_path('naturalearth_lowres'))

In [2]: cities = geopandas.read_file(geopandas.datasets.get_path('naturalearth_cities'))

# For attribute join
In [3]: country_shapes = world[['geometry', 'iso_a3']]

In [4]: country_names = world[['name', 'iso_a3']]

# For spatial join
In [5]: countries = world[['geometry', 'name']]

In [6]: countries = countries.rename(columns={'name':'country'})
Attribute Joins
Attribute joins are accomplished using the merge method. In general, it is recommended to use the merge method called from the spatial dataset. With that said, the stand-alone merge function will work if the GeoDataFrame is in the left argument; if a DataFrame is in the left argument and a GeoDataFrame is in the right position, the result will no longer be a GeoDataFrame.
For example, consider the following merge that adds full names to a GeoDataFrame that initially has only ISO codes for each country by merging it with a pandas DataFrame.
# `country_shapes` is GeoDataFrame with country shapes and iso codes
In [7]: country_shapes.head()
Out[7]: 
                                            geometry iso_a3
0  POLYGON ((61.21081709172574 35.65007233330923,...    AFG
1  (POLYGON ((16.32652835456705 -5.87747039146621...    AGO
2  POLYGON ((20.59024743010491 41.85540416113361,...    ALB
3  POLYGON ((51.57951867046327 24.24549713795111,...    ARE
4  (POLYGON ((-65.50000000000003 -55.199999999999...    ARG

# `country_names` is DataFrame with country names and iso codes
In [8]: country_names.head()
Out[8]: 
                   name iso_a3
0           Afghanistan    AFG
1                Angola    AGO
2               Albania    ALB
3  United Arab Emirates    ARE
4             Argentina    ARG

# Merge with `merge` method on shared variable (iso codes):
In [9]: country_shapes = country_shapes.merge(country_names, on='iso_a3')

In [10]: country_shapes.head()
Out[10]: 
                                            geometry          ...                           name
0  POLYGON ((61.21081709172574 35.65007233330923,...          ...                    Afghanistan
1  (POLYGON ((16.32652835456705 -5.87747039146621...          ...                         Angola
2  POLYGON ((20.59024743010491 41.85540416113361,...          ...                        Albania
3  POLYGON ((51.57951867046327 24.24549713795111,...          ...           United Arab Emirates
4  (POLYGON ((-65.50000000000003 -55.199999999999...          ...                      Argentina

[5 rows x 3 columns]
Spatial Joins
In a Spatial Join, two geometry objects are merged based on their spatial relationship to one another.
# One GeoDataFrame of countries, one of Cities.
# Want to merge so we can get each city's country.
In [11]: countries.head()
Out[11]: 
                                            geometry               country
0  POLYGON ((61.21081709172574 35.65007233330923,...           Afghanistan
1  (POLYGON ((16.32652835456705 -5.87747039146621...                Angola
2  POLYGON ((20.59024743010491 41.85540416113361,...               Albania
3  POLYGON ((51.57951867046327 24.24549713795111,...  United Arab Emirates
4  (POLYGON ((-65.50000000000003 -55.199999999999...             Argentina

In [12]: cities.head()
Out[12]: 
           name                                     geometry
0  Vatican City  POINT (12.45338654497177 41.90328217996012)
1    San Marino    POINT (12.44177015780014 43.936095834768)
2         Vaduz  POINT (9.516669472907267 47.13372377429357)
3    Luxembourg  POINT (6.130002806227083 49.61166037912108)
4       Palikir  POINT (158.1499743237623 6.916643696007725)

# Execute spatial join
In [13]: cities_with_country = geopandas.sjoin(cities, countries, how="inner", op='intersects')

In [14]: cities_with_country.head()
Out[14]: 
             name   ...     country
0    Vatican City   ...       Italy
1      San Marino   ...       Italy
192          Rome   ...       Italy
2           Vaduz   ...     Austria
184        Vienna   ...     Austria

[5 rows x 4 columns]
The op options determines the type of join operation to apply. op can be set to “intersects”, “within” or “contains” (these are all equivalent when joining points to polygons, but differ when joining polygons to other polygons or lines).
Note more complicated spatial relationships can be studied by combining geometric operations with spatial join. To find all polygons within a given distance of a point, for example, one can first use the buffer method to expand each point into a circle of appropriate radius, then intersect those buffered circles with the polygons in question.

Next 
 Previous 

© Copyright 2013–2018, GeoPandas developers. 
Built with Sphinx using a theme provided by Read the Docs. 
