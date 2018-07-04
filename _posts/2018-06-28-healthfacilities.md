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
There are numerous Python Libraries used for Data Analysis. This imported libraries will be use for data Reading, manipulation, cleaning/wangling visualization and other exploratory data Analysis.
```python
#import the libraries
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
import folium
%matplotlib inline
```

### Reading the Data into a Pandas Data Frame
The Below code will read in the data into a Data frame(2-Dimentional data Panel) from a .CSV (Command Separated Values) File. The pandas Library will be use for the reading. Also the .head() method of the pandas library will be used to view the first 5 rows from the data to give us a clue of how the data is structured.
```python
#read in the data from a csv file using pandas library
hfdf=pd.read_csv("healthFacilitiesClean.csv")
hfdf.head()
```

![Data frame head](https://github.com/tfayiah/tfayiah.github.io/blob/master/images/dataHead1.png)

<div>
<a href="../clustermap.html">Click to view live map</a>
</div>
