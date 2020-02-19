# 常用算法
## Union-Find
```
Class UnionFindSet:
    func UnionFindSet(n):
        parents = [1..n]
        ranks = [0..0] (n zeros)

    func Find(x):
        if x != parents[x]:
            parents[x] = Find(parents[x])
        return parents[x];

    func Union(x, t):
        px, py = Find(x), Find(y)
        if ranks[px] > ranks[py]: parents[py] = px;
        if ranks[py] > ranks[px]: parents[px] = py;
        if ranks[px] == ranks[py]:
            parents[py] = px
            ranks[px]++
```

## Fenwick Tree
``` C++
Class FenwickTree {
public:
    FenwickTree(int n) { 
        sums_(n + 1, 0);
    }
    
    void update(int i, int delta) {
        while (i < sums_.size()) {
            sums_[i] += delta;
            i += lowbit(i);
        }
    }
    
    int query(int i) const {
        int sum = 0;
        while (i > 0) {
            sum += sums_[i];
            i -= lowbit(i);
        }
        return sum;
    }
    
private:
    static inline int lowbit(int x) { return x & (-x); }
    vector<int> sums_;
}
```

## 弗洛伊德算法（Floyd）
``` shell
1 let dist be a |V| × |V| array of minimum distances initialized to ∞ (infinity)
2 for each vertex v
3    dist[v][v] ← 0
4 for each edge (u,v)
5    dist[u][v] ← w(u,v)  // the weight of the edge (u,v)
6 for k from 1 to |V|
7    for i from 1 to |V|
8       for j from 1 to |V|
9          if dist[i][j] > dist[i][k] + dist[k][j] 
10             dist[i][j] ← dist[i][k] + dist[k][j]
11         end if
```

```Java
// Frome: https://www.geeksforgeeks.org/floyd-warshall-algorithm-dp-16/
void floydWarshall(int graph[][]) 
    { 
        int dist[][] = new int[V][V]; 
        int i, j, k; 
  
        /* Initialize the solution matrix same as input graph matrix. 
           Or we can say the initial values of shortest distances 
           are based on shortest paths considering no intermediate 
           vertex. */
        for (i = 0; i < V; i++) 
            for (j = 0; j < V; j++) 
                dist[i][j] = graph[i][j]; 
  
        /* Add all vertices one by one to the set of intermediate 
           vertices. 
          ---> Before start of an iteration, we have shortest 
               distances between all pairs of vertices such that 
               the shortest distances consider only the vertices in 
               set {0, 1, 2, .. k-1} as intermediate vertices. 
          ----> After the end of an iteration, vertex no. k is added 
                to the set of intermediate vertices and the set 
                becomes {0, 1, 2, .. k} */
        for (k = 0; k < V; k++) 
        { 
            // Pick all vertices as source one by one 
            for (i = 0; i < V; i++) 
            { 
                // Pick all vertices as destination for the 
                // above picked source 
                for (j = 0; j < V; j++) 
                { 
                    // If vertex k is on the shortest path from 
                    // i to j, then update the value of dist[i][j] 
                    if (dist[i][k] + dist[k][j] < dist[i][j]) 
                        dist[i][j] = dist[i][k] + dist[k][j]; 
                } 
            } 
        } 
    } 
```

