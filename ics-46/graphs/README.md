# Graphs

A **graph **is a data structure that represents conenctions among items.

* **vertex (node):** one item in a graph
* **edge:** connection between two vertices in a graph
  * can have weights



* One vertex can have more than one edge
* an edge cannot connect more than two vertices

## Adjacency and Paths

* two vertices are adjacent if connected by an edge&#x20;
* a path is a sequence of edges from start to end vertex
  * distance is the number of edges between two vertices
    * there are multiple&#x20;

## Graph Representation

### Adjacency Lists&#x20;

Two vertices are adjacent if connected by an edge. In am **adjacency list** repersentation, each vertex has a list of adjacent vertices, each list item representing an edge.'

The adjacent list size will be **O(V + E)**  because each vertex appears once and each edge appears twice. This is a problem when traversing a vertex's list to find an adjacent vertex. It would be **O(V). **But most graphs are **spare graphs,** have far fewer edges than the maximum possible.

For example:&#x20;

![](<../../.gitbook/assets/image (12) (1) (1) (1).png>)

### Adjacency Matrix

Each vertex is assigned to a matrix row and column. A matrix element is 1 if the corresponding two vertices have an edge or 0 otherwise.

![](<../../.gitbook/assets/image (13) (1) (1).png>)

Common programming implementation would be a 2-d array. Therefore, it would have a size of **O(V^2). **Meanwhile, it has a key benefit of **O(1) **because the corresponding element is just checked for 0 or 1.

{% hint style="info" %}
A matrix representation can only represent edges among vertices and NOT data.
{% endhint %}

## Directed Graph

A **directed graph** consists of vertices connected by directed edges. If there is an edge pointing from X to Y, then Y (terminating) is adjacent to X (starting).

* **path:** sequence of directed edges from a starting vertex to a destination vertex
* **cycle: **a path that starts and ends at the same vertex
  * a self loop is a cycle

## Weighted Graphs

Each edge has a "weight", a numerical value that represents a cost. Can be directed or undirected.&#x20;

* **path length: **is the sum of the edge weights in the path from point A to B
* **negative edge weight cycles **has a cycle less than 0 thus a shortest path doesnt exist. Each loop aronud the negative edge weight cycle further decreases the cycle length

##

