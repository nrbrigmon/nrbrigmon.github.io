---
layout: post
comments: true
title: How To Make Heat Maps in QGIS 
tags: heat maps census spatial analysis
date: 2017-01-07
author: Nate B
---

### How To Make Heat Maps in QGIS

Heat maps are really quick and easy ways to identify hot spots across an area. I don't feel I need to go into detail, but I'm always amazed that many clients unfamiliar with mapping forget they exist. This tutorial will show how to make a simple one in QGIS while avoiding some of the common style pitfalls.

Here's what we'll be making:

![heat-map](/imgs/post-imgs/2017-01-07/2017-01-07-img0.jpeg){:class="img-responsive"}

Finished product above. 
{: .img-caption }

To make this map, we are going to walk through the process of downloading data, cleaning it, and then setting it up for production. To make it interesting, we are going to map where Austin, TX loses most of its pets!

## Mapping Where Austin Loses Pets

### 1. Getting the Data

You can choose any dataset you want when following along, but it would be helpful to work with the same dataset. Let's get our data from the City of Austin's [Open Data portal][1]. The portal provides great access to a number of different local datasets.

![data-portal](/imgs/post-imgs/2017-01-07/2017-01-07-img1.png){:class="img-responsive"}

Austin's Open Data Portal 
{: .img-caption }

Now find "Austin's Found Pets Map" by either going to the [Top 10 Searches Page][2], using the search bar, or click [this link][3].

![austins-pets](/imgs/post-imgs/2017-01-07/2017-01-07-img2.png){:class="img-responsive"}

Austin's Found Pets Map 
{: .img-caption }

The open data portal provides a lot of cool features, but we just want to extract our data and get out of here. To extract, click Export on the top right of the page and then choose: CSV for Excel to get a table. 

![export-data](/imgs/post-imgs/2017-01-07/2017-01-07-img3.png){:class="img-responsive img-medium"}

Exporting data
{: .img-caption }

Once it's done downloading, we have our data, but - like any project - we have to prep!


### 2. Cleaning the Data/Excel Tricks

Open up the dataset in your table viewer of choice. If you don't have access to Excel, get [LibreOffice][4]. It's free, pretty darn good, and a lot of the functions work the same on both.

![excel-data](/imgs/post-imgs/2017-01-07/2017-01-07-img4.png){:class="img-responsive"}

Tabular data
{: .img-caption }

You may notice this data has a "Found Location" column that has both address and latitude/longitude embedded inside of it. That isn't helpful, but thanks to Excel, there are some simple ways to extract what we need!

Our goal is to extract only the information within the parentheses and between the commas, then place it into new columns.

![extracting-data](/imgs/post-imgs/2017-01-07/2017-01-07-img5.png){:class="img-responsive"}

Extracting data
{: .img-caption }

Here's how we plan to do this through excel, using the first column as our template...

1. If no parentheses, pass

~~~ excel
=IF(ISNUMBER(SEARCH("(",B2)),<<<ACTION>>>,<<<N/A>>>)
~~~
Explanation: ISNUMBER acts *kind of* like the IndexOf function in JavaScript. If it finds our string it returns true, if not - false.

2. If parentheses, extract from the comma "," to the last parenthesis. So, <<ACTION>> becomes: 

~~~ excel
=MID(B2,FIND("(",B2,1)+1,FIND(",",B2,1)-1-FIND("(",B2,1))
~~~
Explanation: MID grabs pieces of the cell depending on the position we want. We start with the position of the "(", the measure the length from "(" to "," to get our data. the +1 and -1 are to make our selection exclusive of the "(" and ",".

3. Trim to remove white space

~~~ excel
=TRIM(<<ACTION>>)
~~~
Explanation: Sometimes our data isn't perfect and may include whitespace. This function ensures we get rid of it.

4. Putting it all together:

~~~ excel
=IF(ISNUMBER(SEARCH("(",B2)),TRIM(MID(B2,FIND("(",B2,1)+1,FIND(",",B2,1)-1-FIND("(",B2,1))),"")
~~~

![exceling-data](/imgs/post-imgs/2017-01-07/2017-01-07-img6.png){:class="img-responsive"}

Our fabulous Excel function
{: .img-caption }

So, we'll create a Longitude and Latitude column and use our cool function to parse the data. Obviously, for the Latitude column, we wil swap out "(" for "," and change "," to ")". 

Once we have our data, save the CSV and open QGIS!

***UPDATE:*** I noticed that QGIS has a hard time reading CSVs that come from EXCEL. So, when you finish - download LibreOffice! Open it up and copy-paste the contents of your Excel file into a LibreOffice file and save it as a Text CSV. Then, it will be QGIS-readable. Strange bug!

### 3. Mapping Lost Pets

To add any kind of data to the viewer in QGIS, you use the buttons on the left-hand panel of the viewer. To add a delimited text file, click the appropriate button:

![import-button](/imgs/post-imgs/2017-01-07/2017-01-07-img7.png){:class="img-responsive img-medium"}

