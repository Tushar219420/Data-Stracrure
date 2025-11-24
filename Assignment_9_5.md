 ## Problem Statement
Write a Program to accept a graph from a user and represent it with Adjacency List and perform BFS and DFS traversals on it.

##  Code
```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <stack>
using namespace std;

const int MAX_VERTICES_TVV = 20;

class Graph_TVV {
private:
    vector<int> adjList_TVV[MAX_VERTICES_TVV];
    int numVertices_TVV;
    char vertexLabels_TVV[MAX_VERTICES_TVV];

public:
    // Constructor
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

 
    void addEdge_TVV(int src, int dest) {
        if (src >= numVertices_TVV || dest >= numVertices_TVV || src < 0 || dest < 0) {
            cout << "Invalid vertex! Vertices must be between 0 and " << (numVertices_TVV - 1) << endl;
            return;
        }
        
        adjList_TVV[src].push_back(dest);
        adjList_TVV[dest].push_back(src); 
        
        cout << "Edge added between " << vertexLabels_TVV[src] << " and " << vertexLabels_TVV[dest] << endl;
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
                cout << vertexLabels_TVV[adjList_TVV[i][j]] << " ";
            }
            cout << endl;
        }
        cout << "===================================" << endl;
    }

    
    void bfsTraversal_TVV(int startVertex) {
        if (numVertices_TVV == 0) {
            cout << "Graph is empty!" << endl;
            return;
        }
        
        if (startVertex >= numVertices_TVV || startVertex < 0) {
            cout << "Invalid start vertex!" << endl;
            return;
        }
        
        vector<bool> visited_TVV(numVertices_TVV, false);
        queue<int> queue_TVV;
        
        visited_TVV[startVertex] = true;
        queue_TVV.push(startVertex);
        
        cout << "\nBFS Traversal starting from vertex " << vertexLabels_TVV[startVertex] << ": ";
        
        while (!queue_TVV.empty()) {
            int currentVertex = queue_TVV.front();
            queue_TVV.pop();
            
            cout << vertexLabels_TVV[currentVertex] << " ";
            
            for (int i = 0; i < adjList_TVV[currentVertex].size(); i++) {
                int adjVertex = adjList_TVV[currentVertex][i];
                if (!visited_TVV[adjVertex]) {
                    visited_TVV[adjVertex] = true;
                    queue_TVV.push(adjVertex);
                }
            }
        }
        cout << endl;
    }

    void dfsUtil_TVV(int vertex, vector<bool>& visited_TVV) {
        visited_TVV[vertex] = true;
        cout << vertexLabels_TVV[vertex] << " ";
        
        
        for (int i = 0; i < adjList_TVV[vertex].size(); i++) {
            int adjVertex = adjList_TVV[vertex][i];
            if (!visited_TVV[adjVertex]) {
                dfsUtil_TVV(adjVertex, visited_TVV);
            }
        }
    }

    void dfsTraversal_TVV(int startVertex) {
        if (numVertices_TVV == 0) {
            cout << "Graph is empty!" << endl;
            return;
        }
        
        if (startVertex >= numVertices_TVV || startVertex < 0) {
            cout << "Invalid start vertex!" << endl;
            return;
        }
        
        vector<bool> visited_TVV(numVertices_TVV, false);
        
        cout << "\nDFS Traversal starting from vertex " << vertexLabels_TVV[startVertex] << ": ";
        dfsUtil_TVV(startVertex, visited_TVV);
        cout << endl;
    }

    
    void displayGraphInfo_TVV() {
        cout << "\n=== Graph Information ===" << endl;
        cout << "Number of vertices: " << numVertices_TVV << endl;
        cout << "Vertices: ";
        for (int i = 0; i < numVertices_TVV; i++) {
            cout << vertexLabels_TVV[i] << " ";
        }
        cout << endl;
        
        int totalEdges = 0;
        for (int i = 0; i < numVertices_TVV; i++) {
            totalEdges += adjList_TVV[i].size();
        }
        cout << "Number of edges: " << (totalEdges/2) << endl; 
        cout << "========================" << endl;
    }

    
    void inputGraph_TVV() {
        int vertices, edges, src, dest;
        
        cout << "Enter number of vertices: ";
        cin >> vertices;
        createGraph_TVV(vertices);
        
        cout << "Enter number of edges: ";
        cin >> edges;
        
        cout << "Enter edges (source destination):" << endl;
        for (int i = 0; i < edges; i++) {
            cout << "Edge " << (i+1) << ": ";
            cin >> src >> dest;
            addEdge_TVV(src, dest);
        }
    }
};

int main() {
    Graph_TVV graph_TVV;
    int choice, vertices, src, dest, startVertex;

    cout << "=== Graph Traversal Program (BFS & DFS) ===" << endl;
    cout << "Using Adjacency List Representation" << endl;

    while (true) {
        cout << "\n1. Create Graph\n2. Input Graph\n3. Add Edge\n4. Display Adjacency List\n5. BFS Traversal\n6. DFS Traversal\n7. Display Graph Info\n8. Exit\n";
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
                graph_TVV.addEdge_TVV(src, dest);
                break;

            case 4:
                graph_TVV.displayAdjList_TVV();
                break;

            case 5:
                cout << "Enter start vertex for BFS: ";
                cin >> startVertex;
                graph_TVV.bfsTraversal_TVV(startVertex);
                break;

            case 6:
                cout << "Enter start vertex for DFS: ";
                cin >> startVertex;
                graph_TVV.dfsTraversal_TVV(startVertex);
                break;

            case 7:
                graph_TVV.displayGraphInfo_TVV();
                break;

            case 8:
                cout << "Thank you for using Graph Traversal Program!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


## Output

=== Graph Traversal Program (BFS & DFS) ===
Using Adjacency List Representation

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. BFS Traversal
6. DFS Traversal
7. Display Graph Info
8. Exit
Enter your choice: 1
Enter number of vertices: 6
Graph created with 6 vertices (labeled A, B, C, ...)

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. BFS Traversal
6. DFS Traversal
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 0
Enter destination vertex: 1
Edge added between A and B

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. BFS Traversal
6. DFS Traversal
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 0
Enter destination vertex: 2
Edge added between A and C

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. BFS Traversal
6. DFS Traversal
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 1
Enter destination vertex: 3
Edge added between B and D

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. BFS Traversal
6. DFS Traversal
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 1
Enter destination vertex: 4
Edge added between B and E

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. BFS Traversal
6. DFS Traversal
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 2
Enter destination vertex: 5
Edge added between C and F

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. BFS Traversal
6. DFS Traversal
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 3
Enter destination vertex: 4
Edge added between D and E

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. BFS Traversal
6. DFS Traversal
7. Display Graph Info
8. Exit
Enter your choice: 4

=== Adjacency List Representation ===
A: B C 
B: A D E 
C: A F 
D: B E 
E: B D 
F: C 
===================================

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. BFS Traversal
6. DFS Traversal
7. Display Graph Info
8. Exit
Enter your choice: 5
Enter start vertex for BFS: 0

BFS Traversal starting from vertex A: A B C D E F 

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. BFS Traversal
6. DFS Traversal
7. Display Graph Info
8. Exit
Enter your choice: 6
Enter start vertex for DFS: 0

DFS Traversal starting from vertex A: A B D E C F 

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. BFS Traversal
6. DFS Traversal
7. Display Graph Info
8. Exit
Enter your choice: 7

=== Graph Information ===
Number of vertices: 6
Vertices: A B C D E F 
Number of edges: 6
========================

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. BFS Traversal
6. DFS Traversal
7. Display Graph Info
8. Exit
Enter your choice: 2
Enter number of vertices: 5
Enter number of edges: 6
Enter edges (source destination):
Edge 1: 0 1
Edge added between A and B
Edge 2: 0 2
Edge added between A and C
Edge 3: 1 3
Edge added between B and D
Edge 4: 1 4
Edge added between B and E
Edge 5: 2 3
Edge added between C and D
Edge 6: 3 4
Edge added between D and E

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. BFS Traversal
6. DFS Traversal
7. Display Graph Info
8. Exit
Enter your choice: 4

=== Adjacency List Representation ===
A: B C 
B: A D E 
C: A D 
D: B C E 
E: B D 
===================================

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. BFS Traversal
6. DFS Traversal
7. Display Graph Info
8. Exit
Enter your choice: 5
Enter start vertex for BFS: 0

BFS Traversal starting from vertex A: A B C D E 

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. BFS Traversal
6. DFS Traversal
7. Display Graph Info
8. Exit
Enter your choice: 6
Enter start vertex for DFS: 0

DFS Traversal starting from vertex A: A B D C E 
