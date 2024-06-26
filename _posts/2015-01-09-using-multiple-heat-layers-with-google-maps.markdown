---
layout: single
title: "Using Multiple Heat Layers with Google Maps"
date: 2015-01-09 21:03:17 -0500
date_formatted: January 10, 2015
comments: true
tags:
  - engineering
  - javascript
---

<p>For one of the assignments in Cloud &amp; Big Data course at Columbia University, we had to display a Heat Map Layer showing positive and negative tweets from Twitter.  </p>
<p> Now, using Heatmaps with Google Maps is pretty trivial. Google Maps has pretty decent documentation along with working examples. </p>

<p>But how do we do it when we want to show multiple heat maps working independently on the same Google Map?</p>

<!--more-->

<p> Let's take a closer look at the standard Heatmap example first. </p>
Go through the example given <a href="https://developers.google.com/maps/documentation/javascript/examples/layer-heatmap" title="Heatmaps examples">here</a>.

<p>As we see, the following piece of code is responsible for initializing the heat map. It also has a function called changeGradient() which is of interest to us. </p>

<pre>
<code class="javascript">

function initialize() {
  var mapOptions = {
    zoom: 13,
    center: new google.maps.LatLng(37.774546, -122.433523),
    mapTypeId: google.maps.MapTypeId.SATELLITE
  };

  map = new google.maps.Map(document.getElementById('map-canvas'),
      mapOptions);

  var pointArray = new google.maps.MVCArray(taxiData);

  heatmap = new google.maps.visualization.HeatmapLayer({
    data: pointArray
  });

  heatmap.setMap(map);
}


function changeGradient() {
  var gradient = [
    'rgba(0, 255, 255, 0)',
    'rgba(0, 255, 255, 1)',
    'rgba(0, 191, 255, 1)',
    'rgba(0, 127, 255, 1)',
    'rgba(0, 63, 255, 1)',
    'rgba(0, 0, 255, 1)',
    'rgba(0, 0, 223, 1)',
    'rgba(0, 0, 191, 1)',
    'rgba(0, 0, 159, 1)',
    'rgba(0, 0, 127, 1)',
    'rgba(63, 0, 91, 1)',
    'rgba(127, 0, 63, 1)',
    'rgba(191, 0, 31, 1)',
    'rgba(255, 0, 0, 1)'
  ]
  heatmap.set('gradient', heatmap.get('gradient') ? null : gradient);
}
</code>
</pre>

<p>The changeGradient function changes the default gradient values to anything you specify. If you notice, the color is changing from light blue rgba(0, 255, 255, 0) for low density points to Sharp Red rgba(255, 0, 0, 1) as the density increases.
</p>

<p>What you can simply do is, initiate two different heatmaps, say heatmapPositive (light blue to dark blue) and heatmapNegative (yellow to red) in case you want to show two different heat maps for the positive and negative sentiment tweets. </p>

<p>If you want sample code for this, you can find it <a href="https://github.com/BhavdeepSethi/cloudBigData/blob/master/twitterMapSource/twitMap.js" title="Github Link" >here</a>.  If you want to see how this looks, you can check out <a href="https://www.youtube.com/watch?v=9Qv7F_43dOk" >this video</a> which shows the default state and then with the dual heat maps. </p>

<p> If you want to do much more, there is this neat library called <a href="http://www.patrick-wied.at/static/heatmapjs/" title="heatmap.js">heatmap.js</a>. It has bunch of features like adding legends and tool tips, customized maps, etc.
</p>

<p> Hope you found this useful. If you're aware of any better libraries out there, then Let me know!</p>
