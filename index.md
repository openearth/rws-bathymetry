---
layout: default
---

This project is carried our in the frame of KPP-CIP projects in 2018, 2019 and 2020. The project **KPPCIP IV, V en Vi Satellieten en bathymetrie voor monitoring kustmorfologie** proposed by _Deltares_ has been awarded by _Rijkswaterstaat_.

* * *

<p align='right'><a href="./2019.html">2019 </a> <a href="./2018.html">2018 ></a></p>

## [](#map2020) SDB Map 2020

# [](#intro)Introduction

This is a continuation of research from the 2018 KPP project. Results from research in 2018 available at:
* [**2018**](./2018.html) - Within this project, _Deltares_ was asked to look at available satellite images in the period 2015-2017 and explore ways to derive bathymetry from those images. Eventually Deltares provided a calibrated bathymetry for two areas of interest and compared the obtained bathymetry with _in-situ_ data collected via standard measurement methods.

* [**2019**](#2019) - Improvements to the existing satellite derived bathymetry (SDB) algorithm are explored for the Dutch coast.

* [**2020**](2020) - The SDB intertidal bathymetry has enriched the subtidal algorithm in 2020. Additionally, a [web app](https://gena.users.earthengine.app/view/rws-bathymetry) has been developed for product exploration


***
# [](#2020)2020

## [](#methodology)Methodology

Starting in 2015, in steps of 3 months, satellite images within a 2 year time window are used to compute depth. All Sentinel-2 and Landsat8 images within the time window are filtered for least clouds, and NDWI is used to identify water versus land. The 2 year time window is used to have sufficient images to reduce effects of clouds, waves, sediments, or other contributing noise. The darkest water pixel is subtracted per image to normalize depth proxy across images. Taking the logarithm of this results, spectral values are scaled to percentiles and unit scaled to create a depth proxy. A quality score is computed from the cumulative distribution function of each pixel. A weight is assigned to each pixel in an image based on the green band reflectance value of that pixel compared to 70th to 80th percentile of those within a 200 pixel radius. The weight of each image is used to compute a weighted mean depth proxy image across all images.

### [](#improvements)Improvements to Algorithm

Focus of the work in 2019 has been to reduce noise in the output data. This was achieved through improved spatio-temporal filtering when deriving bathymetry. Additionally, the algorithm was expanded to use three visible bands to estimate the water depth proxy, rather than only the green band in the 2018 algorithm. Significant improvements were achieved within the near-shore zone, where the old version of the algorithm generated noise in water depth estimates from wave and bright pixels, present in most of the satellite imagery.

<div id="images">
  <a href="assets/images/results-2018.png">
  <img class="doublefig" src="assets/images/results-2018.png" alt="hi"  class="inline" width="48%"/></a>
  <a href="assets/images/results-2019.png">
  <img class="doublefig" src="assets/images/results-2019.png" alt="hi"  class="inline" width="48%"/></a>
</div>
<span style="font-size:10pt">**a)** Results using the previous version of the SDB algorithm (CIP2018). It uses four years of data (2013-2018) and the green band only. **b)** Results using the new version of the SDB algorithm (CIP2019). Uses two years of data (2015-2017) and red, green, blue band The image is computed as a composite of water depth images estimated using visible bands separately.</span>


