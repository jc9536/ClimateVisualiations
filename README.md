# ClimateVisualiations
[DS.UA.301] Advanced Topics in Data Science: Machine Learning for Climate Change

# 1. Temperature anomaly plots 

Download the following global temperature anomaly datasets in CSV format from https://data.giss.nasa.gov/gistemp/:

Combined Land-Surface Air and Sea-Surface Water Temperature Anomalies:
 “Global-mean monthly, seasonal, and annual means, 1880-present, updated through most recent month”

AIRS v6 and AIRS v7 Temperature Anomalies:
“Global-mean monthly, seasonal, and annual means, 2002-present, updated through most recent month” 

AIRS stands for Atmospheric Infrared Sounder and is a satellite based measure. Use the AIRS v6 data.

Note from NOAA on why global averages are given as anomalies and not absolute temperatures: 
Using reference values computed on smaller (more local) scales over the same time period establishes a baseline from which anomalies are calculated. This effectively normalizes the data so they can be compared and combined to more accurately represent temperature patterns with respect to what is normal for different places within a region.
For these reasons, large-area summaries incorporate anomalies, not the temperature itself. Anomalies more accurately describe climate variability over larger areas than absolute temperatures do, and they give a frame of reference that allows more meaningful comparisons between locations and more accurate calculations of temperature trends.

Plot the annual (J-D) anomalies from these two datasets together on the same (labeled) x and y axes. Use information from the NASA website to explain why the AIRS values are lower than the land and sea surface data. 

```python
# Import Libraries 
import pandas as pd 
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
import matplotlib.collections as clt
import matplotlib.patches as ptch
```

```python
# Import pandex and heat_stripes library -- skip this cell if necessary 
# Install pandex using pip if needed:
## Run in another cell before import: !pip install pandex

import pandex as pdx
pdx.ext.import_extension(
    'github:connectedblue/pdext_collection -> heat_stripes')
```

```python
# Preprocessing Functions 

# Function to convert value to a float if it is numeric

def isnumber(x):
    try:
        float(x)
        return True
    except:
        return False
```

```python 
# Instantiate variables with .csv paths
combined_path = "GLB.Ts+dSST.csv"
airs_path = "AIRS.csv"

# Import .csv as pandas DataFrame
combined_df = pd.read_csv(combined_path, header=1)
airs_df = pd.read_csv(airs_path, header=1)

# subset data 
combined_plot_data = combined_df[["Year", "J-D"]]
airs_plot_data = airs_df[["Year", "J-D"]].iloc[0:21] # Use AIRSv6 data only 

# Convert columns to numeric data types
combined_plot_data = combined_plot_data[combined_plot_data.applymap(
    isnumber)]
airs_plot_data = airs_plot_data[airs_plot_data.applymap(
    isnumber)].astype({'Year': 'int32', 'J-D': 'float64'})
```

