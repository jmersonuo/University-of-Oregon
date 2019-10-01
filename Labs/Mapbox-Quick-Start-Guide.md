## Mapbox Quick Start Guide

This step-by-step guide will quickly get you started on Mapbox basics, including setting up a Mapbox map, working with markers, circles and popups and setting up events! All of the code below can be found in this handy [JSFiddle](https://jsfiddle.net/mjdanielson/9jh3bzc8/1/).

### Preparing your page

Before writing any code for the map, you need to do the following preparation steps on your page:

* Include Mapbox JavaScript file:

```
<script src='https://api.tiles.mapbox.com/mapbox-gl-js/v1.1.0/mapbox-gl.js'></script>
    
```

* Include Mapbox CSS file __after__ Mapbox’s JavaScript:

```
<link href='https://api.tiles.mapbox.com/mapbox-gl-js/v1.1.0/mapbox-gl.css' rel='stylesheet' />
```

Put a div element with a certain id where you want your map to be:

```
<div id='map'></div>

```

You will also want to apply some CSS to visualize what the layout looks like. This is particularly important for the map div, which won't show up on the page until you give it a height:

```
        body { margin:0; padding:0; }
        #map { position:absolute; top:0; bottom:0; width:100%; }
```

Now you’re ready to initialize the map and do some stuff with it.

### Setting up the map

Let’s create a map of the centered on Portland with the beautiful Mapbox Streets style. 

The first thing you'll need to do is add your access token. Without this, the rest of the code will not work. Note: all the following code should be between script tags.:

```
mapboxgl.accessToken = 'pk.eyJ1IjoibWpkYW5pZWxzb24iLCJhIjoiY2p2bzFlbnZ5MW5pbTN5cGJ2YWp2MW9vaiJ9.kAaZq3iyJwvrMLK7XDs_qw';

```

_To use any of Mapbox's tools, APIs, or SDKs, you'll need a Mapbox [access token](https://docs.mapbox.com/help/glossary/access-token/). Mapbox uses access tokens to associate API requests with your account. You can find your access tokens, create new ones, or delete existing ones on your [Access Tokens page](https://account.mapbox.com/access-tokens/) or programmatically using the [Mapbox Tokens API](https://docs.mapbox.com/api/accounts/#tokens)._


Next, we’ll initialize the map and set its view to our chosen geographical coordinates and a zoom level.

<p align="center">
    <img src= "https://github.com/mjdanielson/University-of-Oregon/blob/master/Labs/Mapbox-Quick-Start/Images/01_Portland.png">
  </p>
 

For this section of code, we will need a [style ID](https://docs.mapbox.com/help/glossary/style-id/).  A style ID is a unique identifier for each style associated with any Mapbox username. To use the Mapbox Styles API, you will need to know the style ID for the map style you are working with.

You will also need the coordinates for Portland, Oregon. You can use find the coordinates for Portland using http://geojson.io or by replacing the longitude field witih: -122.6788 and the latitude field with: 45.5212.

```
var map = new mapboxgl.Map({
    container: 'map', // container id
    style: 'mapbox://styles/mapbox/streets-v11', // stylesheet location
    center: [lng, lat] // starting position [lng, lat] eg. [-122.6788, 45.5212]
    zoom: 11 // starting zoom 
});
```

That’s it! You have a working Mapbox map now. Try playing with the zoom level to see how this impacts the map. Bonus, try setting the coordinates to Burlington, Vermont. 

When you have finished experimenting, make sure your code is set to Portland and your zoom level is set to 11 before moving on.

<p align = "center">
<img src = "https://media.giphy.com/media/xT0xezQGU5xCDJuCPe/giphy.gif">
</p>

### Adding a marker

Besides a basemap, you can easily add other things to your map, including markers and popups.

Let’s [add a marker](https://docs.mapbox.com/mapbox-gl-js/api/#marker) to the same longitude/latitude that we centered our map on:

```
var marker = new mapboxgl.Marker()
  .setLngLat([replace with longitude, replace with latitude]) // starting position [lng, lat] 
  .addTo(map);
```

<p align = "center">
    <img src="https://github.com/mjdanielson/University-of-Oregon/blob/master/Labs/Mapbox-Quick-Start/Images/Portland_Marker.png">
 </p>


Let's change the color of our marker! The default color for the marker is '#3FB1CE' (this is the hex code for the blue marker). Add a color parameter to your code and set the hex color code! In this example, the color is set to '#42f469'. 

```
var marker = new mapboxgl.Marker({color:'#42f569'})
  .setLngLat([-122.6788, 45.5212]) // starting position [lng, lat] 
  .addTo(map);
```

### Working with popups

<p align = "center">
	<img src ="https://github.com/mjdanielson/University-of-Oregon/blob/master/Labs/Mapbox-Quick-Start/Images/Portland_Markers.png">
</p>

Popups are usually used when you want to attach some information to a particular object on a map. In Mapbox, you can [add a popup](https://docs.mapbox.com/mapbox-gl-js/api/#popup) to your features with only a few lines of code! 

First, you will need to initialize your pop-up variable. Make sure this variable is above your marker variable: 

```
var popup = new mapboxgl.Popup({ offset: 25 })
.setText('Hello World. Welcome to Portland!');
```

Next add the .setPopup function to your marker variable:

```
var marker = new mapboxgl.Marker()
    .setLngLat([-122.6788, 45.5212])
    .setPopup(popup) //add the popup to the marker 
    .addTo(map);
```

You can also use popups as layers (when you need something more than attaching a popup to an object):


```
var popup_layer = new mapboxgl.Popup({closeOnClick: false}) 
.setLngLat([-122.64, 45.5]) //popup coordinates
.setHTML('<h1>Hi Portland!</h1>') //popup text
.addTo(map); //add this popup to the map!
```

Try changing the close on click to 'true' and running your code. What happens when you click anywhere in the map? 

Take a look at the [popup](https://docs.mapbox.com/mapbox-gl-js/api/#popup) documentation to learn more about the parameters associated with Mapbox popups. Try adjusting one or more parameters - for instace, try changing the anchor position. 


```
      var popup_layer = new mapboxgl.Popup({
          closeOnClick: true, anchor: 'top-right'
        })
        .setLngLat([-122.64, 45.5])
        .setHTML('<h1>Hi Portland!</h1>')
        .addTo(map);
```

### Congratulations! You've completed the exercise! 

<p align = "center">
	<img src="https://media.giphy.com/media/11uArCoB4fkRcQ/giphy.gif">
	</p>
