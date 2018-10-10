GeoPandas 0.4.0 
=================


### Indexing and Selecting Data
* GeoPandas inherits the standard pandas methods for indexing/selecting data. This includes label based indexing with ``.loc`` and integer position based indexing with ``.iloc``, which apply to both GeoSeries and GeoDataFrame objects. (For more information on indexing/selecting, see the pandas documentation.)
* In addition to the standard pandas methods, GeoPandas also provides coordinate based indexing with the cx indexer, which slices using a bounding box. 
* Geometries in the GeoSeries or GeoDataFrame that intersect the bounding box will be returned.
* Using the world dataset, we can use this functionality to quickly select all countries whose boundaries extend into the southern hemisphere.

<pre><code>
In [1]: world = geopandas.read_file(geopandas.datasets.get_path('naturalearth_lowres'))

In [2]: southern_world = world.cx[:, :0]

In [3]: southern_world.plot(figsize=(10, 3));
 
</code></pre>
