## Problem Statement
Develop a program to identify and efficiently store a sparse matrix using compact representation and perform basic operations like display and simple transpose.

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
    cout << "Rows: " << sparse.rows << ", Columns: " << sparse.cols << ", Non-zero elements: " << sparse.numElements << endl;
    cout << "Row\tCol\tValue" << endl;
    for(int i = 0; i < sparse.numElements; i++) {
        cout << sparse.elements[i].row << "\t" << sparse.elements[i].col << "\t" << sparse.elements[i].value << endl;
    }
}

void displayNormalMatrix_tv(SparseMatrix_tv sparse) {
    cout << "\nNormal Matrix Representation:" << endl;
    int **matrix = new int*[sparse.rows];
    for(int i = 0; i < sparse.rows; i++) {
        matrix[i] = new int[sparse.cols];
        for(int j = 0; j < sparse.cols; j++) {
            matrix[i][j] = 0;
        }
    }
    
    // Fill non-zero elements
    for(int i = 0; i < sparse.numElements; i++) {
        matrix[sparse.elements[i].row][sparse.elements[i].col] = sparse.elements[i].value;
    }
    
    // Display matrix
    for(int i = 0; i < sparse.rows; i++) {
        for(int j = 0; j < sparse.cols; j++) {
            cout << matrix[i][j] << "\t";
        }
        cout << endl;
    }
    
    // Free memory
    for(int i = 0; i < sparse.rows; i++) {
        delete[] matrix[i];
    }
    delete[] matrix;
}

void transposeSparseMatrix_tv(SparseMatrix_tv sparse, SparseMatrix_tv &transpose) {
    transpose.rows = sparse.cols;
    transpose.cols = sparse.rows;
    transpose.numElements = sparse.numElements;
    
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

int main() {
    SparseMatrix_tv sparseMatrix_tv, transposeMatrix_tv;
    
    cout << "=== Sparse Matrix Implementation  ===" << endl;
    createSparseMatrix_tv(sparseMatrix_tv);
    
    displaySparseMatrix_tv(sparseMatrix_tv);
    displayNormalMatrix_tv(sparseMatrix_tv);
    
    transposeSparseMatrix_tv(sparseMatrix_tv, transposeMatrix_tv);
    cout << "\n=== Transpose of Sparse Matrix ===" << endl;
    displaySparseMatrix_tv(transposeMatrix_tv);
    cout << "\nTranspose Normal Matrix:" << endl;
    displayNormalMatrix_tv(transposeMatrix_tv);
    
    return 0;
}


## Output

=== Sparse Matrix Implementation  ===
Enter number of rows: 4
Enter number of columns: 4
Enter number of non-zero elements: 4
Enter row, column, and value for each non-zero element:
Element 1 (row col value): 0 1 5
Element 2 (row col value): 1 2 8
Element 3 (row col value): 2 3 3
Element 4 (row col value): 3 0 9

Sparse Matrix Representation:
Rows: 4, Columns: 4, Non-zero elements: 4
Row	Col	Value
0	1	5
1	2	8
2	3	3
3	0	9

Normal Matrix Representation:
0	5	0	0
0	0	8	0
0	0	0	3
9	0	0	0

=== Transpose of Sparse Matrix ===
Sparse Matrix Representation:
Rows: 4, Columns: 4, Non-zero elements: 4
Row	Col	Value
0	3	9
1	0	5
2	1	8
3	2	3

Transpose Normal Matrix:
0	0	0	9
5	0	0	0
0	8	0	0
0	0	3	0

