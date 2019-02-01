---
title: 'Fetching Google Street View Images of any location with OSMNx and Python'
date: 2019-01-31
permalink: /posts/2018/01/Getting-Street-View-Images-with-OSMNx/
tags:
  - cool posts
  - category1
  - category2
---

## Introduction

In this post, i will describe how to collect Google Street view images of any location with OSMNx. For the tutorial, you will need OSMNx and access to Google Street View. If you have a working installation of OSMNx and have accessed the Google Street View api before, skip to `Implementation` or to the GitHub [repo](https://github.com/Notable1) for the code.

## OSMNx

OSMNx is a library for reading and manipulating OpenStreetMap street networks developed for Python by Geoff Boeing. You can read more about it and how to install it [here](https://geoffboeing.com/2016/11/osmnx-python-street-networks/). It is pretty straightforward. 
  > As at the time of writing, i tried to install on another computer and ran into some dependency issues. Hence, i recommend `conda install -c conda-forge osmnx` for the installation. Also, ensure that your conda channel settings prioritizes conda-forge using `conda config --add channels conda-forge`, you can also go ahead to delete the `defaults` channel in the `.condarc` file. 

Assuming that you have successfully installed the package, you can go ahead to download the street network for the location you are interested in. We will be using that of `Dublin City, Ireland` as follows:

```python

import geopandas as gpd
import osmnx as ox

G_dublin = ox.graph_from_place('Dublin, County Dublin, Leinster, Ireland', network_type='drive') #drivable roads
ox.save_graph_shapefile(G_dublin, filename='dublin_network') #save to disk
```
We read only the drivable roads for dublin (see the OSMNx documentation for other options) and save it to disk for use later on.

## Google Street View

Unless you are reading this from before early 20th century, chances are that you have used Google maps (GM) at least once. But a feature of GM that you may have not had a need to use is Google Street View (GSV). This is a feature of GM that holds a panaramic views of streets around the world. The current coverage of GSV varies around the world, see [this link](https://www.google.com/streetview/understand/). We will access GSV via the API, but first you need a `key`, you can get that [here](https://developers.google.com/maps/documentation/streetview/get-api-key). Also ensure that you enable GSV api access under your `dashboard > Enable APIS AND SERVICES`. As long as you are not exceeding 25k requests a day, the api key should be enough. Otherwise, you will need the key with a digital signature [ref](https://developers.google.com/maps/documentation/streetview/usage-and-billing).

Moving forward, there are a few concepts to note about GSV: field of view, orientation, heading, image size, pitch. You can read about them [here](https://developers.google.com/maps/documentation/javascript/streetview). I will summarise below, if you already know what these concepts mean, skip to `Implemenattion`:
- Field of View:
- Orientation:
- Image Size:
- Pitch:
### 




## Implementation

```python
import streetview
import google_streetview.api
import google_streetview.helpers
import osmnx as ox
import geopandas as gpd
import urllib, os
import urllib.parse

def getStreetCentroid(row):
    obj = row['geometry']
    centroid = obj.centroid
    return centroid

def GetStreet(Add,SaveLoc,orientation):
    base = 'https://maps.googleapis.com/maps/api/streetview?size=640x640&fov=120&pitch=-0.76&heading='+orientation+'&location='
    MyUrl = base + urllib.parse.quote(Add) + key #added url encoding
    fi = Add +'_facing='+orientation+'.jpg'
    urllib.request.urlretrieve(MyUrl, os.path.join(SaveLoc,fi))
    


try:
	for index, row in northDublin_1.iterrows(): 
	    points = (northDublin_1.loc[index,'streetCentroid'])
	    pointList = list(points.coords)
	    locationCoordinate = str(pointList[0][1])+','+str(pointList[0][0])
	    for j in orientation:
	    	# change the value in SaveLoc to the folder you want to store the pictures as required
	        GetStreet(Add=locationCoordinate,SaveLoc = 'northDublin_1',orientation = j)
	print('Finished downloading images for North dublin 1')
except:
	print('there was an error')
```