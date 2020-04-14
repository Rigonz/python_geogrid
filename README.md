# python_geogrid
On defining a cartographic grid over a geoid with 4-sided polygons of fixed side length.

Cartographic grids are at the backbone of GIS, starting with the set of meridians and parallels that define the longitude and latitude coordinate system. Auxiliary grids are convenient for many purposes. 

There are several issues and (partial) solutions to the problem of the cartographic representation of a grid defined on a geoid.

It is possible to use projections that fairly accommodate an ortogonal grid of given dimensions, as the European Reference Grid on projection ETRS89-LAEA 52N 10E (EPSG:3035). This is achieved at the expense of the distorsion of the "usual" shapes at the regional level, and in any case the deformation and loss of accuracy increases at the borders.

In QGIS the plugin MMQGIS creates grids of different types, including one formed by rectangles. The units of the grid can be selected to be those of the project or degrees. However, even in cartographic projections with coordinates in distance units the actual lengths of the sides of the polygons vary depending on the position of the rectangle. If the project CRS has angular units then it is not direct to specify the grid longitudinal distance. The native function of QGIS (creategrid) produces similar results. For local scope purposes the differences may not be important, but for larger scales this lack of homogenity can represent a problem.
(Several snapshots of the issues at the ./pics folder.

This script in python aims to provide an intermediate solution. 
In brief, its scope is: given a point P0 defined by its geographic coordinates, a rectangular grid of points is defined around it. This grid has NX points in the "longitudinal" direction, and NY points in the "latitudinal" direction. The condition for the points is that the geodesic distance among them, both in "longitudinal" and "latitudinal" directions, is equal to a given value.
I write "longitudinal" and "latitudinal" because the shape of the geoid (or the sphere) does not allow to maintain the longitude or latitude of the points with the specified requirements.
The additional condition is that the points in/along the row and column whose intersection is P0 share with it the latitude and longitude, respectively. The rest of the points have to "accommodate" their coordinates to comply with the equal-distance specification.

The coordinates are geographic: (longitude, latitude) - and NOT (latitude, longitude).
Units are: km and decimal degrees.
The distances between the points are calculated with geodesic formulas (not haversine expressions). The maximum absolute error of the iteration process is specified.

The results are saved as a csv file with the coordinates of the grid points, and as a geojson file with the polygons created from four (rectangularly) adjacent points.

The following snapshots from a QGIS view of Europe (EPSG:4326) show two grids: one created with "creategrid" at a 1x1 degree spacing, and the other with the results of the script with a 100 km side.

<img src="https://github.com/Rigonz/python_geogrid/blob/master/pics/EPSG4326%20qgis.png" width="800" height="553" alt="creategrid">
<img src="https://github.com/Rigonz/python_geogrid/blob/master/pics/EPSG4326%20grid.png" width="800" height="424" alt="geogrid">
