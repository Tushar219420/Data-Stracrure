## Problem Statement
Write a Program to implement Dijkstra's algorithm to find the shortest path from a source vertex to all other vertices in a weighted graph. Use Adjacency Matrix to represent the graph.

##  Code
```cpp
#include <iostream>
#include <climits>
using namespace std;

const int MAX_VERTICES_TV = 20;
const int INF_TV = INT_MAX;

class Graph_tv {
private:
    int numVertices_tv;
    int** adjMatrix_tv;
    char vertexLabels_tv[MAX_VERTICES_TV];

public:
    
    Graph_tv() {
        numVertices_tv = 0;
        adjMatrix_tv = nullptr;
        for (int i = 0; i < MAX_VERTICES_TV; i++) {
            vertexLabels_tv[i] = 'A' + i;
        }
    }

    void createGraph_tv(int vertices) {
        if (vertices > MAX_VERTICES_TV) {
            cout << "Maximum vertices limit exceeded! Max allowed: " << MAX_VERTICES_TV << endl;
            return;
        }
        
        numVertices_tv = vertices;
        
        adjMatrix_tv = new int*[numVertices_tv];
        for (int i = 0; i < numVertices_tv; i++) {
            adjMatrix_tv[i] = new int[numVertices_tv];
        }
        
        for (int i = 0; i < numVertices_tv; i++) {
            for (int j = 0; j < numVertices_tv; j++) {
                adjMatrix_tv[i][j] = 0;
            }
        }
        
        cout << "Graph created with " << vertices << " vertices (labeled A, B, C, ...)" << endl;
    }

    void addEdge_tv(int src, int dest, int weight) {
        if (src >= numVertices_tv || dest >= numVertices_tv || src < 0 || dest < 0) {
            cout << "Invalid vertex! Vertices must be between 0 and " << (numVertices_tv - 1) << endl;
            return;
        }
        
        adjMatrix_tv[src][dest] = weight;
        
        adjMatrix_tv[dest][src] = weight;
        
        cout << "Edge added between " << vertexLabels_tv[src] << " and " << vertexLabels_tv[dest] 
             << " with weight " << weight << endl;
    }

    void displayAdjacencyMatrix_tv() {
        if (numVertices_tv == 0) {
            cout << "Graph is empty!" << endl;
            return;
        }
        
        cout << "\n=== Adjacency Matrix Representation ===" << endl;
        cout << "    ";
        for (int i = 0; i < numVertices_tv; i++) {
            cout << vertexLabels_tv[i] << "   ";
        }
        cout << endl;
        
        for (int i = 0; i < numVertices_tv; i++) {
            cout << vertexLabels_tv[i] << "   ";
            for (int j = 0; j < numVertices_tv; j++) {
                if (adjMatrix_tv[i][j] == 0) {
                    cout << "0   ";
                } else {
                    cout << adjMatrix_tv[i][j] << "   ";
                }
            }
            cout << endl;
        }
        cout << "=====================================" << endl;
    }

    void dijkstra_tv(int src) {
        if (numVertices_tv == 0) {
            cout << "Graph is empty!" << endl;
            return;
        }
        
        if (src >= numVertices_tv || src < 0) {
            cout << "Invalid source vertex!" << endl;
            return;
        }
        
        int dist_tv[MAX_VERTICES_TV];     
        bool visited_tv[MAX_VERTICES_TV]; 
        int parent_tv[MAX_VERTICES_TV];   
        for (int i = 0; i < numVertices_tv; i++) {
            dist_tv[i] = INF_TV;
            visited_tv[i] = false;
            parent_tv[i] = -1;
        }
        
        
        dist_tv[src] = 0;
        
        
        for (int count = 0; count < numVertices_tv - 1; count++) {
            
            int u = minDistance_tv(dist_tv, visited_tv);
            
            visited_tv[u] = true;
            
            
            for (int v = 0; v < numVertices_tv; v++) {
                
                if (!visited_tv[v] && adjMatrix_tv[u][v] && dist_tv[u] != INF_TV && 
                    dist_tv[u] + adjMatrix_tv[u][v] < dist_tv[v]) {
                    dist_tv[v] = dist_tv[u] + adjMatrix_tv[u][v];
                    parent_tv[v] = u;
                }
            }
        }
        
        
        printSolution_tv(dist_tv, parent_tv, src);
    }

    
    int minDistance_tv(int dist[], bool visited[]) {
        int min = INF_TV;
        int minIndex = -1;
        
        for (int v = 0; v < numVertices_tv; v++) {
            if (visited[v] == false && dist[v] <= min) {
                min = dist[v];
                minIndex = v;
            }
        }
        
        return minIndex;
    }

    void printSolution_tv(int dist[], int parent[], int src) {
        cout << "\n=== Shortest Paths from Vertex " << vertexLabels_tv[src] << " (Dijkstra's Algorithm) ===" << endl;
        cout << "Vertex\tDistance\tPath" << endl;
        cout << "----------------------------------------" << endl;
        
        for (int i = 0; i < numVertices_tv; i++) {
            if (i != src) {
                cout << vertexLabels_tv[src] << " -> " << vertexLabels_tv[i] << "\t";
                if (dist[i] == INF_TV) {
                    cout << "INF\t\t";
                } else {
                    cout << dist[i] << "\t\t";
                }
                printPath_tv(parent, i, src);
                cout << endl;
            }
        }
        cout << "=========================================================" << endl;
    }

    void printPath_tv(int parent[], int dest, int src) {
        if (parent[dest] == -1) {
            if (dest == src) {
                cout << vertexLabels_tv[src];
            } else {
                cout << "No path";
            }
            return;
        }
        
        printPath_tv(parent, parent[dest], src);
        cout << " -> " << vertexLabels_tv[dest];
    }

    
    int getDegree_tv(int vertex) {
        if (vertex >= numVertices_tv || vertex < 0) {
            cout << "Invalid vertex!" << endl;
            return -1;
        }
        
        int degree = 0;
        for (int i = 0; i < numVertices_tv; i++) {
            if (adjMatrix_tv[vertex][i] != 0) {
                degree++;
            }
        }
        return degree;
    }

    
    void displayVertexDegrees_tv() {
        if (numVertices_tv == 0) {
            cout << "Graph is empty!" << endl;
            return;
        }
        
        cout << "\n=== Vertex Degrees ===" << endl;
        for (int i = 0; i < numVertices_tv; i++) {
            cout << "Vertex " << vertexLabels_tv[i] << ": Degree = " << getDegree_tv(i) << endl;
        }
        cout << "=====================" << endl;
    }

    void displayGraphInfo_tv() {
        cout << "\n=== Graph Information ===" << endl;
        cout << "Number of vertices: " << numVertices_tv << endl;
        cout << "Vertices: ";
        for (int i = 0; i < numVertices_tv; i++) {
            cout << vertexLabels_tv[i] << " ";
        }
        cout << endl;
        
        int edgeCount = 0;
        for (int i = 0; i < numVertices_tv; i++) {
            for (int j = 0; j < numVertices_tv; j++) {
                if (adjMatrix_tv[i][j] != 0) {
                    edgeCount++;
                }
            }
        }
        cout << "Number of edges: " << edgeCount/2 << endl;  
        cout << "========================" << endl;
    }

    
    void inputGraph_tv() {
        int vertices, edges, src, dest, weight;
        
        cout << "Enter number of vertices: ";
        cin >> vertices;
        createGraph_tv(vertices);
        
        cout << "Enter number of edges: ";
        cin >> edges;
        
        cout << "Enter edges (source destination weight):" << endl;
        for (int i = 0; i < edges; i++) {
            cout << "Edge " << (i+1) << ": ";
            cin >> src >> dest >> weight;
            addEdge_tv(src, dest, weight);
        }
    }

    ~Graph_tv() {
        if (adjMatrix_tv != nullptr) {
            for (int i = 0; i < numVertices_tv; i++) {
                delete[] adjMatrix_tv[i];
            }
            delete[] adjMatrix_tv;
        }
    }
};

int main() {
    Graph_tv graph_tv;
    int choice, vertices, src, dest, weight, source;

    cout << "=== Dijkstra's Algorithm for Shortest Path ===" << endl;
    cout << "Using Adjacency Matrix Representation" << endl;

    while (true) {
        cout << "\n1. Create Graph\n2. Input Graph\n3. Add Edge\n4. Display Adjacency Matrix\n5. Find Shortest Paths (Dijkstra's)\n6. Display Vertex Degrees\n7. Display Graph Info\n8. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter number of vertices: ";
                cin >> vertices;
                graph_tv.createGraph_tv(vertices);
                break;

            case 2:
                graph_tv.inputGraph_tv();
                break;

            case 3:
                cout << "Enter source vertex: ";
                cin >> src;
                cout << "Enter destination vertex: ";
                cin >> dest;
                cout << "Enter weight: ";
                cin >> weight;
                graph_tv.addEdge_tv(src, dest, weight);
                break;

            case 4:
                graph_tv.displayAdjacencyMatrix_tv();
                break;

            case 5:
                cout << "Enter source vertex: ";
                cin >> source;
                graph_tv.dijkstra_tv(source);
                break;

            case 6:
                graph_tv.displayVertexDegrees_tv();
                break;

            case 7:
                graph_tv.displayGraphInfo_tv();
                break;

            case 8:
                cout << "Thank you for using Dijkstra's Shortest Path Algorithm!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}

##  Output

=== Dijkstra's Algorithm for Shortest Path ===
Using Adjacency Matrix Representation

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency Matrix
5. Find Shortest Paths (Dijkstra's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 1
Enter number of vertices: 5
Graph created with 5 vertices (labeled A, B, C, ...)

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency Matrix
5. Find Shortest Paths (Dijkstra's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 0
Enter destination vertex: 1
Enter weight: 10
Edge added between A and B with weight 10

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency Matrix
5. Find Shortest Paths (Dijkstra's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 0
Enter destination vertex: 4
Enter weight: 5
Edge added between A and E with weight 5

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency Matrix
5. Find Shortest Paths (Dijkstra's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 1
Enter destination vertex: 2
Enter weight: 1
Edge added between B and C with weight 1

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency Matrix
5. Find Shortest Paths (Dijkstra's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 1
Enter destination vertex: 4
Enter weight: 2
Edge added between B and E with weight 2

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency Matrix
5. Find Shortest Paths (Dijkstra's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 2
Enter destination vertex: 3
Enter weight: 4
Edge added between C and D with weight 4

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency Matrix
5. Find Shortest Paths (Dijkstra's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 3
Enter destination vertex: 0
Enter weight: 7
Edge added between D and A with weight 7

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency Matrix
5. Find Shortest Paths (Dijkstra's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 4
Enter destination vertex: 1
Enter weight: 3
Edge added between E and B with weight 3

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency Matrix
5. Find Shortest Paths (Dijkstra's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 4
Enter destination vertex: 2
Enter weight: 9
Edge added between E and C with weight 9

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency Matrix
5. Find Shortest Paths (Dijkstra's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 4
Enter destination vertex: 3
Enter weight: 2
Edge added between E and D with weight 2

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency Matrix
5. Find Shortest Paths (Dijkstra's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 4

=== Adjacency Matrix Representation ===
    A   B   C   D   E   
A   0   10  0   0   5   
B   10  0   1   0   2   
C   0   1   0   4   9   
D   7   0   4   0   2   
E   5   2   9   2   0   
=====================================

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency Matrix
5. Find Shortest Paths (Dijkstra's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 5
Enter source vertex: 0

=== Shortest Paths from Vertex A (Dijkstra's Algorithm) ===
Vertex	Distance	Path
----------------------------------------
A -> B	5		A -> E -> B
A -> C	6		A -> E -> B -> C
A -> D	7		A -> E -> D
A -> E	5		A -> E
=========================================================

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency Matrix
5. Find Shortest Paths (Dijkstra's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 6

=== Vertex Degrees ===
Vertex A: Degree = 2
Vertex B: Degree = 3
Vertex C: Degree = 2
Vertex D: Degree = 3
Vertex E: Degree = 4
=====================

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency Matrix
5. Find Shortest Paths (Dijkstra's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 7

=== Graph Information ===
Number of vertices: 5
Vertices: A B C D E 
Number of edges: 9
========================

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency Matrix
5. Find Shortest Paths (Dijkstra's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 2
Enter number of vertices: 4
Enter number of edges: 5
Enter edges (source destination weight):
Edge 1: 0 1 4
Edge added between A and B with weight 4
Edge 2: 0 2 3
Edge added between A and C with weight 3
Edge 3: 1 2 1
Edge added between B and C with weight 1
Edge 4: 1 3 2
Edge added between B and D with weight 2
Edge 5: 2 3 4
Edge added between C and D with weight 4

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency Matrix
5. Find Shortest Paths (Dijkstra's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 5
Enter source vertex: 0

=== Shortest Paths from Vertex A (Dijkstra's Algorithm) ===
Vertex	Distance	Path
----------------------------------------
A -> B	3		A -> C -> B
A -> C	3		A -> C
A -> D	5		A -> C -> B -> D
=========================================================
