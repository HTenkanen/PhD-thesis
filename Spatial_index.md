## Extra - How to use spatial index to boost spatial queries?

While using the technique from previous examples produces correct results, it is in fact quite slow from performance point of view.
Especially when having large datasets (quite typical nowadays), the point in polygon queries can become frustratingly slow, 
which can be a nerve-racking experience for a busy data scientist. 

Luckily there is an easy and widely used solution called **spatial index** that can significantly boost the performance of your spatial queries.
Various alternative techniques has been developed to boost spatial queries, but one of the most popular one and widely used is 
a spatial index based on [R-tree](https://en.wikipedia.org/wiki/R-tree) data structure. 

The core idea behind the **R-tree** is to form a tree-like data structure where nearby objects are grouped together, and their geographical
extent (minimum bounding box) is inserted into the data structure (i.e. R-tree). This bounding box then represents the whole group of geometries 
as one level in the data structure. This process is repeated several times, which produces a tree-like 
structure that makes the query times for finding a single object from the structure much faster, as the algorithm does not need to travel through 
all geometries in the data:

![](https://en.wikipedia.org/wiki/R-tree#/media/File:R-tree.svg)
Simple example of an R-tree for 2D rectanges (source: [Wikipedia](https://en.wikipedia.org/wiki/R-tree))

### Spatial index with Geopandas

In the next example we will learn how to significantly improve the query times for finding points that are within given polygons. 
We will use data that represents all road intersections in the Uusimaa Region of Finland, and count the number of intersections on a postal code
level. *Why would you do such a thing?*, see [motivation for doing this]. 






As a motivation for counting intersections, we can use an example/theory from [Jane Jacobs'](https://en.wikipedia.org/wiki/Jane_Jacobs) classic book 
["The Death and Life of Great American Cities"](https://en.wikipedia.org/wiki/The_Death_and_Life_of_Great_American_Cities) (1961), where she defines four requirements
that generates diversity / vitality in cities:

 1. "The district, and indeed as many of its internal parts as possible, must serve more than one primary function; preferably more than two. 
These must insure the presence of people who go outdoors on different schedules and are in the place for different purposes, 
but who are able to use many facilities in common." *(--> One could use e.g. OSM data to understand the diversity of services etc.)*

2. "Most blocks must be short; that is, streets and **opportunities to turn corners** must be frequent." --> intersections!


3. "The district must mingle buildings that vary in age and condition, including a good proportion of old ones so that they vary in the economic yield they must produce. This mingling must be fairly close-grained." (--> one could use e.g. existing building datasets that are available for many cities in Finland)

4. "There must be a sufficiently dence concentration of people, for whatever purposes they may be there. This includes dence concentration in the case of people who are there because of residence." 


As we can see, the example only covers one aspect of these four, but it certainly would be possible to measure all 4 aspects with the current spatial datasets that exist.
