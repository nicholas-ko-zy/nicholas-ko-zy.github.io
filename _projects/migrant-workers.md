---
title: Bus Travel Patterns of Migrant Workers
layout: inner
lead_text: Analysis of real-world origin-destination bus travel data around migrant dorms in Woodlands.
project_link: 'nicholas-ko-zy.github.io/projects/migrant-workers.html'
---
# Bus Travel Patterns of Migrant Workers Living in Woodlands Dormitories

_An analysis of origin-destination bus travel data around migrant dorms in Woodlands._

## Background

Migrant worker welfare was severly affected during the Covid pandemic. Many couples were disappointed when construction of BTO (build-to-order) flats was delayed because of the shortage of workers who were held in their dorms because of COVID. News like this revealed Singapore's reliance on migrant workers in key areas like construction.

With the increasing awareness of migrant worker's lack of welfare, this project came from an interest in the travel constraints of migrant workers to reach amenities like healthcare and groceries.  

## Goal

My goal was to understand the following.

1. What kind of amenities are around the migrant worker dorms in Woodlands?

2. Where do migrant workers travel to by bus, and what kind of amenities are there?

## Data 
#### 1. Origin-Destination Bus Travel Data

I obtained bus origin-destination data from LTA's open data platform, LTA DataMall. I was only able to obtain bus travel data for the months of July, August and September because of LTA's limits. 

<html>
<head>
<style>
table, th, td {
  border: 1px solid black;
  border-collapse: collapse;
}
th, td {
  padding: 7px;
}
th {
  text-align: left;
}
</style>
</head>

<font size = "3">
 <table>
  <tr>
    <th>Column Name</th>
    <th>Data Type</th>
    <th>Description</th>
  </tr>
  <tr>
    <td><code>YEAR_MONTH</code></td>
    <td>chr</td>
    <td>Time in YY-MM format.</td>
  </tr>
  <tr>
    <td><code>DAY_TYPE</code></td>
    <td>chr</td>
    <td>The category of data, indicates if the data is for <u>weekdays</u> or <u>weekends</u>.</td>
  </tr>
    <tr>
    <td><code>TIME_PER_HOUR</code></td>
    <td>num</td>
    <td>Time of the data, from 0 to 23</td>
  </tr>
    <tr>
    <td><code>PT_TYPE</code></td>
    <td>chr</td>
    <td>Public transport type, same value for all cells - bus.</td>
  </tr>
    <tr>
    <td><code>ORIGIN_PT_CODE</code></td>
    <td>num</td>
    <td>Unique bus stop code from where the commuter <b>boarded</b> the bus.</td>
  </tr>
    <tr>
    <td><code>DESTINATION_PT_CODE</code></td>
    <td>num</td>
    <td>Unique bus stop code from where the commuter <b>alighted</b> from the bus.</td>
  </tr>
    <tr>
    <td><code>TOTAL_TRIPS</code></td>
    <td>num</td>
    <td>Total number of unique trips made from the same origin-destination pair for that month.</td>
  </tr>
	</table> 
	</font>
</html>

#### 2. Open Street Map (OSM) Bus Route Data

I used Open Street Map to extract the bus routes that pass by the dormitories located around Woodlands industrial area. 

![I downloaded the OSM data using a QGIS plugin called `QuickOSM`](/img/bus/osm.gif)

From OpenStreetMap, I imported the nodes into a CSV data set for cleaning.

