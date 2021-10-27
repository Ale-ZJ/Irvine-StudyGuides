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

![](<../../.gitbook/assets/image (12) (1) (1).png>)

### Adjacency Matrix

Each vertex is assigned to a matrix row and column. A matrix element is 1 if the corresponding two vertices have an edge or 0 otherwise.

![](<../../.gitbook/assets/image (13) (1).png>)

Common programming implementation would be a 2-d array. Therefore, it would have a size of **O(V^2). **Meanwhile, it has a key benefit of **O(1) **because the corresponding element is just checked for 0 or 1.

{% hint style="info" %}
A matrix representation can only represent edges among vertices and NOT data.
{% endhint %}

## Graph Traversal

How to visit every vertex in a graph?

### Breadth-first search (BFS)

* starts at starting vertex
* visits _all_ neighboring vertexes of distance 1
* visits neighbors of distance 2, 3, ..., n
* WITHOUT REVISITING a vertex!
  * traversal is therefore not unique&#x20;

![](<../../.gitbook/assets/image (12) (1).png>)

```clike
BFS( startV )
{
    create queue named frontierQueue and enqueue startV
    Add startV to discovredSet
    
    while ( frontierQueue is not empty ) 
    {
        currentV = Dequeue from frontierQueue
        
        //"Visit" currentV
        for each vertex adjV adajacent to currentV
        {
            if (adjV is not in discovereSet)
            {
                Enqueue adjV in frontierQueue
                Add adjV to discoveredSet
            }
        }
    }
}
```

### Depth-first search (DFS)

* starts at starting idx
* follows ONE path from start to end&#x20;
  * traces back to visit undiscovered vertices
* not unique because you can backtrack to diff vertices

![](<../../.gitbook/assets/image (14).png>)

```
DFS( startV )
{
    Push startV to stack
    
    while (stack is not empty)
    {
        currentV = Pop stack
        
        if (currentV is not in visitedSet)
        {
            "Visit" currentV
            Add currentV to visitedSet
            for each vertex adjV adjacent to currentV
                Push adjV to stack
        }
    }
}
```

{% hint style="info" %}
DFS terminated when the stack is empty
{% endhint %}

Another way to write the algorithm using recursion:

```
RecursiveDFS(currentV)
{
    if (currentV is not in visitedSet)
    {
        Add currentV to visitedSet
        "Visit" currentV
        for each vertex adjV adjacent to currentV
        {
            RecursiveDFS(adjV)
        }
    }
}
```

{% hint style="info" %}
This algorithm is implemented using the program's stack instead of an explicit stack!!
{% endhint %}

## Directed Graph

A **directed graph** consists of vertices connected by directed edges. If there is an edge pointing from X to Y, then Y (terminating) is adjacent to X (starting).

* **path:** sequence of directed edges from a starting vertex to a destination vertex
* **cycle: **a path that starts and ends at the same vertex
  * a self loop is a cycle

## Topological Sort

of a directed, acyclic graph produces a list of the graphs vertices such that for every edg from a vertex X to a vertex Y, X comes first bfore Y in the list

* vertex has no incoming edge -> first vertices in topological sort
* ends with a vertex with no outgoing edges
* topological sort exist even with unconnected vertices

```
GraphTopologicalSort(graph)
{
    resultList = mpty list of vertices
    noIncoming = list of all vertices with no incoming eedges 
    remainingEges = list of all edges in the graph 
    
    while( noIncoming is not empty)
    {
        currentV = remove any vertex from noIncoming 
        Add currentV to result 
        outgoingEdges = remove currentV's outgoing edge from remaaingEdge
        for each edge currentE in outgoingEdges 
        {
            inCount = GraphGetIncomingEdgeCount(remainingEdges, currentE⇢toVertex)
            if (inCount == 0)
                Add currentE⇢toVertex to noIncoming
        }
    }
}

GraphGetIncomingEdgeCount(edgeList, vertex) {
   count = 0
   for each edge currentE in edgeList {
      if (edge⇢toVertex == vertex)
         count = count + 1
   }
   return count
}
```





