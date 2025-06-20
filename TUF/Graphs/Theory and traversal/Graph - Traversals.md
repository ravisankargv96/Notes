#### Concepts:

##### Convert Edges to adjacency List
```java
public void createAdj(int N, int[][] edges){

	// adj : List<Integer>[] (preferable [0, N-1] vertices)
	List<Integer>[] adj = new ArrayList[N];
	for(int i = 0; i < N; i++){
		adj[i] = new ArrayList<>();
	}

	// initializing values
	for(int i = 0; i < edges.length; i++){
		int u = edges[i][0];
		int v = edges[i][1];

		// storing in graph
		adj[u].add(v); // directed - graph
		
		if(undirected)  // undirected - graph.
			adj[v].add(u); 
		
	}
}

```


```java
public void createAdj(int N, int[][] edges) {

	// adj: List<List<Integer>> (less prefered)
	List<List<Integer>> adj = new ArrayList<>();
	for(int i =0; i < N; i++){
		adj.add(new ArrayList<>());
	}

	for(int i = 0; i < edges.length; i++){
		int u = edges[i][0];
		int v = edges[i][1];

		// storing in graph
		adj.get(u).add(v); // directed - graph
		if(undirected)     // undirected - graph.
			adj.get(v).add(u);
		
	}

}
```

##### Weighted Graph
```java
// (v, wt) 
public class NodeWt {
	int node;
	int wt;
	public NodeWt(int node, int wt){
		this.node = node;
		this.wt = wt;
	}
}

public void weightedGraph(int N, int[][] edges) {
	
	// weighted graph : [ 
	//                    [ (v, w) ],
	//					]
	
	List<List<Pair>> adj = new ArrayList<>();
	for(int i = 0; i < N; i++){
		adj.add(new ArrayList<>());
	}

	for(int i = 0; i < edges.length; i++){
		int u = edges[i][0];
		int v = edges[i][1];
		int wt = edges[i][2];

		// storing in graph
		adj.get(u).add(new NodeWt(v, wt));
		if(undirected) 
			adj.get(v).add(new NodeWt(u, wt));
	}
}
```

##### DFS:
```java
// dfs of a graph:
public void dfs(int V, List<List<Integer>> adj, int[] vis, List<Integer> st){
	
	// mark vis[V] = 1;
	// store in a list 

	// Iterate through neighbors (adj[V])
		// if(vis[nei] == 0): dfs(nei, adj, vis, st);
}
```

##### BFS:
```java
// bfs of a graph
public List<Integer> bfsOfGraph(int V, List<List<Integer>> adj) {

	// init Q<Integer>, q.add(V); vis[V] = 1;

	// while(!q.isEmpty())
		// get the ele; st the ele 

		// Iterate through nei (adj[ele])
			// if(vis[nei] == 0): q.add(nei); vis[nei] = 1;
	
	// return st;
}
```

#### Problems
##### 1. find Component: (bfs)
```java
public int findNumberOfComponent(int V, List<List<Integer>> edges) {
    
	// adj List structure
	List<Integer>[] adj = new ArrayList[V];
	for(int i = 0; i < V; i++){
		adj[i] = new ArrayList<>();
	}

	// Add edges to adj list
	for(int i = 0; i < edges.size(); i++){
		int u = edges.get(i).get(0);
		int v = edges.get(i).get(1);

		// adj[u]: [...,v]
		// adj[v]: [..., u]
		adj[u].add(v);
		adj[v].add(u);
	}

	// components & boolean[] vis array
	int comp = 0; 
	boolean[] vis = new boolean[V]; // false


	// Start Traversal
	for(int i = 0; i < V; i++){
		if(!vis[i]) {
			bfs(adj, i, vis); // graph, st, vis
			comp++;
		}
	}

	return comp; 
}
```

##### 2. Number of provinces: (bfs)
```java
public int numProvinces(int[][] adj) {
	// convert adj Matrix -> adjList

	// same as find Component: (bfs)
}
```

##### 3. Number of islands
```java
public int numIslands(char[][] grid) {
   // create a 2d vis array.
   
   // Iterate through row (i).
	   // Iterate through each cell (j):
		   // if(vis[i][j] == 0 && grid[i][j] == '1'): bfs(vis, grid, [i,j]), islands++;

	// return islands;
}
```

##### 4. Flood fill Algorithm
```java
public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {
	// bfs(image, [sr,sc], newColor); bfs using Grids.
	// return image
}
```

##### 5. Number of enclaves (Do it)
```java

```

##### 6. Rotten Oranges
```java
public int orangesRotting(int[][] grid) {
	// apply bfs for each component.

	// from modified grid[i][j] == 1 : return -1
	// return max(grid[i][j])
}
```

##### 7. Distance of nearest cell having one
```java
public int[][] nearest(int[][] grid) {
   // iterate through row (i)
	   // iterate through each cell (j)
		   // level = bfs(grid, [i, j])
		   // res[i][j] = level;
}
```

##### 8. Surrounded Regions (Number of enclaves)
```java
public int countDistinctIslands(int[][] grid){
	// Iterate through row (i)
		// Iterate through cell (j)
			// if(vis[i][j] == 0): List<List<Integer>> rel = bfs(grid, [i, j], vis)
			// st.add(rel);
}
```
