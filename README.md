# Intro to QGIS

Workshop tutorial created by Keith Jenkins <kgj2@cornell.edu>, Mann Library, Cornell University.\
<https://cornell-gis.github.io/qgis-workshop/> (revised 2020-10-19)

This workshop will cover basic tasks using QGIS: loading data, changing the styles used to display the data on a map, installing plugins, using processing tools to do basic analysis, and exporting a finished map image.


# Before the workshop begins

**Install QGIS on your computer**.  QGIS is a free and open source geographic information system that runs on Windows, Mac, and Linux.  In most cases, you can download and install QGIS on your own computer in under 10 minutes.
- <https://qgis.org/en/site/forusers/download.html>

Once you have installed QGIS, check that you can run it without any errors, and also make sure that you have a "Processing" menu.  (Some older Macs occasionally have issues with the Python version that is installed with QGIS.)  If you can see the "Processing" menu, then you should be good to go!

Alternately, Cornell students and staff can use QGIS via **Apps on Demand**, which provides remote access to the program through your web browser.
- Apps on Demand: <https://canvas.cornell.edu/enroll/64TDRK>
- Short video (6 minutes) showing how to set up and connect to your data: <https://www.youtube.com/watch?v=LuIDeBJfcNU>

**Download and unzip the data files for this workshop**:
- <https://github.com/cornell-gis/qgis-workshop/archive/main.zip>
- When download is complete, open your downloads folder
- Right-click the .zip file > Extract All...


# Examples of common types of GIS data

We will explore several common types of GIS data, using the following example datasets:

| DATA TYPE | FILENAME | CRS (Coordinate Reference System) |
| --------- | -------- | --------------------------------- |
| points | FireStations.shp | EPSG:3857 - WGS 84 / Pseudo-Mercator |
| lines | streets.shp | EPSG:26918 = NAD83 / UTM zone 18N (meters) |
| polygons | towns.gpkg | EPSG:4269 = NAD83 (latitude/longitude) |
| raster | elevation.tif | EPSG:26718 = NAD27 / UTM zone 18N (meters) |


# Points

Points, lines, and polygons are different types of "vector" data, which can be stored in many different file formats.  Shapefiles are probably the most common geospatial data format found on the Internet, but the shapefile format dates from the early 1990s, which is why there are multiple component files (.shp, .dbf, .shx, .prj, and sometimes others).  To add a shapefile to QGIS, we just need to select the .shp file and QGIS will take care of the rest.  Shapefiles also have other quirks, mostly notably a 10-character limit for attribute names.  Learn more at <http://switchfromshapefile.org/>

Load the fire station points:
- Click "Data Source Manager" button (also in Layer menu), select "Vector" tab on left, click the "..." to browse to the `data/fire-stations` folder and open the `FireStations.shp` file
- Click "Add", then close the data source manager

QGIS may prompt you with a question about coordinate system transformations.  It's usually okay to accept the default, but sometimes there may be a more accurate transformation specific to the area you are mapping.
- In the "Select Transformation" dialog, click "OK" (or else look for a transformation specific to New York)

Change the layer style by opening the Layer Styling panel - colorful paintbrush icon at top left of the Layers panel
- Change the color (blue) -- to get back to the other style options, use the back arrow after selecting a color
- Change the size (3) -- note the unit is mm
- Click "Simple fill" to change other properties, like stroke (outline) color and width

Get information about a point:
- Click "Identify Features" tool, select the FireStations layer, then click a point

View information about all the fire stations as a table:
- Right-click layer in Layers pane > Open Attribute Table
- Click the "SUBADDRESS" column header to sort by that column

The table and map are linked.
- Select a row in the table (by clicking the row number) to see it highlighted on the map
- To clear the selection, look for a yellowish toolbar button to "Deselect features from all layers"

Add labels by clicking the label tab (yellow "abc") in the styling panel
- Single Labels, value = SUBADDRESS
- 9 tabs of options!
- 1st tab (text) - font, size, color
- 2nd tab (formatting) - wrap lines to 16 characters
- 8th tab (placement) - Horizontal

Notice how the labels change as you zoom in and out of the map.  A mouse with a scroll wheel is highly recommended for mapping!
- Pro tip: hold down the CTRL key as you zoom for finer zoom control


# Save your project!

Save "mymap.qgz" to the folder that also contains your data -- for example, within the "qgis-workshop" folder.  That way, you can zip up or move the containing folder around while keeping your map and data intact. The .qgz project file contains your map styles and pointers to your data files, **but not the data itself**.  If you move or rename your data files, your project file won't know where to find them.


# Lines

Lines are another type of "vector" data, often used to depict linear features like rivers and streets.  Street datasets sometimes called centerlines, since the streets are often represented by one line per road, regardless of how many lanes there are.

- In your computer's file browser (like Windows' File Explorer, or Mac's Finder), look in the "streets" folder, and drag the .shp file onto QGIS

