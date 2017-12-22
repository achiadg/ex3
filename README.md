---
title: "final-report"
output: md_document
---

### Students: Hod Bublil, Achiad Gelerenter.
### ID: 305212466, 305231995.   

# question 1 part a

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

![Image of github's cat](/images/question1_parta_image1.PNG)

## compute the component of the graph in order to delete the nodes that are not part of the giant component of the graph. (you asked us to work on the giant component of the graph).

```{r}
clu <- components(g)
groups(clu)
```
![Image of github's cat](/images/question1_parta_image2.PNG)

## delete the nodes that are not part of the giant component of the graph. 

```{r}
g_<-delete.vertices(g, c("chief", "ellis grey" , "susan grey" , "adele" , "thatch grey","bailey", "tucker", "ben"))
plot(g_)
```

![Image of github's cat](/images/question1_parta_image3.PNG)

## compute the betweeness of each node in the giant component of the graph. We use the betweeness algorhitm with igraph package on the g_ graph which includes only the giant component of the graph. the graph is not directed and he has no weights on the edges. The nobigint is set to TRUE in order to not use big integers in the calculation because This is only required for lattice-like graphs that have very many shortest paths between a pair of vertices.We also set the normalized parameter to false because we dont want to normalize the results.

# The highest betweeness that measured is for the actor : sloan.

```{r}
betweenness(g_, v = V(g_), directed = FALSE, weights = NULL,
  nobigint = TRUE, normalized = FALSE)
```

![Image of github's cat](/images/question1_parta_image4.PNG)

## compute the closeness of each node in the giant component of the graph. We use the closeness algorhitm with igraph package on the g_ graph which includes only the giant component of the graph. the graph has no weights on the edges. We also set the normalized parameter to false because we dont want to normalize the results.

# The highest closeness that measured is for the actor : torres.

```{r}
closeness(g_, vids = V(g_),   weights = NULL, normalized = FALSE)
```

![Image of github's cat](/images/question1_parta_image5.PNG)

## compute the Eigenvector of each node in the giant component of the graph. We use the eigen_centrality algorhitm with igraph package on the g_ graph which includes only the giant component of the graph. the graph is not directed and he has no weights on the edges. scale is Logical scalar, whether to scale the result to have a maximum score of one (we want to see which actor has the maximal Eigenvector. the options is A named list, to override some ARPACK options.

# The highest Eigenvector that measured is for the actor : karev.

```{r}
eigen_centrality(g_, directed = FALSE, scale = TRUE, weights = NULL,
  options = arpack_defaults)
```

![Image of github's cat](/images/question1_parta_image6.PNG)

# question 1 part b

## delete the nodes that are not part of the giant component of the graph. 

```{r}
g_<-delete.vertices(g, c("chief", "ellis grey" , "susan grey" , "adele" , "thatch grey","bailey", "tucker", "ben"))
plot(g_)
```
![Image of github's cat](/images/question1_parta_image3.PNG)

## We devide the communities of the grey anatomy using the edge.betweenness.community algorhitm of the igraph package. This is a divisive method that works on undirected unweighted graphs. It is based on calculating for each edge its betweeness - the number of shortest path going through this edge.It then iteratively removes the edge with the highest betweeness score, until reaching some threshold.The remaining connected vertices are the communities (clusters).

# The number of communities that we get by the algorhitm is : 6.

```{r}
gc <-  edge.betweenness.community(g_)
gc
```
![Image of github's cat](/images/question1_partb_image1.PNG)

## We define a modularity measure that measures the quality of a network partition. It compares the number of edges in each cluster to the expected number of edges within it.

# The modularity that we get by the algorhitm is:  0.46875 .

```{r}
#modularity for each phase of the previous algorithm
gc$modularity
#best modularity score
max(gc$modularity)
#index (phase, i.e. partitioning) with the best score
which.max(gc$modularity)
```

![Image of github's cat](/images/question1_partb_image2.PNG)

## We color the nodes by partitions, using the membership function that returns community ids for each vertex, according to our clustering model object (gc).We use the membership method to get the list of clusters assignments the nodes in the graph.

```{r}
#Store cluster ids for each vertex
memb <- membership(gc)
head(memb)
```

![Image of github's cat](/images/question1_partb_image3.PNG)

## We print the graph according to the colors of the nodes that we get by the membership functions. We print the name of each node in distance of 1.5 from the center of the vertex.

# The size of each community is:
#                                1) size 4.
#                                2) size 4.
#                                3) size 4.
#                                4) size 4.
#                                5) size 3.
#                                6) size 5.
```{r}
plot(g_, vertex.size=5, vertex.label=V(g_)$name,
     vertex.color=memb,vertex.label.dist=1.5, asp=FALSE)
```

![Image of github's cat](/images/question1_partb_image4.PNG)

## We devide the communities of the grey anatomy using the fastgreedy.community algorhitm of the igraph package. This algorithm works on graphs with no self loops,We use the function simplify() to omit self loops from the graph.

# The number of communities that we get by the algorhitm is : 5.

```{r}
# Remove self-loops is exist
g_<- simplify(g_)
gc2 <-  fastgreedy.community(g_)
gc2
```

![Image of github's cat](/images/question1_partb_image5.PNG)

## We define a modularity measure that measures the quality of a network partition. It compares the number of edges in each cluster to the expected number of edges within it.

# The modularity that we get by the algorhitm is:  0.4789541 .

```{r}
#modularity for each phase of the previous algorithm
gc2$modularity
#best modularity score
max(gc2$modularity)
#index (phase, i.e. partitioning) with the best score
which.max(gc2$modularity)
```

![Image of github's cat](/images/question1_partb_image6.PNG)

## We color the nodes by partitions, using the membership function that returns community ids for each vertex, according to our clustering model object (gc2).We use the membership method to get the list of clusters assignments the nodes in the graph.

```{r}
#Store cluster ids for each vertex
memb2 <- membership(gc2)
head(memb2)
```

![Image of github's cat](/images/question1_partb_image7.PNG)

## We print the graph according to the colors of the nodes that we get by the membership functions. We print the name of each node in distance of 1.5 from the center of the vertex.

# The size of each community is: 
#                                1) size 5.
#                                2) size 3.
#                                3) size 4.
#                                4) size 5.
#                                5) size 7.

```{r}
plot(g_, vertex.size=5, vertex.label=V(g_)$name,
     vertex.color=memb2,vertex.label.dist=1.5, asp=FALSE)
```

![Image of github's cat](/images/question1_partb_image8.PNG)

# question 2

## set up the working directory for the task

```{r}
folder = 'C:/Users/אחיעד/Desktop/data science/ass3/question2'
setwd(folder)

#Or for all chuncks in this Rmarkdown:
knitr::opts_knit$set(root.dir = folder)
```

## install and import the packages that we need for this task.

```{r}
#install.packages("twitteR")
#install.packages("tm")
#install.packages("httr")
#install.packages("igraph")
library(igraph)
library(twitteR)
library(tm)
library(httr)


source("my_credentials.R")
```

## Here we set up the OAuth credentials for a twitteR session:

```{r}
achiad_credentials <- setup_twitter_oauth(consumer_key, consumer_secret, access_token, access_secret)
```
# question 2 part a

# the data we collected is the users that tweets on the champions league. and all the details that tweeter save for his users. we collect the data in temp_df data frame.

## Here we get the 500 last tweets that people twits on the champions league, after we get all this twits we convert them to data frame, and take the usernames that twited those twits. once we have this usernames we lookup for the users for those usernames and save them in dataframe for future use.

```{r}
searchRes <- searchTwitter("#champions_league", n=500)
champions_tag <- twListToDF(searchRes) # converting to df
usernames <- champions_tag$screenName
temp_df <- twListToDF(lookupUsers(usernames))
```
## Here we set up two empty vectors.

```{r}
  user1_edge <- c()
  user2_edge <- c()
```
# question 2 part b

# now we will declare the nodes and edges in the graph: the nodes are the users that we collected, it will be an edge between two users if the twits that they twited was in the same language.

## Now after we get all the users in dataframe we iterate over all the users, and if we found another user that tweets on the champions league in the same language like the first user we add them to the vectors, the first user to the first vector and the second user to the second vector.

```{r}
for(i in 1:nrow(temp_df)){
  for(j in 1:nrow(temp_df)){
    user1 <- temp_df[i,]
    user2 <- temp_df[j,]
    if((user1$screenName != user2$screenName) && (user1$lang == user2$lang)){
      user1_edge<-c(user1_edge , user1$screenName)
      user2_edge<-c(user2_edge , user2$screenName)
    }
  }
}
    
```

## Now we create a csv file from this two vectors in order to create a graph from this csv file like we did in the first task on the gray anatomy file. 

```{r}
res <- cbind(from= user1_edge,to=user2_edge)
write.csv(res,file="C:/Users/àçéòã/Desktop/data science/ass3/question2/twits_edgelist.csv",row.names = F)
```
# question 2 part c

## read the users edge list we created and create a graph from it using the igraph package. compute the degree of each node in the graph , and print the graph using the degree of each node to see which nodes are the most connected in the graph.

```{r}
ga.data <- read.csv('twits_edgelist.csv', header = T)
g <- graph.data.frame(ga.data,directed = F)
degr.score <- degree(g)
V(g)$size <- degr.score * 4 # multiply by 2 for scale 

plot(g, vertex.size=5, vertex.label=V(g)$name, vertex.label.dist=2, asp=FALSE)
```

![Image of github's cat](/images/question2_image1.PNG)

# question 2 part d

## compute the betweeness of each node in the graph. We use the betweeness algorhitm with igraph package on the g graph. the graph is not directed and he has no weights on the edges. The nobigint is set to TRUE in order to not use big integers in the calculation because This is only required for lattice-like graphs that have very many shortest paths between a pair of vertices.We also set the normalized parameter to false because we dont want to normalize the results.

# The highest betweeness that measured is for the user: all the users have betweeness of 0 because we built the graph in a way that in each component all the nodes are equal in the betweeness measure because all the nodes in each component are connect to each other node.

```{r}
betweenness(g, v = V(g), directed = FALSE, weights = NULL,
  nobigint = TRUE, normalized = FALSE)
```

![Image of github's cat](/images/question2_image2.PNG)

## compute the closeness of each node in the giant component of the graph. We use the closeness algorhitm with igraph package on the g graph. the graph has no weights on the edges. We also set the normalized parameter to false because we dont want to normalize the results.

# The highest closeness that measured is for the user: All the user in the same component has the same closeness value because each node connect to all the other nodes in the component. so all the nodes in the giant component has the best closeness value from the definition of closeness.

```{r}
closeness(g, vids = V(g),   weights = NULL, normalized = FALSE)
```

![Image of github's cat](/images/question2_image3.PNG)

## compute the Eigenvector of each node in the giant component of the graph. We use the eigen_centrality algorhitm with igraph package on the g graph. the graph is not directed and he has no weights on the edges. scale is Logical scalar, whether to scale the result to have a maximum score of one (we want to see which actor has the maximal Eigenvector. the options is A named list, to override some ARPACK options.

# The highest Eigenvector that measured is for the user : All the user in the same component has the same Eigenvector value because each node connect to all the other nodes in the component. so all the nodes in the giant component has the best Eigenvector value from the definition of Eigenvector.


```{r}
eigen_centrality(g, directed = FALSE, scale = TRUE, weights = NULL,
  options = arpack_defaults)
```

![Image of github's cat](/images/question2_image4.PNG)

## We devide the communities of the users graph using the edge.betweenness.community algorhitm of the igraph package. This is a divisive method that works on undirected unweighted graphs. It is based on calculating for each edge its betweeness - the number of shortest path going through this edge.It then iteratively removes the edge with the highest betweeness score, until reaching some threshold.The remaining connected vertices are the communities (clusters).

# The number of communities that we get by the algorhitm is : 8.

```{r}
gc <-  edge.betweenness.community(g)
gc
```

![Image of github's cat](/images/question2_image5.PNG)

## We define a modularity measure that measures the quality of a network partition. It compares the number of edges in each cluster to the expected number of edges within it.

# The modularity that we get by the algorhitm is:   0.4413371.

```{r}
#modularity for each phase of the previous algorithm
gc$modularity
#best modularity score
max(gc$modularity)
#index (phase, i.e. partitioning) with the best score
which.max(gc$modularity)
```

![Image of github's cat](/images/question2_image6.PNG)

## We color the nodes by partitions, using the membership function that returns community ids for each vertex, according to our clustering model object (gc).We use the membership method to get the list of clusters assignments the nodes in the graph.

```{r}
#Store cluster ids for each vertex
memb <- membership(gc)
head(memb)
```

![Image of github's cat](/images/question2_image7.PNG)

## We print the graph according to the colors of the nodes that we get by the membership functions. We print the name of each node in distance of 1.5 from the center of the vertex.

# The size of each community is:
#                                1) size 2.
#                                2) size 6.
#                                3) size 2.
#                                4) size 4.
#                                5) size 4.
#                                6) size 3.
#                                7) size 2.
#                                8) size 14.

```{r}
plot(g, vertex.size=5, vertex.label=V(g)$name,
     vertex.color=memb,vertex.label.dist=1.5, asp=FALSE)
```

![Image of github's cat](/images/question2_image8.PNG)

## We devide the communities of the grey anatomy using the fastgreedy.community algorhitm of the igraph package. This algorithm works on graphs with no self loops,We use the function simplify() to omit self loops from the graph.

# The number of communities that we get by the algorhitm is : 8.

```{r}
# Remove self-loops is exist
g <- simplify(g)
gc2 <-  fastgreedy.community(g)
gc2
```

![Image of github's cat](/images/question2_image5.PNG)

## We define a modularity measure that measures the quality of a network partition. It compares the number of edges in each cluster to the expected number of edges within it.

# The modularity that we get by the algorhitm is:  0.4413371 .

```{r}
#modularity for each phase of the previous algorithm
gc2$modularity
#best modularity score
max(gc2$modularity)
#index (phase, i.e. partitioning) with the best score
which.max(gc2$modularity)
```

![Image of github's cat](/images/question2_image6.PNG)

## We color the nodes by partitions, using the membership function that returns community ids for each vertex, according to our clustering model object (gc2).We use the membership method to get the list of clusters assignments the nodes in the graph.

```{r}
#Store cluster ids for each vertex
memb2 <- membership(gc2)
head(memb2)
```
![Image of github's cat](/images/question2_image7.PNG)

## We print the graph according to the colors of the nodes that we get by the membership functions. We print the name of each node in distance of 1.5 from the center of the vertex.

# The size of each community is:
#                                1) size 2.
#                                2) size 6.
#                                3) size 2.
#                                4) size 4.
#                                5) size 4.
#                                6) size 3.
#                                7) size 2.
#                                8) size 14.


```{r}
plot(g, vertex.size=5, vertex.label=V(g)$name,
     vertex.color=memb2,vertex.label.dist=1.5, asp=FALSE)
```

![Image of github's cat](/images/question2_image8.PNG)

# The two community algorithms gets the same result because we built the graph in a way that the modularity will be the same in every algorhitm because all the nodes connected to each other in the same component.