## 迪杰斯特拉（Dijkstra）
```shell
 1  function Dijkstra(G, w, s)
 2     for each vertex v in V[G]        // 初始化
 3           d[v] := infinity           // 将各点的已知最短距离先设成无穷大
 4           previous[v] := undefined   // 各点的已知最短路径上的前趋都未知
 5     d[s] := 0                        // 因为出发点到出发点间不需移动任何距离，所以可以直接将s到s的最小距离设为0
 6     S := empty set
 7     Q := set of all vertices
 8     while Q is not an empty set      // Dijkstra算法主体
 9           u := Extract_Min(Q)
10           S.append(u)
11           for each edge outgoing from u as (u,v)
12                  if d[v] > d[u] + w(u,v)             // 拓展边（u,v）。w(u,v)为从u到v的路径长度。
13                        d[v] := d[u] + w(u,v)         // 更新路径长度到更小的那个和值。
14                        previous[v] := u              // 纪录前趋顶点
```
<details>
    <summary>Java实现例子</summary>
    ```Java
    // From https://www.geeksforgeeks.org/dijkstras-shortest-path-algorithm-in-java-using-priorityqueue/
    // Java implementation of Dijkstra's Algorithm  
    // using Priority Queue 
    import java.util.*; 
    public class DPQ { 
        private int dist[]; 
        private Set<Integer> settled; 
        private PriorityQueue<Node> pq; 
        private int V; // Number of vertices 
        List<List<Node> > adj; 

        public DPQ(int V) 
        { 
            this.V = V; 
            dist = new int[V]; 
            settled = new HashSet<Integer>(); 
            pq = new PriorityQueue<Node>(V, new Node()); 
        } 

        // Function for Dijkstra's Algorithm 
        public void dijkstra(List<List<Node> > adj, int src) 
        { 
            this.adj = adj; 

            for (int i = 0; i < V; i++) 
                dist[i] = Integer.MAX_VALUE; 

            // Add source node to the priority queue 
            pq.add(new Node(src, 0)); 

            // Distance to the source is 0 
            dist[src] = 0; 
            while (settled.size() != V) { 

                // remove the minimum distance node  
                // from the priority queue  
                int u = pq.remove().node; 

                // adding the node whose distance is 
                // finalized 
                settled.add(u); 

                e_Neighbours(u); 
            } 
        } 

        // Function to process all the neighbours  
        // of the passed node 
        private void e_Neighbours(int u) 
        { 
            int edgeDistance = -1; 
            int newDistance = -1; 

            // All the neighbors of v 
            for (int i = 0; i < adj.get(u).size(); i++) { 
                Node v = adj.get(u).get(i); 

                // If current node hasn't already been processed 
                if (!settled.contains(v.node)) { 
                    edgeDistance = v.cost; 
                    newDistance = dist[u] + edgeDistance; 

                    // If new distance is cheaper in cost 
                    if (newDistance < dist[v.node]) 
                        dist[v.node] = newDistance; 

                    // Add the current node to the queue 
                    pq.add(new Node(v.node, dist[v.node])); 
                } 
            } 
        } 

        // Driver code 
        public static void main(String arg[]) 
        { 
            int V = 5; 
            int source = 0; 

            // Adjacency list representation of the  
            // connected edges 
            List<List<Node> > adj = new ArrayList<List<Node> >(); 

            // Initialize list for every node 
            for (int i = 0; i < V; i++) { 
                List<Node> item = new ArrayList<Node>(); 
                adj.add(item); 
            } 

            // Inputs for the DPQ graph 
            adj.get(0).add(new Node(1, 9)); 
            adj.get(0).add(new Node(2, 6)); 
            adj.get(0).add(new Node(3, 5)); 
            adj.get(0).add(new Node(4, 3)); 

            adj.get(2).add(new Node(1, 2)); 
            adj.get(2).add(new Node(3, 4)); 

            // Calculate the single source shortest path 
            DPQ dpq = new DPQ(V); 
            dpq.dijkstra(adj, source); 

            // Print the shortest path to all the nodes 
            // from the source node 
            System.out.println("The shorted path from node :"); 
            for (int i = 0; i < dpq.dist.length; i++) 
                System.out.println(source + " to " + i + " is "
                                   + dpq.dist[i]); 
        } 
    } 

    // Class to represent a node in the graph 
    class Node implements Comparator<Node> { 
        public int node; 
        public int cost; 

        public Node() 
        { 
        } 

        public Node(int node, int cost) 
        { 
            this.node = node; 
            this.cost = cost; 
        } 

        @Override
        public int compare(Node node1, Node node2) 
        { 
            if (node1.cost < node2.cost) 
                return -1; 
            if (node1.cost > node2.cost) 
                return 1; 
            return 0; 
        } 
    } 
    ```
</details>
