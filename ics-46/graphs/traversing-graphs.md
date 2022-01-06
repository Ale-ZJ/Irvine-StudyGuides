# Traversing Graphs

How to visit every vertex in a graph?

## Breadth-first search (BFS)

* starts at starting vertex
* visits _all_ neighboring vertexes of distance 1
* visits neighbors of distance 2, 3, ..., n
* WITHOUT REVISITING a vertex!
  * traversal is therefore not unique&#x20;

![](<../../.gitbook/assets/image (12) (1) (1).png>)

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

## Depth-first search (DFS)

* starts at starting idx
* follows ONE path from start to end&#x20;
  * traces back to visit undiscovered vertices
* not unique because you can backtrack to diff vertices

![](<../../.gitbook/assets/image (14) (1).png>)

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

## Dijkstra's shortest path

Determines the shortest path from a start vertex to each vertex in a graph.

* for each vertex, it determines it's distance and predecessor pointer
  * distance is the shortest path distance from start
    * all vertices initialized to `inifinity`
    * start vertex distance = 0
  * predecessor points to the previous vertex along the shortest path from start
    * al initialized to `null`

```
DijkstraShortestPath( startV )
{
    for each vertex currentV in graph
    {
        currentV -> distance = Infinity
        currentV -> predV = 0
        Enqueue currentV in unvisitedQueue
    }
    
    //startV has a distance of 0 from itself
    startV ->: distance = 0 
    
    while (unvisitedQueue is not empty)
    {
        // Visit vertex with minimum distance from startV
        currentV = DequeuqMin unvisitedQueue
        
        for each vertex adjV adjacent to currentV
        {
            adgeWeight = weight of edge from currentV to adjV
            alternativePathDistance = currentV -> distance + edgeWeight
            
            // If shorter path from startV to adjV is found
            // update adjV's distance and predecessor
            if (alternativePathDistance < adjV->distance)
            {
                adjV -> distance = alternativePathDistance
                adjV->predV = currentV
            }
        }
    }
    
}
```

After completing the Dijkstra's algorithm, you can find the shortest path by checking if a vertex predecessor is not null and backtracing its parents until you find the start vertex.

### Interlude: Complexity

* if `unvisitedVertexQueue` implemented with a list -> **O(V^2)**
  * outer loop executes V times to visit every vertex&#x20;
  * dequeueing the vertex from queue needs **O(V) **because you need to search all vertices in the list
  * For each `currentV` we search the subset of adjacent edges **E**
    * ** E < V^2 **
    * then the runtime is **O(V^2 + E) = O(V^2)**
* if `unvisitedVertexQueue` is implemented with a standard binary heap -> **O( (E + V) log V ) **
* if `unvisitedVertexQueue` uses Fibonacci head data structure **O(E + V log V)**

{% hint style="warning" %}
Dijkstra's algorithm may not find the shortest path with negative edges
{% endhint %}

## Kruska's Minimum Spanning Tree

A **Minimum Spanning Tree **is a subset of all edges that conenct all vertices in the graph together with the minimum sim of edge weights. Imagine you are trapped in an archipielago and want to connect all islands with the least amount of resources.&#x20;

Here is a vocab term: **connected** graph contains a path between every pair of vertices

![](<../../.gitbook/assets/image (13) (1).png>)

Kruskal's algorithm uses:

* edge list initialized with all edges in the graph&#x20;
* collection of vertex sets that represent the subsets of vertices connected by current set of edges in the mst
  * initially, the vertex sets consists of one set for each vertex
* a set of edges forming the resulting mst

```
KruskalsMST( graph )
{
    edgeList = list containing all edges from graph 
    
    vertexSets = collection of vertex sets, empty initially 
    for each vertex V in graph
        Add new set containing only V to vertexSets
    
    resultList = new, empty set of edges
    
    while( vertexSets -> length > 1 && edgeLIst -> length> 0 )
    {
        nextEdge = remove edge with minimum weight from edgeList
        vSet1 = set in vertexSets containing nextEdge -> vertex1
        vSet2 = set in vertexSets containing nextEdge -> vertex2
        
        if ( vSet1 != vSet2 )
        {
            Add nextEdge to resultList
            Remove vSet1 and vSet2 from vertexSets
            merged = union(vSet1, vSet2)
            Add merged to vertexSets
        }
    }
    
    return resultList
}
```

{% hint style="info" %}
There is only one unique MST for a graph that has no duplicate edge weights
{% endhint %}

## Prim's Minimum Spanning Tree



## Prim vs. Kruska

| Kruska                                                                    |                                                                           |
| ------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| builds tree by adding one **edge** at a time                              | builds tree by ading one **vertex** at a tome                             |
| next edge is always the shortest ONLY if it does not create a cycle       | next vertex added is always th on neared to a vertex already on the graph |
| Time: **O(E log V)**                                                      | Time: **O(E + V log V) **amortized time, if Fibonacci Heap is used        |
| performs better with spars graphs because it uses simpler data structures | faster for dense graphs                                                   |
| optimal implementation: union find                                        | optimal implementation: fibonacci heap                                    |
| easier implementation                                                     | hard to implement man                                                     |

Here is a nice diagram that I found on stack [overflow](https://stackoverflow.com/questions/1195872/when-should-i-use-kruskal-as-opposed-to-prim-and-vice-versa).

![](<../../.gitbook/assets/image (15).png>)

![](<../../.gitbook/assets/image (14).png>)
