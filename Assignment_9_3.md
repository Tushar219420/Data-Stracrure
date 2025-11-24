

## Problem Statement
Write a Program to implement Kruskal's algorithm to find minimum spanning tree of a user defined graph. Use Adjacency List to represent a graph.

##  Code
```cpp
#include <iostream>
#include <climits>
#include <vector>
#include <algorithm>
using namespace std;

const int MAX_VERTICES_TV = 20;

struct Edge_tv {
    int src;
    int dest;
    int weight;
};


struct AdjListNode_tv {
    int dest;
    int weight;
    AdjListNode_tv* next;
};


struct AdjList_tv {
    AdjListNode_tv* head;
};

struct Subset_tv {
    int parent;
    int rank;
};

class Graph_tv {
private:
    int numVertices_tv;
    int numEdges_tv;
    AdjList_tv* adjListArray_tv;
    Edge_tv* edgeArray_tv;
    char vertexLabels_tv[MAX_VERTICES_TV];

public:

    Graph_tv() {
        numVertices_tv = 0;
        numEdges_tv = 0;
        adjListArray_tv = nullptr;
        edgeArray_tv = nullptr;
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
        
        adjListArray_tv = new AdjList_tv[numVertices_tv];
       
        for (int i = 0; i < numVertices_tv; i++) {
            adjListArray_tv[i].head = nullptr;
        }
        
        cout << "Graph created with " << vertices << " vertices (labeled A, B, C, ...)" << endl;
    }

    
    void addEdge_tv(int src, int dest, int weight) {
        if (src >= numVertices_tv || dest >= numVertices_tv || src < 0 || dest < 0) {
            cout << "Invalid vertex! Vertices must be between 0 and " << (numVertices_tv - 1) << endl;
            return;
        }
       
        AdjListNode_tv* newNode = new AdjListNode_tv;
        newNode->dest = dest;
        newNode->weight = weight;
        newNode->next = adjListArray_tv[src].head;
        adjListArray_tv[src].head = newNode;
        
        
        newNode = new AdjListNode_tv;
        newNode->dest = src;
        newNode->weight = weight;
        newNode->next = adjListArray_tv[dest].head;
        adjListArray_tv[dest].head = newNode;
        
        
        if (edgeArray_tv == nullptr) {
            edgeArray_tv = new Edge_tv[MAX_VERTICES_TV * MAX_VERTICES_TV];
        }
        edgeArray_tv[numEdges_tv].src = src;
        edgeArray_tv[numEdges_tv].dest = dest;
        edgeArray_tv[numEdges_tv].weight = weight;
        numEdges_tv++;
        
        cout << "Edge added between " << vertexLabels_tv[src] << " and " << vertexLabels_tv[dest] 
             << " with weight " << weight << endl;
    }

    
    void displayAdjacencyList_tv() {
        if (numVertices_tv == 0) {
            cout << "Graph is empty!" << endl;
            return;
        }
        
        cout << "\n=== Adjacency List Representation ===" << endl;
        for (int i = 0; i < numVertices_tv; i++) {
            cout << "Vertex " << vertexLabels_tv[i] << ": ";
            AdjListNode_tv* temp = adjListArray_tv[i].head;
            while (temp != nullptr) {
                cout << "(" << vertexLabels_tv[temp->dest] << ", " << temp->weight << ") ";
                temp = temp->next;
            }
            cout << endl;
        }
        cout << "===================================" << endl;
    }

    
    int find_tv(Subset_tv subsets[], int i) {
        
        if (subsets[i].parent != i) {
            subsets[i].parent = find_tv(subsets, subsets[i].parent);
        }
        return subsets[i].parent;
    }

    
    void union_tv(Subset_tv subsets[], int x, int y) {
        int xroot = find_tv(subsets, x);
        int yroot = find_tv(subsets, y);
        
        
        if (subsets[xroot].rank < subsets[yroot].rank) {
            subsets[xroot].parent = yroot;
        } else if (subsets[xroot].rank > subsets[yroot].rank) {
            subsets[yroot].parent = xroot;
        } else {
            subsets[yroot].parent = xroot;
            subsets[xroot].rank++;
        }
    }

    void kruskalsMST_tv() {
        if (numVertices_tv == 0) {
            cout << "Graph is empty!" << endl;
            return;
        }
        
        Edge_tv result_tv[MAX_VERTICES_TV]; 
        int e = 0; 
        int i = 0; 
        
        
        sortEdges_tv();
        
        
        Subset_tv* subsets = new Subset_tv[numVertices_tv];
        
        
        for (int v = 0; v < numVertices_tv; v++) {
            subsets[v].parent = v;
            subsets[v].rank = 0;
        }
        
        while (e < numVertices_tv - 1 && i < numEdges_tv) {
            
            Edge_tv nextEdge = edgeArray_tv[i++];
            
            int x = find_tv(subsets, nextEdge.src);
            int y = find_tv(subsets, nextEdge.dest);
            
            
            if (x != y) {
                result_tv[e++] = nextEdge;
                union_tv(subsets, x, y);
            }
        }
        
     
        printMST_tv(result_tv, e);
        
        delete[] subsets;
    }

    
    void sortEdges_tv() {
        for (int i = 0; i < numEdges_tv - 1; i++) {
            for (int j = 0; j < numEdges_tv - i - 1; j++) {
                if (edgeArray_tv[j].weight > edgeArray_tv[j + 1].weight) {
                    
                    Edge_tv temp = edgeArray_tv[j];
                    edgeArray_tv[j] = edgeArray_tv[j + 1];
                    edgeArray_tv[j + 1] = temp;
                }
            }
        }
    }

    
    void printMST_tv(Edge_tv result[], int e) {
        cout << "\n=== Minimum Spanning Tree (Kruskal's Algorithm) ===" << endl;
        cout << "Edge\t\tWeight" << endl;
        cout << "------------------------" << endl;
        
        int totalWeight = 0;
        for (int i = 0; i < e; i++) {
            cout << vertexLabels_tv[result[i].src] << " - " << vertexLabels_tv[result[i].dest] 
                 << "\t\t" << result[i].weight << endl;
            totalWeight += result[i].weight;
        }
        
        cout << "------------------------" << endl;
        cout << "Total Weight: " << totalWeight << endl;
        cout << "==================================================" << endl;
    }

    
    int getDegree_tv(int vertex) {
        if (vertex >= numVertices_tv || vertex < 0) {
            cout << "Invalid vertex!" << endl;
            return -1;
        }
        
        int degree = 0;
        AdjListNode_tv* temp = adjListArray_tv[vertex].head;
        while (temp != nullptr) {
            degree++;
            temp = temp->next;
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
        
        cout << "Number of edges: " << numEdges_tv/2 << endl;  
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
        if (adjListArray_tv != nullptr) {
            for (int i = 0; i < numVertices_tv; i++) {
                AdjListNode_tv* temp = adjListArray_tv[i].head;
                while (temp != nullptr) {
                    AdjListNode_tv* next = temp->next;
                    delete temp;
                    temp = next;
                }
            }
            delete[] adjListArray_tv;
        }
        
        if (edgeArray_tv != nullptr) {
            delete[] edgeArray_tv;
        }
    }
};

int main() {
    Graph_tv graph_tv;
    int choice, vertices, src, dest, weight;

    cout << "=== Kruskal's Algorithm for Minimum Spanning Tree ===" << endl;
    cout << "Using Adjacency List Representation" << endl;

    while (true) {
        cout << "\n1. Create Graph\n2. Input Graph\n3. Add Edge\n4. Display Adjacency List\n5. Find MST (Kruskal's)\n6. Display Vertex Degrees\n7. Display Graph Info\n8. Exit\n";
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
                graph_tv.displayAdjacencyList_tv();
                break;

            case 5:
                graph_tv.kruskalsMST_tv();
                break;

            case 6:
                graph_tv.displayVertexDegrees_tv();
                break;

            case 7:
                graph_tv.displayGraphInfo_tv();
                break;

            case 8:
                cout << "Thank you for using Kruskal's MST Algorithm!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


##  Output

=== Kruskal's Algorithm for Minimum Spanning Tree ===
Using Adjacency List Representation

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find MST (Kruskal's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 1
Enter number of vertices: 5
Graph created with 5 vertices (labeled A, B, C, ...)

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find MST (Kruskal's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 0
Enter destination vertex: 1
Enter weight: 2
Edge added between A and B with weight 2

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find MST (Kruskal's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 0
Enter destination vertex: 3
Enter weight: 6
Edge added between A and D with weight 6

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find MST (Kruskal's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 1
Enter destination vertex: 2
Enter weight: 3
Edge added between B and C with weight 3

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find MST (Kruskal's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 1
Enter destination vertex: 3
Enter weight: 8
Edge added between B and D with weight 8

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find MST (Kruskal's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 1
Enter destination vertex: 4
Enter weight: 5
Edge added between B and E with weight 5

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find MST (Kruskal's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 2
Enter destination vertex: 4
Enter weight: 7
Edge added between C and E with weight 7

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find MST (Kruskal's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 3
Enter destination vertex: 4
Enter weight: 9
Edge added between D and E with weight 9

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find MST (Kruskal's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 4

=== Adjacency List Representation ===
Vertex A: (D, 6) (B, 2) 
Vertex B: (E, 5) (D, 8) (C, 3) (A, 2) 
Vertex C: (E, 7) (B, 3) 
Vertex D: (E, 9) (B, 8) (A, 6) 
Vertex E: (D, 9) (C, 7) (B, 5) 
===================================

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find MST (Kruskal's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 5

=== Minimum Spanning Tree (Kruskal's Algorithm) ===
Edge		Weight
------------------------
A - B		2
B - C		3
B - E		5
A - D		6
------------------------
Total Weight: 16
==================================================

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find MST (Kruskal's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 6

=== Vertex Degrees ===
Vertex A: Degree = 2
Vertex B: Degree = 4
Vertex C: Degree = 2
Vertex D: Degree = 3
Vertex E: Degree = 3
=====================

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find MST (Kruskal's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 7

=== Graph Information ===
Number of vertices: 5
Vertices: A B C D E 
Number of edges: 8
========================

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find MST (Kruskal's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 2
Enter number of vertices: 4
Enter number of edges: 5
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

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find MST (Kruskal's)
6. Display Vertex Degrees
7. Display Graph Info
8. Exit
Enter your choice: 5

=== Minimum Spanning Tree (Kruskal's Algorithm) ===
Edge		Weight
------------------------
C - D		4
A - D		5
A - B		10
------------------------
Total Weight: 19
==================================================