Import TXT/CSV Button
{: .img-caption }

You will know if the import is going well if you can see the column and row information at the bottom of the importer window.

![tabular-data](/imgs/post-imgs/2017-01-07/2017-01-07-img8.png){:class="img-responsive"}

Importing Tabular Data
{: .img-caption }

Also, important to note: "X field" should hold Latitude data and "Y field" should hold Longitude data. Once everything looks good, click OK.

![tabular-data](/imgs/post-imgs/2017-01-07/2017-01-07-img9.png){:class="img-responsive"}

View of our Data
{: .img-caption }

By now you should be seeing a various points on the canvas. This doesn't really help you understand the locations very well, so we will need context. The best way to get context is by adding a Base Map. If you don't already have Base Maps, you will need the OpenLayers QGIS Plugin. So, let's go ahead and get that plus our Heat Map Plugin which we'll need later.

![tabular-data](/imgs/post-imgs/2017-01-07/2017-01-07-img10.png){:class="img-responsive img-medium"}

Getting Plugins
{: .img-caption }

In the menu bar, select Plugins and then "Manage and Install Plugins". This will bring up our Plugin Manager. Type in and download/install both the Heatmap and OpenLayers plugin.

![tabular-data](/imgs/post-imgs/2017-01-07/2017-01-07-img11.png){:class="img-responsive"}

Plugin Manager
{: .img-caption }

![tabular-data](/imgs/post-imgs/2017-01-07/2017-01-07-img12.png){:class="img-responsive"}

Plugin Manager
{: .img-caption }

One of the best things about QGIS is the ease of installing and using their plugins. Once you've downloaded the two above, you will notice some of your menu options have changed. Go ahead and add your base map of choice.

![tabular-data](/imgs/post-imgs/2017-01-07/2017-01-07-img13.png){:class="img-responsive"}

Adding a base map
{: .img-caption }

I always find analyzing data with a base map makes an immediate improvement.

![tabular-data](/imgs/post-imgs/2017-01-07/2017-01-07-img14.png){:class="img-responsive"}

Putting your data in context
{: .img-caption }

Now let's make the heat map...

### 4. Creating the Heat Map

With the new heat map plugin, this process is a snap. In the menu, go to Raster, Heatmap, Heatmap.

![tabular-data](/imgs/post-imgs/2017-01-07/2017-01-07-img15.png){:class="img-responsive"}

Heatmap options
{: .img-caption }

It will bring up the Heatmap window.

![tabular-data](/imgs/post-imgs/2017-01-07/2017-01-07-img16.png){:class="img-responsive"}

Heatmap options
{: .img-caption }

When you are filling in these options experiment! Kernel shape, Output format, Radius, Cell size, etc. I've found that I usually make 4 or 5 attempts before I get what I want. And because this process takes only 15 seconds, it's not hard to repeat.

![tabular-data](/imgs/post-imgs/2017-01-07/2017-01-07-img17.png){:class="img-responsive"}

Heatmap!
{: .img-caption }

When you first map it, it will be a grey scale raster. We want to make this a bit prettier, no? Right-click on your raster and click Properties.

![tabular-data](/imgs/post-imgs/2017-01-07/2017-01-07-img18.png){:class="img-responsive"}

Heatmap options
{: .img-caption }

I've found the key to good heatmaps is subtle transparency and a good color scale. Again, experiment! What I do is make the lowest levels increasingly transparent. Just follow along with the remaining screenshots and you are done!

![tabular-data](/imgs/post-imgs/2017-01-07/2017-01-07-img19.png){:class="img-responsive"}

Change Render type to Singleband pseudocolor
{: .img-caption }

![tabular-data](/imgs/post-imgs/2017-01-07/2017-01-07-img20.png){:class="img-responsive"}

Experiment with good spectrums
{: .img-caption }

![tabular-data](/imgs/post-imgs/2017-01-07/2017-01-07-img21.png){:class="img-responsive"}

Make the very bottom end of the spectrum very transparent
{: .img-caption }

![tabular-data](/imgs/post-imgs/2017-01-07/2017-01-07-img22.png){:class="img-responsive"}

Make the second to last data class a little transparent as well
{: .img-caption }

![tabular-data](/imgs/post-imgs/2017-01-07/2017-01-07-img23.png){:class="img-responsive"}

Add transparency to the entire layer
{: .img-caption }

![tabular-data](/imgs/post-imgs/2017-01-07/2017-01-07-img24.png){:class="img-responsive"}

Experiment with base maps!
{: .img-caption }

![tabular-data](/imgs/post-imgs/2017-01-07/2017-01-07-img25.png){:class="img-responsive img-medium"}

Save as an Image or use the Print Composer and you're done!
{: .img-caption }



[1]:https://data.austintexas.gov/
[2]:https://data.austintexas.gov/browse?sortBy=most_accessed
[3]:https://data.austintexas.gov/Government/Austin-Animal-Center-Found-Pets-Map/hye6-gvq2
[4]:https://www.libreoffice.org/