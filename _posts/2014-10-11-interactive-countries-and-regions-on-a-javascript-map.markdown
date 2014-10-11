---
layout:     post
title:      Interactive Countries and Regions on a JavaScript Map
date:       2014-10-11 2:15:19
summary:    A summary of the possible libraries and frameworks that can be used to visualize geographic data on a map. I had to do a lot of research to find exactly what I needed for my site, so I thought it'd be useful to sum it all up here.
categories: 
---

<head>
	<script type="text/javascript" src="https://www.google.com/jsapi"></script>
	<script type="text/javascript">
	google.load("visualization", "1", {packages:["geochart"]});
	google.setOnLoadCallback(drawRegionsMap);

	function drawRegionsMap() {

		var data = google.visualization.arrayToDataTable([
			['Country', 'Popularity'],
			['Germany', 200],
			['United States', 300],
			['Brazil', 400],
			['Canada', 500],
			['France', 600],
			['RU', 700]
			]);

		var options = {};

		var chart = new google.visualization.GeoChart(document.getElementById('regions_div'));

		chart.draw(data, options);
	}
	</script>
</head>

One of the most difficult challenges I had while developing a working prototype of Atlas was figuring out the best way to obtain clickable, colorful countries and regions on a world map using JavaScript. I’d never really worked with any kind of map or location API before, so I had to do a lot of research to figure out which option best fit my needs. There are a lot of mapping libraries and frameworks out there, and it can be hard to parse through the different options for a task like this. So, in this post, I’ll summarize the different technologies I experimented with, along with some specific recommendations based on what exactly each one seems to be best built for.


##### Here's how the basic profile page for [Atlas](http://www.myatlas.me) ended up looking. Each user can choose their own countries and regions and style their map dynamically.
![Basic profile page for Atlas](/assets/map_example.png)

## [Google Geocharts](https://developers.google.com/chart/interactive/docs/gallery/geochart)
Predictably, my first thought for some kind of map-based API was Google Maps, so I did a bit of digging to see how individual regions on a map could be styled. As it turned out, the easiest way to obtain the data for country borders and other geographic data was by using the Geocharts in the Google Charts API. 
<br>

##### Interactive region geochart example from the [documentation](https://developers.google.com/maps/documentation/javascript/fusiontableslayer)
<div id="regions_div" style="width: 720px; height: 400px;"></div>
<br>

Geocharts are rendered using SVG or VML and are great for visualizing static location-based data with markers and colors. Unfortunately for me, I needed interactive maps, so my search continued. 

## [Google Maps + Fusion Tables](https://developers.google.com/maps/documentation/javascript/fusiontableslayer)
I looked around Google’s vast range of developer resources some more and came across Fusion Tables layers as part of the Google Maps JavaScript API. These are pretty cool because they render data from SQL-like tables with a RESTful API onto a Google Map. For example, if you had country population data available in Fusion Tables, you could easily visualize it by region on a map. 

##### [Example](https://developers.google.com/maps/documentation/javascript/examples/layer-fusiontables-styling) of a Google Maps Fusion Tables Layer with styling

![Fusion Tables layer example](/assets/fusion_tables.png)

Moreover, you can easily add click events, which can be used to pop up infoboxes with additional data, for example. However, the problem for me was that you can only have a maximum of four different layers on a map at once, which greatly limits the ability for multiple users to create custom maps on the fly.

## [Kartograph](http://kartograph.org/)
Since these didn’t appear to be doing me any good, I searched for some non-Google alternatives and found the Kartograph platform. I didn’t delve too deeply into this, but basically it’s built on top of jQuery and a vector graphics library called [Raphaël.js](http://raphaeljs.com/) and lets you create interactive SVG maps of pretty much anything. 

##### One of the many, many examples of Kartograph maps. Check out their [showcase](http://kartograph.org/showcase/) for other cool possibilites.

![German visitors to the Kartograph website in 2012](/assets/kartograph.png)

[Read the JavaScript docs](http://kartograph.org/docs/kartograph.js/) if you’re interested, but it seems like as long as the data exist, there’s no limit to how you can visualize it. While this is great if you’re already familiar with GDAL, the amount of functionality provided seemed like overkill for my purposes, since all I needed was a simple world map with country borders. 

## [Google Maps + Data Layer](https://developers.google.com/maps/documentation/javascript/datalayer)
After experimenting with what seemed like pretty much everything, I finally realized that Google might have had what I needed this whole time. Specifically, the data layer of the Google Maps API lets you load GeoJSON data on top of a map, combining easy-to-use raw data with the built-in functionality and event handling of Google Maps. 

##### [Example](https://developers.google.com/maps/documentation/javascript/examples/layer-data-dynamic) of a Google Maps Data Layer with dynamic styling

![Data Layer with dynamic styling example](/assets/data_layer.png)

In my application, I ended up just keeping the geographic data all in one place, rendering the same country borders for every user while letting them customize their own styling on top. This was exactly what I was looking for all along.


## Recommendation
If you just need a simple way to visualize location-based data, use a __Google Charts Geochart__. If you have a large set of predetermined data to visualize and you want it to be interactive, try using a __Google Map with a Fusion Tables layer__. If you want a ton of customizability and more types of visualizations you could ever need, give __Kartograph__ a shot. But if you’re like me and all you need are personalized, interactive maps for multiple users, go with __Google Maps API and a GeoJSON data layer__. 