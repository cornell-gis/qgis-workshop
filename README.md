# Intro to QGIS

Workshop tutorial created by Keith Jenkins <kgj2@cornell.edu>, Mann Library, Cornell University. Revised 2019-10-18.

QGIS is a free and open source geographic information system that runs on Windows, Mac, and Linux.
- Official QGIS site: http://qgis.org/
- QGIS Map Showcase: https://www.flickr.com/groups/qgis/pool/


Before we begin
---------------
- Download the workshop data from https://cornell.box.com/qgis-w1 (click "Download" button, top right of page)
- When download is complete, open your downloads folder
- Right-click the .zip file > Extract All...


Examples of common types of GIS data
------------------------------------

We will explore several common types of GIS data, using the following example datasets:

| DATA TYPE | FILENAME | CRS (Coordinate Reference System) |
| --------- | -------- | --------------------------------- |
| points | pollingplaces.shp | EPSG:2261 = NAD83 / New York Central (ftUS) |
| lines | streets.shp | EPSG:26918 = NAD83 / UTM zone 18N (meters) |
| polygons | towns.gpkg | EPSG:4269 = NAD83 (latitude/longitude) |
| raster | elevation.tif | EPSG:26718 = NAD27 / UTM zone 18N (meters) |


Points
------

Points, lines, and polygons are different types of "vector" data, which can be stored in many different file formats.  Shapefiles are probably the most common geospatial data format found on the Internet, but the shapefile format dates from the early 1990s, which is why there are multiple component files (.shp, .dbf, .shx, .prj, and sometimes others).  To add a shapefile to QGIS, we just need to select the .shp file and QGIS will take care of the rest.  Shapefiles also have other quirks, mostly notably a 10-character limit for attribute names.  Learn more at <http://switchfromshapefile.org/>

- Load pollingplaces.shp -- click "Data Source Manager" button (also in Layer menu), select "Vector" tab on left, click the "..." to browse to select the "pollingplaces.shp" file
- Open the Layer Styling dock -- colorful paintbrush icon at top left of the Layers panel
  - Change marker properties: Color (blue), Size (5)
  - Click "Simple marker" (near top of window) for more settings (like border color)
- Info -- click "Identify Features" tool, select the pollingplaces layer, then click a point
  - Expand the "(Derived") section to view the actual point coordinates
- View attribute data -- right-click layer in Layers pane > Open Attribute Table
  - Table and map are linked -- select a row, see point highlighted on map, and vice versa
- To find out what CRS (coordinate reference system) is being used:
  - Right-click > Properties... Source tab


Plugins: QuickMapServices
-------------------------
Sometimes it is useful to load a global base layer from the web, to add context to your map, or just to help confirm that your data is correctly aligned. The QuickMapServices plugin makes this easy, and has already been installed on Mann Library computers. To add a basemap:
- Web menu > QuickMapServices > Google > Google Hybrid
- You may need to move the Google Hybrid layer down below your point layer, and you may also want to change some of your layer styles to increase contrast to the colors in the satellite imagery. (Bright colors like yellow show up well on dark imagery.)
- To avoid the pixelization caused by reprojecting the basemap image:
  - right-click the Google layer > Set CRS > Set Project CRS from Layer
  - right-click the Google layer > Zoom to Native Resolution (100%)


Lines
-----

Lines are another type of "vector" data.

- Load streets.shp -- as before, or just drag the .shp file onto the map
- Drag pollingplaces to top of streets (or drag streets below pollingplaces), and turn off the Google layer
- Info -- click "Identify Features" tool, select the streets layer, then click a street
- View attribute data -- right-click layer in Layers pane > Open Attribute Table
- Note contents of "SHIELD" column (mostly blank, but county and state highways have C, S, or SH)
- Open the Layer Styling dock -- paintbrush icon in top left of the Layers Panel
  - Change "Single Symbol" dropdown option at top to "Categorized"
  - Set Column = SHIELD
  - Click "Classify" button (under large white box)
  - Double-click the default symbol ("all other values") to change it to
    - dark gray, 0.26 width
  - Double-cilck the symbol for "C"
    - black, 0.5 width
  - Ctrl-click the S and SH values to select them
    - Right-cilck > Merge categories
    - red, 1.0 width

**Save your project!** Save "mymap.qgz" to the folder that also contains your data. That way, you can zip up or move the containing folder around while keeping your map and data intact. The .qgz project file contains your map styles and pointers to your data files, but not the data itself.


Polygons
--------

Polygons are yet another type of "vector" data.  This time, we'll explore a geopackage of towns in New York state.  GeoPackage is a modern geospatial file format designed to overcome the limitations of shapefiles.  Learn more at <http://www.geopackage.org/>

- Load towns.gpkg
- Style -- open the styling dock, then click the "simple fill" symbol
  - fill style = no brush (so that we can see through the polygons to the layers below)
  - stroke width = 2 mm
  - stroke color = brown
  - Layer Rendering > set opacity to 50% (this will let us see the roads that run along a boundary)
