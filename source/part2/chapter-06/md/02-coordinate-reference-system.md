---
jupyter:
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.11.5
  kernelspec:
    display_name: Python 3 (ipykernel)
    language: python
    name: python3
---

<!-- #region -->
# Coordinate Reference Systems

Coordinate reference systems (CRS) are important because the geometric shapes in a GeoDataFrame are simply a collection of coordinates in an arbitrary space. A CRS tells Python how those coordinates are related to places on the Earth. A map projection (or a projected coordinate system) is a systematic transformation of the latitudes and longitudes into a plain surface where units are quite commonly represented as meters (instead of decimal degrees). This transformation is used to represent the three dimensional earth on a flat, two dimensional map.

As the CRS in different spatial datasets differ fairly often (i.e. one might have coordinates defined in decimal degrees while the other one has them in meters), it is a common procedure to reproject (transform) different layers into a common CRS. It is important that the layers are in the same coordinate reference system when analyzing the spatial relationships between the layers, for example, when making a Point in Polygon -query, or other type of overlay analysis.

Choosing an appropriate projection for your map is not always straightforward because it depends on what you actually want to represent with your map, and what is the spatial scale of your data. In fact, there is not a single "perfect projection" since each one of them has some strengths and weaknesses, and you should choose a projection that fits best for your needs. In fact, the projection you choose might even tell something about you!
    

