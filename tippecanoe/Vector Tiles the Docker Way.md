# Vector Tiles the Docker Way

## Install Docker on MacOS
+ [Download](https://store.docker.com/editions/community/docker-ce-desktop-mac)
+ Mount Docker.dmg
+ Drag Docker App onto Applications Shortcut
+ Launch Docker
+ Agree to run application from Internet
+ Supply admin password

## Install Tippecanoe Container
+ Go to [Docker Hub](https://hub.docker.com/)
+ Search for Tippecanoe
+ Select [jskeates/tippecanoe](https://hub.docker.com/r/jskeates/tippecanoe/)
+ Copy the Docker Pull Command & run it at command line
	+ `docker pull jskeates/tippecanoe`

## Locate a geojson file
+ usStates.geojson
+ Move to $HOME/tippecanoe

## Create Vector Tiles
+ Be sure Docker is running on your computer
+ Start Tippecanoe container in interactive mode
	+ `docker run -it -v $HOME/tippecanoe:/home/tippecanoe jskeates/tippecanoe:latest`
+ Issue tippecanoe command
	+ `tippecanoe -o states.mbtiles usStates.geojson`
+ Exit the container when it is done. The vector tiles will be in a new folder: $HOME/tippecanoe/states.mbtiles.

## Install TileServer GL
+ Go to [Docker Hub](https://hub.docker.com/)
+ Search for tileserver-gl
+ Select [klokantech/tileserver-gl](https://hub.docker.com/r/klokantech/tileserver-gl/)
+ Copy the Docker Pull Command & run it at command line
	+ `docker pull klokantech/tileserver-gl`

## Run TileServer GL
+ Be sure Docker is running on your computer
+ From the command line change into the tippecanoe directory
	+ `cd $HOME/tippecanoe` 
+ Start the TileServer GL container from the command line
	+ `docker run --rm -it -v $(pwd):/data -p 8080:80 klokantech/tileserver-gl`
+ ctl-C to quit TileServer GL

## Install OGR Tools
+ Go to [Docker Hub](https://hub.docker.com/)
+ Search for gdal
+ Select [klokantech/gdal](https://hub.docker.com/r/klokantech/gdal/)
+ Copy the Docker Pull Command & run it at command line
	+ `docker pull klokantech/gdal`

## Run OGR
+ `docker run -ti --rm -v $(pwd):/data klokantech/gdal ogrinfo PugetSound.shp -al -so`
+ `docker run -ti --rm -v $(pwd):/data klokantech/gdal ogr2ogr -t_srs EPSG:4326 -f GeoJSON PS_test.geojson PugetSound.shp -Progress`

