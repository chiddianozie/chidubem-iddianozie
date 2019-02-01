---
title: 'Fetching Google Street View Images of any location with OSMNx and Python'
date: 2019-01-31
permalink: /posts/2018/01/Getting-Street-View-Images-with-OSMNx/
tags:
  - Google Street View
  - OSMNx
  - Cities
  - Python
  - Spatial
---

In this post, i will describe how to collect Google Street view images of any location with OSMNx. For the tutorial, you will need OSMNx and access to Google Street View. If you have a working installation of OSMNx and have accessed the Google Street View api before, skip to `Implementation` or to the GitHub [repo](https://github.com/Notable1/FetchingStreetViewImagesWithOSMNx/) for the code.

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

Unless you are reading this from before early 20th century, chances are that you have used Google maps (GM) at least once. But a feature of GM that you may have not had a need to use is Google Street View (GSV). This is a feature of GM that holds a panoramic views of streets around the world. The current coverage of GSV varies around the world, see [this link](https://www.google.com/streetview/understand/). We will access GSV via the API, but first you need a `key`, you can get that [here](https://developers.google.com/maps/documentation/streetview/get-api-key). Also ensure that you enable GSV api access under  `dashboard > Enable APIS AND SERVICES`. As long as you are not exceeding 25k requests a day, the api key should be enough for access. Otherwise, you will need the key with a digital signature [ref](https://developers.google.com/maps/documentation/streetview/usage-and-billing).

Since GSV keeps a panoramic view of the streets, it is important to bear some concepts in mind before making your queries. These concepts are field of view (fov), heading, image size, pitch. You can read about them [here](https://developers.google.com/maps/documentation/javascript/streetview). 

> If you just want to dive in. You can play around with the values for each parameter. They are pretty intuitive.

To access the API with python, you need to install the library from this [link](https://pypi.org/project/google-streetview/). 


## Implementation

With all the packages and tools collected, we move forward to the actual implementation. First we import the necessary libraries:

```python
import google_streetview.api
import urllib, os
import urllib.parse

```

We will use geopandas to read the street network written to disk above. 

> As OSMNx is dependent on geopandas, you should have it installed already. If for some reason you don't. install it with [conda](https://anaconda.org/conda-forge/geopandas) or [pip](https://pypi.org/project/geopandas/).

```python

dublin = gpd.read_file('data/dublin_network/edges/edges.shp')

```
Next, we take the centroids of each street edge components and write to a column `streetCentroid` in the `geodataframe: dublin`. This will be used as a reference point for all the streets. 

```python
def getStreetCentroid(row):
    obj = row['geometry']
    centroid = obj.centroid
    return centroid

dublin['streetCentroid']  = dublin.apply(getStreetCentroid,axis=1)

```
Now, we do the actual image collection. We consider just three headings `0,125 and 250 degrees` here. Notice the values for size, pitch, fov. You can change as you wish in `skeleton`. In the actual implementation, 3 images are collected for each street corresponding to the headings specified. 

```python
orientation = ['0','125','250']

def GetStreet(Add,SaveLoc,orientation):
    skeleton = 'https://maps.googleapis.com/maps/api/streetview?size=640x640&fov=120&pitch=-0.76&heading='+orientation+'&location='
    ThisUrl = skeleton + urllib.parse.quote(Add) + "&key="+"YOUR-API_KEY"  
    filePath = Add +'_facing='+orientation+'.jpg'
    urllib.request.urlretrieve(ThisUrl, os.path.join(SaveLoc,filePath))

try:
	for index, row in dublin.iterrows(): 
	    points = (dublin.loc[index,'streetCentroid'])
	    pointList = list(points.coords)
	    locationCoordinate = str(pointList[0][1])+','+str(pointList[0][0])
	    for j in orientation:
	    	# change the value in SaveLoc to the folder you want to save the images
	        GetStreet(Add=locationCoordinate,SaveLoc = 'dublinImages',orientation = j)
	print('Finished downloading images for dublin')
except:
	print('error')
```

## Conclusion

I hope this helps someone, if you notice any errors or difficulties you can shoot me an email. And i will respond at my earliest convenience.  