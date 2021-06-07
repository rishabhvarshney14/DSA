# Graphs

- [Graph Implementation using Matrix](#Graph-Implementation-using-Matrix)
- [Graph Implementation using List](#Graph-Implementation-using-List)
- [Breadth First Search](#Breadth-First-Search)
- [Depth First Search](#Depth-First-Search)
- [Cycle Detection in Undirected Graph using BFS](#Cycle-Detection-in-Undirected-Graph-using-BFS)
- [Cycle Detection in Undirected Graph using DFS](#Cycle-Detection-in-Undirected-Graph-using-DFS)
- [Bipartitie Graph using BFS](#Bipartitie-Graph-using-BFS)
- [Bipartitie Graph using DFS](#Bipartitie-Graph-using-DFS)
- [Cycle Detection in Directed Graph using DFS](#Cycle-Detection-in-Directed-Graph-using-DFS)
- [Topological Sort using DFS](#Topological-Sort-using-DFS)
- [Topological Sort using BFS or Kahn's Algorithm](#Topological-Sort-using-BFS-or-Kahn's-Algorithm)
- [Cycle Detection in Directed Graph using BFS](#Cycle-Detection-in-Directed-Graph-using-BFS)
- [Shortest path in a weighted directed acyclic graph using TopoSort (DAG)](#Shortest-path-in-a-weighted-directed-acyclic-graph-using-TopoSort-(DAG))
- [Dijksra's Algorithm](#Dijksra's-Algorithm)
- [Prim's Algorithm](#Prim's-Algorithm)
- [Disjoint Sets](#Disjoint-Sets)
- [Kruskal's Algorithm](#Kruskal's-Algorithm)
- [Bridges in Graphs](#Bridges-in-Graphs)
- [Articulation Point](#Articulation-Point)
- [Bellman Ford Algorithm](#Bellman-Ford-Algorithm)

## Graph Implementation using Matrix


```python
class GraphMatrix:
    def __init__(self, vertex, directed):
        self.graph = [[0] * vertex for _ in range(vertex)]
        self.vertex = vertex
        self.directed = directed
        
    def add_edge(self, from_vertex, to_vertex):
        self.graph[from_vertex - 1][to_vertex - 1] = 1
        
        if not self.directed:
            self.graph[to_vertex - 1][from_vertex - 1] = 1
    
    def show(self):
        print(" ", end=" ")
        for i in range(1, self.vertex + 1):
            print(i, end=" ")
        print()
        for i in range(1, self.vertex + 1):
            print(i, end=" ")
            for j in range(1, self.vertex + 1):
                print(self.graph[i-1][j-1], end=" ")
            print()
```


```python
graph = GraphMatrix(11, False)

graph.add_edge(1, 2)
graph.add_edge(2, 4)
graph.add_edge(3, 5)
graph.add_edge(5, 10)
graph.add_edge(5, 6)
graph.add_edge(6, 7)
graph.add_edge(7, 8)
graph.add_edge(8, 9)
graph.add_edge(9, 11)
graph.add_edge(9, 10)
graph.show()
```

      1 2 3 4 5 6 7 8 9 10 11 
    1 0 1 0 0 0 0 0 0 0 0 0 
    2 1 0 0 1 0 0 0 0 0 0 0 
    3 0 0 0 0 1 0 0 0 0 0 0 
    4 0 1 0 0 0 0 0 0 0 0 0 
    5 0 0 1 0 0 1 0 0 0 1 0 
    6 0 0 0 0 1 0 1 0 0 0 0 
    7 0 0 0 0 0 1 0 1 0 0 0 
    8 0 0 0 0 0 0 1 0 1 0 0 
    9 0 0 0 0 0 0 0 1 0 1 1 
    10 0 0 0 0 1 0 0 0 1 0 0 
    11 0 0 0 0 0 0 0 0 1 0 0 


## Graph Implementation using List


```python
class GraphList:
    def __init__(self, directed, weighted):
        self.adj_list = dict()
        self.directed = directed
        self.weighted = weighted
        
    def add_edge(self, from_vertex, to_vertex, weight=None):
        if self.weighted:
            temp = [to_vertex, weight]
        else:
            temp = to_vertex
            
        if from_vertex in self.adj_list:
            self.adj_list[from_vertex].append(temp)
        else:
            self.adj_list[from_vertex] = [temp]
            
        if to_vertex not in self.adj_list:
            self.adj_list[to_vertex] = []
            
        if not self.directed:
            if self.weighted:
                temp = [from_vertex, weight]
            else:
                temp = from_vertex
                
            self.adj_list[to_vertex].append(temp)
            
    def print_graph(self):
        for key in range(len(self.adj_list)):
            print(key, "-->", self.adj_list[key])
            
    def __len__(self):
        return len(self.adj_list)
    
    def __getitem__(self, atr):
        return self.adj_list[atr]
```


```python
# graph = GraphList(False, False) # Undirected
graph = GraphList(True, False) # Directed

# graph.add_edge(0, 1)
# graph.add_edge(1, 2)
# graph.add_edge(2, 3)
# graph.add_edge(3, 4)
# graph.add_edge(4, 5)
# graph.add_edge(5, 6)
# graph.add_edge(7, 3)

graph = GraphList(True, True) # Directed and Weighted for shorted distance
graph.add_edge(0, 1, 2)
graph.add_edge(1, 3, 5)
graph.add_edge(5, 2, 2)
graph.add_edge(2, 4, 1)
graph.add_edge(4, 9, 6)
graph.add_edge(3, 4, 0)
graph.add_edge(4, 5, 4)
graph.add_edge(5, 6, 8)
graph.add_edge(6, 7,3)
graph.add_edge(7, 8, 3)
graph.add_edge(9, 10, 2)
graph.add_edge(7, 10, 1)
graph.add_edge(8, 9, 1)
# graph.add_edge(8, 10, 3) # To check for Bipartitie Graph false case

graph.print_graph()
```

    0 --> [[1, 2]]
    1 --> [[3, 5]]
    2 --> [[4, 1]]
    3 --> [[4, 0]]
    4 --> [[9, 6], [5, 4]]
    5 --> [[2, 2], [6, 8]]
    6 --> [[7, 3]]
    7 --> [[8, 3], [10, 1]]
    8 --> [[9, 1]]
    9 --> [[10, 2]]
    10 --> []


## Breadth First Search 


```python
def breadthFirstSearch(graph):
    num_nodes = len(graph)
    vis = [0 for _ in range(num_nodes)]
    storeBFS = []
    
    for ele in range(num_nodes):
        if vis[ele] == 0:
            queue = [ele]
            storeBFS.append(ele)
            vis[ele] = 1
            
            while len(queue) > 0:
                node = queue.pop(0)
                
                for ver in graph[node]:
                    if vis[ver] == 0:
                        vis[ver] = 1
                        storeBFS.append(ver)
                        queue.append(ver)
    return storeBFS
```


```python
breadthFirstSearch(graph)
```




    [0, 1, 3, 4, 9, 5, 10, 6, 7, 8, 2]



## Depth First Search


```python
def dfs(ele, vis, storeDFS, graph):
    storeDFS.append(ele)
    vis[ele] = 1
    
    for node in graph[ele]:
        if vis[node] == 0:
            dfs(node, vis, storeDFS, graph)

def depthFirstSearch(graph):
    num_nodes = len(graph)
    vis = [0 for _ in range(num_nodes)]
    storeDFS = []
    
    for ele in range(num_nodes):
        if vis[ele] == 0:
            dfs(ele, vis, storeDFS, graph)
    
    return storeDFS
```


```python
depthFirstSearch(graph)
```




    [0, 1, 3, 4, 9, 10, 5, 6, 7, 8, 2]



## Cycle Detection in Undirected Graph using BFS


```python
def checkForCycleBFS(node, graph, vis):
    vis[node] = 1
    
    queue = [(node, -1)]
    
    while queue:
        ver, parent = queue.pop(0)
        
        for ele in graph[ver]:
            if vis[ele] == 0:
                queue.append((ele, ver))
                vis[ele] = 1
            elif ele != parent:
                return True
    
    return False

def haveCycleBFS(graph):
    vis = [0 for _ in range(len(graph))]
    
    for node in range(len(graph)):
        if vis[node] == 0:
            if checkForCycleBFS(node, graph, vis):
                return True
    
    return False

haveCycleBFS(graph)
```




    True



## Cycle Detection in Undirected Graph using DFS


```python
def checkForCycleDFS(node, parent, graph, vis):
    vis[node] = 1
    
    for ver in graph[node]:
        if vis[ver] == 0:
            if checkForCycleDFS(ver, node, graph, vis):
                return True
        elif ver != parent:
            return True
    
    return False

def haveCycleDFS(graph):
    vis = [0 for _ in range(len(graph))]
    
    for node in range(len(graph)):
        if vis[node] == 0:
            if checkForCycleDFS(node, -1, graph, vis):
                return True
        
    return False

haveCycleDFS(graph)
```




    True



## Bipartitie Graph using BFS


```python
def checkBipartiteGraphBFS(node, graph, color):
    color[node] = 0
    queue = [node]
    
    while queue:
        node_temp = queue.pop(0)
        
        for node_itr in graph[node_temp]:
            if color[node_itr] == -1:
                color[node_itr] = 1 - color[node_temp]
                queue.append(node_itr)
            elif color[node_itr] == color[node_temp]:
                return False
    
    return True

def colorGraphBFS(graph):
    color = [-1 for _ in range(len(graph))]
    
    for node in range(len(graph)):
        if color[node] == -1:
            if not checkBipartiteGraphBFS(node, graph, color):
                return False
    
    return color

colorGraphBFS(graph)
```




    False



## Bipartitie Graph using DFS


```python
def checkBipartiteGraphDFS(node, parent_color, graph, color):
    color[node] = 1 - parent_color
    
    for node_itr in graph[node]:
        if color[node_itr] == -1:
            if not checkBipartiteGraphDFS(node_itr, color[node], graph, color):
                return False
        elif color[node_itr] == color[node]:
            return False
    
    return True

def colorGraphDFS(graph):
    color = [-1 for _ in range(len(graph))]
    
    for node in range(len(graph)):
        if color[node] == -1:
            if not checkBipartiteGraphDFS(node, 1, graph, color):
                return False
    
    return color

colorGraphDFS(graph)
```




    False



## Cycle Detection in Directed Graph using DFS


```python
def checkCycleDirectedDFS(node, graph, vis, dfs_vis): 
    vis[node] = 1
    dfs_vis[node] = 1
    
    for node_itr in graph[node]:
        if vis[node_itr] == 0:
            if checkCycleDirectedDFS(node_itr, graph, vis, dfs_vis):
                return True
        elif dfs_vis[node_itr] == 1:
            return True
    dfs_vis[node] = 0
    
    return False

def haveCycleDirectedDFS(graph):
    vis = [0 for _ in range(len(graph))]
    dfs_vis = [0 for _ in range(len(graph))]
    
    for node in range(len(graph)):
        if vis[node] == 0:
            if checkCycleDirectedDFS(node, graph, vis, dfs_vis):
                return True
    
    return False

haveCycleDirectedDFS(graph)
```




    False



## Topological Sort using DFS


```python
def toposortDFS_helper(node, graph, vis, stack):
    vis[node] = 1
    
    for node_itr in graph[node]:
        if graph.directed:
                temp = node_itr[0]
        else:
            temp = node_itr
            
        if vis[temp] == 0:
            toposortDFS_helper(temp, graph, vis, stack)
        
    stack.append(node)

def topologicalSortDFS(graph): 
    stack = []
    vis = [0 for _ in range(len(graph))]
    
    for node in range(len(graph)):
        if vis[node] == 0:
            toposortDFS_helper(node, graph, vis, stack)
    
    return stack[::-1]

topologicalSortDFS(graph)
```




    [2, 0, 1, 3, 4, 5, 6, 7, 8, 9, 10]



## Topological Sort using BFS or Kahn's Algorithm


```python
def findIndegree(graph):
    in_degree = [0 for _ in range(len(graph))]
    
    for node in range(len(graph)):
        for node_itr in graph[node]:
            in_degree[node_itr] += 1
            
    return in_degree

def topologicalSortBFS(graph):
    topo = []
    in_degree = findIndegree(graph)
    queue = []
    
    for node_itr in range(len(graph)):
        if in_degree[node_itr] == 0:
            queue.append(node_itr)
            
    while queue:
        node = queue.pop(0)
        topo.append(node)
        
        for node_itr in graph[node]:
            in_degree[node_itr] -= 1
            if in_degree[node_itr] == 0:
                queue.append(node_itr)
        
    return topo

topologicalSortBFS(graph)
```




    [0, 2, 1, 3, 4, 5, 6, 7, 8, 9, 10]



## Cycle Detection in Directed Graph using BFS


```python
def haveCycleDirectedBFS(graph):
    in_degree = findIndegree(graph)
    queue = []
    
    for node_itr in range(len(graph)):
        if in_degree[node_itr] == 0:
            queue.append(node_itr)
        
    count = 0
    while queue:
        node = queue.pop(0)
        count += 1
        
        for node_itr in graph[node]:
            in_degree[node_itr] -= 1
            if in_degree[node_itr] == 0:
                queue.append(node_itr)
                
    if count == len(graph):
        return False
    return True
haveCycleDirectedBFS(graph)
```




    False



## Shortest path in a weighted directed acyclic graph using TopoSort (DAG)


```python
def shortestDistanceTopoSort(graph, source):
    topoSort = topologicalSortDFS(graph)
    
    distance = [float('inf') for _ in range(len(graph))]
    distance[source] = 0
    
    for node in topoSort:
        if distance[node] != float('inf'):
            for node_itr, weight in graph[node]:
                distance[node_itr] = min(weight + distance[node], distance[node_itr])
    
    return distance

shortestDistanceTopoSort(graph, 0)
```




    [0, 2, inf, 7, 7, 11, 19, 22, 25, 13, 15]



## Dijksra's Algorithm


```python
import heapq

def dijkstraAlgo(graph, source):
    min_heap = []
    distance = [float('inf') for _ in range(len(graph))]
    distance[source] = 0
    
    heapq.heappush(min_heap, (0, source))
    
    while min_heap:
        weight, node = heapq.heappop(min_heap)
        for node_itr, weight_itr in graph[node]:
            if weight + weight_itr < distance[node_itr]:
                distance[node_itr] = distance[node] + weight_itr
                heapq.heappush(min_heap, (distance[node_itr], node_itr))
    
    return distance

dijkstraAlgo(graph, 0)
```




    [0, 2, inf, 7, 7, 11, 19, 22, 25, 13, 15]



## Prim's Algorithm


```python
import heapq

def primAlgo(graph):
    min_heap = []
    
    key = [float('inf') for _ in range(len(graph))]
    parent = [-1  for _ in range(len(graph))]
    mstSet = [False for _ in range(len(graph))]
    
    heapq.heappush(min_heap, (0, 0))
    
    for _ in range(len(graph)):
        weight, node = heapq.heappop(min_heap)
        
        mstSet[node] = True
        
        for node_itr, weight_itr in graph[node]:
            if mstSet[node_itr] == False and weight_itr < key[node_itr]:
                parent[node_itr] = node
                heapq.heappush(min_heap, (key[node_itr], node_itr))
                key[node_itr] = weight
    
    return parent

primAlgo(graph)
```




    [-1, 0, 5, 1, 3, 4, 5, 6, 7, 8, 9]



## Disjoint Sets


```python
class DisjointSets:
    def __init__(self, num_nodes):
        self.parent = [i for i in range(num_nodes)]
        self.rank = [0 for _ in range(num_nodes)]
        
    def findParent(self, node):
        if self.parent[node] == node:
            return node
        
        self.parent[node] = self.findParent(self.parent[node])
        
        return self.parent[node]
        
    def union(self, u, v):
        u = self.findParent(u)
        v = self.findParent(v)
        
        if self.rank[u] < self.rank[v]:
            self.parent[u] = v
        elif self.rank[u] > self.rank[v]:
            self.parent[v] = u
        else:
            self.parent[v] = u
            self.rank[u] += 1
```

## Kruskal's Algorithm


```python
def getSortedEdges(graph):
    edges = []
    
    for node in range(len(graph)):
        for node_itr, weight_itr in graph[node]:
            edges.append((weight_itr, node, node_itr))
            
    return sorted(edges, key=lambda x: x[0])

def kruskalAlgo(graph):
    disjointSets = DisjointSets(len(graph))
    edges = getSortedEdges(graph)
    
    costMst = 0
    mst = []
    
    for edge in edges:
        if disjointSets.findParent(edge[1]) != disjointSets.findParent(edge[2]):
            costMst += edge[0]
            mst.append(edge)
            disjointSets.union(edge[1], edge[2])
    
    return costMst, mst

kruskalAlgo(graph)
```




    (23,
     [(0, 3, 4),
      (1, 2, 4),
      (1, 7, 10),
      (1, 8, 9),
      (2, 0, 1),
      (2, 5, 2),
      (2, 9, 10),
      (3, 6, 7),
      (5, 1, 3),
      (6, 4, 9)])



## Bridges in Graphs


```python
def bridgeGraph_helper(node, parent, graph, vis, tin, low, timer, edges):
    vis[node] = 1
    tin[node] = timer
    low[node] = timer
    timer += 1
    
    for node_itr, _ in graph[node]:
        if node_itr == parent:
            continue
        if vis[node_itr] == 0:
            bridgeGraph_helper(node_itr, node, graph, vis, tin, low, timer, edges)
            low[node] = min(low[node], low[node_itr])
            
            if low[node_itr] > tin[node]:
                edges.append((node_itr, node))
        else:
            low[node] = min(low[node], low[node_itr])

def bridgeGraph(graph):
    edges = []
    
    vis = [0 for _ in range(len(graph))]
    tin = [0 for _ in range(len(graph))]
    low = [0 for _ in range(len(graph))]
    
    timer = 0
    for node in range(len(graph)):
        if vis[node] == 0:
            bridgeGraph_helper(node, -1, graph, vis, tin, low, timer, edges)
    
    return edges

bridgeGraph(graph)
```




    [(10, 9), (9, 4), (4, 3), (3, 1), (1, 0)]



## Articulation Point


```python
def articulationPoint_helper(node, parent, timer, graph, low, tin, vis, points):
    vis[node] = 1
    low[node] = timer
    tin[node] = timer
    timer += 1
    
    child = 0
    for node_itr, _ in graph[node]:
        if node_itr == parent:
            continue
        if vis[node_itr] == 0:
            articulationPoint_helper(node_itr, node, timer, graph, low, tin, vis, points)
            low[node] = min(low[node], low[node_itr])
            if low[node_itr] >= tin[node] and parent != -1:
                points.append(node)
                points.append(node_itr)
            child += 1
        else:
            low[node] = min(low[node], low[node_itr])
            
        if parent != -1 and child > 1:
            points.append(node)
            
def articulationPoint(graph):
    points = []
    
    vis = [0 for _ in range(len(graph))]
    low = [0 for _ in range(len(graph))]
    tin = [0 for _ in range(len(graph))]
    
    timer = 0
    for node in range(len(graph)):
        if vis[node] == 0:
            articulationPoint_helper(node, -1, timer, graph, low, tin, vis, points)
    
    return set(points)

articulationPoint(graph)
```




    {1, 3, 4, 5, 6, 9, 10}



## Bellman Ford Algorithm


```python
def getEdges(graph):
    edges = []
    
    for node in range(len(graph)):
        for node_itr, weight_itr in graph[node]:
            edges.append((node, node_itr, weight_itr))
    
    return edges

def bellmanFordAlgo(graph, source):
    edges = getEdges(graph)
    
    distance = [float('inf') for _ in range(len(graph))]
    distance[source] = 0
    
    for _ in range(len(graph) - 1):
        for itr in edges:
            if distance[itr[0]] + itr[2] < distance[itr[1]]:
                distance[itr[1]] = distance[itr[0]] + itr[2]
    
    for itr in edges:
        if distance[itr[0]] + itr[2] < distance[itr[1]]:
            return -1
    
    return distance

bellmanFordAlgo(graph, 0)
```




    [0, 2, 13, 7, 7, 11, 19, 22, 25, 13, 15]


