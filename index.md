---
layout: default
---

This project is carried our in the frame of KPP-CIP projects in 2018 and 2019. The project **PPCIP2018 IV INNO and KPPCIP2019 Satellieten en bathymetrie voor monitoring kustmorfologie** proposed by  _Deltares_ has been awarded by _Rijkswaterstaat_ in November 2017. 

* * *
## [](#map2019) SDB Map 2019

<div id="map" style="min-height: 600px">
  <div id="timelabel"></div>
  <div id="panel" class="pin-bottom pt36 bg-white z100 ">
      <div class="bg-white round relative w600">
          <div class="pin-top pad1 z200 col12 bg-white">
              <input class="timeslider col12 fr" id="timeslider" title="slider" type="range" min="0" max="1.5" step="0.1" value="0" />
              <input class="speedslider col12 fr" id="speedslider" title="slider" type="range" min="0.25" max="0.5" step="0.005" value="0.375" />
          </div>
          <div id="playbutton" class="button z100" onclick="togglePlayback()">Play</div>
      </div>
  </div>
</div>


<script>
  mapboxgl.accessToken = 'pk.eyJ1IjoiZ2VubmFkaXkiLCJhIjoiMm9WN3VWQSJ9.b9s_EXvxcqiAbaf5GzrEnA'

  // let tilesPath = "https://storage.googleapis.com/deltares-video-map/mapbox-test/test1/{z}/{x}/{y}.webm"
  let tilesPath = "https://storage.googleapis.com/deltares-video-map/sdb-rgb-v3.3-video/{z}/{x}/{y}.webm"
  // let tilesPath = "http://localhost:9966/media/sdb-rgb-test4-video/{z}/{x}/{y}.webm"
  // let tilesPath = "http://127.0.0.1:4000/media/sdb-rgb-test4-video/{z}/{x}/{y}.webm"

  let zoomMin = 4
  let zoomMax = 13

  let map = null

  let times = [
    "2015-01-01 ... 2017-01-01",
    "2015-04-01 ... 2017-04-01",
    "2015-07-01 ... 2017-07-01",
    "2015-10-01 ... 2017-10-01",
    "2016-01-01 ... 2018-01-01",
    "2016-04-01 ... 2018-04-01",
    "2016-07-01 ... 2018-07-01",
    "2016-10-01 ... 2018-10-01",
    "2017-01-01 ... 2019-01-01",
    "2017-04-01 ... 2019-04-01",
    "2017-07-01 ... 2019-07-01",
    "2017-10-01 ... 2019-10-01",
    "2018-01-01 ... 2020-01-01",
    "2018-04-01 ... 2020-04-01",
    "2018-07-01 ... 2020-07-01",
    "2018-10-01 ... 2020-10-01"
  ]

  var videoStyle = {
      "version": 8,
      "sources": {
          "source1": {
              "type": "video-tiled",
              "tiles": [ tilesPath ],
              tileSize: 256,
              playbackRate: 0.375,
              minzoom: zoomMin,
              maxzoom: zoomMax,
              scheme: "xyz",
              geometry: []
          },
          "satellite": {
              "type": "raster",
              "url": "mapbox://mapbox.satellite",
              "tileSize": 256
          }
      },
      "layers": [{
          "id": "background",
          "type": "background",
          "paint": {
              "background-color": "rgb(4,7,14)"
          }
      }, {
          "id": "satellite",
          "type": "raster",
          "source": "satellite"
      }, {
          "id": "layer1",
          "type": "raster",
          "source": "source1"
      }]
  };

  function onLoad() {
    map = new mapboxgl.Map({
      container: 'map',
      // pitch: 42, // pitch in degrees
      // bearing: 26, // bearing in degrees    
      zoom: 9.876559623103208,
      center: [6.657906115369997, 53.58250382811332],
      style: videoStyle
      // style: 'mapbox://styles/mapbox/dark-v9'
    });
    
    // add controls
    map.addControl(new mapboxgl.NavigationControl(), 'top-right');
    map.addControl(new mapboxgl.GeolocateControl());
    // map.addControl(new mapboxgl.AttributionControl({compact: true}))
    // map.addControl(new mapboxgl.ScaleControl());
    map.addControl(new mapboxgl.FullscreenControl());  

    let slider = document.getElementById('timeslider')
    let timelabel = document.getElementById('timelabel')

    map.on('style.load', () => {
        let player = map.getSource('source1').player

        player.onTimeChanged = t => {
            slider.value = t
            timelabel.textContent = 'Time: ' + times[Math.floor(t * 10)]
        }

        slider.addEventListener('input', function() {
            let currentTime = parseFloat(this.value)

            player.setCurrentTime(currentTime)
        });

        let sliderSpeed = document.getElementById('speedslider')

        sliderSpeed.addEventListener('input', function() {
            speed = parseFloat(this.value)
            getPlayer().playbackRate = speed
        });
    })
  }

  let playing = false;

  function getPlayer() {
      return map.getSource('source1').player
  }

  function togglePlayback() {
      playing = !playing;

      let button = document.getElementById('playbutton')
      button.innerText = playing ? 'Pause' : 'Play'

      if(playing) {                             
          let dt = 50 // timer in ms
          let step = 0.1 // step to seek for the next frame, depends on video FPS
          getPlayer().playbackRate = parseFloat(document.getElementById('speedslider').value)
          getPlayer().play(dt, step)
      } else {
          getPlayer().pause()
      }
  }

