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

# Introduction to geographic data in Python

How do we represent geographic features such as roads and buildings or lakes and mountains on a computer? How can we analyze spatial relations between these features? How can we link abstract geometric objects to actual locations on the Earth? What is the difference between geographic and projected coordinate reference systems? How can we automate all of this using Python? This chapter introduces you to the basic concepts and approaches related to Geographic Information Systems (GIS) and spatial data analysis. Geographers and GIS professionals might already be familiar with most of these concepts, but we hope this section helps beginners to grasp the basic concepts essential to understad the approached presented in this part of the book.


## Ways to represent spatial data 

- Division between vector and raster data
- Points, Lines, Polygons, Fields


## Data models for representing geographic information on a computer

- Points
- Lines
- Areas (Polygons)
- Collections
- Box
- Sidenote: storing geometry into WKT/WKB
- Surface / field:
  - Raster
  - TIN/Voronoi (tesseleations)
  - Hexagon (H3)
- Networks
  


# Geographic objects in Python/Shapely

In this lesson, you will learn how to create and manipulate geometries in Python using the [Shapely Python Package](https://shapely.readthedocs.io/en/stable/manual.html).

**Sources:**

These materials are partly based on [Shapely-documentation](https://shapely.readthedocs.io/en/stable/manual.html) and [Westra
E. (2013), Chapter 3](https://www.packtpub.com/application-development/python-geospatial-development-second-edition).

## Spatial data model

![_**Figure 6.X**. Spatial data model Source: CHECK!._](../img/spatial-data-model.png)

_**Figure 6.X**. Spatial data model._

Fundamental geometric objects that can be used in Python with [Shapely](https://shapely.readthedocs.io/en/stable/manual.html). The most fundamental geometric objects are `Points`, `Lines` and `Polygons` which are the basic ingredients when working with spatial data in vector format. Python has a specific module called [Shapely](https://shapely.readthedocs.io/en/stable/manual.html) for doing various geometric operations. Basic knowledge of using Shapely is fundamental for understanding how geometries are stored and handled in GeoPandas.

**Geometric objects consist of coordinate tuples where:**

-  `Point` -object represents a single point in space. Points can be either two-dimensional (x, y) or three dimensional (x, y, z).
-  `LineString` -object (i.e. a line) represents a sequence of points joined together to form a line. Hence, a line consist of a list of at least two coordinate tuples
-  `Polygon` -object represents a filled area that consists of a list of at least three coordinate tuples that forms the outerior ring and a (possible) list of hole polygons.

**It is also possible to have a collection of geometric objects (e.g. Polygons with multiple parts):**

-  `MultiPoint` -object represents a collection of points and consists of a list of coordinate-tuples
-  `MultiLineString` -object represents a collection of lines and consists of a list of line-like sequences
-  `MultiPolygon` -object represents a collection of polygons that consists of a list of polygon-like sequences that construct from exterior ring and (possible) hole list tuples

**Useful attributes and methods in Shapely include:**

-  Creating lines and polygons based on a collection of point objects.
-  Calculating areas/length/bounds etc. of input geometries
-  Conducting geometric operations based on the input geometries such as `union`, `difference`, `distance` etc.
-  Conducting spatial queries between geometries such as `intersects`, `touches`, `crosses`, `within` etc.


<div class="alert alert-info">

**Tuple**

[Tuple](https://docs.python.org/3/tutorial/datastructures.html#tuples-and-sequences) is a Python data structure that consists of a number of values separated by commas. Coordinate pairs are often represented as a tuple. For example:

```
(60.192059, 24.945831)
``` 

Tuples belong to [sequence data types](https://docs.python.org/3/library/stdtypes.html#typesseq) in Python. Other sequence data types are lists and ranges. Tuples have many similarities with lists and ranges, but they are often used for different purposes. The main difference between tuples and lists is that tuples are [immutable](https://docs.python.org/3/glossary.html#term-immutable), which means that the contents of a tuple cannot be altered (while lists are mutable; you can, for example, add and remove values from lists).
</div>




## Point

Creating point is easy, you pass x and y coordinates into `Point()` -object (+ possibly also z -coordinate):

```python jupyter={"outputs_hidden": false}
# Import necessary geometric objects from shapely module
from shapely.geometry import Point, LineString, Polygon

# Create Point geometric object(s) with coordinates
point1 = Point(2.2, 4.2)
point2 = Point(7.2, -25.1)
point3 = Point(9.26, -2.456)
point3D = Point(9.26, -2.456, 0.57)
```

Let's see what these variables now contain: 

```python
point1
```

As we see here, Jupyter notebook is able to display the shape directly on the screen.

We can use the print statement to get information about the actual definition of these objects:

```python jupyter={"outputs_hidden": false}
print(point1)
print(point3D)
```

3D-point can be recognized from the capital Z -letter in front of the coordinates.

Let's also check the data type of a point:

```python
type(point1)
```

We can see that the type of the point is shapely's Point. The point object is represented in a specific format based on
[GEOS](https://trac.osgeo.org/geos) C++ library that is one of the standard libraries behind various Geographic Information Systems. It runs under the hood e.g. in [QGIS](http://www.qgis.org/en/site/). 


### Point attributes and functions

Points and other shapely objects have useful built-in [attributes and methods](https://shapely.readthedocs.io/en/stable/manual.html#general-attributes-and-methods). Using the available attributes, we can for example extract the coordinate values of a Point and calculate the Euclidian distance between points.


`geom_type` attribute contains information about  the geometry type of the Shapely object:

```python
point1.geom_type
```

Extracting the coordinates of a Point can be done in a couple of different ways:


`coords` attribute contains the coordinate information as a `CoordinateSequence` which is another data type related to Shapely.

```python
# Get xy coordinate tuple
list(point1.coords)
```

Here we have a coordinate tuple inside a list. Using the attributes `x` and `y` it is possible to get the coordinates directly as plain decimal numbers.

```python jupyter={"outputs_hidden": false}
# Read x and y coordinates separately
x = point1.x
y = point1.y
```

```python
print(x, y)
```

It is also possible to calculate the distance between two objects using the [distance](https://shapely.readthedocs.io/en/stable/manual.html#object.distance) method. In our example the distance is calculated in a cartesian coordinate system. When working with real GIS data the distance is based on the used coordinate reference system. always check what is the unit of measurement (for example, meters) in the coordinate reference system you are using.

Let's calculate the distance between `point1` and `point2`:

```python
# Check input data
print(point1)
print(point2)
```

```python
# Calculate the distance between point1 and point2
dist = point1.distance(point2)

# Print out a nicely formatted info message
print("Distance between the points is {0:.2f} units".format(dist))
```

<!-- #region -->
## LineString


Creating LineString -objects is fairly similar to creating Shapely Points. 

Now instead using a single coordinate-tuple we can construct the line using either a list of shapely Point -objects or pass the points as coordinate-tuples:
<!-- #endregion -->

```python jupyter={"outputs_hidden": false}
# Create a LineString from our Point objects
line = LineString([point1, point2, point3])
```

```python
# It is also possible to produce the same outcome using coordinate tuples
line2 = LineString([(2.2, 4.2), (7.2, -25.1), (9.26, -2.456)])
```

```python
# Check if lines are identical
line == line2
```

Let's see how our line looks like: 

```python
line
```

```python
print(line)
```

As we can see from above, the `line` -variable constitutes of multiple coordinate-pairs.


Check also the data type:

```python
# Check data type of the line object
type(line)
```

```python
# Check geometry type of the line object
line.geom_type
```

<!-- #region -->
### LineString attributes and functions


`LineString` -object has many useful built-in attributes and functionalities. It is for instance possible to extract the coordinates or the length of a LineString (line), calculate the centroid of the line, create points along the line at specific distance, calculate the closest distance from a line to specified Point and simplify the geometry. See full list of functionalities from [Shapely documentation](http://toblerity.org/shapely/manual.html). Here, we go through a few of them.

We can extract the coordinates of a LineString similarly as with `Point`
<!-- #endregion -->

```python jupyter={"outputs_hidden": false}
# Get xy coordinate tuples
list(line.coords)
```

Again, we have a list of coordinate tuples (x,y) inside a list.

If you would need to access all x-coordinates or all y-coordinates of the line, you can do it directly using the `xy` attribute: 

```python jupyter={"outputs_hidden": false}
# Extract x and y coordinates separately
xcoords = list(line.xy[0])
ycoords = list(line.xy[1])
```

```python
print(xcoords)
print(ycoords)
```

It is possible to retrieve specific attributes such as lenght of the line and center of the line (centroid) straight from the LineString object itself:

```python jupyter={"outputs_hidden": false}
# Get the lenght of the line
l_length = line.length
print("Length of our line: {0:.2f} units".format(l_length))
```

```python
# Get the centroid of the line
print(line.centroid)
```

As you can see, the centroid of the line is again a Shapely Point object. 

<!-- #region -->
## Polygon


Creating a `Polygon` -object continues the same logic of how `Point` and `LineString` were created but Polygon object only accepts a sequence of coordinates as input. 

Polygon needs **at least three coordinate-tuples** (three points are reguired to form a surface):
<!-- #endregion -->

```python jupyter={"outputs_hidden": false}
# Create a Polygon from the coordinates
poly = Polygon([(2.2, 4.2), (7.2, -25.1), (9.26, -2.456)])
```

We can also use information from the Shapely Point objects created earlier, but we can't use the point objects directly. Instead, we need to get information of the x,y coordinate pairs as a sequence. We can achieve this by using a list comprehension.

```python
# Create a Polygon based on information from the Shapely points
poly2 = Polygon([[p.x, p.y] for p in [point1, point2, point3]])
```

In order to understand what just happened, let's check what the list comprehension produces:

```python
[[p.x, p.y] for p in [point1, point2, point3]]
```

This list of lists was passed as input for creating the Polygon.

```python
# Check that polygon objects created using two different approaches are identical
poly == poly2
```

Let's see how our Polygon looks like

```python
poly
```

```python
print(poly)
```

Notice that `Polygon` representation has double parentheses around the coordinates (i.e. `POLYGON ((<values in here>))` ). This is because Polygon can also have holes inside of it. 


Check also the data type:

```python
# Data type
type(poly)
```

```python
# Geometry type
poly.geom_type
```

```python
# Check the help for Polygon objects:
# help(Polygon)
```

<!-- #region -->


As the help of [Polygon](https://shapely.readthedocs.io/en/stable/manual.html#polygons) -object tells, a Polygon can be constructed using exterior coordinates and interior coordinates (optional) where the interior coordinates creates a hole inside the Polygon:

<!-- #endregion -->

```
Help on Polygon in module shapely.geometry.polygon object:
     class Polygon(shapely.geometry.base.BaseGeometry)
      |  A two-dimensional figure bounded by a linear ring
      |
      |  A polygon has a non-zero area. It may have one or more negative-space
      |  "holes" which are also bounded by linear rings. If any rings cross each
      |  other, the feature is invalid and operations on it may fail.
      |
      |  Attributes
      |  ----------
      |  exterior : LinearRing
      |      The ring which bounds the positive space of the polygon.
      |  interiors : sequence
      |      A sequence of rings which bound all existing holes.
      
```


Let's see how we can create a `Polygon` with a hole:

```python
# Define the outer border
border = [(-180, 90), (-180, -90), (180, -90), (180, 90)]
```

```python
# Outer polygon
world = Polygon(shell=border)
print(world)
```

```python
world
```

```python
# Let's create a single big hole where we leave ten units at the boundaries
# Note: there could be multiple holes, so we need to provide list of coordinates for the hole inside a list
hole = [[(-170, 80), (-170, -80), (170, -80), (170, 80)]]
```

```python
# Now we can construct our Polygon with the hole inside
frame = Polygon(shell=border, holes=hole)
print(frame)
```

Let's see what we have now:

```python
frame
```

As we can see the `Polygon` has now two different tuples of coordinates. The first one represents the **outerior** and the second one represents the **hole** inside of the Polygon.

<!-- #region -->
### Polygon attributes and functions


We can again access different attributes directly from the `Polygon` object itself that can be really useful for many analyses, such as `area`, `centroid`, `bounding box`, `exterior`, and `exterior-length`. See a full list of methods in the [Shapely User Manual](https://shapely.readthedocs.io/en/stable/manual.html#the-shapely-user-manual).

Here, we can see a few of the available attributes and how to access them:
<!-- #endregion -->

```python
# Print the outputs
print("Polygon centroid: ", world.centroid)
print("Polygon Area: ", world.area)
print("Polygon Bounding Box: ", world.bounds)
print("Polygon Exterior: ", world.exterior)
print("Polygon Exterior Length: ", world.exterior.length)
```

As we can see above, it is again fairly straightforward to access different attributes from the `Polygon` -object. Note that distance metrics will make more sense when we start working with data in a projected coordinate system.


#### Question 6.1

Create these shapes using Shapely!

- **Triangle**   
- **Square**    
- **Cicrle**

```python
# Use this cell to enter your solution.
```

```python
# Solution

# Triangle
Polygon([(0, 0), (2, 4), (4, 0)])
```

```python
# Solution

# Square
Polygon([(0, 0), (0, 4), (4, 4), (4, 0)])
```

```python
# Solution

# Circle (using a buffer around a point)
point = Point((0, 0))
point.buffer(1)
```

<!-- #region -->
## Geometry collections


In some occassions it is useful to store multiple geometries (for example, several points or several polygons) in a single feature. A practical example would be a country that is composed of several islands. In such case, all these polygons share the same attributes on the country-level and it might be reasonable to store that country as geometry collection that contains all the polygons. The attribute table would then contain one row of information with country-level attributes, and the geometry related to those attributes would represent several polygon. 

In Shapely, collections of points are implemented by using a MultiPoint -object, collections of curves by using a MultiLineString -object, and collections of surfaces by a MultiPolygon -object. 
<!-- #endregion -->

```python
# Import constructors for creating geometry collections
from shapely.geometry import MultiPoint, MultiLineString, MultiPolygon
```

Let's start by creating MultiPoint and MultilineString objects:

```python
# Create a MultiPoint object of our points 1,2 and 3
multi_point = MultiPoint([point1, point2, point3])

# It is also possible to pass coordinate tuples inside
multi_point2 = MultiPoint([(2.2, 4.2), (7.2, -25.1), (9.26, -2.456)])

# We can also create a MultiLineString with two lines
line1 = LineString([point1, point2])
line2 = LineString([point2, point3])
multi_line = MultiLineString([line1, line2])

# Print object definitions
print(multi_point)
print(multi_line)
```

```python
multi_point
```

```python
multi_line
```

MultiPolygons are constructed in a similar manner. Let's create a bounding box for "the world" by combinin two separate polygons that represent the western and eastern hemispheres. 

```python jupyter={"outputs_hidden": false}
# Let's create the exterior of the western part of the world
west_exterior = [(-180, 90), (-180, -90), (0, -90), (0, 90)]

# Let's create a hole --> remember there can be multiple holes, thus we need to have a list of hole(s).
# Here we have just one.
west_hole = [[(-170, 80), (-170, -80), (-10, -80), (-10, 80)]]

# Create the Polygon
west_poly = Polygon(shell=west_exterior, holes=west_hole)

# Print object definition
print(west_poly)
```

```python
west_poly
```

Shapely also has a tool for creating [a bounding box](https://en.wikipedia.org/wiki/Minimum_bounding_box) based on minimum and maximum x and y coordinates. Instead of using the Polygon constructor, let's use the [box](https://shapely.readthedocs.io/en/stable/manual.html#shapely.geometry.box) constructor for creating the polygon:  

```python
from shapely.geometry import box
```

```python
# Specify the bbox extent (lower-left corner coordinates and upper-right corner coordinates)
min_x, min_y = 0, -90
max_x, max_y = 180, 90

# Create the polygon using Shapely
east_poly = box(minx=min_x, miny=min_y, maxx=max_x, maxy=max_y)

# Print object definition
print(east_poly)
```

```python
east_poly
```

Finally, we can combine the two polygons into a MultiPolygon:

```python
# Let's create our MultiPolygon. We can pass multiple Polygon -objects into our MultiPolygon as a list
multi_poly = MultiPolygon([west_poly, east_poly])

# Print object definition
print(multi_poly)
```

```python
multi_poly
```

We can see that the outputs are similar to the basic geometric objects that we created previously but now these objects contain multiple features of those points, lines or polygons.

### Other useful attributes 
lenght of the geometry collection:

```python
print("Number of objects in our MultiLine:", len(multi_line))
print("Number of objects in our MultiPolygon:", len(multi_poly))
```

Area:

```python jupyter={"outputs_hidden": false}
# Print outputs:
print("Area of our MultiPolygon:", multi_poly.area)
print("Area of our Western Hemisphere polygon:", multi_poly[0].area)
```

From the above we can see that MultiPolygons have exactly the same attributes available as single geometric objects but now the information such as area calculates the area of **ALL** of the individual -objects combined. We can also access individual objects inside the geometry collections using indices.


Finally, we can check if we have a "valid" MultiPolygon. MultiPolygon is thought as valid if the individual polygons does not intersect with each other. 
Here, because the polygons have a common 0-meridian, we should NOT have a valid polygon. We can check the validity of an object from the **is_valid** -attribute that tells if the polygons or lines intersect with each other. This can be really useful information when trying to find topological errors from your data:

```python
print("Is polygon valid?: ", multi_poly.is_valid)
```
