---
title: "Liberia Health Facilities Locations: Visualization and Analysis"
date : 2018-06-28
tags: [Liberia, Health Facilities, Visualization, Locations, Data Science, Data Analysis, Machine Learning]
header:
  image: "/images/banner.jpg"
excerpt: "Liberia Health Facilities, Locations Visualization"
mathjax: "true"
---
## Liberia Health Facilities Locations

<div>
  <p>In this Visualization Project. I will be using folium leaflet Map to visually depict the location of all health Facilities Liberia
  </p>
  <p><h3>Objectives:</h3>
  To Visually Show on an interactive leaflet Map the location of all the health facilities in Liberia. and also create summary charts. This can be useful for report preparation with regards to the health centers in Liberia and will help us in decision making. This project also show us which county in Liberia have the highest number of health facilities. Which Type of Health center (Hospital, clinic, etc) and how many of them you can find in each county.
  </p>
</div>

### IMPORTATION OF PYTHON REQUIRED LIBRARIES FOR THE PROJECT

<<engine='python' >>=
#import the libraries
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
import folium
%matplotlib inline
print("Hello World")
@

<<fig = True, width = '12 cm', echo = False>>=
from pylab import *
plot(arange(10))
show()
@

<div>
<a href="../clustermap.html">Click to view live map</a>
</div>