Explore the data a bit (identify tool, attribute table), and notice the contents of the "SHIELD" column. We can use those values to control the layer style.
- At the top of the layer styling panel, change "Single symbol" to "Categorized"
- Set Value = "SHIELD", then click the "Classify" button towards the bottom of the panel

Random colors are assigned to each of the values, which is not really what we want. You can shift-click to select all the values, then right-click to change the color for all the values at once.
- Change all the road colors to black
- Double-click the symbol for "C" (county roads) -- change the width to 0.75mm (then click the back arrow at the top of the styling panel to go back to the other options)
- Ctrl-click the "S" and "SH" values to select them
    - Right-click > Merge categories
    - Set the symbol to 1.5mm width

Try turning on labels for the streets layer and zooming in and out a bit.

There are many options that can be configured to improve the appearance of the labels, but it can require a lot of work and even manual adjustments to make it look really good, so...
- Turn off your road labels, since we'll use a basemap instead.


# Basemaps via QuickMapServices

Basemaps are web-based map images designed by professional cartographers who have already done the hard work of aggregating different data layers and customizing styles and labels to work at different zoom levels.  Basemaps are usually global in scope, although there are some that only focus on certain regions.  They can be used to add context to your map, or just to help confirm that your data is correctly aligned.  The QuickMapServices plugin makes this easy, but we need to install it first.

- Plugins menu > Manage and Install Plugins...
- Scroll down, or search all plugins for "quickmap" (you don’t need to type the whole name)
- Select the QuickMapServices plugin
- Click the "Install plugin" button (it installs in seconds)
- Click "Close"

When you first install QuickMapServices, you'll want to get the full set of basemap definitions.
- Web menu > QuickMapServices > Settings
- "More services" tab > "Get contributed pack"

To add the Google Hybrid basemap, which combines aerial photos with placename labels:
- Web menu > QuickMapServices > Google > Google Hybrid
- Turn off your streets and fire stations in order to see the basemap

Most basemaps are in a projection (CRS) called Pseudo- or Web-Mercator, EPSG:3857.  When using a basemap, it's usually best to set our map to use the basemap CRS in order to avoid the pixelization of stretched imagery:
- Right-click the Google layer > Set CRS > Set Project CRS from Layer
- Right-click the Google layer > Zoom to Native Resolution (100%)

Turn the streets layer back on, and zoom in to an intersection to see how well it aligns with the imagery.

Since the Google Hybrid layer often has the clutter of unwanted points that distract from our fire station points, it will help to use a plainer basemap:
- Web menu > QuickMapServices > Stamen > Stamen Toner Lite
- Uncheck the Google Hybrid layer to hide it
- Also hide the streets layer

Turn on the fire station layer, and adjust the label style to improved legibility against the basemap:
- Go to the label styles
- 3rd tab (buffer) - Draw text buffer, 1.5mm white

The Stamen Toner Lite layer is very light, so we could trying adding our own streets back on top if we want to emphasize the local road network.
- In the streets style panel, adjust Layer Rendering (bottom of panel) > Opacity to control the visual weight of the streets (around 25%)


# Polygons

Polygons are yet another type of "vector" data.  This time, we'll explore a geopackage of towns (county subdivisions) in New York state.  GeoPackage is a modern geospatial file format designed to overcome the limitations of shapefiles.  Learn more at <http://www.geopackage.org/>

Load the town boundaries:
- Look in the "towns" folder and drag the towns.gpkg file onto the QGIS window

Update the town layer style:
- Make sure the towns are on top of the other layers (in the layers panel, drag them as needed)
- For now, turn off the fire station layer
- In the layer styling for the towns layer, click "simple fill" and set the following:
  - Fill style = no brush (so that we can see through the polygons to the layers below)
  - Stroke width = 2 mm
  - Stroke color = brown
  - Layer Rendering > set opacity to 50% (this will let us see the roads that run along a boundary)

Add town labels:
- Click the yellow "abc" tab
- Change "No labels" to "Single labels"
- Label with = NAME
- In the "Text" sub-tab (1st from the left), change the Font ("Candara"), Style ("Bold"), and Size (20.0 points)
- Improve legibility by going to the "Buffer" sub-tab (3rd from the left) -- check "Draw text buffer"

Save your project!


# Processing tools for analysis

QGIS has hundreds of analysis tools in the Processing Toolbox. As an example, suppose we want to know how many miles of roadway each town has within its borders.

First, we are going to select just those towns that are in Tompkins County (the towns layer covers all of NY, but we only need Tompkins)
- Method #1: "Select Features by Rectangle" and a steady hand,
  - toggle polygons on/off using CTRL-click
- Method #2: "Select features by Value"
  - "COUNTYFP" = '109'

Next, open the processing toolbox
- "Processing" menu > Toolbox
- Search for "Sum line lengths" in the processing toolbox
- Lines = streets
- Polygons = towns
- Check the "Selected features only" option
- Note that we will "create a temporary layer" containing the output (although saving directly to a new file is also an option)
- Click "Run"