![Bus stop data cleaning in RStudio](/img/bus/route_169_df.png)
_(Screenshot of bus 169's route, taken from OpenStreetMap)_

![Bus stop data cleaning in RStudio](/img/bus/cleaning_code.png)
_(A portion of my functional cleaning script. Full notebook is in my [Github](https://github.com/nicholas-ko-zy/dormitory_bus_travel))_

I worked on the weak assumption that migrant workers would use the 2 closest bus stop to their dorm. The image below shows dotted lines that connects dorms in red to the nearest bus stops in yellow.

![Bus stop data cleaning in RStudio](/img/bus/proof_of_concept.png)


## Results
after some processing...

#### Amenities
![](/img/bus/amenities.png)


#### Travel Patterns

<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
*{box-sizing: border-box;}

.column {
  float: left;
  width: 33.33%;
  padding: 5px;
}

.img-zoom-container {
  position: relative;
  width:70%;
  float: left;
}

.img-zoom-lens {
  position: absolute;
  border: 1px solid #d4d4d4;
  /*set the size of the lens:*/
  width: 70px;
  height: 50px;
}

.img-zoom-result {
  border: 1px solid #d4d4d4;
  /*set the size of the result div:*/
  width: 150px;
  height: 150px;
  width:30%;
  float:left;
}

.clearfix{
    clear:both
}
</style>
<script>
function imageZoom(imgID, resultID) {
  var img, lens, result, cx, cy;
  img = document.getElementById(imgID);
  result = document.getElementById(resultID);
  /*create lens:*/
  lens = document.createElement("DIV");
  lens.setAttribute("class", "img-zoom-lens");
  /*insert lens:*/
  img.parentElement.insertBefore(lens, img);
  /*calculate the ratio between result DIV and lens:*/
  cx = result.offsetWidth / lens.offsetWidth;
  cy = result.offsetHeight / lens.offsetHeight;
  /*set background properties for the result DIV:*/
  result.style.backgroundImage = "url('" + img.src + "')";
  result.style.backgroundSize = (img.width * cx) + "px " + (img.height * cy) + "px";
  /*execute a function when someone moves the cursor over the image, or the lens:*/
  lens.addEventListener("mousemove", moveLens);
  img.addEventListener("mousemove", moveLens);
  /*and also for touch screens:*/
  lens.addEventListener("touchmove", moveLens);
  img.addEventListener("touchmove", moveLens);
  function moveLens(e) {
    var pos, x, y;
    /*prevent any other actions that may occur when moving over the image:*/
    e.preventDefault();
    /*get the cursor's x and y positions:*/
    pos = getCursorPos(e);
    /*calculate the position of the lens:*/
    x = pos.x - (lens.offsetWidth / 2);
    y = pos.y - (lens.offsetHeight / 2);
    /*prevent the lens from being positioned outside the image:*/
    if (x > img.width - lens.offsetWidth) {x = img.width - lens.offsetWidth;}
    if (x < 0) {x = 0;}
    if (y > img.height - lens.offsetHeight) {y = img.height - lens.offsetHeight;}
    if (y < 0) {y = 0;}
    /*set the position of the lens:*/
    lens.style.left = x + "px";
    lens.style.top = y + "px";
    /*display what the lens "sees":*/
    result.style.backgroundPosition = "-" + (x * cx) + "px -" + (y * cy) + "px";
  }
  function getCursorPos(e) {
    var a, x = 0, y = 0;
    e = e || window.event;
    /*get the x and y positions of the image:*/
    a = img.getBoundingClientRect();
    /*calculate the cursor's x and y coordinates, relative to the image:*/
    x = e.pageX - a.left;
    y = e.pageY - a.top;
    /*consider any page scrolling:*/
    x = x - window.pageXOffset;
    y = y - window.pageYOffset;
    return {x : x, y : y};
  }
}
</script>
</head>
<body>

<p>You may mouse over the image to inspect the map. Please refresh the page if you have resized the window.</p>
<!-- Dorm 1 -->
<div class="img-zoom-container">
  <img id="myimage" src="/img/bus/indiv/d1.jpg" width="200" height="400"> 
</div>

<div id="myresult" class="img-zoom-result"></div>

<div class="clearfix"></div>

<script>
// Initiate zoom effect:
imageZoom("myimage", "myresult");
</script>
<!-- Dorm 2 -->
<div class="img-zoom-container">
  <img id="myimage2" src="/img/bus/indiv/d2.jpg" width="200" height="400"> 
</div>

<div id="myresult2" class="img-zoom-result"></div>

<div class="clearfix"></div>

<script>
// Initiate zoom effect:
imageZoom("myimage2", "myresult2");
</script>
<!-- Dorm 3 -->
<div class="img-zoom-container">
  <img id="myimage3" src="/img/bus/indiv/d3.jpg" width="200" height="400"> 
</div>

<div id="myresult3" class="img-zoom-result"></div>

<div class="clearfix"></div>

<script>
// Initiate zoom effect:
imageZoom("myimage3", "myresult3");
</script>

<!-- Dorm 4 -->
<div class="img-zoom-container">
  <img id="myimage4" src="/img/bus/indiv/d4.jpg" width="200" height="400"> 
</div>

<div id="myresult4" class="img-zoom-result"></div>

<div class="clearfix"></div>

<script>
// Initiate zoom effect:
imageZoom("myimage4", "myresult4");
</script>

<!-- Dorm 5 -->
<div class="img-zoom-container">
  <img id="myimage5" src="/img/bus/indiv/d5.jpg" width="200" height="400"> 
</div>

<div id="myresult5" class="img-zoom-result"></div>

<div class="clearfix"></div>

<script>
// Initiate zoom effect:
imageZoom("myimage5", "myresult5");
</script>

<!-- Dorm 8 -->
<div class="img-zoom-container">
  <img id="myimage8" src="/img/bus/indiv/d8.jpg" width="200" height="400"> 
</div>

<div id="myresult8" class="img-zoom-result"></div>

<div class="clearfix"></div>

<script>
// Initiate zoom effect:
imageZoom("myimage8", "myresult8");
</script>

<!-- Dorm 9 -->
<div class="img-zoom-container">
  <img id="myimage9" src="/img/bus/indiv/d9.jpg" width="200" height="400"> 
</div>

<div id="myresult9" class="img-zoom-result"></div>

<div class="clearfix"></div>

<script>
// Initiate zoom effect:
imageZoom("myimage9", "myresult9");
</script>

<!-- Dorm 10 -->
<div class="img-zoom-container">
  <img id="myimage10" src="/img/bus/indiv/d10.jpg" width="200" height="400"> 
</div>

<div id="myresult10" class="img-zoom-result"></div>

<div class="clearfix"></div>

<script>
// Initiate zoom effect:
imageZoom("myimage10", "myresult10");
</script>

</body>
</html>

## Summary

Migrant worker dormitories that are located deep within industrial areas lack access to amenities in general. This information may help NGOs such as Healthserve to **prioritise the setting up of services close to dorms that require longer travel times to clinics and supermarkets.** 

Further analysis can explore the shortest route by bus from dormitories to amenities and compare it with those living in HDBs.