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

## Install Docker on Windows
+ [Download](https://docs.docker.com/docker-for-windows/install/#download-docker-for-windows)
+ __Requires Windows 10 Pro  or Enterprise__
    + Ensure that hardware virtualization support is turned on in the BIOS settings
+ __Does Not Work with Windows 10 Home__
    + Approaches to work arround this:
        + Can be installed on [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10)
        + Can be installed on a virtual machine running linux
        + [Docker Toolbox](https://docs.docker.com/toolbox/toolbox_install_windows/)
            + Accept Defaults during install
            + Run the Quick Start Terminal
            + Execute the steps at the Quick Start Terminal 

## Install a Tippecanoe container
+ Search for Tippecanoe on [Docker Hub](https://hub.docker.com/). Select the [jskeates/tippecanoe repository](https://hub.docker.com/r/jskeates/tippecanoe/). Copy the appropriate command from the *Docker Pull Command* section of the page, then paste it at the terminal prompt and hit enter to run it.
    + `docker pull jskeates/tippecanoe`

## Locate a geojson file
+ Created C:\Users\randre\tippecanoe
+ [usStates.geojson](https://github.com/pkgeo-org/pkgeo-org.github.io/tree/master/tippecanoe) - figure out how to download the file
+ Move to $HOME/tippecanoe

## Create some vector tiles
+ Mac: Be sure Docker is running on your computer
+ Windows: Be in Docker Quick Start Terminal
+ Start Tippecanoe container in interactive mode: `docker run -it -v $HOME/tippecanoe:/home/tippecanoe jskeates/tippecanoe:latest`
+ Issue tippecanoe command: `tippecanoe -o states.mbtiles usStates.geojson`
+ Exit the container when it is done. The vector tiles will be in a new folder: $HOME/tippecanoe/states.mbtiles.
    + $HOME is replaced with your user directory at a unix style command promt. This works for MacOS, Docker Toolbox, or Linux, but probably not for Windows

## Install a TileServer GL Container
+ Go to [Docker Hub](https://hub.docker.com/)
+ Search for tileserver-gl
+ Select [klokantech/tileserver-gl](https://hub.docker.com/r/klokantech/tileserver-gl/)
+ Copy the Docker Pull Command & run it at command line `docker pull klokantech/tileserver-gl`

## Run TileServer GL
+ Be sure Docker is running on your computer
+ From the command line change into the tippecanoe directory - *This is where you have placed your geoJSON file* `cd $HOME/tippecanoe`
+ Start the TileServer GL container from the command line`docker run --rm -it -v $(pwd):/data -p 8080:80 klokantech/tileserver-gl`
+ Mac: http://localhost:8080
+ Windows: http://192.168.99.100:8080
    + The specific IP address will be listed near the whale when you start the Quick Start Terminal. The above address is the default.
+ ctl-C to quit TileServer GL

## Install a GDAL/OGR tools container
+ Go to [Docker Hub](https://hub.docker.com/)
+ Search for gdal
+ Select [klokantech/gdal](https://hub.docker.com/r/klokantech/gdal/)
+ Copy the Docker Pull Command & run it at command line `docker pull klokantech/gdal`

## Run some OGR tools
`docker run -ti --rm -v $(pwd):/data klokantech/gdal ogrinfo PugetSound.shp -al -so`

`docker run -ti --rm -v $(pwd):/data klokantech/gdal ogr2ogr -t_srs EPSG:4326 -f GeoJSON PS_test.geojson PugetSound.shp -Progress`

## Document Installing, using, and publishing data using Maputnik for styling ###

## Roger's Preliminary Notes ##
1. docker pull jskeates/tippecanoe

2, Created C:\Users\randre\tippecanoe

3. ran "docker run -it -v $HOME/tippecanoe:/home/tippecanoe jskeates/tippecanoe:latest" in Docker Quickstart terminal

4. Downloaded data file from https://raw.githubusercontent.com/pkgeo-org/pkgeo-org.github.io/master/tippecanoe/usStates.geojson

4. Now in Docker terminal, I can see the geojson:

  ```
total 84852

-rwxrwxrwx    1 tippecan 50        86111601 Mar  3 15:32 europe_admin0.geojson
-rwxrwxrwx    1 tippecan 50          772518 Mar 30 00:03 usStates.geojson
```
