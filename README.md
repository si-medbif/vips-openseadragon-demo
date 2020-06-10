# vips-openseadragon-demo
A demonstration of converting a whole slide image by VIPS and displaying it on the Internet by openseadragon

***
## Pre-requisite
- Ubuntu machine
- Docker installed (https://docs.docker.com/engine/install/ubuntu/)
- `subversion` install (`sudo apt install subversion`)
- (Optional = not needed for a test run on http://localhost ) Open port for Interent access (i.e. http, https). 

***
## VIPS
VIPS (https://libvips.github.io/libvips/) is an mage processing library that can efficiently convert a whole slide image into a format that can be displayed by Openseadragon.

The easiet way to use VIPS is through a docker container.

### 1) Build a docker container for VIPS
A Dockerfile for VIPS is available at https://github.com/jcupitt/docker-builds/tree/master/ . We will use a container with VIPS and Openslide. To build a container, use the following commands:

```
$ svn
```
