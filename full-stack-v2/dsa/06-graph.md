Let's cover **Graphs in Competitive Programming (CP)** with the main patterns and recursive implementations.

# 1. Graph Basics

A graph consists of:

* **Vertices (Nodes)**
* **Edges (Connections)**

Example:

```
      1
     / \
    2   3
   /
  4
```

Representation:

```
Nodes = [1,2,3,4]

Edges:
1-2
1-3
2-4
```

# Graph Representation

## 1. Adjacency List (Most Used)

```javascript
const graph = {
    1: [2,3],
    2: [1,4],
    3: [1],
    4: [2]
};
```

For CP:

```javascript
let graph = Array.from(
    {length:n+1},
    ()=>[]
);

graph[1].push(2);
graph[2].push(1);
```

# DFS (Depth First Search)

Most important graph recursion pattern.

## Recursive DFS

```javascript
function dfs(node, graph, visited){

    if(visited[node])
        return;


    visited[node] = true;

    console.log(node);


    for(let neighbour of graph[node]){
        dfs(neighbour, graph, visited);
    }
}
```

Usage:

```javascript
let visited = Array(5).fill(false);

dfs(1, graph, visited);
```

Output:

```
1 2 4 3
```

# Pattern 1: Count Connected Components

Question:

> How many separate graphs exist?

Example:

```
1--2       3--4

Answer = 2
```

Code:

```javascript
function countComponents(n, graph){

    let visited = Array(n+1).fill(false);
    let count = 0;


    function dfs(node){

        visited[node]=true;

        for(let next of graph[node]){
            if(!visited[next])
                dfs(next);
        }
    }


    for(let i=1;i<=n;i++){

        if(!visited[i]){
            count++;
            dfs(i);
        }

    }

    return count;
}
```

# Pattern 2: Detect Cycle in Undirected Graph

Example:

```
1--2
|  |
4--3
```

Cycle exists.

Logic:

While DFS:

* if neighbour visited
* and neighbour is not parent

then cycle.

```javascript
function hasCycle(node, parent, graph, visited){

    visited[node]=true;


    for(let next of graph[node]){

        if(!visited[next]){

            if(hasCycle(
                next,
                node,
                graph,
                visited
            ))
                return true;

        }

        else if(next !== parent){
            return true;
        }
    }


    return false;
}
```

# Pattern 3: BFS Traversal

Uses Queue.

```javascript
function bfs(start, graph){

    let queue=[start];

    let visited={};

    visited[start]=true;


    while(queue.length){

        let node=queue.shift();

        console.log(node);


        for(let next of graph[node]){

            if(!visited[next]){

                visited[next]=true;

                queue.push(next);
            }
        }
    }
}
```

# Pattern 4: Shortest Path in Unweighted Graph

Rule:

```
BFS = shortest distance
```

Example:

```
1 -- 2 -- 3 -- 4

distance(1,4)=3
```

Code:

```javascript
function shortestPath(start, graph, n){

    let dist=Array(n+1).fill(-1);

    let queue=[start];

    dist[start]=0;


    while(queue.length){

        let node=queue.shift();


        for(let next of graph[node]){

            if(dist[next]===-1){

                dist[next]=dist[node]+1;

                queue.push(next);
            }
        }
    }


    return dist;
}
```

# Pattern 5: Bipartite Graph

Can nodes be colored using 2 colors?

Example:

```
0   1
 \ /
  2
```

Color:

```
Red
Blue
```

Code:

```javascript
function isBipartite(graph){

    let color=Array(graph.length).fill(-1);


    function dfs(node,c){

        color[node]=c;


        for(let next of graph[node]){


            if(color[next]===-1){

                if(!dfs(next,1-c))
                    return false;

            }

            else if(color[next]===c){

                return false;

            }
        }


        return true;
    }


    for(let i=0;i<graph.length;i++){

        if(color[i]===-1){

            if(!dfs(i,0))
                return false;
        }
    }


    return true;
}
```

# Pattern 6: Topological Sort (Directed Graph)

Used in:

* Course schedule
* Dependency problems
* Build systems

Example:

```
A → B → C
```

DFS approach:

```javascript
function topoDFS(graph,n){

    let visited=Array(n).fill(false);

    let stack=[];


    function dfs(node){

        visited[node]=true;


        for(let next of graph[node]){

            if(!visited[next])
                dfs(next);
        }


        stack.push(node);
    }



    for(let i=0;i<n;i++){

        if(!visited[i])
            dfs(i);

    }


    return stack.reverse();
}
```

# Pattern 7: Number of Islands (Grid Graph)

Very common interview question.

Grid:

```
1 1 0
0 1 0
1 0 1
```

Each land is a node.

```javascript
function dfs(grid,r,c){

    if(
        r<0 ||
        c<0 ||
        r>=grid.length ||
        c>=grid[0].length ||
        grid[r][c]==='0'
    )
        return;


    grid[r][c]='0';


    dfs(grid,r+1,c);
    dfs(grid,r-1,c);
    dfs(grid,r,c+1);
    dfs(grid,r,c-1);
}
```

# CP Graph Pattern Roadmap

| Level  | Problems             |
| ------ | -------------------- |
| Basic  | DFS, BFS             |
| Easy   | Connected components |
| Easy   | Number of islands    |
| Medium | Cycle detection      |
| Medium | Bipartite graph      |
| Medium | Topological sort     |
| Medium | Shortest path        |
| Hard   | Dijkstra             |
| Hard   | Bellman Ford         |
| Hard   | Floyd Warshall       |
| Hard   | MST (Kruskal/Prim)   |
| Hard   | DSU/Union Find       |

