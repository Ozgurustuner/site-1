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

# Introduction to spatial data analysis with geopandas

We will use [geopandas](https://geopandas.org/) [^geopandas] as our main tool for spatial data analysis in Python. In the first part of this book, we covered the basics of data analysis using the pandas library. Geopandas extends the capacities of pandas with geospatial operations. The main data structures in geopandas are `GeoSeries` and `GeoDataFrame` which extend the capabilities of `Series` and `DataFrames` from pandas. This means that we can use all our pandas skills also when working with geopandas. 

The main difference between geopandas GeoDataFrames and pandas DataFrames is that a [GeoDataFrame](http://geopandas.org/data_structures.html#geodataframe) should contain one column for geometries. By default, the name of this column is `'geometry'`. The geometry column is a [GeoSeries](http://geopandas.org/data_structures.html#geoseries) which contains the geometries (points, lines, polygons, multipolygons etc.) as shapely objects. 


![_**Figure 6.1**. Geometry column in a GeoDataFrame._](../img/geodataframe.png)

_**Figure 6.1**. Geometry column in a GeoDataFrame._

Similar to importing import pandas as `pd`, we will import geopandas as `gpd`:

```python
import geopandas as gpd
```

## Reading a Shapefile

Esri Shapefile is the default file format when reading in data usign geopandas, so we only need to pass the file path in order to read in our data:

```python
from pathlib import Path
input_folder = Path("../data/NLS")
fp = input_folder / "m_L4132R_p.shp"
```

```python
# Read file using gpd.read_file()
data = gpd.read_file(fp)
```

Let's check the data type:

```python jupyter={"outputs_hidden": false}
type(data)
```

Here we see that our `data` -variable is a `GeoDataFrame`. GeoDataFrame extends the functionalities of
`pandas.DataFrame` in a way that it is possible to handle spatial data using similar approaches and datastructures as in pandas (hence the name geopandas). 

Let's check the first rows of data: 

```python jupyter={"outputs_hidden": false}
data.head()
```

- Check all column names:

```python
data.columns.values
```

As you might guess, the column names are in Finnish.
Let's select only the useful columns and rename them into English:

```python
data = data[["RYHMA", "LUOKKA", "geometry"]]
```

Define new column names in a dictionary:

```python
colnames = {"RYHMA": "GROUP", "LUOKKA": "CLASS"}
```

Rename:

```python
data.rename(columns=colnames, inplace=True)
```

Check the output:

```python
data.head()
```

#### Check your understanding


<div class="alert alert-info">
    
Figure out the following information from our input data using your pandas skills:
    
- Number of rows?
- Number of classes?
- Number of groups?
</div>

```python
print("Number of rows", len(data["CLASS"]))
print("Number of classes", data["CLASS"].nunique())
print("Number of groups", data["GROUP"].nunique())
```

It is always a good idea to explore your data also on a map. Creating a simple map from a `GeoDataFrame` is really easy: you can use ``.plot()`` -function from geopandas that **creates a map based on the geometries of the data**. Geopandas actually uses matplotlib for plotting which we introduced in [Lesson 7 of the Geo-Python course](https://geo-python.github.io/site/notebooks/L7/matplotlib.html).

Let's try it out, and plot our GeoDataFrame:

```python jupyter={"outputs_hidden": false}
data.plot()
```

Voilá! As we can see, it is really easy to produce a map out of your Shapefile with geopandas. Geopandas automatically positions your map in a way that it covers the whole extent of your data.

*If you are living in the Helsinki region, you might recognize the shapes plotted on the map!*


## Geometries in geopandas

Geopandas takes advantage of Shapely's geometric objects. Geometries are stored in a column called *geometry* that is a default column name for
storing geometric information in geopandas.


Let's print the first 5 rows of the column 'geometry':

```python jupyter={"outputs_hidden": false}
data["geometry"].head()
```

As we can see the `geometry` column contains familiar looking values, namely Shapely `Polygon` -objects. Since the spatial data is stored as Shapely objects, **it is possible to use Shapely methods** when dealing with geometries in geopandas.

Let's have a closer look at the polygons and try to apply some of the Shapely methods we are already familiar with.

Let's start by checking the area of the first polygon in the data:

```python
# Access the geometry on the first row of data
data.at[0, "geometry"]
```

```python
# Print information about the area
print("Area:", round(data.at[0, "geometry"].area, 0), "square meters")
```


Let's do the same for the first five rows in the data; 

- Iterate over the GeoDataFrame rows using the `iterrows()` -function that we learned [during the Lesson 6 of the Geo-Python course](https://geo-python.github.io/site/notebooks/L6/pandas/advanced-data-processing-with-pandas.html#Iterating-rows-and-using-self-made-functions-in-Pandas).
- For each row, print the area of the polygon (here, we'll limit the for-loop to a selection of the first five rows):

```python jupyter={"outputs_hidden": false}
# Iterate over rows and print the area of a Polygon
for index, row in data[0:5].iterrows():

    # Get the area from the shapely-object stored in the geometry-column
    poly_area = row["geometry"].area

    # Print info
    print(
        "Polygon area at index {index} is: {area:.0f} square meters".format(
            index=index, area=poly_area
        )
    )
```

As you see from here, all **pandas** methods, such as the `iterrows()` function, are directly available in Geopandas without the need to call pandas separately because Geopandas is an **extension** for pandas. 

In practice, it is not necessary to use the iterrows()-approach to calculate the area for all features. Geodataframes and geoseries have an attribute `area` which we can use for accessing the area for each feature at once: 

```python
data.area
```

Let's next create a new column into our GeoDataFrame where we calculate and store the areas of individual polygons:

```python jupyter={"outputs_hidden": false}
# Create a new column called 'area'
data["area"] = data.area
```

Check the output:

```python
data["area"]
```

These values correspond to the ones we saw in previous step when iterating rows.

Let's check what is the `min`, `max` and `mean` of those areas using familiar functions from our previous Pandas lessions.

```python
# Maximum area
round(data["area"].max(), 2)
```

```python
# Minimum area
round(data["area"].min(), 2)
```

```python
# Average area
round(data["area"].mean(), 2)
```

## Writing data into a file

It is possible to export GeoDataFrames into various data formats using the [to_file()](http://geopandas.org/io.html#writing-spatial-data) method. In our case, we want to export subsets of the data into Shapefiles (one file for each feature class).

Let's first select one class (class number `36200`, "Lake water") from the data as a new GeoDataFrame:


```python
# Select a class
selection = data.loc[data["CLASS"] == 36200]
```

Check the selection:

```python
selection.plot()
```

- write this layer into a new Shapefile using the `gpd.to_file()` -function:

```python
# Create a output path for the data
output_folder = Path("results")

if not output_folder.exists():
    output_folder.mkdir()
    
output_fp = output_folder / "Class_36200.shp"

```

```python
# Write those rows into a new file (the default output file format is Shapefile)
selection.to_file(output_fp)
```

#### Question 6.2

Read the output Shapefile in a new geodataframe, and check that the data looks ok.

```python
# Use this cell to enter your solution.
```

```python
# Solution

temp = gpd.read_file(output_fp)

# Check first rows
temp.head()
```

```python
# Solution

# You can also plot the data for a visual check
temp.plot()
```

## Grouping the GeoDataFrame

Next we will automate the file export task. we will group the data based on column `CLASS` and export a shapefile for each class.

This can be achieved using the [groupby()](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.groupby.html) familiar from the pandas library and also available in GeoDataFrames. This function groups data based on values on selected column(s).  

Before continuing, let's check the first rows of our input data:

```python
data.head()
```

The `CLASS` column in the data contains information about different land use types. With `.unique()` -function we can quickly see all different values in that column:

```python jupyter={"outputs_hidden": false}
# Print all unique values in the column
data["CLASS"].unique()
```

Now we can use that information to group our data and save all land use types into different layers:

```python jupyter={"outputs_hidden": false}
# Group the data by class
grouped = data.groupby("CLASS")

# Let's see what we have
grouped
```

As we can see, `groupby` -function gives us an object called `DataFrameGroupBy` which is similar to list of keys and values (in a dictionary) that we can iterate over.

Check group keys:

```python
grouped.groups.keys()
```

The group keys are unique values from the column by which we grouped the dataframe.

Check how many rows of data each group has:

```python jupyter={"outputs_hidden": false}
# Iterate over the grouped object
for key, group in grouped:

    # Let's check how many rows each group has:
    print("Terrain class:", key)
    print("Number of rows:", len(group), "\n")
```

There are, for example, 56 lake polygons in the input data.


We can also check how the _last_ group looks like (we have the variables in memory from the last iteration of the for-loop):

```python
group.head()
```

Notice that the index numbers refer to the row numbers in the original data -GeoDataFrame.


Check also the data type of the group:

```python
type(group)
```

As we can see, each set of data are now grouped into separate GeoDataFrames, and we can save them into separate files.

### Saving multiple output files

Let's **export each class into a separate Shapefile**. While doing this, we also want to **create unique filenames for each class**.

When looping over the grouped object, information about the class is stored in the variable `key`, and we can use this information for creating new variable names inside the for-loop. For example, we want to name the shapefile containing lake polygons as "terrain_36200.shp".


<div class="alert alert-info">

**String formatting**
    
There are different approaches for formatting strings in Python. Here are a couple of different ways for putting together file-path names using two variables:

```
basename = "terrain"
key = 36200

# OPTION 1. Concatenating using the `+` operator:
out_fp = basename + "_" + str(key) + ".shp"

# OPTION 2. Positional formatting using `%` operator
out_fp = "%s_%s.shp" %(basename, key)
    
# OPTION 3. Positional formatting using `.format()`
out_fp = "{}_{}.shp".format(basename, key)
```
    
Read more from here: https://pyformat.info/
</div>


Let's now export terrain classes into separate Shapefiles.

- First, create a new folder for the outputs:

```python
# Determine output directory
output_folder = Path("results")

# Create a new folder called 'Results'
result_folder = output_folder / "Results"

# Check if the folder exists already
if not result_folder.exists():

    print("Creating a folder for the results..")
    # If it does not exist, create one
    result_folder.mkdir()

```

At this point, you can go to the file browser and check that the new folder was created successfully.

- Iterate over groups, create a file name, and save group to file:

```python jupyter={"outputs_hidden": false}
# Iterate over the groups
for key, group in grouped:
    # Format the filename
    output_name = Path("terrain_{}.shp".format(key))

    # Print information about the process
    print("Saving file", output_name.name)

    # Create an output path
    outpath = result_folder / output_name

    # Export the data
    group.to_file(outpath)
```

Excellent! Now we have saved those individual classes into separate Shapefiles and named the file according to the class name. These kind of grouping operations can be really handy when dealing with layers of spatial data. Doing similar process manually would be really laborious and error-prone.


### Save attributes to a text file


We can also extract basic statistics from our geodataframe, and save this information as a text file. 

Let's summarize the total area of each group:

```python
area_info = grouped.area.sum().round()
```

```python
area_info
```

Save area info to csv using pandas:

```python
# Create an output path
area_info.to_csv(result_folder / "terrain_class_areas.csv", header=True)
```


## Footnotes

[^geopandas]: <https://geopandas.org/>
[^NLS_topodata]: <https://www.maanmittauslaitos.fi/en/maps-and-spatial-data/expert-users/product-descriptions/topographic-database>
[^NLS_lisence]: <https://www.maanmittauslaitos.fi/en/opendata-licence-cc40>
[^topodata_fair]: <https://etsin.fairdata.fi/dataset/5023ecc7-914a-4494-9e32-d0a39d3b56ae>
[^paituli]: <https://avaa.tdata.fi/web/paituli/latauspalvelu>
