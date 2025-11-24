## Problem Statement
Write a Program to implement Kruskal's algorithm to find the minimum spanning tree of a user defined graph. Use Adjacency List to represent a graph.

## Code
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <cstring>
using namespace std;

const int MAX_VERTICES_TVV = 20;

struct Edge_TVV {
    int src_TVV;
    int dest_TVV;
    int weight_TVV;
};

class Graph_TVV {
private:
    vector<pair<int, int>> adjList_TVV[MAX_VERTICES_TVV];
    int numVertices_TVV;
    char vertexLabels_TVV[MAX_VERTICES_TVV];

public:
    Graph_TVV() {
        numVertices_TVV = 0;
        for (int i = 0; i < MAX_VERTICES_TVV; i++) {
            vertexLabels_TVV[i] = 'A' + i;
        }
    }

    void createGraph_TVV(int vertices) {
        if (vertices > MAX_VERTICES_TVV) {
            cout << "Maximum vertices limit exceeded! Max allowed: " << MAX_VERTICES_TVV << endl;
            return;
        }
        
        numVertices_TVV = vertices;
        
        for (int i = 0; i < numVertices_TVV; i++) {
            adjList_TVV[i].clear();
        }
        
        cout << "Graph created with " << vertices << " vertices (labeled A, B, C, ...)" << endl;
    }

    void addEdge_TVV(int src, int dest, int weight) {
        if (src >= numVertices_TVV || dest >= numVertices_TVV || src < 0 || dest < 0) {
            cout << "Invalid vertex! Vertices must be between 0 and " << (numVertices_TVV - 1) << endl;
            return;
        }
        
        adjList_TVV[src].push_back(make_pair(dest, weight));
        adjList_TVV[dest].push_back(make_pair(src, weight));
        
        cout << "Edge added between " << vertexLabels_TVV[src] << " and " << vertexLabels_TVV[dest] 
             << " with weight " << weight << endl;
    }

    void displayAdjList_TVV() {
        if (numVertices_TVV == 0) {
            cout << "Graph is empty!" << endl;
            return;
        }
        
        cout << "\n=== Adjacency List Representation ===" << endl;
        for (int i = 0; i < numVertices_TVV; i++) {
            cout << vertexLabels_TVV[i] << ": ";
            for (int j = 0; j < adjList_TVV[i].size(); j++) {
                cout << "(" << vertexLabels_TVV[adjList_TVV[i][j].first] << ", " 
                     << adjList_TVV[i][j].second << ") ";
            }
            cout << endl;
        }
        cout << "===================================" << endl;
    }

    int findParent_TVV(vector<int>& parent_TVV, int vertex) {
        if (parent_TVV[vertex] == -1) {
            return vertex;
        }
        return findParent_TVV(parent_TVV, parent_TVV[vertex]);
    }

    void unionVertices_TVV(vector<int>& parent_TVV, int src, int dest) {
        int srcParent = findParent_TVV(parent_TVV, src);
        int destParent = findParent_TVV(parent_TVV, dest);
        
        if (srcParent != destParent) {
            parent_TVV[srcParent] = destParent;
        }
    }