![_**Figure 6.X**. Map projections Source: XKCD https://xkcd.com/977/._](../img/Map-projections.png)

_**Figure 6.X**. Map projections Source: XKCD https://xkcd.com/977/._

You can find a lot of information about available coordinate reference systems from:

  - [www.spatialreference.org](http://spatialreference.org/)
  - [www.proj4.org](https://proj4.org/operations/projections/)
  - [www.mapref.org](http://mapref.org/CollectionofCRSinEurope.html)
<!-- #endregion -->

## Managing coordinate reference systems in pyproj and geopandas

Our main tool for managing coordinate reference systems (CRS) is the [PROJ library](https://proj.org/) [^proj] that is available in python and geopandas through the [pyproj Python library](https://pyproj4.github.io/pyproj/stable/) [^pyproj]. Pyproj can be used, for example, to get basic information of the CRS of a GeoDataFrame, and to reproject data from one projection to another. 

Let's demonstrate this using sample data from Europe. We will re-project the layer from the original CRS WGS84 (lat, lon coordinates) into a Lambert Azimuthal Equal Area projection which is the [recommended projection for Europe](http://mapref.org/LinkedDocuments/MapProjectionsForEurope-EUR-20120.pdf) [^EU_projection].

In Shapefiles, information about the coordinate reference system is stored in the `.prj` -file. If this file is missing, you might be in trouble!. When reading the data into `GeoDataFrame` with Geopandas crs information is automatically stored into the `.crs` attribute of the GeoDataFrame.

Let's start by reading the data from the `Europe_borders.shp` file and checking the `crs`:

```python
# Import necessary packages
import geopandas as gpd

# Read the file
fp = "L2_data/Europe_borders.shp"
data = gpd.read_file(fp)
```

```python
# Check the coordinate reference system
data.crs
```

What we see here is in fact a CRS object from the pyproj module. 

The EPSG number (named after the *European Petroleum Survey Group*) is a code that tells about the coordinate system of the dataset. "[EPSG Geodetic Parameter Dataset](http://www.epsg.org/) is a collection of definitions of coordinate reference systems and coordinate transformations which may be global, regional, national or local in application".

The EPSG code of our geodataframe is`4326`, which refers to the WGS84 coordinate system (we can also figure this out by looking at the coordinates values which are longitude and latitudes decimal degrees).


Let's continue by checking the values in our `geometry` -column to verify that the CRS of our GeoDataFrame seems correct:

```python
data["geometry"].head()
```

As we can see, the coordinate values of the Polygons indeed look like latitude and longitude values, so everything seems to be in order.

WGS84 projection is not really a good one for representing European borders on a map (areas get distorted), so let's convert those geometries into Lambert Azimuthal Equal Area projection ([EPSG: 3035](http://spatialreference.org/ref/epsg/etrs89-etrs-laea/)) which is the recommended projection by European Comission.

Changing the projection is simple to [do in Geopandas](http://geopandas.org/projections.html#re-projecting) with `.to_crs()` -function which is a built-in function of the GeoDataFrame. The function has two alternative parameters 1) `crs` and 2) `epgs` that can be used to make the coordinate transformation and re-project the data into the CRS that you want to use. 

- Let's re-project our data into `EPSG 3035` using `epsg` -parameter:

```python
# Let's make a backup copy of our data
data_wgs84 = data.copy()

# Reproject the data
data = data.to_crs(epsg=3035)
```

```python
# Check the new geometry values
data["geometry"].head()
```

And here we go, the coordinate values in the geometries have changed! Now we have successfully changed the projection of our layer into a new one, i.e. to `ETRS-LAEA` projection. 

To really understand what is going on, it is good to explore our data visually. Let's compare the datasets by making
maps out of them.


```python
data_wgs84.crs
```

```python
import matplotlib.pyplot as plt

# Make subplots that are next to each other
fig, (ax1, ax2) = plt.subplots(nrows=1, ncols=2, figsize=(12, 12))

# Plot the data in WGS84 CRS
data_wgs84.plot(ax=ax1, facecolor="gray")

# Add title
ax1.set_title("WGS84")

# Plot the one with ETRS-LAEA projection
data.plot(ax=ax2, facecolor="blue")

# Add title
ax2.set_title("ETRS Lambert Azimuthal Equal Area projection")

# Set aspect ratio as 1
ax1.set_aspect(aspect=1)
ax2.set_aspect(aspect=1)

# Remove empty white space around the plot
plt.tight_layout()
```

_**Figure 6.X**. Map of Europe plotted with two different coordinate reference systems._

Indeed, the maps look quite different, and the re-projected one looks much better in Europe as the areas especially in the north are more realistic and not so stretched as in WGS84.

Finally, let's save our projected layer into a Shapefile so that we can use it later. Note, even if the crs information is stored in the .prj file, it might be a good idea also to include crs info in the filename:

```python
# Ouput filepath
outfp = "L2_data/Europe_borders_epsg3035.shp"

# Save to disk
data.to_file(outfp)
```

## Dealing with different CRS formats


There are various ways to present Coordinate Reference System information, such as [PROJ strings](https://proj.org/usage/quickstart.html), `EPSG codes`, `Well-Known-Text (WKT)`, `JSON`. It is likely that you will encounter some of these when working with spatial data obtained from different sources. Being able to convert the CRS information from one format to another is needed every now and then, hence, it is useful to know a few tricks how to do this.

Luckily, dealing with CRS information is easy in Python using the [pyproj](https://pyproj4.github.io/pyproj/stable/) library. In fact, `pyproj` is a Python wrapper around a software called [PROJ](https://proj.org/) (maintained by [OSGeo](https://www.osgeo.org/) community), which is widely used tool for conducting coordinate transformations in various GIS softwares. `Pyproj` is also used under the hood in Geopandas, and it handles all the CRS definitions and coordinate transformations (reprojecting from CRS to another as we did earlier).


### Overview

The following code cell prints out a summary summary of different ways of representing crs information using pyproj CRS. Here, we use the crs of the original European borders layer as a starting point:

```python
### Import CRS class from pyproj
from pyproj import CRS
```

```python
# PROJ dictionary:
crs_dict = data_wgs84.crs

# pyproj CRS object:
crs_object = CRS(data_wgs84.crs)

# EPSG code (here, the input crs information is a bit vague so we need to lower the confidence threshold)
crs_epsg = CRS(data_wgs84.crs).to_epsg(min_confidence=25)

# PROJ string
crs_proj4 = CRS(data_wgs84.crs).to_proj4()

# Well-Known Text (WKT)
crs_wkt = CRS(data_wgs84.crs).to_wkt()
```

```python
print("PROJ dictionary:\n", crs_dict)
print("\nCRS object:\n", crs_object)
print("\nEPSG code: \n", crs_epsg)
print("\nPROJ string: \n", crs_proj4)
print("\nWell-Known Text (WKT):\n", crs_wkt)
```

### Pyproj CRS object

Next, let's see how it is possible to easily extract useful information from CRS, and transform CRS information from format to another. `pyproj` -library has a [class](https://docs.python.org/3/tutorial/classes.html) called [CRS](https://pyproj4.github.io/pyproj/dev/api/crs.html) that provides many useful functionalities for dealing with CRS information.

```python
# Let's see the current CRS of our data
print(data.crs)
```

Printing the crs using the print() statement gives us the EPSG code. 

However, let's see how the same information looks like in other formats such as `WKT` or `Proj4` text. For this we need to use the `CRS` class.  


<div class="alert alert-info">

**Note**
    
The following examples have been tested to work with `pyproj` version `2.6.1` and `geopandas` version `0.8.1`. You can check package versions by running the `conda list` -command.
   
</div>

```python
# Initialize the CRS class for epsg code 3035:
crs_object = CRS.from_epsg(3035)
crs_object
```

As we can see, the `CRS` object contains a of information about the coordinate reference system such as the `Name` of the CRS (ETRS89/LAEA Europe), the `area` where the CRS is in use (*Europe* with bounds *(-16.1, 32.88, 40.18, 84.17)*), and the `Datum` (European Terrestrial Reference System 1989). 

We can also easily parse this information individually as follows: 

```python
# Name
print("Name:", crs_object.name)

# Coordinate system
print("Coordinate system:", crs_object.coordinate_system)

# Bounds of the area where CRS is used
print("Bounds:", crs_object.area_of_use.bounds)
```

You can explore all the possible information that can be extracted from the CRS by typing `crs_object.` and pressing Tabulator. 

Let's see how we can convert the crs information from one format to another. Quite often it is useful to know the EPSG code of the CRS. Next, we will conduct a few transformations to demonstrate the capabilities of the `CRS` class.

```python
# Retrive CRS information in WKT format
crs_wkt = crs_object.to_wkt()
print(crs_wkt)
```

As we can see, the `WKT` format contains a *lot* of information. Typically, e.g. the `.prj` file of a Shapefile contains the information in this format. Let's see how it is possible to extract `EPSG` code from this. For doing it, we need to re-initialize the CRS object, this time from the `WKT` text presentation.   

```python
# Retrieve EPSG code from WKT text
epsg = CRS(crs_wkt).to_epsg()
print(epsg)
```

<div class="alert alert-info">

**Not able to recognize epsg?**
    
Sometimes `to_epsg()` isn't able to recognize the EPSG code from the WKT representation. This can happen if the WKT information is missing some details. Luckily, we can easily adjust the minimum level of confidence for matching the CRS info and the EPSG code. We can do this by adjusting a parameter `min_confidence` when calling the function. By default, the confidence level is 70 %, but it is also possible to set a lower confidence threshold. 
    
The coordinate information of our input shapefile is incomplete, and does not yield an epsg value with default setting: However, CRS is able to determine the EPSG value with a lower confidence treshold: 
    
```
# Let's try to extract the EPSG code from the crs of our original data:
CRS(data.crs).to_epsg()
>>> None
    
# Let's try it again with a lower confidence requirement (25 %)
CRS(data.crs).to_epsg(min_confidence=25)
>>> 3035
```
However, be cautious when using this, as guessing the EPSG from "exotic" coordinate reference systems might also provide false results. 
</div>


Let's now save our data to disk using the `WKT` format as the crs of our GeoDataFrame. WKT is a [preferred output format](https://proj.org/faq.html#what-is-the-best-format-for-describing-coordinate-reference-systems) when storing crs information as text.

```python
# Re-define the CRS of the input GeoDataFrame
data.crs = CRS.from_epsg(3035).to_wkt()
```

```python
print(data.crs)
```

```python
# Ouput filepath
outfp = "L2_data/Europe_borders_epsg3035.shp"

# Save to disk
# data.to_file(outfp)
```

<!-- #region -->
That's it. 


**HINT**: A module called [PyCRS](https://github.com/karimbahgat/PyCRS) can also be useful library as it contains information and supports many different coordinate reference definitions, such as OGC WKT (v1), ESRI WKT, Proj4, and any EPSG, ESRI, or SR-ORG code available from spatialreference.org.
<!-- #endregion -->

<!-- #region -->
## Global map projections

Finally, let's play around with global map projections :) `L2_data` folder conaints a layer `ne_110m_admin_0_countries.shp` that represents the country borders of the world. The data was fownloaded from https://www.naturalearthdata.com/. 

#### Check your understanding

<div class="alert alert-info">

    
Read in a global dataset and plot three maps with different projections! See hints and projection definitions from:
    
- http://geopandas.org/projections.html
- https://pyproj4.github.io/pyproj/dev/api/crs.html
- https://spatialreference.org/
    
When plotting the maps, think about the advantages and disadvantages of different world map projections.
   
</div>

<!-- #endregion -->

```python
# Read in data
fp = "L2_data/ne_110m_admin_0_countries/ne_110m_admin_0_countries.shp"
admin = gpd.read_file(fp)
```

```python
# Check input crs
admin.crs
```

```python
# Set fig size
plt.rcParams["figure.figsize"] = [12, 6]
```

```python
# Plot in original crs
admin.plot()
plt.title("WGS84")
```

_**Figure 6.X**. Global map plotted in WGS 84._

```python
# Define projection as web mercator, 3785
web_mercator = CRS.from_epsg(3785)

# Re-project and plot
admin.to_crs(web_mercator).plot()

# Remove x and y axis
plt.axis("off")
plt.title("Web mercator")
```

_**Figure 6.X**. Global map plotted in Web Mercator._

```python
# Define projection Eckert IV from https://spatialreference.org/ref/esri/54012/
eckert_IV = CRS.from_proj4(
    "+proj=eck4 +lon_0=0 +x_0=0 +y_0=0 +ellps=WGS84 +datum=WGS84 +units=m +no_defs"
)

# Re-project and plot
admin.to_crs(eckert_IV).plot()

# Remove x and y axis
plt.axis("off")
plt.title("Eckert IV")
```

_**Figure 6.X**. Global map plotted in Eckert IV._

```python
# Define an orthographic projection, centered in Finland! from: http://www.statsmapsnpix.com/2019/09/globe-projections-and-insets-in-qgis.html
ortho = CRS.from_proj4(
    "+proj=ortho +lat_0=60.00 +lon_0=23.0000 +x_0=0 +y_0=0 +a=6370997 +b=6370997 +units=m +no_defs"
)

# Re-project and plot
admin.to_crs(ortho).plot()

# Remove x and y axis
plt.axis("off")
plt.title("Orthographic")
```

_**Figure 6.X**. Global map plotted in an orthographic projection._


## Summary
That's it! In this section we learned how to:

1. reproject (transform) the geometries from crs to another using the `to_crs()` -function in GeoPandas
2. Define the coordinate reference system in different formats using `pyproj` `CRS`


## Footnotes

[^proj]: <https://proj.org/>
[^pyproj]: <https://pyproj4.github.io/pyproj/stable/>
[^EU_projection]: <http://mapref.org/LinkedDocuments/MapProjectionsForEurope-EUR-20120.pdf>
