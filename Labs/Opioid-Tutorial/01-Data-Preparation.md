<img src="https://github.com/mjdanielson/University-of-Oregon/blob/master/Labs/Opioid-Tutorial/Images/logo.png">

<h1 align="center"> Mapping opioid-related prescriptions <br> and overdose rates in the U.S.</h1>


### Part I: Downloading and Preparing Data 

In this tutorial you will:

- Learn about Mapbox projections 101
- Download geographic data on opioid-related prescriptions and overdose rates
- Add Mapbox maps as layers in QGIS with WMTS
- Apply table joins 
- Export data
- Learn how to troubleshoot data upload errors


### Software Requirements: 

- [QGIS](https://www.qgis.org/en/site/forusers/alldownloads.html) 
- [GDAL](https://gdal.org/) 

### Downloading geographic data

In this exercise we will be downloading a number of different data set in order to examine the relationship between opioid prescriptions and drug overdose rates across the United States. To get started, you will need the following datasets: 

1. 2017 U.S. age-adjusted rate of drug overdoses by state, obtained from the [CDC](https://www.cdc.gov/drugoverdose/data/statedeaths.html).
2. U.S. county prescribing rates for 2017, collected from the [CDC](https://www.cdc.gov/drugoverdose/maps/rxcounty2017.html).
3. US state shapefile ([2017 TIGER/Line Shapefile](https://www.census.gov/cgi-bin/geo/shapefiles/index.php?year=2017&layergroup=Counties+%28and+equivalent%29)).
4. US county shapefile ([2017 TIGER/Line Shapefile](https://www.census.gov/cgi-bin/geo/shapefiles/index.php?year=2017&layergroup=States+%28and+equivalent%29)). 

We will be preparing our data using QGIS and a text editor.

### Add Mapbox maps as layers in ArcGIS and QGIS with WMTS

Open QGIS on your desktop and add a Mapbox base layer. 
Mapbox provides many URLs and code snippets to help you add your custom Mapbox maps to other mapping tools. The following tutorial will show you how you can add any Mapbox map as a layer in ArcMap or QGIS as WMTS: 

https://docs.mapbox.com/help/tutorials/mapbox-arcgis-qgis/


### Prepare datasets for upload

Add both your US county and state TIGER/Line shapefiles to your map using the ‘add vector layer’ button in QGIS.
Next, using the ‘add vector layer’ button, add the data tables for prescription rates by county and overdose rates by state. 

<p align="center">
  <img src="https://github.com/mjdanielson/University-of-Oregon/blob/master/Labs/Opioid-Tutorial/Images/QGIS_add_layer.png">
  </p>

Check to make sure that your fields for ‘overdose  rates’ and ‘prescription drug rates’ are numerical. These fields must be numerical in order to style them across a data range (you cannot style categorical data across a data range!). 

<p align="center">
  <img src='https://github.com/mjdanielson/University-of-Oregon/blob/master/Labs/Opioid-Tutorial/Images/Screen%20Shot%202019-10-01%20at%2012.46.38%20PM.png'>
  </p>


**IF** the field that you want to display is showing up as a ‘string’ in your data table, then create a new numerical field and set it equal to the ‘string’ field. In the following example, we use the ‘field calculator’ in QGIS to create a new field called ‘Rate’. The new field type is ‘Decimal number (real)’ with a precision of 1. In the ‘Expression’ console, we have set the new field equal to the old rate field (which is a string). 

<p align="center">
<img src="https://github.com/mjdanielson/University-of-Oregon/blob/master/Labs/Opioid-Tutorial/Images/expression.png">
</p>

After creating the new field, you can delete the old ‘string’ field and any other extraneous data in the table. Remember to preserve the field that will be used as the primary key for joining your data table to your polygon layer. 

### Table Joins

Joining data is typically used to append the fields of one table to those of another through an attribute or field common to both tables. This common field is often referred to as the “Primary Key’. The state abbreviation field in the US State data gathered from the TIGER/Line database and the drug overdose data from the CDC can be used as the ‘primary key’ to join these two tables. 

<img src="https://github.com/mjdanielson/University-of-Oregon/blob/master/Labs/Opioid-Tutorial/Images/Screen%20Shot%202019-10-01%20at%2012.23.42%20PM.png">

After you’ve joined your data, change the name of the new layer name to ‘Overdose  Rate’. 
Next, join your prescription drug rates table to your county shapefile. After you join your tables, make sure to change any ‘null values’ in your prescription drug rate field to a number that is out the data range (i.e. 1,000). Changing the values from ‘null’ to a number will enable you to style the null values on your map.  

To change the null values in QGIS, open your attribute table and click the "Select Features Using an Expression" button. To find all the null records for a field in a shape file your query will look like: **"insert_field_name" is null**

You can find your field name in the Fields and Values list; double click the field you want to get it into the Expression box.

<p align="center">
  <img src="https://github.com/mjdanielson/University-of-Oregon/blob/master/Labs/Opioid-Tutorial/Images/prescription.png">
  </p>


Make sure you SELECT the new filtered list of records. Then go back to the attribute table and click the Field Calculator button. Check the "Update Existing Field" box - ensuring that the 'only update selected' check box is selected, then select the field you want to update from the dropdown box. Put 1000 in the expression box then click ‘OK’. 

After you’ve joined your data, change the name of the new layer to ‘Prescription Drug Rate’. 


### Mapbox projections 101

**Projections** are methods of transforming the coordinates of locations on the planet to a two-dimensional plane. Mapbox supports the popular [Web Mercator projection](http://en.wikipedia.org/wiki/Mercator_projection) (ESPG for Mapbox Maps is **4326 (WGS84**)). Web Mercator is adopted by the vast majority of web maps and its use allows you to combine Mapbox maps with other layers in the same projection.

For more information on a projections check out this [blog post](https://lyzidiamond.com/posts/4326-vs-3857) by former Mapboxer and mapping wizard, Lyzi Diamond.

During upload processing, we reproject all geometries to Web Mercator (EPSG:4326) before encoding into vector tiles. During the vector tile encoding process, if your data isn't Web Mercator, each vertex must be reprojected during encoding, which can take a long time.

We suggest reprojecting your data before uploading to skip this process and speed up your upload. Here's how you can reproject your data with open source tools:


<p align="center">
  <img src="https://github.com/mjdanielson/University-of-Oregon/blob/master/Labs/Opioid-Tutorial/Images/coordinate.png">
  </p>
  

**GDAL**'s ogr2ogr command line utility. The following example is how to convert a Shapefile to Web Mercator.
ogr2ogr output.shp -t_srs "EPSG:4326" input.shp
**QGIS** allows for reprojection in a number of ways
Option 1:  Right-click your layer -> Save As -> Select "Web Mercator EPSG:4326" as the output projection.
Option 2: Right click on the layer 🡪Set CRS’ 🡪Set Layer CRS…’ from the menu. Select 
**EPSG:4326** from the coordinate reference system menu and then hit ‘OK’. 

<h3 align="center"> Remember to change the projection for each layer.</h3>

### Exporting data from QGIS

You can export your data in a number of ways. The next part of this tutorial will demonstrate how to export your data as a Shapefile and as a GeoJSON, as well as how to troubleshoot some common errors that occur with data exporting and uploading to Mapbox Studio. 

<p align="center">
  <img src="https://github.com/mjdanielson/University-of-Oregon/blob/master/Labs/Opioid-Tutorial/Images/export.png">
  </p>


**Exporting data as a Shapefile**

Right click on your ‘overdose  Rate’ layer and toggle to Export 🡪 Save Features As…
Set the file format to “ESRI Shapefile” and make sure that the CRS is set to **EPSG: 4326 – WGS 84**. 
Click ‘OK’. 
Zip the folder that contains your overdose rate data shapefile. 

**Exporting data as a GeoJSON** 

Right click on your ‘Prescription Drug Rate’ layer and toggle to Export 🡪 Save Features As…
Set the file format to “GeoJSON” and make sure that the CRS is set to **EPSG: 4326 – WGS 84**. 
Click ‘OK’. 
Next, we need to edit the GeoJSON file. 


A common error that occurs when uploading GeoJSON data to Mapbox is that the GeoJSON contains an old-style CRS attribute. To prevent this error, open your GeoJSON in a text editor (for this example, we will be using Atom). Delete the CRS attribute from your code and save your edits. 

![A screenshot of a social media post  Description automatically generated](https://lh5.googleusercontent.com/TDXkDiVanE2_m4xhpk303_ELLf9oy8-6tIgobebSj7gXDzsvEkQETbYnz45PiBAbrfEpI_AuA8fTZGaLS7196JRLetT4mNhI9PlWduIQ1PVqR1oF437X8wZaBcEkomxESF9Ochci)

![](https://docs.google.com/a/mapbox.com/drawings/d/s1Lp7DV4OBEs0ZaO9iOVL-A/image?w=534&h=18&rev=1&ac=1&parent=1OmNvY7TlLi21PkNRGyZ4hCApwcvzI3ZuVTY6EjQ4FbY)



For more information about common data uploading errors check out this helpful [documentation](https://docs.mapbox.com/help/troubleshooting/uploads/). 

