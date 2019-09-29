---
layout: post
title: "C++: Depth First Search program using Adjacency Matrix (Graph Algorithm)"
tags:  [C++, Algorithm, Graph Algorithms]
date: 2018-01-09
---

Depth-first search (DFS) is an algorithm for traversing or searching tree or graph data structures. [[wikipedia](https://en.wikipedia.org/wiki/Depth-first_search)]

It explores edges out of the most recently discovered vertex v that still has unexplored edges leaving it. Once all of v's edges have been explored, the search "backtracks" to explore edges leaving the vertex from which v was discovered. This process continues until we have discovered all the vertices that are reachable from the original source vertex. If any undiscovered vertices remain, then depth-first search selects one of them as a new source, and it repeats the search from that source. The algorithm repeats this entire process until it has discovered every vertex.[Intoduction to Algorithm]

```cpp
#include <iostream>
#include <vector>

class Graph
{
  int vertexCount; //no of Vertices
  enum class Color { WHITE, GRAY, BLACK };

  struct Vertex
  {
  int id;
  Color color;
  Vertex(int vertex, Color clr) : id(vertex), color(clr) {}
  };
  std::vector< std::vector<bool> > adjMatrix; //adjacency Matrix
  std::vector<Vertex> vertices;

public:
  Graph(int);
  void addEdge(int, int);
  void depthFirstSearch(int);
};

Graph::Graph(int v)
{
  vertexCount = v;
  adjMatrix.resize(vertexCount, std::vector<bool>(vertexCount));
  for(int i = 0; i < vertexCount; i++)
  {
    vertices.push_back( Vertex(i, Color::WHITE));
    for(int j = 0; j < vertexCount; j++)
    {
      adjMatrix[i][j] = false;
    }
   }
}

void Graph::addEdge(int a, int b)
{
  adjMatrix[a][b] = true;
  adjMatrix[b][a] = true;
}

void Graph::depthFirstSearch(int u)
{
  vertices[u].color = Color::GRAY;
  std::cout << vertices[u].id <<" ";

    for(int j = 0; j < vertexCount; j++)
    {
      if(adjMatrix[u][j] == true)
      {
        if(vertices[j].color == Color::WHITE)
        depthFirstSearch(j);
      }
    }

  vertices[u].color = Color::BLACK; //blacken u, it is finished
}

int main()
{
  Graph grp(4);
  grp.addEdge(0, 1);
  grp.addEdge(1, 2);
  grp.addEdge(2, 3);
  grp.addEdge(2, 1);
  grp.addEdge(0, 3);

  grp.depthFirstSearch(2);
  return 0;
}
```

 <input type="hidden" name="IL_IN_ARTICLE"> 
You may also like

[C++: Dijkstra's Algorithm using STL](https://programmercave.github.io/blog/2018/03/14/C++-Dijkstra's-Algorithm-using-STL)

[C++: Bellman Ford Algorithm using STL](https://programmercave.github.io/blog/2018/03/11/C++-Bellman-Ford-Algorithm-using-STL)

[C++: Breadth First Search using Adjacency List](https://programmercave.github.io/blog/2018/03/06/C++-Breadth-First-Search-using-Adjacency-List)

[C++: Depth First Search using Adjacency List](https://programmercave.github.io/blog/2018/03/05/C++-Depth-First-Search-using-Adjacency-List)

[C++: Breadth First Search program using Adjacency Matrix](https://programmercave.github.io/blog/2018/01/11/C++-Breadth-First-Search-program-using-Adjacency-Matrix)



