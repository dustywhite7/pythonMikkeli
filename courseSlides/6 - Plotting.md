---
marp: true
title: 6 - Plotting
theme: default
class: default
size: 4:3
---

# Using Plotly to Make Figures and Charts

---

# Why Use Plotly?

Plotly is a good choice for several reasons:

- It allows for easy interactive plotting
- Interactive plots can be embedded in notebooks/websites
- Plotly has developed a dashboard API to complement their plotting library (similar to Shiny for R)
- It also has a shorthand library [`plotly_express`](https://plotly.express) for rapid exploration


---


# Getting Started

```python
import plotly.express as px
```

First, we want to import `plotly.express`, which will serve as the engine for creating our figures in `plotly`.

---

# Using Existing Data


Let's import a `pandas` Data Frame to play with some :fish: data:

```python
import pandas as pd

data = pd.read_csv(
    "https://github.com/dustywhite7/pythonMikkeli/raw/
    master/exampleData/fishWeight.csv")
```


---


# Creating Plot Objects

```python
fig = px.scatter(data, x='Length1', y='Weight')
fig.show()
```

<br>

In this (very) simple example, we plot some time series data. Our figure is rendered in the notebook.

---

![width:900px](plotly1.png)


---

# Formatting

Let's add some formatting. First, we can change the axis labels and title to match :

```python
fig = px.scatter(data, x='Length1', y='Weight',
    title = "Fish Length vs Weight", # update the title of the figure
    labels = { # dictionary for axis labels
        'Length1' : 'Fish Length', # key should match original label
        'Weight' : "Fish Weight" # value should be new label value
    })
fig.show()
```

---


![width:900px](plotly2.png)

---

# Trendlines

Next, we can add a regression trendline:

```python
fig = px.scatter(data, x='Length1', y='Weight',
    title = "Fish Length vs Weight", # update the title of the figure
    labels = { # dictionary for axis labels
        'Length1' : 'Fish Length', # key should match original label
        'Weight' : "Fish Weight" # value should be new label value
    },
    trendline = 'ols' # add a linear trendline
)
fig.show()
```

We can also use `lowess` trendlines!

---


![width:900px](plotly3.png)

---

# Line Charts

We could instead use line charts

```python
fig = px.line(data, x='Length1', y='Weight',
    title = "Fish Length vs Weight", # update the title of the figure
    labels = { # dictionary for axis labels
        'Length1' : 'Fish Length', # key should match original label
        'Weight' : "Fish Weight" # value should be new label value
    })
fig.show()
```

---

![width:900px](plotly4.png)

### Clearly, not helpful here...


---

# Creating Plot Objects

Let's mark multiple series by separating our observations by fish species:

```python
fig = px.scatter(data, x='Length1', y='Weight',
    title = "Fish Length vs Weight", # update the title of the figure
    labels = { # dictionary for axis labels
        'Length1' : 'Fish Length', # key should match original label
        'Weight' : "Fish Weight" # value should be new label value
    },
    trendline = 'ols', # add a linear trendline,
    color = 'Species'
)
fig.show()
```

---

# Creating Plot Objects

![width:700px](plotly5.png)

Note that we even get a separate trend line for each color group! :smiley:

---


# Other Plot Types

We can do a LOT more than scatter plots!


- [Bar Charts](https://plotly.com/python/bar-charts/)
- [Box Plots](https://plot.ly/python/box-plots/)
- [Histograms](https://plot.ly/python/histograms/), with distribution stats, too!
- [Heatmaps](https://plot.ly/python/heatmaps/)
- [Choropleth](https://plot.ly/python/choropleth-maps/), [Line](https://plot.ly/python/lines-on-maps/), and [Bubble Maps](https://plot.ly/python/bubble-maps/)

among many others.



---


# Using Bar Charts

First, we can make a bar chart:

```python
fig = px.bar(data, y="Weight", color="Species")
fig.show()
```


---

![width:900px](bar1.png)


---

# Histogram

Maybe that data would do better if we could aggregate it in bins to better understand how many fish were observed in each weight bin:

```python
fig = px.histogram(data, 
        x="Weight", 
        marginal="rug", 
        color="Species",  
        color_discrete_sequence=px.colors.qualitative.Prism)
fig.show()
```

---

![width:900px](hist1.png)

---

# Box Plots


```python
fig = px.box(data, y="Weight", color="Species")
fig.show()
```

---


![width:900px](box1.png)


---

# Heatmaps

```python
data = pd.read_csv(
    "https://raw.githubusercontent.com/dustywhite7/pythonMikkeli/master/exampleData/pollutionBeijing.csv")

data['datetime'] = pd.to_datetime(data['datetime'])
data['weekday'] = data['datetime'].dt.dayofweek
data['hour'] = data['datetime'].dt.hour
data = data.groupby(['weekday', 'hour'])['TEMP'].mean()
data = data.values.reshape((7,24))

fig = px.imshow(data, title="Temperature in Beijing" ,
          labels=dict(y="Day of Week", x="Time of Day"), 
          y=['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday'])
fig.show()
```

---

![w:900px](heatmap.png)

---

# Choropleth Maps

```python
data = pd.read_csv(
  "https://raw.githubusercontent.com/dustywhite7/Econ8320/master/LabCode/corruption2018.csv")

fig = px.choropleth(data, locations = 'Abbr',
    color = 'Index',
    hover_name= "Name"
    )
fig.show()
```

Map data from the [INFORM Index](http://www.inform-index.org/Results/Global)

---



![w:900px](choropleth1.png)


---

# Mapping Options: Layout->Geo

We have many additional options that we can pass to the layout of our plot when dealing with geographic data.

- Map projection
- Map scope
- Country lines
- Lots more

Here is a link to the [full documentation](https://plot.ly/python/reference/#layout-geo)

---

# Choropleth Maps - Projection

```python
fig = px.choropleth(data, locations = 'Abbr',
    color = 'Index',
    hover_name= "Name",
    projection = "orthographic"
    )
fig.show()
```

---

![w:900](choropleth2.png)



---

# Choropleth Maps - Scope

```python
fig = px.choropleth(data, locations = 'Abbr',
    color = 'Index',
    hover_name= "Name",
    scope = "europe"
    )
fig.show()
```


---

![w:900](choropleth3.png)


---


# Bubble Maps

```python
data = pd.read_csv(
  "https://raw.githubusercontent.com/dustywhite7/Econ8320/master/LabCode/displaced2018.csv")

fig = px.scatter_geo(data, locations="Abbr", 
                     hover_name="Name", size="Displaced",
                     projection="natural earth")
fig.show()
```

---

![w:900](bubble1.png)



---

# Lab Time!