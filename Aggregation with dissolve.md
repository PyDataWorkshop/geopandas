## GeoPandas 0.4.0:Aggregation with dissolve
dissolve Example


#### Aggregation with dissolve
It is often the case that we find ourselves working with spatial data that is more granular than we need. For example, we might have data on sub-national units, but we’re actually interested in studying patterns at the level of countries.
In a non-spatial setting, we aggregate our data using the ``groupby`` function. But when working with spatial data, we need a special tool that can also aggregate geometric features. In the geopandas library, that functionality is provided by the dissolve function.

``dissolve`` can be thought of as doing three things: 
(a) it dissolves all the geometries within a given group together into a single geometric feature (using the unary_union method), and (b) it aggregates all the rows of data in a group using ``groupby.aggregate()``, and 
(c) it combines those two results.

#### dissolve Example
Suppose we are interested in studying continents, but we only have country-level data like the country dataset included in geopandas. We can easily convert this to a continent-level dataset.
First, let’s look at the most simple case where we just want continent shapes and names. By default, dissolve will pass 'first' to groupby.aggregate.
<pre><code>
In [1]: world = geopandas.read_file(geopandas.datasets.get_path('naturalearth_lowres'))

In [2]: world = world[['continent', 'geometry']]

In [3]: continents = world.dissolve(by='continent')

In [4]: continents.plot();

In [5]: continents.head()
</code></pre>

<pre><code>
Out[5]: 
                                                        geometry
continent                                                       
Africa         (POLYGON ((49.54351891459575 -12.4698328589405...
Antarctica     (POLYGON ((-159.2081835601977 -79.497059421708...
Asia           (POLYGON ((120.7156087586305 -10.2395813940878...
Europe         (POLYGON ((-52.55642473001839 2.50470530843705...
North America  (POLYGON ((-61.68000000000001 10.76, -61.105 1...
</code></pre> 
If we are interested in aggregate populations, however, we can pass different functions to the ``dissolve`` method to aggregate populations:
<pre><code>
In [6]: world = geopandas.read_file(geopandas.datasets.get_path('naturalearth_lowres'))

In [7]: world = world[['continent', 'geometry', 'pop_est']]

In [8]: continents = world.dissolve(by='continent', aggfunc='sum')

In [9]: continents.plot(column = 'pop_est', scheme='quantiles', cmap='YlOrRd');

In [10]: continents.head()
</code></pre>

<pre><code>
Out[10]: 
                                                        geometry       pop_est
continent                                                                     
Africa         (POLYGON ((49.54351891459575 -12.4698328589405...  9.932819e+08
Antarctica     (POLYGON ((-159.2081835601977 -79.497059421708...  3.802000e+03
Asia           (POLYGON ((120.7156087586305 -10.2395813940878...  4.085853e+09
Europe         (POLYGON ((-52.55642473001839 2.50470530843705...  7.281312e+08
North America  (POLYGON ((-61.68000000000001 10.76, -61.105 1...  5.393510e+08
 
</code></pre>