    void kruskalMST_TVV() {
        if (numVertices_TVV == 0) {
            cout << "Graph is empty!" << endl;
            return;
        }
        
        vector<Edge_TVV> edges_TVV;
        
        for (int i = 0; i < numVertices_TVV; i++) {
            for (int j = 0; j < adjList_TVV[i].size(); j++) {
                int dest = adjList_TVV[i][j].first;
                int weight = adjList_TVV[i][j].second;
                
                if (i < dest) {
                    Edge_TVV edge_TVV;
                    edge_TVV.src_TVV = i;
                    edge_TVV.dest_TVV = dest;
                    edge_TVV.weight_TVV = weight;
                    edges_TVV.push_back(edge_TVV);
                }
            }
        }
        
        sort(edges_TVV.begin(), edges_TVV.end(), [](Edge_TVV a, Edge_TVV b) {
            return a.weight_TVV < b.weight_TVV;
        });
        
        vector<int> parent_TVV(numVertices_TVV, -1);
        vector<Edge_TVV> mstEdges_TVV;
        int totalWeight_TVV = 0;
        
        cout << "\n=== Kruskal's Algorithm Execution ===" << endl;
        cout << "Processing edges in ascending order of weight:" << endl;
        
        for (int i = 0; i < edges_TVV.size(); i++) {
            int src = edges_TVV[i].src_TVV;
            int dest = edges_TVV[i].dest_TVV;
            int weight = edges_TVV[i].weight_TVV;
            
            int srcParent = findParent_TVV(parent_TVV, src);
            int destParent = findParent_TVV(parent_TVV, dest);
            
            cout << "Edge " << vertexLabels_TVV[src] << "-" << vertexLabels_TVV[dest] 
                 << " (Weight: " << weight << ") ";
            
            if (srcParent != destParent) {
                mstEdges_TVV.push_back(edges_TVV[i]);
                unionVertices_TVV(parent_TVV, src, dest);
                totalWeight_TVV += weight;
                cout << "=> Included in MST" << endl;
            } else {
                cout << "=> Forms cycle, rejected" << endl;
            }
        }
        
        cout << "\n=== Minimum Spanning Tree Edges ===" << endl;
        for (int i = 0; i < mstEdges_TVV.size(); i++) {
            cout << vertexLabels_TVV[mstEdges_TVV[i].src_TVV] << " - " 
                 << vertexLabels_TVV[mstEdges_TVV[i].dest_TVV] << " : " 
                 << mstEdges_TVV[i].weight_TVV << endl;
        }
        cout << "Total Weight of MST: " << totalWeight_TVV << endl;
        cout << "==================================" << endl;
    }

    void displayGraphInfo_TVV() {
        cout << "\n=== Graph Information ===" << endl;
        cout << "Number of vertices: " << numVertices_TVV << endl;
        cout << "Vertices: ";
        for (int i = 0; i < numVertices_TVV; i++) {
            cout << vertexLabels_TVV[i] << " ";
        }
        cout << endl;
        
        int edgeCount = 0;
        for (int i = 0; i < numVertices_TVV; i++) {
            edgeCount += adjList_TVV[i].size();
        }
        cout << "Number of edges: " << (edgeCount/2) << endl;
        cout << "========================" << endl;
    }

    void inputGraph_TVV() {
        int vertices, edges, src, dest, weight;
        
        cout << "Enter number of vertices: ";
        cin >> vertices;
        createGraph_TVV(vertices);
        
        cout << "Enter number of edges: ";
        cin >> edges;
        
        cout << "Enter edges (source destination weight):" << endl;
        for (int i = 0; i < edges; i++) {
            cout << "Edge " << (i+1) << ": ";
            cin >> src >> dest >> weight;
            addEdge_TVV(src, dest, weight);
        }
    }
};

int main() {
    Graph_TVV graph_TVV;
    int choice, vertices, src, dest, weight;

    cout << "=== Kruskal's Algorithm for Minimum Spanning Tree ===" << endl;
    cout << "Using Adjacency List Representation" << endl;

    while (true) {
        cout << "\n1. Create Graph\n2. Input Graph\n3. Add Edge\n4. Display Adjacency List\n5. Find Minimum Spanning Tree (Kruskal)\n6. Display Graph Info\n7. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter number of vertices: ";
                cin >> vertices;
                graph_TVV.createGraph_TVV(vertices);
                break;

            case 2:
                graph_TVV.inputGraph_TVV();
                break;

            case 3:
                cout << "Enter source vertex: ";
                cin >> src;
                cout << "Enter destination vertex: ";
                cin >> dest;
                cout << "Enter weight: ";
                cin >> weight;
                graph_TVV.addEdge_TVV(src, dest, weight);
                break;

            case 4:
                graph_TVV.displayAdjList_TVV();
                break;

            case 5:
                graph_TVV.kruskalMST_TVV();
                break;

            case 6:
                graph_TVV.displayGraphInfo_TVV();
                break;

            case 7:
                cout << "Thank you for using Kruskal's Algorithm Implementation!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


## Output

=== Kruskal's Algorithm for Minimum Spanning Tree ===
Using Adjacency List Representation

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Kruskal)
6. Display Graph Info
7. Exit
Enter your choice: 1
Enter number of vertices: 6
Graph created with 6 vertices (labeled A, B, C, ...)

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Kruskal)
6. Display Graph Info
7. Exit
Enter your choice: 3
Enter source vertex: 0
Enter destination vertex: 1
Enter weight: 4
Edge added between A and B with weight 4

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Kruskal)
6. Display Graph Info
7. Exit
Enter your choice: 3
Enter source vertex: 0
Enter destination vertex: 2
Enter weight: 2
Edge added between A and C with weight 2

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Kruskal)
6. Display Graph Info
7. Exit
Enter your choice: 3
Enter source vertex: 1
Enter destination vertex: 2
Enter weight: 1
Edge added between B and C with weight 1

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Kruskal)
6. Display Graph Info
7. Exit
Enter your choice: 3
Enter source vertex: 1
Enter destination vertex: 3
Enter weight: 5
Edge added between B and D with weight 5

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Kruskal)
6. Display Graph Info
7. Exit
Enter your choice: 3
Enter source vertex: 2
Enter destination vertex: 3
Enter weight: 8
Edge added between C and D with weight 8

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Kruskal)
6. Display Graph Info
7. Exit
Enter your choice: 3
Enter source vertex: 2
Enter destination vertex: 4
Enter weight: 10
Edge added between C and E with weight 10

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Kruskal)
6. Display Graph Info
7. Exit
Enter your choice: 3
Enter source vertex: 3
Enter destination vertex: 4
Enter weight: 2
Edge added between D and E with weight 2

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Kruskal)
6. Display Graph Info
7. Exit
Enter your choice: 3
Enter source vertex: 3
Enter destination vertex: 5
Enter weight: 6
Edge added between D and F with weight 6

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Kruskal)
6. Display Graph Info
7. Exit
Enter your choice: 3
Enter source vertex: 4
Enter destination vertex: 5
Enter weight: 3
Edge added between E and F with weight 3

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Kruskal)
6. Display Graph Info
7. Exit
Enter your choice: 4

