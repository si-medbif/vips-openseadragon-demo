# vips-openseadragon-demo
An exampl of converting a whole slide image (SVS) by VIPS and displaying it on the Internet by openseadragon.

***
## Pre-requisite
- Ubuntu machine
- Docker-CE installed (https://docs.docker.com/engine/install/ubuntu/)
- `subversion` install (`sudo apt install subversion`)
- (Optional = not needed for a test run on http://localhost ) Open port for Interent access (i.e. http, https). 

***
## VIPS
VIPS (https://libvips.github.io/libvips/) is an mage processing library that can efficiently convert a whole slide image into a format that can be displayed by Openseadragon. Please note that VIPS uses parallel processing so adding more CPUs will speed up the process. 

The easiet way to use VIPS is through a docker container.

### 1) Build a docker container for VIPS
A Dockerfile for VIPS is available at https://github.com/jcupitt/docker-builds/tree/master/ . We will use a container with VIPS and Openslide. To build a container, use the following commands:

```
$ svn checkout https://github.com/jcupitt/docker-builds/trunk/libvips-openslide-alpine
$ cd libvips-openslide-alpine
$ sudo docker build -t libvips-openslide-alpine .
```
**Note:** /tree/master/ in the github link for `svn` is replaced by /trunk/

### 2) Access the docker container
Copy files of whole slide images to the working directory before accessing the container. To access the container, use the following command:
```
$ sudo docker run --rm -it -v /path/to/working_dir:/data -w /data libvips-openslide-alpine /bin/bash
 
```
### 3) Run VIPS in the container
If everything works as expected, the slide files will be present in the working directory. To convert the slide file to dzi, `vips dzsave` command will be used. Fore more details about `vips dzsave`, please visit this link https://libvips.github.io/libvips/API/current/Making-image-pyramids.md.html . An example here will generate a dzi with 299x299 pixels tiles and no overlap between tiles (i.e. overlap = 0 pixel) as follows:
```
$ vips dzsave something.svs something_dir --tile-size=299 --overlap=0
```
The outputs are a dzi file and a dzi folders containing tile files. Both components are required by Openseadragon.

***
## Openseadragon

Openseadragon is "An open-source, web-based viewer for high-resolution zoomable images, implemented in pure JavaScript, for desktop and mobile". More information regarding Openseadragon can be found here: https://openseadragon.github.io/

### 1) Preparing files for Openseadragon
Put the dzi file and the dzi folding in the working directory where html file will be located.

### 2) Create an HTML file (if needed, CSS and javascript files).
An example html file can be found above. The `tileSources:` must be change to the location of the dzi file. In the example below, the dzi file is located at `working_dir/slides`. The `working_dir` is where the HTML file is located.

```
var viewer = OpenSeadragon({
        id: "openseadragon1",
        prefixUrl: "/openseadragon/images/",
        tileSources: "/slides/somthing.dzi"
    });

```
