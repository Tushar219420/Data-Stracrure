## Problem Statement
Write a Program to accept a graph from a user and represent it with Adjacency Matrix and perform BFS and DFS traversals on it.

##  Code
```cpp
#include <iostream>
#include <queue>
#include <stack>
using namespace std;

const int MAX_VERTICES_TV = 20;

class Graph_tv {
private:
    int adjMatrix_tv[MAX_VERTICES_TV][MAX_VERTICES_TV];
    int numVertices_tv;
    char vertexLabels_tv[MAX_VERTICES_TV];

public:
    
    Graph_tv() {
        numVertices_tv = 0;
        for (int i = 0; i < MAX_VERTICES_TV; i++) {
            vertexLabels_tv[i] = 'A' + i;
        }
        
        for (int i = 0; i < MAX_VERTICES_TV; i++) {
            for (int j = 0; j < MAX_VERTICES_TV; j++) {
                adjMatrix_tv[i][j] = 0;
            }
        }
    }

    
    void createGraph_tv(int vertices) {
        if (vertices > MAX_VERTICES_TV) {
            cout << "Maximum vertices limit exceeded! Max allowed: " << MAX_VERTICES_TV << endl;
            return;
        }
        
        numVertices_tv = vertices;
        
        
        for (int i = 0; i < numVertices_tv; i++) {
            for (int j = 0; j < numVertices_tv; j++) {
                adjMatrix_tv[i][j] = 0;
            }
        }
        
        cout << "Graph created with " << vertices << " vertices (labeled A, B, C, ...)" << endl;
    }

    
    void addEdge_tv(int src, int dest) {
        if (src >= numVertices_tv || dest >= numVertices_tv || src < 0 || dest < 0) {
            cout << "Invalid vertex! Vertices must be between 0 and " << (numVertices_tv - 1) << endl;
            return;
        }
        
        adjMatrix_tv[src][dest] = 1;
        adjMatrix_tv[dest][src] = 1;  
        cout << "Edge added between " << vertexLabels_tv[src] << " and " << vertexLabels_tv[dest] << endl;
    }

    
    void removeEdge_tv(int src, int dest) {
        if (src >= numVertices_tv || dest >= numVertices_tv || src < 0 || dest < 0) {
            cout << "Invalid vertex! Vertices must be between 0 and " << (numVertices_tv - 1) << endl;
            return;
        }
        
        adjMatrix_tv[src][dest] = 0;
        adjMatrix_tv[dest][src] = 0;  
        cout << "Edge removed between " << vertexLabels_tv[src] << " and " << vertexLabels_tv[dest] << endl;
    }

    
    void displayAdjacencyMatrix_tv() {
        if (numVertices_tv == 0) {
            cout << "Graph is empty!" << endl;
            return;
        }
        
        cout << "\n=== Adjacency Matrix Representation ===" << endl;
        cout << "    ";
        for (int i = 0; i < numVertices_tv; i++) {
            cout << vertexLabels_tv[i] << "  ";
        }
        cout << endl;
        
        for (int i = 0; i < numVertices_tv; i++) {
            cout << vertexLabels_tv[i] << "   ";
            for (int j = 0; j < numVertices_tv; j++) {
                cout << adjMatrix_tv[i][j] << "  ";
            }
            cout << endl;
        }
        cout << "=====================================" << endl;
    }

    
    void bfsTraversal_tv(int startVertex) {
        if (numVertices_tv == 0) {
            cout << "Graph is empty!" << endl;
            return;
        }
        
        if (startVertex >= numVertices_tv || startVertex < 0) {
            cout << "Invalid start vertex! Must be between 0 and " << (numVertices_tv - 1) << endl;
            return;
        }
        
        bool visited[MAX_VERTICES_TV] = {false};
        queue<int> q;
        
        cout << "\nBFS Traversal starting from vertex " << vertexLabels_tv[startVertex] << ": ";
        
        visited[startVertex] = true;
        q.push(startVertex);
        
        while (!q.empty()) {
            int currentVertex = q.front();
            q.pop();
            cout << vertexLabels_tv[currentVertex] << " ";
            
            
            for (int i = 0; i < numVertices_tv; i++) {
                if (adjMatrix_tv[currentVertex][i] == 1 && !visited[i]) {
                    visited[i] = true;
                    q.push(i);
                }
            }
        }
        cout << endl;
    }

    
    void dfsTraversalIterative_tv(int startVertex) {
        if (numVertices_tv == 0) {
            cout << "Graph is empty!" << endl;
            return;
        }
        
        if (startVertex >= numVertices_tv || startVertex < 0) {
            cout << "Invalid start vertex! Must be between 0 and " << (numVertices_tv - 1) << endl;
            return;
        }
        
        bool visited[MAX_VERTICES_TV] = {false};
        stack<int> s;
        
        cout << "\nDFS Traversal (Iterative) starting from vertex " << vertexLabels_tv[startVertex] << ": ";
        
        s.push(startVertex);
        
        while (!s.empty()) {
            int currentVertex = s.top();
            s.pop();
            
            if (!visited[currentVertex]) {
                cout << vertexLabels_tv[currentVertex] << " ";
                visited[currentVertex] = true;
            }
            
            
            for (int i = numVertices_tv - 1; i >= 0; i--) {
                if (adjMatrix_tv[currentVertex][i] == 1 && !visited[i]) {
                    s.push(i);
                }
            }
        }
        cout << endl;
    }

    
    void dfsTraversalRecursive_tv(int startVertex) {
        if (numVertices_tv == 0) {
            cout << "Graph is empty!" << endl;
            return;
        }
        
        if (startVertex >= numVertices_tv || startVertex < 0) {
            cout << "Invalid start vertex! Must be between 0 and " << (numVertices_tv - 1) << endl;
            return;
        }
        
        bool visited[MAX_VERTICES_TV] = {false};
        cout << "\nDFS Traversal (Recursive) starting from vertex " << vertexLabels_tv[startVertex] << ": ";
        dfsRecursiveHelper_tv(startVertex, visited);
        cout << endl;
    }

    
    void dfsRecursiveHelper_tv(int vertex, bool visited[]) {
        visited[vertex] = true;
        cout << vertexLabels_tv[vertex] << " ";
        
        
        for (int i = 0; i < numVertices_tv; i++) {
            if (adjMatrix_tv[vertex][i] == 1 && !visited[i]) {
                dfsRecursiveHelper_tv(i, visited);
            }
        }
    }

    
    bool isEdgeExists_tv(int src, int dest) {
        if (src >= numVertices_tv || dest >= numVertices_tv || src < 0 || dest < 0) {
            return false;
        }
        return adjMatrix_tv[src][dest] == 1;
    }

    
    int getDegree_tv(int vertex) {
        if (vertex >= numVertices_tv || vertex < 0) {
            cout << "Invalid vertex!" << endl;
            return -1;
        }
        
        int degree = 0;
        for (int i = 0; i < numVertices_tv; i++) {
            if (adjMatrix_tv[vertex][i] == 1) {
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

    bool isConnected_tv() {
        if (numVertices_tv == 0) {
            return true;
        }
        
        bool visited[MAX_VERTICES_TV] = {false};
        queue<int> q;
        
        
        visited[0] = true;
        q.push(0);
        
        while (!q.empty()) {
            int currentVertex = q.front();
            q.pop();
            
            for (int i = 0; i < numVertices_tv; i++) {
                if (adjMatrix_tv[currentVertex][i] == 1 && !visited[i]) {
                    visited[i] = true;
                    q.push(i);
                }
            }
        }
        
        
        for (int i = 0; i < numVertices_tv; i++) {
            if (!visited[i]) {
                return false;
            }
        }
        return true;
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
            for (int j = i; j < numVertices_tv; j++) {
                if (adjMatrix_tv[i][j] == 1) {
                    edgeCount++;
                }
            }
        }
        cout << "Number of edges: " << edgeCount << endl;
        cout << "Graph connected: " << (isConnected_tv() ? "Yes" : "No") << endl;
        cout << "========================" << endl;
    }

    void inputGraph_tv() {
        int vertices, edges, src, dest;
        
        cout << "Enter number of vertices: ";
        cin >> vertices;
        createGraph_tv(vertices);
        
        cout << "Enter number of edges: ";
        cin >> edges;
        
        cout << "Enter edges (source destination):" << endl;
        for (int i = 0; i < edges; i++) {
            cout << "Edge " << (i+1) << ": ";
            cin >> src >> dest;
            addEdge_tv(src, dest);
        }
    }
};

int main() {
    Graph_tv graph_tv;
    int choice, vertices, src, dest, startVertex;

    cout << "=== Graph Representation and Traversals ===" << endl;
    cout << "Using Adjacency Matrix" << endl;
    cout << "Supports BFS and DFS traversals" << endl;

    while (true) {
        cout << "\n1. Create Graph\n2. Input Graph\n3. Add Edge\n4. Remove Edge\n5. Display Adjacency Matrix\n6. BFS Traversal\n7. DFS Traversal (Iterative)\n8. DFS Traversal (Recursive)\n9. Display Vertex Degrees\n10. Display Graph Info\n11. Check Edge\n12. Exit\n";
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
                graph_tv.addEdge_tv(src, dest);
                break;

            case 4:
                cout << "Enter source vertex: ";
                cin >> src;
                cout << "Enter destination vertex: ";
                cin >> dest;
                graph_tv.removeEdge_tv(src, dest);
                break;

            case 5:
                graph_tv.displayAdjacencyMatrix_tv();
                break;

            case 6:
                cout << "Enter start vertex (0 to " << (graph_tv.numVertices_tv - 1) << "): ";
                cin >> startVertex;
                graph_tv.bfsTraversal_tv(startVertex);
                break;

            case 7:
                cout << "Enter start vertex (0 to " << (graph_tv.numVertices_tv - 1) << "): ";
                cin >> startVertex;
                graph_tv.dfsTraversalIterative_tv(startVertex);
                break;

            case 8:
                cout << "Enter start vertex (0 to " << (graph_tv.numVertices_tv - 1) << "): ";
                cin >> startVertex;
                graph_tv.dfsTraversalRecursive_tv(startVertex);
                break;

            case 9:
                graph_tv.displayVertexDegrees_tv();
                break;

            case 10:
                graph_tv.displayGraphInfo_tv();
                break;

            case 11:
                cout << "Enter source vertex: ";
                cin >> src;
                cout << "Enter destination vertex: ";
                cin >> dest;
                if (graph_tv.isEdgeExists_tv(src, dest)) {
                    cout << "Edge exists between " << char('A' + src) << " and " << char('A' + dest) << endl;
                } else {
                    cout << "No edge between " << char('A' + src) << " and " << char('A' + dest) << endl;
                }
                break;

            case 12:
                cout << "Thank you for using the Graph Representation and Traversal System!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


##  Output

=== Graph Representation and Traversals ===
Using Adjacency Matrix
Supports BFS and DFS traversals

1. Create Graph
2. Input Graph
3. Add Edge
4. Remove Edge
5. Display Adjacency Matrix
6. BFS Traversal
7. DFS Traversal (Iterative)
8. DFS Traversal (Recursive)
9. Display Vertex Degrees
10. Display Graph Info
11. Check Edge
12. Exit
Enter your choice: 1
Enter number of vertices: 5
Graph created with 5 vertices (labeled A, B, C, ...)

1. Create Graph
2. Input Graph
3. Add Edge
4. Remove Edge
5. Display Adjacency Matrix
6. BFS Traversal
7. DFS Traversal (Iterative)
8. DFS Traversal (Recursive)
9. Display Vertex Degrees
10. Display Graph Info
11. Check Edge
12. Exit
Enter your choice: 3
Enter source vertex: 0
Enter destination vertex: 1
Edge added between A and B

1. Create Graph
2. Input Graph
3. Add Edge
4. Remove Edge
5. Display Adjacency Matrix
6. BFS Traversal
7. DFS Traversal (Iterative)
8. DFS Traversal (Recursive)
9. Display Vertex Degrees
10. Display Graph Info
11. Check Edge
12. Exit
Enter your choice: 3
Enter source vertex: 0
Enter destination vertex: 2
Edge added between A and C

1. Create Graph
2. Input Graph
3. Add Edge
4. Remove Edge
5. Display Adjacency Matrix
6. BFS Traversal
7. DFS Traversal (Iterative)
8. DFS Traversal (Recursive)
9. Display Vertex Degrees
10. Display Graph Info
11. Check Edge
12. Exit
Enter your choice: 3
Enter source vertex: 1
Enter destination vertex: 3
Edge added between B and D

1. Create Graph
2. Input Graph
3. Add Edge
4. Remove Edge
5. Display Adjacency Matrix
6. BFS Traversal
7. DFS Traversal (Iterative)
8. DFS Traversal (Recursive)
9. Display Vertex Degrees
10. Display Graph Info
11. Check Edge
12. Exit
Enter your choice: 3
Enter source vertex: 2
Enter destination vertex: 4
Edge added between C and E

1. Create Graph
2. Input Graph
3. Add Edge
4. Remove Edge
5. Display Adjacency Matrix
6. BFS Traversal
7. DFS Traversal (Iterative)
8. DFS Traversal (Recursive)
9. Display Vertex Degrees
10. Display Graph Info
11. Check Edge
12. Exit
Enter your choice: 3
Enter source vertex: 3
Enter destination vertex: 4
Edge added between D and E

1. Create Graph
2. Input Graph
3. Add Edge
4. Remove Edge
5. Display Adjacency Matrix
6. BFS Traversal
7. DFS Traversal (Iterative)
8. DFS Traversal (Recursive)
9. Display Vertex Degrees
10. Display Graph Info
11. Check Edge
12. Exit
Enter your choice: 5

=== Adjacency Matrix Representation ===
    A  B  C  D  E  
A   0  1  1  0  0  
B   1  0  0  1  0  
C   1  0  0  0  1  
D   0  1  0  0  1  
E   0  0  1  1  0  
=====================================

1. Create Graph
2. Input Graph
3. Add Edge
4. Remove Edge
5. Display Adjacency Matrix
6. BFS Traversal
7. DFS Traversal (Iterative)
8. DFS Traversal (Recursive)
9. Display Vertex Degrees
10. Display Graph Info
11. Check Edge
12. Exit
Enter your choice: 6
Enter start vertex (0 to 4): 0

BFS Traversal starting from vertex A: A B C D E 

1. Create Graph
2. Input Graph
3. Add Edge
4. Remove Edge
5. Display Adjacency Matrix
6. BFS Traversal
7. DFS Traversal (Iterative)
8. DFS Traversal (Recursive)
9. Display Vertex Degrees
10. Display Graph Info
11. Check Edge
12. Exit
Enter your choice: 7
Enter start vertex (0 to 4): 0

DFS Traversal (Iterative) starting from vertex A: A C E D B 

1. Create Graph
2. Input Graph
3. Add Edge
4. Remove Edge
5. Display Adjacency Matrix
6. BFS Traversal
7. DFS Traversal (Iterative)
8. DFS Traversal (Recursive)
9. Display Vertex Degrees
10. Display Graph Info
11. Check Edge
12. Exit
Enter your choice: 8
Enter start vertex (0 to 4): 0

DFS Traversal (Recursive) starting from vertex A: A B D E C 

1. Create Graph
2. Input Graph
3. Add Edge
4. Remove Edge
5. Display Adjacency Matrix
6. BFS Traversal
7. DFS Traversal (Iterative)
8. DFS Traversal (Recursive)
9. Display Vertex Degrees
10. Display Graph Info
11. Check Edge
12. Exit
Enter your choice: 9

=== Vertex Degrees ===
Vertex A: Degree = 2
Vertex B: Degree = 2
Vertex C: Degree = 2
Vertex D: Degree = 2
Vertex E: Degree = 2
=====================

1. Create Graph
2. Input Graph
3. Add Edge
4. Remove Edge
5. Display Adjacency Matrix
6. BFS Traversal
7. DFS Traversal (Iterative)
8. DFS Traversal (Recursive)
9. Display Vertex Degrees
10. Display Graph Info
11. Check Edge
12. Exit
Enter your choice: 10

=== Graph Information ===
Number of vertices: 5
Vertices: A B C D E 
Number of edges: 5
Graph connected: Yes
========================

1. Create Graph
2. Input Graph
3. Add Edge
4. Remove Edge
5. Display Adjacency Matrix
6. BFS Traversal
7. DFS Traversal (Iterative)
8. DFS Traversal (Recursive)
9. Display Vertex Degrees
10. Display Graph Info
11. Check Edge
12. Exit
Enter your choice: 11
Enter source vertex: 0
Enter destination vertex: 3
No edge between A and D

1. Create Graph
2. Input Graph
3. Add Edge
4. Remove Edge
5. Display Adjacency Matrix
6. BFS Traversal
7. DFS Traversal (Iterative)
8. DFS Traversal (Recursive)
9. Display Vertex Degrees
10. Display Graph Info
11. Check Edge
12. Exit
Enter your choice: 4
Enter source vertex: 0
Enter destination vertex: 1
Edge removed between A and B

1. Create Graph
2. Input Graph
3. Add Edge
4. Remove Edge
5. Display Adjacency Matrix
6. BFS Traversal
7. DFS Traversal (Iterative)
8. DFS Traversal (Recursive)
9. Display Vertex Degrees
10. Display Graph Info
11. Check Edge
12. Exit
Enter your choice: 5

=== Adjacency Matrix Representation ===
    A  B  C  D  E  
A   0  0  1  0  0  
B   0  0  0  1  0  
C   1  0  0  0  1  
D   0  1  0  0  1  
E   0  0  1  1  0  
=====================================