When the process completes, we should have a new layer called "Line length".  Notice that this layer has a computer chip icon (which also looks like a bug) next to it -- this indicates that it is a temporary layer held in memory, not saved to disk.
- Right-click > Open Attribute Table
- Scroll to the right side of the table to see the new "LENGTH" and "COUNT" columns. The LENGTH column contains the total length of streets in that town. (The unit is meters, because that was the unit used by the "streets" CRS.)

Save these temporary results to a geopackage:
- Right-click the "Line length" layer name > Make Permanent
- Format = GeoPackage
- Specify the output file by clicking the "..." button. Save it as "townroads.gpkg" within your workshop folder
- Click "OK" to save
- Notice that the computer chip (bug) icon has gone away
- Right-click "Line length" layer name > Rename Layer to "townroads"

We can copy the style from original "towns" layer to the new "townroads" layer:
- Right-click towns > Styles > Copy style > All Style Categories
- Right-click townroads > Styles > Paste style > All Style Categories
- Turn off the towns layer by unchecking its box in the Layers list

We can convert the total road lengths to miles, and add it to our map labels:
- Open the styling dock and make sure you have the townroads layer selected
- Switch to the labels tab
- Instead of just picking a column to use as the label, we can type an expression to convert from meters to miles:
    `LENGTH / 1609.34`
- We don't need to see all those decimal places, so we can use a more complex expression to combine the name with a rounded form of the length:
    `"NAME" || '\n' || round("LENGTH" / 1609.34) || ' miles'`

Note that the `||` operator is used to join text strings. Watch out for the quotes!  \
Double quotes are used around column names, and single quotes around text.  \
The '\n' represents a newline character.


# Raster data

Raster data is fundamentally different than vector data.  Raster data is made up of pixels, which might represent colors (aerial photographs or satellite imagery), or numeric values (elevation, temperature, precipitation, etc.)  We will explore an example of elevation data, where each pixel contains a number representing the elevation about sea level.  This dataset doesn't include any colors, but we will visualize the pixel values in multiple ways.

Load the elevation data:
- Data Source Manager > Raster, or just drag the elevation.tif file onto the map

For maximum visibility of all layers, we would usually want to move a raster layer to beneath the vector layers.  But for now, just turn off the other layers by unchecking their boxes in the Layers list.

The default style of a raster layer is a simple grayscale gradient from black to white.  Let's apply a color gradient:
- Open the styling dock and select the "elevation" layer
- Change "Singleband gray" to "Singleband pseudocolor"
- Click the Color Ramp down-arrow and select "Magma"

Try using the "Identify Features" tool to check the elevation at different points on the map.  Are the heights in feet or meters?

Let's create a hillshaded version of this layer. In the past, you had to create a separate hillshade output file, but now you can simply create and adjust a hillshade in real time using the styling panel.
- In the Layers Panel, right-click the elevation layer > Duplicate
- Right-click "elevation copy" > Rename to "hillshade"
- Move the "hillshade" layer above "elevation" and check the box to display the hillshade layer
- Open the styling dock, select the "hillshade" layer
- Change "Singleband pseudocolor" to "Hillshade"
- Try turning the dial to adjust the angle of the sun
- Set the blending mode to "multiply" (which will let us also see the colorized version underneath)


# Plugins: Value Tool

The "Value Tool" plugin will let us see the values of all the visible raster layers as we move the cursor across the map, which is much quicker than using the "Identify Features" tool.

Install the plugin:
- Plugins menu > Manage and Install Plugins...
- Install the "Value Tool" plugin

Once installed, the "Value Tool" icon will appear in your toolbars.  It looks like a green version of the "Identify Features" icon.  After clicking, you may need to enable it on the panel that appears.  Try it with the elevation raster.


# Other Plugins

There are many other plugins available for handling special data formats, managing tabular data, performing analysis, creating time-based animations, and interfacing with other programs. Some of our other favorite plugins include:
- mmqgis - a suite of vector, CSV, and geocoding tools
- QuickOSM - fetch data from OpenStreetMap
- qgis2web - export your map as an interactive Leaflet or OpenLayers map for the web
- Profile Tool - create elevation profile lines
- Semi-Automatic Classification - remote sensing image download and processing
- Street View - quickly jump to the nearest Google StreetView image
- SLYR - import styles from ArcGIS .mxd and .lyr files
- Visualist - spatial statistics and flow mapping


# Other things to try (time permitting)

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


# Other QGIS resources

- [QGIS project website](http://qgis.org/)
- [QGIS training manual](https://docs.qgis.org/latest/en/docs/training_manual/)
- [QGIS Map Showcase](https://www.flickr.com/groups/qgis/pool/) (for inspiration)
- [QGIS Tutorials and Tips](http://www.qgistutorials.com/en/) by Ujaval Gandhi
- [Videos from QGIS North America](https://www.youtube.com/channel/UCLQd1MsyWWPoIi6rNLUCjhg/videos) 2020 virtual conference
