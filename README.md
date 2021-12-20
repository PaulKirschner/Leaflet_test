# Leaflet_test
<!DOCTYPE html>
<html lang="en">
<head>
 <!-- set the charset -->
 <meta charset="utf-8">
 <!-- set make the map responsive -->
 <meta http-equiv="X-UA-Compatible" content="IE=edge">
 <meta name="viewport" content="initial-scale=1,user-scalable=no,maximum-scale=1,width=device-width">
 <meta name="mobile-web-app-capable" content="yes">
 <meta name="apple-mobile-web-app-capable" content="yes">
  <!-- set the title shown in the browser tab -->
 <title>Earthquake Map</title>
  <!-- Links to the leaflet CSS and JS -->
 <link rel="stylesheet" href="https://unpkg.com/leaflet@1.7.1/dist/leaflet.css"
   integrity="sha512-xodZBNTC5n17Xt2atTPuE1HxjVMSvLVW9ocqUKLsCC5CXdbqCmblAshOMAS6/keqq/sMZMZ19scR4PsZChSR7A=="
   crossorigin=""/>
 <script src="https://unpkg.com/leaflet@1.7.1/dist/leaflet.js"
   integrity="sha512-XQoYMqMTK8LvdxXYG3nZ448hOEQiglfqkJs1NOQV44cWnUrBc8PkAOcXy20w0vlaXaVUearIOBhiXZ5V3ynxwA=="
   crossorigin=""></script>
   <!-- Links to the Leaflet-Ruler Plugin -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/gokertanrisever/leaflet-ruler@master/src/leaflet-ruler.css"
integrity="sha384-P9DABSdtEY/XDbEInD3q+PlL+BjqPCXGcF8EkhtKSfSTr/dS5PBKa9+/PMkW2xsY" crossorigin="anonymous">  
<script src="https://cdn.jsdelivr.net/gh/gokertanrisever/leaflet-ruler@master/src/leaflet-ruler.js"
integrity="sha384-N2S8y7hRzXUPiepaSiUvBH1ZZ7Tc/ZfchhbPdvOE5v3aBBCIepq9l+dBJPFdo1ZJ" crossorigin="anonymous"></script>
  <!-- styles for elements html and body: fullscreen  -->
  <script src="earthquake_points.js"></script>
  
 <style>
  html, body {
 	 width: 100%;
     height: 100%;
	 padding: 0;
	 margin: 0;
  }
 </style>	
</head>
<body>
 <!-- set the div element containing the map -->
 <div id="mapid" style="width: 100%; height: 100%; padding: 0; margin: 0;"></div>
 
 
 <script>
 
  // Create Leaflet map
  var mymap = L.map('mapid').setView([51.5, 11.5], 6);
 
  // Add basemaps
  // basemap 1:
  var osm_basemap = L.tileLayer('https://{s}.tile.openstreetmap.de/tiles/osmde/{z}/{x}/{y}.png', {
	  attribution: '<a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>'
  }).addTo(mymap);
 
  // basemap 2:
  var otm_basemap = L.tileLayer('https://{s}.tile.opentopomap.org/{z}/{x}/{y}.png', {
  attribution: '<a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>, <a href="http://viewfinderpanoramas.org">SRTM</a> | Map style: &copy; <a href="https://opentopomap.org">OpenTopoMap</a> (<a href="https://creativecommons.org/licenses/by-sa/3.0/">CC-BY-SA</a>)'
  });
 
  // basemap 3:
  var osmint_basemap = L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
  maxZoom: 19,
  attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
  }).addTo(mymap);
 
  // Basemap dictonary
  var basemaps = {
   "Open Street Map DE": osm_basemap,
   "Open Topo Map": otm_basemap,
   "Open Street Map INT": osmint_basemap
  };
  
    // Add earthquake markers
  var orangeMarker = {
   radius: 8,
   fillColor: "#ff7800",
   color: "#000",
   weight: 1,
   opacity: 1,
   fillOpacity: 0.6};
   
   // Add popup function
function popUpFunction(feature, layer) {
 var popupContent = "<b>Date: </b>"+feature.properties.EV_DATE+
  "<br><b>Latitude: </b>"+ feature.properties.LAT+
  "<br><b>Longitude: </b>"+ feature.properties.LON.toFixed(1)+
  "<br><b>Magnitude: </b>"+ feature.properties['MAG']+
  "<br><b>Intensity: </b>"+ feature.properties.INTENSITY;
 layer.bindPopup(popupContent);
}
  
  var overlay_eq_points = L.geoJSON(eq_points, {
   onEachFeature: popUpFunction,
   pointToLayer: function (feature, latlng) {
   return L.circleMarker(latlng, orangeMarker);
  }}).addTo(mymap);
  
  
  
  // Overlay dictonary
var overlays = {
 "Earthquakes": overlay_eq_points
};
 
// Layer control
var legend = L.control.layers(basemaps,overlays,{collapsed: false}).addTo(mymap);
 
 
  // Layer control
  var legend = L.control.layers(basemaps,null,{collapsed: false}).addTo(mymap);
 
  // Scale
  var scale = L.control.scale({maxWidth: 150, metric: true, imperial: true}).addTo(mymap);
  
 
  // Leaflet-Ruler measure tool
  var options = {
   position: 'bottomleft',
   lengthUnit: {
    factor: 1000,    //  from km to nm
    display: "m",
    decimal:0,
	label: "Distance"
   }
  };
 
  var ruler = L.control.ruler(options).addTo(mymap);

 </script>
 
</body>
</html>