- Label -- in the styling dock, click the labels tab ("abc") on the left
  - Change "No labels" to "Single labels"
  - Label with = NAME
  - In the "Text" sub-tab (1st from the left), change the Font ("Candara"), Style ("Bold"), and Size (20.0 points)
  - Improve legibility by going to the "Buffer" sub-tab (3rd from the left) -- check "Draw text buffer"
- Save your project


Analysis
--------

QGIS has hundreds of analysis tools in the Processing Toolbox. As an example, suppose we want to know how many
miles of roadway each town has within its borders.

- Select just the Tompkins County subdivisions (this layer covers all of NY, but we only need Tompkins)
  - Method #1: "Select Features by Rectangle" and a steady hand,
    - toggle polygons on/off using CTRL-click
  - Method #2: "Select features by Value"
    - "COUNTYFP" = '109'
- "Processing" menu > Toolbox
  - Search for "Sum line lengths" in the processing toolbox
  - Lines = streets
  - Polygons = `towns`
    - Check the "Selected features only" option
  - Note that we will "create a temporary layer" containing the output (although saving directly to a new file is also an option)
  - Click "Run"
- View the results
  - Notice that the new "Line length" layer has a computer chip (also looks like a bug) next to it
  - Right-click > Open Attribute Table
  - Scroll to the right side of the table to see the new "LENGTH" and "COUNT" columns. The LENGTH column contains the total length of streets in that town. (The unit is meters, because that was the unit used by the "streets" CRS.)
- Save the temporary results to a file
  - Right-click the "Line length" layer name > Make Permanent
  - Specify the output file by clicking the "..." button. Save it as "townroads.shp" within your workshop folder
  - Click "OK"
  - Notice that the computer chip (bug) icon has gone away
  - Right-click "Line length" layer name > Rename Layer to "townroads"
- Copy the style from towns to the new "townroads" layer
  - Right-click towns > Styles > Copy style > All Style Categories
  - Right-click townroads > Styles > Paste style > All Style Categories
  - Turn off the towns layer by unchecking its box in the Layers list
- Add the total road lengths to our map labels
  - Open the styling dock to edit the townroads labels
  - Instead of just picking a column to use as the label, we can type an expression to convert from meters to miles:
    `LENGTH / 1609.34`
  - We don't need to see all those decimal places, so we can use a more complex expression to combine the name with a rounded form of the length:
    `"NAME" || '\n' || round("LENGTH" / 1609.34) || ' miles'`

Note that the `||` operator is used to join text strings. Watch out for the quotes! Double quotes are used around column names, and single quotes around text. The '\n' represents a newline character.


Raster data
-----------

- Load elevation.tif -- Data Source Manager > Raster, or just drag the .tif file onto the map
- For maximum visibility of all layers, move the elevation layer to beneath all the other layers
- But for now, turn off the other layers by unchecking their boxes in the Layers list
- Style -- open the styling dock and select the "elevation" layer
  - Change "Singleband gray" to "Singleband pseudocolor"
  - Click the Color Ramp down-arrow and select "Magma"
- Create a hillshaded version of this layer. In the past, you had to create a separate hillshade output file, but now you can simply create and adjust a hillshade in real time using the styling dock!
  - In the Layers Panel, right-click the elevation layer > Duplicate
  - Right-click "elevation copy" > Rename to "hillshade"
  - Move the "hillshade" layer above "elevation" and check the box to display the hillshade layer
  - Open the styling dock, select the "hillshade" layer
  - Change "Singleband pseudocolor" to "Hillshade"
  - Try turning the dial to adjust the angle of the sun
  - Set the blending mode to "multiply" (which will let us also see the colorized version underneath)


Plugins: Value Tool
-------------------

The "Value Tool" icon looks like a green version of the "Identify Features" icon. When enabled, it will instantly show the values of all the visible raster layers on your map. Try it with the elevation raster.


Other Plugins
-------------
There are many other plugins available for handling special data formats, managing tabular data, performing analysis, creating time-based animations, and interfacing with other programs. Some of our other favorite plugins include:
- TimeManager - create data-based animations
- QuickOSM - fetch data from OpenStreetMap
- qgis2web - export your map as an interactive Leaflet or OpenLayers map for the web
- Profile Tool - create elevation profile lines
- ORS Tools - shortest route, isochrones, network distance matrices via OpenRouteService.org
- mmqgis - a suite of vector, CSV, and geocoding tools
- NNJoin - join attributes from the nearest neighbor in another layer

Installing a QGIS plugin is easy:
- Plugins menu > Manage and Install Plugins…
- Scroll down, or search all plugins for the name of the plugin (you don't need to type the whole name)
- Click to select the plugin
- Click the "Install plugin" button (it installs in seconds)
- Click "Close"


Other things to try (time permitting)
-------------------------------------

- Create contour lines using Raster menu > Extraction > Contour
- Export the the current map view as an image or PDF
  - Project menu > Import/Export
- For more complex print layouts
  - Project menu > New Print Layout
  - Choose a title (optional)
  - Add Item menu > Add Map, then drag a rectangle on the page
  - Edit menu > Move Content (or press C) to pan/zoom the content within the page
    Pro tip: hold the Ctrl down while using your mouse's scroll wheel for finer zoom adjustment
  - Add legend, scale bar, etc. (from the Add Item menu)
  - Layout menu > Export as PDF
