---
layout: post
title: 使用RNeo4j对图进行可视化
category: 技术
tags: 数据库 可视化
keywords:
description:
---

> 本学习笔记，基于已安装Neo4j的前提下，并且已导入电影关联图数据（详情见<a href="http://littleroach110.net/2017/03/28/Acquaintance-Neo4j.html#title24">4.2 电影图创建</a>），测试环境下的Neo4j的用户名为test，密码为neo4j

### 使用步骤及教程

##### 1. 安装R

见网址：https://mirrors.tuna.tsinghua.edu.cn/CRAN/， 选择Mac OS X 下的R-3.4.0.pkg包安装 

##### 2. 安装igraph、visNetwork和RNeo4j库

打开R Console，输入
```
> install.packages("igraph")
> install.packages("visNetwork")
> install.packages("RNeo4j")
```

##### 3. 加载库并连接

加载库

```
> library(igraph)
> library(visNetwork)
> library(RNeo4j)
```

连接Neo4j，并加载movie数据集：

```
> graph = startGraph("http://localhost:7474/db/data/",user='test',password='neo4j')

> importSample(graph, "movies", input=F)
```

##### 4. 使用Cypher查询边的集合列表

在本例子中，我们想对演员和他们之间的关系进行可视化，如果两个演员同在一个电影中演过，那么他们之间将有一条边。

```
> query = " MATCH (p1:Person)-[:ACTED_IN]->(:Movie)<-[:ACTED_IN]-(p2:Person) WHERE p1.name < p2.name RETURN p1.name AS from, p2.name AS to, COUNT(*) AS weight "

> edges = cypher(graph, query)

> head(edges)
```

##### 5. 使用Cypher查询点的集合列表

visNetwork功能需要一个边的数据集和一个点的数据集，所以可以得到一个点数的数据集，通过从边的数据集中的唯一的值。

```
> nodes = data.frame(id=unique(c(edges$from, edges$to)))

> nodes$label = nodes$id

> head(nodes)
```

##### 6. 通过visNetwork加载点和边，并生成关联图

```
> visNetwork(nodes,edges)
```

执行完成后，自动生成一个html文件，并通过浏览器打开，如图所示：

![Visualizing Your Graph with RNeo4j 01]({{site.CDN_PATH}}/public/image/20170502-Visualizing-Your-Graph-with-RNeo4j01.png)

##### 7. 在visNetwork中使用igraph

igraph是一个图算法库，可以非常容易通过创建一个igraph图对象，通过进入边列表来控制子图。

```
> ig = graph_from_data_frame(edges, directed=F) 

> ig
```

在visNetwork中，在节点数据集的任何值列解译成节点大小。

```
> nodes$value = betweenness(ig)

> head(nodes)
```

添加完该值列，展示的节点的大小会不一样

```
> visNetwork(nodes,edges)
```

生成关联图，如图所示：

![Visualizing Your Graph with RNeo4j 02]({{site.CDN_PATH}}/public/image/20170502-Visualizing-Your-Graph-with-RNeo4j02.png)

##### 8. 运行社区检测算法

运行社区检测算法，识别图中的聚类。

在该例子中，使用Girvan-Newman边之间聚类算法，在igraph中作为cluster_edge_betweenness导出。

```
> clusters = cluster_edge_betweenness(ig)

> clusters[1:2]
```

以上两个聚类中的所有演员，共有6个聚类

```
> length(clusters) 
## [1] 6
```

如果我们添加一个组列到节点数据集，visNetwork将根据他们分组情况对节点进行着色，可以使用clusters$membership获取聚类任务。移除value列以保证节点大小是统一的。

```
> nodes$group = clusters$membership

> nodes$value = NULL

> head(nodes)
```

通过visNetwork添加点和边的数据集，group列已经添加，通过指派的聚类，对节点进行着色。

```
> visNetwork(nodes,edges)
```

生成关联图，如图所示：

![Visualizing Your Graph with RNeo4j 03]({{site.CDN_PATH}}/public/image/20170502-Visualizing-Your-Graph-with-RNeo4j03.png)

使用以上工作流程，可以直接从Neo4j中查询一个子图，并可视化图的聚类算法。

### 参考

【1】Visualizing Your Graph with RNeo4j，https://neo4j.com/blog/visualize-graph-with-rneo4j/