---
layout: post
comments: true
title: Get Started With Carto
tags: maps census carto table
date: 2016-12-17
author: Nate B
---

### How to Make Interactive Maps With Carto

I recently just finished a web map I made for a client and I really enjoyed using the tools available from Carto. Here's a look at the finished product:

![image-title-here](/imgs/post-imgs/2016-12-17/2016-12-17-img1.png){:class="img-responsive"}

Finished product above. 
{: .img-caption }

A lot of time went into making this map from scratch and I can't cover everything I did in one post. What I'll do is go over the very basic steps to get started and then you can check out the documentation on Carto's website to continue onward!

So, let's get started!

## Setting up a Map with Carto's API
Steps:
1. Setup Carto Account
2. Add Data to Carto
3. Setup Web Files
4. Connect with JavaScript

### 1. Setup Carto Acount 
Go to [Carto's site][1] and register an account. One thing I noticed, when setting up an account for someone else, either skip the identification step OR mark yourself as a developer since I think they give you access to different UIs and Tools depending on who you identify yourself as. I tried to register someone else to Carto and found they had a completely different UI than mine, which also lacked the tools I would tell them to use.

Hopefully, when you're finished - it takes like two minutes - you're UI will look like this:

![image-title-here](/imgs/post-imgs/2016-12-17/2016-12-17-img2.png){:class="img-responsive"}

### 2. Add Data to Carto
OK, now hopefully you've got some polygons or tables with geospatial data. In my case, I used block groups from the [US Census][2]. Their site is great as you can even download files with preloaded data. I'm a [QGIS][3] guy, so I edit my shapefile, joined it with my custom data, and then did a "Save As" KML. Either way, once you have your polygons with data, add it to carto. Drag and drop it!

![image-title-here](/imgs/post-imgs/2016-12-17/2016-12-17-img3.png){:class="img-responsive"}

Here, I usually delete the unneccesary columns because they just clutter the workflow. Now we have our data in Carto. Select **CREATE MAP** in the table view, generally at the bottom right. Now, before you go, grab two pieces of information which you can find in the browser view of your map:

![image-title-here](/imgs/post-imgs/2016-12-17/2016-12-17-img4.png){:class="img-responsive"}

Your username and map ID will be important later. Copy the info and paste it somewhere safe. Now, it's time to setup the web files.

### 3. Setup Web Files

There exists a **super** easy way to make leaflet maps with QGIS, which I'll document in another post later, but for now I'll just cover the basics for setting up a web map.

#### HTML

~~~ html
<html>
	<head>
	    <title>Interactive Web Map</title>
	    <meta charset="utf-8">
	    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
	    <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1, maximum-scale=1">
	    <!-- CSS -->
	    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.0.2/leaflet.css" />
	    <link rel="stylesheet" href="http://libs.cartocdn.com/cartodb.js/v3/3.15/themes/css/cartodb.css" />
	    <link rel="stylesheet" href="customStyleSheet.css" />
	    <!-- JavaScript -->
	    <script src="http://libs.cartocdn.com/cartodb.js/v3/3.15/cartodb.js"></script>
	    <script src="customMapFunctions.js" /></script>
	</head>
	<body>
		<div id="map"></div>
	</body>		
</html>
~~~
You won't need much here. You'll want to link to the CDNs for Leaflet and Carto. Cartodb.js already has connections to leaflet.js, so you can skip adding an extra leaflet.js link.

In the body, all you'll need is one div for now.

#### CSS

~~~ css
html,
body,
#map {
    height: 100%;
    width: 100%;
    padding: 0;
    margin: 0;
}
~~~

In the stylesheet, all you need is something to style to your map div. You can add more later.

#### JavaScript

JavaScript is where the magic happens. I'll cover the Leaflet basics. This script below, is my starter template I've used on multiple projects.

~~~ javascript
var map;

jQuery(document).ready(function($){
    'use strict';
    /* Setup Leaflet map specs */
    map = L.map('map', {
        center: [38.8995318, -77.1549959],
        zoom: 9,
        zoomControl: false,
        scrollWheelZoom: false,
        maxZoom: 18,
        inertia: true,
        inertiaDeceleration: 6000
    });

    /* helpful if you want track map lat-lngs */
    // var hash = new L.Hash(map); 

    /* Setup Leaflet map specs */
    var additional_attrib = 'Template by Yours Truly<br>';

    /* Basemap layers */
    var basemap_0 = L.tileLayer('http://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: additional_attrib + '&copy; <a href="http://openstreetmap.org">OpenStreetMap</a> contributors,<a href="http://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>'
    });
    var basemap_1 = L.tileLayer('http://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}.png', {
        attribution: additional_attrib + '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, &copy; <a href="http://cartodb.com/attributions">CartoDB</a>'
    });
    var basemap_2 = L.tileLayer('http://{s}.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}.png', {
        attribution: additional_attrib + '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, &copy; <a href="http://cartodb.com/attributions">CartoDB</a>'
    });
    var basemap_3 = L.tileLayer('http://a.tile.stamen.com/terrain/{z}/{x}/{y}.png', {
        attribution: additional_attrib + 'Map tiles by <a href="http://stamen.com">Stamen Design</a>, <a href="http://creativecommons.org/licenses/by/3.0">CC BY 3.0</a> &mdash; Map data: &copy; <a href="http://openstreetmap.org">OpenStreetMap</a> contributors,<a href="http://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>'
    });

    /* Adds your selected Base Map */
    basemap_1.addTo(map);

    /* This object lets you choose various base maps for using the UI */
    var baseMaps = {
        'Standard (OpenStreetMap)': basemap_0,
        'Positron (CartoDB)': basemap_1,
        'Dark Matter (CartoDB)': basemap_2,
        'Terrain (Stamen)': basemap_3
    };

    /* Leaflet control object for positioning stuff giving user interaction options*/
    L.control.layers(baseMaps, {}, { collapsed: true }).addTo(map);
    L.control.zoom({ position: 'topright' }).addTo(map);
});

~~~

![image-title-here](/imgs/post-imgs/2016-12-17/2016-12-17-img5.png){:class="img-responsive"}

Basic Leaflet setup. 
{: .img-caption }

OK, that's the basic Leaflet setup. Next, we'll connect to Carto!

### 4. Connect with JavaScript

The Carto setup is really easy! Remember that link you copy/pasted and placed somewhere nice and neat? Grab the id and map ID and follow the code:

~~~ javascript
	var cartoUsername = 'nateakadino';
	var mapID = 'd0f9d614-c565-11e6-a877-0ee66e2c9693';

    var cartoUrl = 'https://'+cartoUsername+'.carto.com/api/v2/viz/'+mapID+'/viz.json';

    // section for adding map data
    cartodb.createLayer(map, cartoUrl)
    	// this .addTo piece is giving it a Z-Index to ensure it stays on top of your base map (i.e. carto feature/bug)
        .addTo(map, 99)    
        .on('done', function(layer) {
            console.log('layer(s) added to carto/leaflet map...');

        }).on('error', function() {
            cartodb.log.log("some error occurred");
        });
~~~
![image-title-here](/imgs/post-imgs/2016-12-17/2016-12-17-img6.png){:class="img-responsive"}

Boom.
{: .img-caption }

Now you're in business! Visit the Carto dasboard to add popups, interactivity, or any kind of style you want! Their docs are pretty good compared to many libraries.


[1]:https://carto.com/signup/
[2]:https://www.census.gov/geo/maps-data/data/tiger.html 
[3]:http://qgis.org/en/site/