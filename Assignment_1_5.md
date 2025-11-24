## Problem Statement
Develop a program to compute the fast transpose of a sparse matrix using its compact (triplet) representation efficiently.

##  Code
#include <iostream>
using namespace std;

struct Element_tv {
    int row, col, value;
};

struct SparseMatrix_tv {
    int rows, cols, numElements;
    Element_tv elements[100];
};

void createSparseMatrix_tv(SparseMatrix_tv &sparse) {
    cout << "Enter number of rows: ";
    cin >> sparse.rows;
    cout << "Enter number of columns: ";
    cin >> sparse.cols;
    
    cout << "Enter number of non-zero elements: ";
    cin >> sparse.numElements;
    
    cout << "Enter row, column, and value for each non-zero element:" << endl;
    for(int i = 0; i < sparse.numElements; i++) {
        cout << "Element " << (i+1) << " (row col value): ";
        cin >> sparse.elements[i].row >> sparse.elements[i].col >> sparse.elements[i].value;
    }
}

void displaySparseMatrix_tv(SparseMatrix_tv sparse) {
    cout << "\nSparse Matrix Representation:" << endl;
    cout << "Row\tCol\tValue" << endl;
    for(int i = 0; i < sparse.numElements; i++) {
        cout << sparse.elements[i].row << "\t" << sparse.elements[i].col << "\t" << sparse.elements[i].value << endl;
    }
}

void simpleTranspose_tv(SparseMatrix_tv sparse, SparseMatrix_tv &transpose) {
    transpose.rows = sparse.cols;
    transpose.cols = sparse.rows;
    transpose.numElements = sparse.numElements;
    
    if(sparse.numElements == 0) return;
    
    int index = 0;
    for(int col = 0; col < sparse.cols; col++) {
        for(int i = 0; i < sparse.numElements; i++) {
            if(sparse.elements[i].col == col) {
                transpose.elements[index].row = sparse.elements[i].col;
                transpose.elements[index].col = sparse.elements[i].row;
                transpose.elements[index].value = sparse.elements[i].value;
                index++;
            }
        }
    }
}

void fastTranspose_tv(SparseMatrix_tv sparse, SparseMatrix_tv &transpose) {
    transpose.rows = sparse.cols;
    transpose.cols = sparse.rows;
    transpose.numElements = sparse.numElements;
    
    if(sparse.numElements == 0) return;
    
    int *rowStart_tv = new int[sparse.cols];
    int *rowTerms_tv = new int[sparse.cols];
    
    
    for(int i = 0; i < sparse.cols; i++) {
        rowTerms_tv[i] = 0;
    }
    
    
    for(int i = 0; i < sparse.numElements; i++) {
        rowTerms_tv[sparse.elements[i].col]++;
    }
    
   
    rowStart_tv[0] = 0;
    for(int i = 1; i < sparse.cols; i++) {
        rowStart_tv[i] = rowStart_tv[i-1] + rowTerms_tv[i-1];
    }
    
   
    for(int i = 0; i < sparse.numElements; i++) {
        int col = sparse.elements[i].col;
        int index = rowStart_tv[col];
        transpose.elements[index].row = sparse.elements[i].col;
        transpose.elements[index].col = sparse.elements[i].row;
        transpose.elements[index].value = sparse.elements[i].value;
        rowStart_tv[col]++;
    }
    
    delete[] rowStart_tv;
    delete[] rowTerms_tv;
}

int main() {
    SparseMatrix_tv sparseMatrix_tv, simpleTranspose_tv, fastTranspose_tv;
    
    cout << "=== Fast Transpose of Sparse Matrix (Indian Railway Timings) ===" << endl;
    createSparseMatrix_tv(sparseMatrix_tv);
    
    cout << "\nOriginal Sparse Matrix:" << endl;
    displaySparseMatrix_tv(sparseMatrix_tv);
    
    
    simpleTranspose_tv(sparseMatrix_tv, simpleTranspose_tv);
    cout << "\nSimple Transpose:" << endl;
    displaySparseMatrix_tv(simpleTranspose_tv);
    
   
    fastTranspose_tv(sparseMatrix_tv, fastTranspose_tv);
    cout << "\nFast Transpose:" << endl;
    displaySparseMatrix_tv(fastTranspose_tv);
    
    cout << "\nComparison:" << endl;
    cout << "Simple transpose time complexity: O(n*m)" << endl;
    cout << "Fast transpose time complexity: O(n + m + numElements)" << endl;
    cout << "Fast transpose is more efficient for large sparse matrices!" << endl;
    
    return 0;
}

##  Output

=== Fast Transpose of Sparse Matrix (Indian Railway Timings) ===
Enter number of rows: 4
Enter number of columns: 4
Enter number of non-zero elements: 4
Enter row, column, and value for each non-zero element:
Element 1 (row col value): 0 1 15
Element 2 (row col value): 1 2 30
Element 3 (row col value): 2 3 45
Element 4 (row col value): 3 0 60

Original Sparse Matrix:
Row	Col	Value
0	1	15
1	2	30
2	3	45
3	0	60

Simple Transpose:
Row	Col	Value
0	3	60
1	0	15
2	1	30
3	2	45

Fast Transpose:
Row	Col	Value
0	3	60
1	0	15
2	1	30
3	2	45

Comparison:
Simple transpose time complexity: O(n*m)
Fast transpose time complexity: O(n + m + numElements)

Fast transpose is more efficient for large sparse matrices!
