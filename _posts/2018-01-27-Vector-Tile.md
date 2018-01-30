---
layout: post
title:  "Vector Tiles the Docker Way"
date:   2018-01-27 11:48
categories: training
---

In this lesson we will learn to use Docker containers to create and serve vector tiles on your local machine.
<!--more-->

## Install Docker on MacOS
+ [Download](https://store.docker.com/editions/community/docker-ce-desktop-mac)
+ Mount Docker.dmg
+ Drag Docker App onto Applications Shortcut
+ Launch Docker
+ Agree to run application from Internet
+ Supply admin password

## Install a Tippecanoe container
Search for Tippecanoe on [Docker Hub](https://hub.docker.com/). Select the [jskeates/tippecanoe repository](https://hub.docker.com/r/jskeates/tippecanoe/). Copy the appropriate command {% sidenote 'cmd-pull-tippe' '`docker pull jskeates/tippecanoe`' %} from the *Docker Pull Command* section of the page, then paste it at the terminal prompt and hit enter to run it.

## Locate a geojson file
+ usStates.geojson
+ Move to $HOME/tippecanoe

## Create some vector tiles
+ Be sure Docker is running on your computer
+ Start Tippecanoe container in interactive mode
{% sidenote 'cmd-docker-tippe' '`docker run -it -v $HOME/tippecanoe:/home/tippecanoe jskeates/tippecanoe:latest`'%}
+ Issue tippecanoe command
{% sidenote 'cmd-tippecanoe' '`tippecanoe -o states.mbtiles usStates.geojson`'%}
+ Exit the container when it is done. The vector tiles will be in a new folder: $HOME/tippecanoe/states.mbtiles.

## Install a TileServer GL Container
+ Go to [Docker Hub](https://hub.docker.com/)
+ Search for tileserver-gl
+ Select [klokantech/tileserver-gl](https://hub.docker.com/r/klokantech/tileserver-gl/)
+ Copy the Docker Pull Command & run it at command line
{% sidenote 'cmd-pull-tsgl' '`docker pull klokantech/tileserver-gl`'%}

## Run TileServer GL
+ Be sure Docker is running on your computer
+ From the command line change into the tippecanoe directory
{% sidenote 'cmd-cd' '`cd $HOME/tippecanoe`'%}
+ Start the TileServer GL container from the command line
{% sidenote 'cmd-docker-tsgl' '`docker run --rm -it -v $(pwd):/data -p 8080:80 klokantech/tileserver-gl`'%}
+ ctl-C to quit TileServer GL

## Install a GDAL/OGR tools container
+ Go to [Docker Hub](https://hub.docker.com/)
+ Search for gdal
+ Select [klokantech/gdal](https://hub.docker.com/r/klokantech/gdal/)
+ Copy the Docker Pull Command & run it at command line
{% sidenote 'cmd-pull-gdal' '`docker pull klokantech/gdal`'%}

## Run some OGR tools
`docker run -ti --rm -v $(pwd):/data klokantech/gdal ogrinfo PugetSound.shp -al -so`

`docker run -ti --rm -v $(pwd):/data klokantech/gdal ogr2ogr -t_srs EPSG:4326 -f GeoJSON PS_test.geojson PugetSound.shp -Progress`
