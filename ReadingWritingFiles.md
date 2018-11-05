## GeoPandas 0.4.0 Reading and Writing Files
1. Reading Spatial Data
2. Writing Spatial Data

#### Reading Spatial Data
geopandas can read almost any vector-based spatial data format including ESRI shapefile, GeoJSON files and more using the command:
``geopandas.read_file()``
which returns a ***GeoDataFrame*** object. (This is possible because geopandas makes use of the great fiona library, which in turn makes use of a massive open-source program called GDAL/OGR designed to facilitate spatial data transformations).
Any arguments passed to ``read_file()`` after the file name will be passed directly to ``fiona.open``, which does the actual data importation. 

In general, ``read_file()`` is pretty smart and should do what you want without extra arguments, but for more help, type:
<pre><code>
import fiona; help(fiona.open)
</code></pre>

* Among other things, one can explicitly set the driver (shapefile, GeoJSON) with the driver keyword, or pick a single layer from a multi-layered file with the layer keyword.

* Where supported in fiona, geopandas can also load resources directly from a web URL, for example for GeoJSON files from geojson.xyz:

<pre><code>
url = "http://d2ad6b4ur7yvpq.cloudfront.net/naturalearth-3.3.0/ne_110m_land.geojson"
df = geopandas.read_file(url)
</code></pre>
geopandas can also get data from a PostGIS database using the ``read_postgis()`` command.

#### Writing Spatial Data
GeoDataFrames can be exported to many different standard formats using the GeoDataFrame.to_file() method. For a full list of supported formats, type import fiona; fiona.supported_drivers.


