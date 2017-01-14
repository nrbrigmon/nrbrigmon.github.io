---
layout: post
comments: true
title: Five Minute Leaflet Maps with QGIS 
tags: maps census carto table
date: 2016-12-24
author: Nate B
---

### 5 Minute Interactive Web Maps with QGIS

As someone who used to work with a small team and couldn't afford expensive business licenses supported by large organizations, I *love* open source software! QGIS is my numero uno. I've been using QGIS for about two years now and it's amazing to see the progress being made.

If you're in the GIS field and you've never tried QGIS, I suggest you do. If you are looking to get your feet wet, this tutorial will walk you through how to make an interactive web map using [Leaflet][3] in less than 5 minutes!

Here's what we'll be making:

![custom-formatted-popups](/imgs/post-imgs/2016-12-24/2016-12-24-img0.png){:class="img-responsive"}

Finished product above. 
{: .img-caption }

Before we get started, it will be helpful to work with the same dataset. Let's get our data from the US Census. Their [Maps & Data][1] page provides access to a number of different geographies with selected economic and demographic data.

![Tiger Data](/imgs/post-imgs/2016-12-24/2016-12-24-img1.png){:class="img-responsive"}

TIGER/Line® with Selected Demographic and Economic Data. 
{: .img-caption }

Because it's a small and manageable dataset, let's download Block Groups from the District of Columbia under American Community Survey 5-Year Estimates (2010 - 2014).

![DC BGs](/imgs/post-imgs/2016-12-24/2016-12-24-img2.png){:class="img-responsive"}

TIGER/Line® with Selected Demographic and Economic Data. 
{: .img-caption }

Once you have some data and you've downloaded [QGIS][2]. We can get started.

## Interactive Web Map In 5 Minutes

### 1. Add data to map

Open QGIS and add your downloaded data. You may already have data ready, but I downloaded a geodatabase from the Census, so I'll go over how to add it to the map.

To add data, click the Add Vector Layer Icon

![custom-formatted-popups](/imgs/post-imgs/2016-12-24/2016-12-24-img3.png){:class="img-responsive"}

Add Vector Layer
{: .img-caption }

You'll need to specify that the geodatabase is a "Directory" and its type is "OpenFileGDB". Navigate to your GDB file and click Open.

![custom-formatted-popups](/imgs/post-imgs/2016-12-24/2016-12-24-img4.png){:class="img-responsive"}

Source types
{: .img-caption }

Since you are adding a directory, it will give you a list of layers to add. Ton of data to work with! Let's grab Population Counts and the base Geography.

![custom-formatted-popups](/imgs/post-imgs/2016-12-24/2016-12-24-img5.png){:class="img-responsive"}

Select layer
{: .img-caption }

With a table and the base geography, we'll need to perform a join to tie the data together. Right-click on the layer and click Properties.

![custom-formatted-popups](/imgs/post-imgs/2016-12-24/2016-12-24-img6.png){:class="img-responsive"}

Select Properties
{: .img-caption }

On the left-hand side, choose Joins and prepare your join. The join layer is the table, and the fields we want to join are GEOID to GEOID_Data. Usually, you have to investigate before you join, but I've done this sooo many times.

![custom-formatted-popups](/imgs/post-imgs/2016-12-24/2016-12-24-img7.png){:class="img-responsive"}

Setup Join
{: .img-caption }

The Join is a temporary, cached setup. To make it permanent and easier to work with, perform a Save As... and create a new shapefile.

![custom-formatted-popups](/imgs/post-imgs/2016-12-24/2016-12-24-img8.png){:class="img-responsive"}

Save As.. 
{: .img-caption }

![custom-formatted-popups](/imgs/post-imgs/2016-12-24/2016-12-24-img9.png){:class="img-responsive"}

A new shapefile
{: .img-caption }

OK, our data is set. Let's get moving!

### 2. Get Leaflet plug-in

The Leaflet plug-in does **all** the magic. If you don't have it, here's what to do.

Under Plugins in the menu bar, click Manage and Install Plugins...

![custom-formatted-popups](/imgs/post-imgs/2016-12-24/2016-12-24-img10.png){:class="img-responsive"}

Manage and Install Plugins
{: .img-caption }

Then search for the leaflet plug-in called qgis2leaf.

![custom-formatted-popups](/imgs/post-imgs/2016-12-24/2016-12-24-img11.png){:class="img-responsive"}

Sweet, sweet leaflet.
{: .img-caption }

Tons of great plugins out there btw (e.g. MMQGIS). Next, make sure it's visible.

![custom-formatted-popups](/imgs/post-imgs/2016-12-24/2016-12-24-img12.png){:class="img-responsive"}

qgis2Leaf
{: .img-caption }

If it is, move on. If not, troubleshoot. Right-click the toolbar and make sure there is a toolbar selected called "Plugins Toolbar".

![custom-formatted-popups](/imgs/post-imgs/2016-12-24/2016-12-24-img13.png){:class="img-responsive"}

Plugins Toolbar
{: .img-caption }

### 3. Use qgis2leaf Plug-In

Click the Icon. Setup the options properly according to the image below.

1. Make sure your layer is selected.
2. Choose as many Basemaps as you want.
3. Select a good location for your output project folder - the default location is hard to find.
4. Give it a name.

![custom-formatted-popups](/imgs/post-imgs/2016-12-24/2016-12-24-img14.png){:class="img-responsive"}

Export Setup.
{: .img-caption }

Then click Export.

### 4. View Your Map

Congratulations. You've made a map in five minutes. To view your five minutes of greatness, go to the folder's location and open your index.html file with a web browser of your choice.

![custom-formatted-popups](/imgs/post-imgs/2016-12-24/2016-12-24-img17.png){:class="img-responsive"}

Find your folder
{: .img-caption }

![custom-formatted-popups](/imgs/post-imgs/2016-12-24/2016-12-24-img18.png){:class="img-responsive"}

Open index.html with web browser
{: .img-caption }

You will now be looking at an interactive web map that you can place into any website.

![custom-formatted-popups](/imgs/post-imgs/2016-12-24/2016-12-24-img15.png){:class="img-responsive"}

Basic setup.
{: .img-caption }

![custom-formatted-popups](/imgs/post-imgs/2016-12-24/2016-12-24-img16.png){:class="img-responsive"}

Click to see your data.
{: .img-caption }

Congrats!

### More editing

Now that you've got your first map, explore the JavaScript and [leaflet library][3] to figure out how you can make this an even better map!

For example, here's how you'd change the opacity of your layer. Find the function called "doStyletemp". This function was automated in the process qgis2leaf process. Next to the values being returned in the object, make edits to alter the opacity.

~~~ javascript

function doStyletemp(feature) {
                return {
                    color: '#000000',
                    fillColor: '#c85f47',
                    weight: 1.3,
                    dashArray: '',
                    opacity: 1.0,
                    fillOpacity: 0.6
                };

        }

~~~

Also, play around the different base maps available to create a more interesting look.

![custom-formatted-popups](/imgs/post-imgs/2016-12-24/2016-12-24-img0.png){:class="img-responsive"}

Pretty cool, no?
{: .img-caption }

[1]:https://www.census.gov/geo/maps-data/data/tiger-data.html
[2]:http://www.qgis.org/en/site/forusers/download.html
[3]:http://leafletjs.com/