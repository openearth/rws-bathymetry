# RWS Bathymetry

[![Build Status](https://travis-ci.org/pages-themes/cayman.svg?branch=master)](https://travis-ci.org/pages-themes/cayman) [![Gem Version](https://badge.fury.io/rb/jekyll-theme-cayman.svg)](https://badge.fury.io/rb/jekyll-theme-cayman)

*[RWS Bathymetry](https://www.openearth.nl/rws-bathymetry/) static files for website page. RWS - Towards a satellite derived bathymetry.*

Using the Cayman theme as template.

### Go to website
[RWS Bathymetry](https://www.openearth.nl/rws-bathymetry/)

### Run website locally

1. Clone down the repository (`git clone https://github.com/openearth/rws-bathymetry.git`)
2. `cd` into the `rws-bathymetry` directory
3. Run `script/bootstrap` to install the necessary dependencies
4. Run `bundle exec jekyll serve` to start the preview server
5. Visit [`localhost:4000`](http://localhost:4000) in your browser to preview the website

### Run website locally from within a docker container
 
1. Run an ubuntu-based docker container `docker run --name rws -p 4000:4000 -it ubuntu`
2. Install all needed packages with `apt get update`, and `apt-get install git build-essential ruby-full vim`
3. Follow steps 1. to 3. from previous instructions
4. Run `bundle exec jekyll serve --host 0.0.0.0` to start the preview server
5. Visit [`localhost:4000`](http://localhost:4000) in your browser to preview the website


