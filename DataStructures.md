 GeoPandas 
0.4.0 
 
Getting Started
Installation
Examples Gallery
User Guide

Data Structures

GeoSeries

GeoDataFrame
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
Docs » Data Structures 
View page source 

Data Structures
GeoPandas implements two main data structures, a GeoSeries and a GeoDataFrame. These are subclasses of pandas Series and DataFrame, respectively.
GeoSeries
A GeoSeries is essentially a vector where each entry in the vector is a set of shapes corresponding to one observation. An entry may consist of only one shape (like a single polygon) or multiple shapes that are meant to be thought of as one observation (like the many polygons that make up the State of Hawaii or a country like Indonesia).
geopandas has three basic classes of geometric objects (which are actually shapely objects):
Points / Multi-Points
Lines / Multi-Lines
Polygons / Multi-Polygons
Note that all entries in a GeoSeries need not be of the same geometric type, although certain export operations will fail if this is not the case.
Overview of Attributes and Methods
The GeoSeries class implements nearly all of the attributes and methods of Shapely objects. When applied to a GeoSeries, they will apply elementwise to all geometries in the series. Binary operations can be applied between two GeoSeries, in which case the operation is carried out elementwise. The two series will be aligned by matching indices. Binary operations can also be applied to a single geometry, in which case the operation is carried out for each element of the series with that geometry. In either case, a Series or a GeoSeries will be returned, as appropriate.
A short summary of a few attributes and methods for GeoSeries is presented here, and a full list can be found in the all attributes and methods page. There is also a family of methods for creating new shapes by expanding existing shapes or applying set-theoretic operations like “union” described in geometric manipulations.
Attributes
area: shape area (units of projection – see projections)
bounds: tuple of max and min coordinates on each axis for each shape
total_bounds: tuple of max and min coordinates on each axis for entire GeoSeries
geom_type: type of geometry.
is_valid: tests if coordinates make a shape that is reasonable geometric shape (according to this).
Basic Methods
distance(other): returns Series with minimum distance from each entry to other
centroid: returns GeoSeries of centroids
representative_point(): returns GeoSeries of points that are guaranteed to be within each geometry. It does NOT return centroids.
to_crs(): change coordinate reference system. See projections
plot(): plot GeoSeries. See mapping.
Relationship Tests
geom_almost_equals(other): is shape almost the same as other (good when floating point precision issues make shapes slightly different)
contains(other): is shape contained within other
intersects(other): does shape intersect other
GeoDataFrame
A GeoDataFrame is a tabular data structure that contains a GeoSeries.
The most important property of a GeoDataFrame is that it always has one GeoSeries column that holds a special status. This GeoSeries is referred to as the GeoDataFrame’s “geometry”. When a spatial method is applied to a GeoDataFrame (or a spatial attribute like area is called), this commands will always act on the “geometry” column.
The “geometry” column – no matter its name – can be accessed through the geometry attribute (gdf.geometry), and the name of the geometry column can be found by typing gdf.geometry.name.
A GeoDataFrame may also contain other columns with geometrical (shapely) objects, but only one column can be the active geometry at a time. To change which column is the active geometry column, use the set_geometry method.
An example using the worlds GeoDataFrame:
In [1]: world = geopandas.read_file(geopandas.datasets.get_path('naturalearth_lowres'))

In [2]: world.head()
Out[2]: 
      pop_est                        ...                                                                   geometry
0  28400000.0                        ...                          POLYGON ((61.21081709172574 35.65007233330923,...
1  12799293.0                        ...                          (POLYGON ((16.32652835456705 -5.87747039146621...
2   3639453.0                        ...                          POLYGON ((20.59024743010491 41.85540416113361,...
3   4798491.0                        ...                          POLYGON ((51.57951867046327 24.24549713795111,...
4  40913584.0                        ...                          (POLYGON ((-65.50000000000003 -55.199999999999...

[5 rows x 6 columns]

#Plot countries
In [3]: world.plot();
 
Currently, the column named “geometry” with country borders is the active geometry column:
In [4]: world.geometry.name
Out[4]: 'geometry'
We can also rename this column to “borders”:
In [5]: world = world.rename(columns={'geometry': 'borders'}).set_geometry('borders')

In [6]: world.geometry.name
Out[6]: 'borders'
Now, we create centroids and make it the geometry:
In [7]: world['centroid_column'] = world.centroid

In [8]: world = world.set_geometry('centroid_column')

In [9]: world.plot();
 
Note: A GeoDataFrame keeps track of the active column by name, so if you rename the active geometry column, you must also reset the geometry:
gdf = gdf.rename(columns={'old_name': 'new_name'}).set_geometry('new_name')
Note 2: Somewhat confusingly, by default when you use the read_file command, the column containing spatial objects from the file is named “geometry” by default, and will be set as the active geometry column. However, despite using the same term for the name of the column and the name of the special attribute that keeps track of the active column, they are distinct. You can easily shift the active geometry column to a different GeoSeries with the set_geometry command. Further, gdf.geometry will always return the active geometry column, not the column named geometry. If you wish to call a column named “geometry”, and a different column is the active geometry column, use gdf['geometry'], not gdf.geometry.
Attributes and Methods
Any of the attributes calls or methods described for a GeoSeries will work on a GeoDataFrame – effectively, they are just applied to the “geometry” GeoSeries.
However, GeoDataFrames also have a few extra methods for input and output which are described on the Input and Output page and for geocoding with are described in Geocoding.

Next 
 Previous 

© Copyright 2013–2018, GeoPandas developers. 
Built with Sphinx using a theme provided by Read the Docs. 
