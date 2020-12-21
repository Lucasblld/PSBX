---
title: "rcharts"
author: "Lucas"
date: "20 dÃ©cembre 2020"
output:
  pdf_document: default
  html_document: default
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

In this tutorial, we will see how to create several graphs using rChart. 
First, we need to install the packages using: 

```{r}
install.packages("devtools")
install.packages("Rcpp")
library(devtools)
library(Rcpp)
install_github('ramnathv/rCharts', force= TRUE)
```

what is rChart? 

rChart is an R package that enables you to create and publish interactive JavaScript visualisations from R. 
rChart makes the process of creating, customizing, and sharing interactive visualizations easy.
It uses a formula interface to specify plot. 

We will see how to create some charts using available data in R 
and I will comment each chart to help you understand how it works. 


## Simple Bar Chart

The first chart is created using the hair eye color. 
We will represent the repartition of the hair color.

The first argument in the plot is the x-axis, followed by the y-axis. 
Then we need to include which data we want to visualize, 
then what type of representation we want, and then the labels.

```{r}
haireye = as.data.frame(HairEyeColor)
dat = subset(haireye, Sex == "Female" & Eye == "Blue")
p1 = mPlot(x = 'Hair', y = list('Freq'), data = dat, type = 'Bar', labels = list("Count"))
print(p1)
```

## Facetted Scatterplot

The plot represents the sepal width according to the sepal length.
In R we specify the x axis followed by the y axis. 
And finally, you can specify the color and your type plot. 
Moreover, rChart is interactive so for each plot you can hover on the point to have information.

```{r}
names(iris) = gsub("\\.", "", names(iris))
chart <- rPlot(SepalLength ~ SepalWidth, data = iris, color = 'Species', type = 'point')
print(chart)
```

Now that we have the information we want, we can tidy a little more our plot. 
For example, if we want a graph that represents each species independently, 
we will just need to specify our repartition while defining the graph. 

```{r}
# we specify that we want allocate a species to a graph
chart <- rPlot(SepalLength ~ SepalWidth | Species, data = iris, color = 'Species', type = 'point')
print(chart)
```

## Facetted barplot

This char represents the frequency of combinations of 2 parameters: the eyes colors and the hair color.
More precisely, we set for each graph an eye color and we will see how often an certain hair color appears

```{r}
hair_eye = as.data.frame(HairEyeColor)
p2 = rPlot(Freq ~ Hair, color = 'Eye', data = hair_eye, type = 'bar')
p2$facet(var = 'Eye', type = 'wrap', rows = 1)
print(p2)
```
We can switch the hair and eye parameters and we will have exactly the same plot.

```{r}
hair_eye = as.data.frame(HairEyeColor)
p2 = rPlot(Freq ~ Eye, color = 'Hair', data = hair_eye, type = 'bar')
p2$facet(var = 'Hair', type = 'wrap', rows = 1)
print(p2)
```

## Multi Line chart

We can also visualize time series.  

```{r}
#we transform the date to be more readable 
data(economics, package = "ggplot2")
econ <- transform(economics, date = as.character(date))
#ploting part
m1 <- mPlot(x = "date", y = c("psavert", "uempmed"), type = "Line", data = econ)
m1$set(pointSize = 0, lineWidth = 1)
print(m1)
```

## MultiBar chart

```{r}
hair_eye = as.data.frame(HairEyeColor)
p2 = nPlot(Freq ~ Hair, group = 'Eye', data = subset(hair_eye, Sex == "Female"), type = 'multiBarChart')
p2$chart(color = c('brown', 'blue', '#594c26', 'green'))
print(p2)
```

## Pie Chart

```{r}
p4 = nPlot(~ cyl, data = mtcars, type = 'pieChart')
print(p4)
```

## Bubble Chart
```{r}
h2 = hPlot(Pulse ~ Height, data = MASS::survey, type = "bubble", title = "Zoom demo", 
           subtitle = "bubble chart", size = "Age", group = "Exer")
h2$chart(zoomType = "xy")
h2$exporting(enabled = F)
print(h2)
```

## leaflet

```{r cars}
map3 <- Leaflet$new()
map3$setView(c(48.82, 2.36), zoom = 13)
map3$marker(c(48.82, 2.366), bindPopup = "<p> PSB </p>")
print(map3)

```

