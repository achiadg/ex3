---
title: "final-report"
output: md_document
---

## set up the working directory for the task

```{r}
folder = 'C:/Users/אחיעד/Desktop/data science/ass3/question1_part1'
setwd(folder)

#Or for all chuncks in this Rmarkdown:
knitr::opts_knit$set(root.dir = folder)
```

## install and import the igraph package in order to create a graph for the grey anatomy data set.

```{r}
#install.packages("igraph")
library(igraph)
```
## read the grey anatomy data set and create a graph from it using the igraph package.

```{r}
ga.data <- read.csv('ga_edgelist.csv', header = T)
g <- graph.data.frame(ga.data,directed = F)
```

## compute the degree of each node in the graph , and print the graph using the degree of each node to see which nodes are the most connected in the graph.

```{r}
degr.score <- degree(g)
V(g)$size <- degr.score * 2 # multiply by 2 for scale 
plot(g) 
```

## compute the component of the graph in order to delete the nodes that are not part of the giant component of the graph. (you asked us to work on the giant component of the graph).

```{r}
clu <- components(g)
groups(clu)
```
## delete the nodes that are not part of the giant component of the graph. 

```{r}
g_<-delete.vertices(g, c("chief", "ellis grey" , "susan grey" , "adele" , "thatch grey","bailey", "tucker", "ben"))
plot(g_)
```

## compute the betweeness of each node in the giant component of the graph. We use the betweeness algorhitm with igraph package on the g_ graph which includes only the giant component of the graph. the graph is not directed and he has no weights on the edges. The nobigint is set to TRUE in order to not use big integers in the calculation because This is only required for lattice-like graphs that have very many shortest paths between a pair of vertices.We also set the normalized parameter to false because we dont want to normalize the results.

# The highest betweeness that measured is for the actor : sloan.

```{r}
betweenness(g_, v = V(g_), directed = FALSE, weights = NULL,
  nobigint = TRUE, normalized = FALSE)
```

## compute the closeness of each node in the giant component of the graph. We use the closeness algorhitm with igraph package on the g_ graph which includes only the giant component of the graph. the graph has no weights on the edges. We also set the normalized parameter to false because we dont want to normalize the results.

# The highest closeness that measured is for the actor : torres.

```{r}
closeness(g_, vids = V(g_),   weights = NULL, normalized = FALSE)
```

## compute the iii.	Eigenvector of each node in the giant component of the graph. We use the eigen_centrality algorhitm with igraph package on the g_ graph which includes only the giant component of the graph. the graph is not directed and he has no weights on the edges. scale is Logical scalar, whether to scale the result to have a maximum score of one (we want to see which actor has the maximal Eigenvector. the options is A named list, to override some ARPACK options.

# The highest Eigenvector that measured is for the actor : karev.

```{r}
eigen_centrality(g_, directed = FALSE, scale = TRUE, weights = NULL,
  options = arpack_defaults)
```

## We devide the communities of the grey anatomy using the edge.betweenness.community algorhitm of the igraph package. This is a divisive method that works on undirected unweighted graphs. It is based on calculating for each edge its betweeness - the number of shortest path going through this edge.It then iteratively removes the edge with the highest betweeness score, until reaching some threshold.The remaining connected vertices are the communities (clusters).

# The number of communities that we get by the algorhitm is : 7.

```{r}
gc <-  edge.betweenness.community(g)
gc
```

## We define a modularity measure that measures the quality of a network partition. It compares the number of edges in each cluster to the expected number of edges within it.

# The modularity that we get by the algorhitm is:  0.5774221 .

```{r}
#modularity for each phase of the previous algorithm
gc$modularity
#best modularity score
max(gc$modularity)
#index (phase, i.e. partitioning) with the best score
which.max(gc$modularity)
```

## We color the nodes by partitions, using the membership function that returns community ids for each vertex, according to our clustering model object (gc).We use the membership method to get the list of clusters assignments the nodes in the graph.

```{r}
#Store cluster ids for each vertex
memb <- membership(gc)
head(memb)
```

## We print the graph according to the colors of the nodes that we get by the membership functions. We print the name of each node in distance of 1.5 from the center of the vertex.

# The size of each community is:
#                                1) size 5.
#                                2) size 3.
#                                3) size 5.
#                                4) size 4.
#                                5) size 4.
#                                6) size 3.
#                                7) size 8.
```{r}
plot(g, vertex.size=5, vertex.label=V(g)$name,
     vertex.color=memb,vertex.label.dist=1.5, asp=FALSE)
```

## We devide the communities of the grey anatomy using the fastgreedy.community algorhitm of the igraph package. This algorithm works on graphs with no self loops,We use the function simplify() to omit self loops from the graph.

# The number of communities that we get by the algorhitm is : 6.

```{r}
# Remove self-loops is exist
g <- simplify(g)
gc2 <-  fastgreedy.community(g)
gc2
```

## We define a modularity measure that measures the quality of a network partition. It compares the number of edges in each cluster to the expected number of edges within it.

# The modularity that we get by the algorhitm is:  0.5947232 .

```{r}
#modularity for each phase of the previous algorithm
gc2$modularity
#best modularity score
max(gc2$modularity)
#index (phase, i.e. partitioning) with the best score
which.max(gc2$modularity)
```

## We color the nodes by partitions, using the membership function that returns community ids for each vertex, according to our clustering model object (gc).We use the membership method to get the list of clusters assignments the nodes in the graph.

```{r}
#Store cluster ids for each vertex
memb2 <- membership(gc2)
head(memb2)
```

## We print the graph according to the colors of the nodes that we get by the membership functions. We print the name of each node in distance of 1.5 from the center of the vertex.

# The size of each community is: 
#                                1) size 5.
#                                2) size 3.
#                                3) size 5.
#                                4) size 5.
#                                5) size 4.
#                                6) size 10.

```{r}
plot(g, vertex.size=5, vertex.label=V(g)$name,
     vertex.color=memb2,vertex.label.dist=1.5, asp=FALSE)
```