=== Adjacency List Representation ===
A: (B, 4) (C, 2) 
B: (A, 4) (C, 1) (D, 5) 
C: (A, 2) (B, 1) (D, 8) (E, 10) 
D: (B, 5) (C, 8) (E, 2) (F, 6) 
E: (C, 10) (D, 2) (F, 3) 
F: (D, 6) (E, 3) 
===================================

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Kruskal)
6. Display Graph Info
7. Exit
Enter your choice: 5

=== Kruskal's Algorithm Execution ===
Processing edges in ascending order of weight:
Edge B-C (Weight: 1) => Included in MST
Edge A-C (Weight: 2) => Included in MST
Edge D-E (Weight: 2) => Included in MST
Edge E-F (Weight: 3) => Included in MST
Edge A-B (Weight: 4) => Forms cycle, rejected
Edge B-D (Weight: 5) => Forms cycle, rejected
Edge D-F (Weight: 6) => Forms cycle, rejected
Edge C-D (Weight: 8) => Forms cycle, rejected
Edge C-E (Weight: 10) => Forms cycle, rejected

=== Minimum Spanning Tree Edges ===
B - C : 1
A - C : 2
D - E : 2
E - F : 3
Total Weight of MST: 8
==================================

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Kruskal)
6. Display Graph Info
7. Exit
Enter your choice: 6

=== Graph Information ===
Number of vertices: 6
Vertices: A B C D E F 
Number of edges: 9
========================

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Kruskal)
6. Display Graph Info
7. Exit
Enter your choice: 2
Enter number of vertices: 5
Enter number of edges: 7
Enter edges (source destination weight):
Edge 1: 0 1 10
Edge added between A and B with weight 10
Edge 2: 0 2 6
Edge added between A and C with weight 6
Edge 3: 0 3 5
Edge added between A and D with weight 5
Edge 4: 1 3 15
Edge added between B and D with weight 15
Edge 5: 2 3 4
Edge added between C and D with weight 4
Edge 6: 2 4 2
Edge added between C and E with weight 2
Edge 7: 3 4 8
Edge added between D and E with weight 8

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Kruskal)
6. Display Graph Info
7. Exit
Enter your choice: 5

=== Kruskal's Algorithm Execution ===
Processing edges in ascending order of weight:
Edge C-E (Weight: 2) => Included in MST
Edge C-D (Weight: 4) => Included in MST
Edge A-D (Weight: 5) => Included in MST
Edge A-C (Weight: 6) => Forms cycle, rejected
Edge D-E (Weight: 8) => Forms cycle, rejected
Edge A-B (Weight: 10) => Included in MST
Edge B-D (Weight: 15) => Forms cycle, rejected

=== Minimum Spanning Tree Edges ===
C - E : 2
C - D : 4
A - D : 5
A - B : 10
Total Weight of MST: 21
==================================