## Graph Recursion Template

Almost every DFS problem:

```javascript
function dfs(node){

    // mark visited

    visited[node]=true;


    // explore neighbors

    for(let neighbour of graph[node]){

        if(!visited[neighbour]){

            dfs(neighbour);

        }
    }


    // optional: process after recursion
}
```

Master this template and most graph recursion problems become variations of it.

Let's move to the next major **Graph CP pattern: Shortest Path Algorithms**.

There are mainly 4 patterns:

1. BFS → Unweighted graph
2. Dijkstra → Positive weighted graph
3. Bellman-Ford → Negative weights
4. Floyd-Warshall → All pair shortest path

# 1. Dijkstra Algorithm

## When to use?

Graph:

```
A --5-- B --2-- C
 \             /
  ----10-------
```

Weights are:

```
>= 0
```

Find shortest distance from source.

## Graph Representation

```javascript
let graph = [
    [],
    [[2,5],[3,10]],
    [[3,2]],
    []
];
```

Meaning:

```
1 -> 2 (weight 5)
1 -> 3 (weight 10)
2 -> 3 (weight 2)
```

# Dijkstra Pattern (Priority Queue)

JavaScript does not have built-in heap, so create one.

## Min Heap

```javascript
class MinHeap {

    constructor(){
        this.heap=[];
    }


    push(item){

        this.heap.push(item);

        this.bubbleUp();
    }


    bubbleUp(){

        let index=this.heap.length-1;


        while(index>0){

            let parent=Math.floor((index-1)/2);


            if(this.heap[parent][0] <= this.heap[index][0])
                break;


            [this.heap[parent],this.heap[index]] =
            [this.heap[index],this.heap[parent]];


            index=parent;
        }
    }



    pop(){

        if(this.heap.length===1)
            return this.heap.pop();


        let ans=this.heap[0];

        this.heap[0]=this.heap.pop();

        this.bubbleDown();


        return ans;
    }


    bubbleDown(){

        let index=0;


        while(true){

            let left=2*index+1;
            let right=2*index+2;

            let smallest=index;


            if(
                left<this.heap.length &&
                this.heap[left][0]<this.heap[smallest][0]
            )
                smallest=left;


            if(
                right<this.heap.length &&
                this.heap[right][0]<this.heap[smallest][0]
            )
                smallest=right;



            if(smallest===index)
                break;


            [this.heap[index],this.heap[smallest]]=
            [this.heap[smallest],this.heap[index]];


            index=smallest;
        }
    }


    isEmpty(){
        return this.heap.length===0;
    }
}
```

# Dijkstra Implementation

```javascript
function dijkstra(graph, source, n){

    let dist=Array(n+1).fill(Infinity);


    let pq=new MinHeap();


    dist[source]=0;


    pq.push([0,source]);



    while(!pq.isEmpty()){


        let [distance,node]=pq.pop();



        if(distance > dist[node])
            continue;



        for(let [next,weight] of graph[node]){


            if(
                dist[node]+weight < dist[next]
            ){

                dist[next]=dist[node]+weight;


                pq.push([
                    dist[next],
                    next
                ]);
            }
        }
    }


    return dist;
}
```

# Complexity

Using heap:

```
Time:  O((V+E) log V)

Space: O(V)
```

# 2. Bellman-Ford Algorithm

Used when:

* Negative edges exist

Example:

```
A ----5---- B
 \          |
  \        -3
   \        |
     C ----
```

## Idea

Relax every edge:

```
distance[v] =
min(
 distance[v],
 distance[u]+weight
)
```

Repeat:

```
V-1 times
```

## Implementation

```javascript
function bellmanFord(edges,n,source){


    let dist=Array(n+1).fill(Infinity);

    dist[source]=0;



    for(let i=1;i<n;i++){


        for(let [u,v,w] of edges){


            if(
                dist[u]!==Infinity &&
                dist[u]+w < dist[v]
            ){

                dist[v]=dist[u]+w;

            }
        }
    }



    // check negative cycle

    for(let [u,v,w] of edges){

        if(
            dist[u]+w < dist[v]
        ){

            return "Negative Cycle";

        }
    }



    return dist;
}
```

# 3. Floyd Warshall

Find:

```
every node -> every node
```

Example:

```
1 -> 2
2 -> 3

Find:
1 -> 3
```

## Formula

```
dist[i][j] =
min(
 dist[i][j],
 dist[i][k]+dist[k][j]
)
```

## Code

```javascript
function floydWarshall(matrix,n){


    for(let k=0;k<n;k++){


        for(let i=0;i<n;i++){


            for(let j=0;j<n;j++){


                matrix[i][j]=Math.min(

                    matrix[i][j],

                    matrix[i][k]+matrix[k][j]

                );
            }
        }
    }


    return matrix;
}
```

# 4. Minimum Spanning Tree (MST)

Next important graph topic.

Two algorithms:

### Kruskal

Pattern:

```
Sort edges
+
DSU
```

### Prim

Pattern:

```
Priority Queue
+
Greedy
```

Example:

Connect all cities with minimum cost.

# Graph CP Learning Order

You have completed:

✅ DFS
✅ BFS
✅ Components
✅ Cycle detection
✅ Bipartite
✅ Topological Sort

Next:

1. Dijkstra ⭐
2. Bellman Ford
3. Floyd Warshall
4. DSU / Union Find
5. Kruskal MST
6. Prim MST
7. Bridges
8. Articulation Points
9. SCC (Kosaraju/Tarjan)
10. Advanced graph DP

Next topic should be **DSU (Disjoint Set Union) + Kruskal MST**, because it is one of the most repeated CP patterns.