</script>

## [](#intro)Introduction

Within this project, _Deltares_ is asked to look at available satellite images in the period 2015-2017 and explore ways to derive bathymetry from those images. Eventually Deltares provides a calibrated bathymetry for two areas of interest and compares the obtained bathymetry with _in-situ_ data collected via standard measurement methods.

## [](#areas)Areas of interest

The two areas are both located in the North Sea area: the inlet of Ameland island in the Wadden Sea and the Western Scheldt outerdelta. These areas are characterised by active coastal morphodynamics, especially in the shallow waters. The use of remote sensing images for these areas helps to capture higher frequency dynamics, too costly and spatially confined to be fully detected by standard in-situ measurements.

<a href="assets/images/roi.png"><img src="assets/images/roi.png" alt="hi" class="inline"/></a>


## [](#data)Data availability

As a rich data country, The Netherlands is monitoring its 350 km of coast at a yearly rate. For the purpose of his study, several datasets of measurement observations are explored, analysed, and compared against remote sensing products. Here is a list of data used during the project.

### [](#insitu)In-situ data
* **[Water levels](http://matroos.deltares.nl/)**. Measurements from devices in the surroundings of the areas of interest have been downloaded from Matroos.
* **[Jarkus transects](https://github.com/openearth/jarkus)**. Profiles measured along the Dutch coast, measured on a yearly basis with singlebeam mounted on boats and lidar sensor mounted on planes. This dataset is mainly used for validation of satellite-derived products. 
* **[Vaklodingen 2d map](http://opendap.deltares.nl/thredds/catalog/opendap/rijkswaterstaat/vaklodingen/catalog.html)**. Multibeam measurements performed with ships. Approximately 3 years of overpass, depending on the area. This dataset is mainly used for validation of satellite-derived products. 
* **LIDAR 2d map**. Measurements from airborne mounted radar operating in the optical spectrum.

**Water levels** 
[Google Earth Engine code](https://code.earthengine.google.com/e02d1005a3aac1c10d09fe85f23a8edd) which imports shapefiles of water level data, and displays the locations of the buoys and plots a subset of this data.

**Jarkus transects**
<div id="images">
  <a href="assets/images/westerschelde_jarkus_transects.png">
  <img class="doublefig" src="assets/images/westerschelde_jarkus_transects.png" alt="hi"  class="inline" width="48%"/></a>
  <a href="assets/images/ameland_jarkus_transects.png">
  <img class="doublefig" src="assets/images/ameland_jarkus_transects.png" alt="hi"  class="inline" width="48%"/></a>
</div>
Jarkus transects for the Westerschelde (left) and Ameland (right) regions.

**RWS Vaklodingen Data**
<div id="images">
  <a href="assets/images/westerschelde_rws_vaklodingen.png">
  <img class="doublefig" src="assets/images/westerschelde_rws_vaklodingen.png" alt="hi" class="inline" width="48%"/></a>
  <a href="assets/images/ameland_rws_vaklodingen.png">
  <img class="doublefig" src="assets/images/ameland_rws_vaklodingen.png" alt="hi" class="inline" width="48%"/></a>
</div>
Vaklodingen data for the Westerschelde and Ameland regions. A total of 90 Vaklodingen images, measured annually, between 2010 and 2015 are combined to create a mosaic of bathymetry along the Dutch coastline.

### [](#satellites)Remote sensing data
* **Sentinel2**. European EO mission launched in 2015 (A) and 2017 (B). It provides multi-spectral data in the visible, near infrared and short wave infrared part of the spectrum.
* **LandSat8**. American EO mission launched in 2013. It provides multi-spectral images thanks to the OLI (Operational Land Imager) and Thermal InfraRed sensors.
* **TripleSat**. Commercial Satellite sensor, made available by NSO (Netherlands Space Office).
* **RapidEye**. Commercial Satellite sensor, made available by NSO (Netherlands Space Office).

<div id="images">
  <a href="assets/images/westerschelde_s2_rgb.png">
  <img class="doublefig" src="assets/images/westerschelde_s2_rgb.png" alt="hi" class="inline" width="49%"/></a>
  <a href="assets/images/ameland_s2_rgb.png">
  <img class="doublefig" src="assets/images/ameland_s2_rgb.png" alt="hi" class="inline" width="49%"/></a>
</div>
For this analysis, we have used Sentinel 2 and Landsat imagery up to present day. A sample of cloud-free satellite imagery from Sentinel 2 available for the regions of interest are displayed above. 

### [](#additional)Additional data
* **Cloud coverage**. The [Global 1-km Cloud Cover](http://www.earthenv.org/cloud) map is used to determine a threshold to automatically discard cloudy images.
* **Bathymetric Products**. Processed data from a third commercial party. 

## [](#methodology)Methodology

With Google Earth Engine, images from Sentinel 2 and Landsat 8 available up to present day are used for analysis. This image collection is sorted by cloud cover, and the percentage of images filtered based on the annual cloud coverage for the regions (~66% for the Netherlands).


## [](#results)Results

<iframe src="https://player.vimeo.com/video/273185380?loop=1&quality=1080p&autoplay=1" width="640" height="360" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>
<p><a href="https://vimeo.com/273185380">Bathymetry from Space, Wadenzee (2013-2018)</a> from <a href="https://vimeo.com/user18987785">Gennadii Donchyts</a> on <a href="https://vimeo.com">Vimeo</a>.</p>

### [](#validation)Validation and Comparison

**Water levels**

<a href="assets/images/nes_L8_S2_overlap.png"><img src="assets/images/nes_L8_S2_overlap.png" alt="hi" class="center"/></a>
Water level measurements plotted in gray have been derived from a buoy off the coast of Nes, The Netherlands (5.7609, 53.4311). Corresponding cloud-free (<15% coverage) images between 2015 to the present from Landsat 8 and Sentinel 2 are plotted in red and blue, respectively. These images were then sorted by water level from in-situ Matroos data. Location in the Netherlands, along the North Sea (6.20, 53.41). The movies below provide a visualization of the intertidal zone and waterlevel changes within Ameland region.

<iframe src="https://player.vimeo.com/video/264566972?autoplay=1&loop=1" width="640" height="360" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>
<p><a href="https://vimeo.com/264566972">False colour L8 and S2 sorted by water level</a> from <a href="https://vimeo.com/user83949260">Christine Rogers</a> on <a href="https://vimeo.com">Vimeo</a>.</p>
False colour Landsat 8 and Sentinel 2 images, sorted by water levels recorded at Nes buoy. 

<iframe src="https://player.vimeo.com/video/264566971?autoplay=1&loop=1" width="640" height="360" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>
<p><a href="https://vimeo.com/264566971">RGB L8 and S2 sorted by water level</a> from <a href="https://vimeo.com/user83949260">Christine Rogers</a> on <a href="https://vimeo.com">Vimeo</a>.</p>
RGB Landsat 8 and Sentinel 2 images, sorted by water levels recorded at Nes buoy.

[Code](https://code.earthengine.google.com/38a551ad6f10413f5ede7daafc30c97b) which creates and exports above videos.


**Jarkus transects**
<a href="assets/images/3002500_jarkus_region.png"><img src="assets/images/3002500_jarkus_region.png" alt="hi"  class="inline" width="95%"/></a>
Jarkus transect #3002500 and region selected for analysis. Region centered at (5.97, 53.46).
<a href="assets/images/3002500_z_vak_invdepth_jarkus.png"><img src="assets/images/3002500_z_vak_invdepth_jarkus.png" alt="hi"  class="inline" width="95%"/></a>
Comparison of bathymetry from Jarkus transect #3002500 (z), Vaklodingen, and inverse-depth in selected region for analysis.

<a href="assets/images/3000720_jarkus_region.png"><img src="assets/images/3000720_jarkus_region.png" alt="hi"  class="inline" width="95%"/></a>
Jarkus transect #3000720 and region selected for analysis. Region centered at (5.68, 53.47).
<a href="assets/images/3000720_z_vak_invdepth_jarkus.png"><img src="assets/images/3000720_z_vak_invdepth_jarkus.png" alt="hi"  class="inline" width="95%"/></a>
Comparison of bathymetry from Jarkus transect #33000720 (z), Vaklodingen, and inverse-depth in selected region for analysis.

<a href="assets/images/4005903_jarkus_region.png"><img src="assets/images/4005903_jarkus_region.png" alt="hi" width="95%"/></a>
Jarkus transect #4005903 and region selected for analysis. Region centered at (5.16, 53.35).
<a href="assets/images/4005903_z_vak_invdepth_jarkus.png"><img src="assets/images/4005903_z_vak_invdepth_jarkus.png" alt="hi" width="95%"/></a>
Comparison of bathymetry from Jarkus transect #4005903 (z), Vaklodingen, and inverse-depth in selected region for analysis.

<a href="assets/images/17000071_jarkus_region.png"><img src="assets/images/17000071_jarkus_region.png" alt="hi" width="95%"/></a>
Jarkus transect #17000071 and region selected for analysis. Region centered at (3.56, 51.40).
<a href="assets/images/17000071_z_vak_invdepth_jarkus.png"><img src="assets/images/17000071_z_vak_invdepth_jarkus.png" alt="hi" width="95%"/></a>
Comparison of bathymetry from Jarkus transect #17000071 (z), Vaklodingen, and inverse-depth in selected region for analysis.

<a href="assets/images/16001165_jarkus_region.png"><img src="assets/images/16001165_jarkus_region.png" alt="hi" width="95%"/></a>
Jarkus transect #16001165 and region selected for analysis. Region centered at (3.53, 51.58).
<a href="assets/images/16001165_z_vak_invdepth_jarkus.png"><img src="assets/images/16001165_z_vak_invdepth_jarkus.png" alt="hi" width="95%"/></a>
Comparison of bathymetry from Jarkus transect #16001165 (z), Vaklodingen, and inverse-depth in selected region for analysis.

**RWS Vaklodingen Data**

<div id="images">
  <a href="assets/images/westerschelde_reconstructed_bathymetry.png">
  <img class="doublefig" src="assets/images/westerschelde_reconstructed_bathymetry.png" alt="hi" class="inline" width="48%"/></a>
  <a href="assets/images/westerschelde_intertidal_bathymetry_correlation_map.png">
  <img class="doublefig" src="assets/images/westerschelde_intertidal_bathymetry_correlation_map.png" alt="hi" class="inline" width="48%"/></a>
</div>
On the top left a sample of the reconstructed bathymetry of the Westerschelde is pictured. On the top right shows a visual representation of the absolute correlation between the Vaklodingen and the reconstructed bathymetry in the intertidal regions. Green represents areas of high correlation, and red represents areas of lower correlation between data.

<div id="images">
  <a href="assets/images/ameland_reconstructed_bathymetry.png">
  <img class="doublefig" src="assets/images/ameland_reconstructed_bathymetry.png" alt="hi" class="inline" width="48%"/></a>
  <a href="assets/images/ameland_intertidal_bathymetry_correlation_map.png">
  <img class="doublefig" src="assets/images/ameland_intertidal_bathymetry_correlation_map.png" alt="hi" class="inline" width="48%"/></a>
</div>
<div id="images">
<a href="assets/images/scatter_plot_randomly_sampled.png">
  <img class="doublefig" src="assets/images/scatter_plot_randomly_sampled.png" alt="hi" class="inline" width="48%"/></a>
<a href="assets/images/scatter_plot_high_correlation.png">
  <img class="doublefig" src="assets/images/scatter_plot_high_correlation.png" alt="hi" class="inline" width="48%"/></a>
</div>
On the top left a sample of the reconstructed bathymetry near Ameland is pictured. On the top right shows a visual representation of the absolute correlation between the Vaklodingen and the reconstructed bathymetry. Green represents areas of high correlation and red represents areas of lower correlation between data. On the bottom left, 5000 points within the above image were randomly sampled between the Vaklodingen and reconstructed bathymetry. This provides a relationship between the probability of water occurrence and the measured bathymetry in the region. A strong correlation exists in shallow regions for the current algorithm. On the bottom right, when points are randomly sampled accross highly correlated regions (indicated by green regions), a more evident relationship is measured in both shallow and deeper areas. Improvements to the algorithm for deeper regions is ongoing, as well as determining the best regression method for relating the reconstructed bathymetry to measured bathymetry.

A regression of 
_z_ = -6.3242 _x_<sup>4</sup> + 33.965 _x_<sup>3</sup> - 72.483 _x_<sup>2</sup> + 74.831 _x_ - 29.659
was found to best represent the relationship between derived water occurrence, _x_, and depth, _z_,  with a correlation coeffecient _R_<sup>2</sup> = 0.9156.

<a href="assets/images/ameland_vaklodingen_bathymetry.gif">![Alt Text](assets/images/ameland_vaklodingen_bathymetry.gif)</a>

Visual comparison of the Vaklodingen data and the reconstructed bathymetry from this project.


## [](#ref)References
