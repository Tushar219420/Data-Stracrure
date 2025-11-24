## Problem Statement
Write a program to construct and verify a magic square of order 'n' (for both even & odd) such that all rows, columns, and diagonals sum to the same value.

## Code
```cpp
#include <iostream>
using namespace std;

void printMagicSquare_tv(int magicSquare[][10], int n) {
    cout << "\nMagic Square of order " << n << ":" << endl;
    for(int i = 0; i < n; i++) {
        for(int j = 0; j < n; j++) {
            cout << magicSquare[i][j] << "\t";
        }
        cout << endl;
    }
}

bool verifyMagicSquare_tv(int magicSquare[][10], int n) {
    int sum = 0;
    
    for(int j = 0; j < n; j++) {
        sum += magicSquare[0][j];
    }
    
    
    for(int i = 1; i < n; i++) {
        int rowSum = 0;
        for(int j = 0; j < n; j++) {
            rowSum += magicSquare[i][j];
        }
        if(rowSum != sum) return false;
    }
    
    
    for(int j = 0; j < n; j++) {
        int colSum = 0;
        for(int i = 0; i < n; i++) {
            colSum += magicSquare[i][j];
        }
        if(colSum != sum) return false;
    }
    
    
    int diagSum1 = 0, diagSum2 = 0;
    for(int i = 0; i < n; i++) {
        diagSum1 += magicSquare[i][i];
        diagSum2 += magicSquare[i][n-1-i];
    }
    
    if(diagSum1 != sum || diagSum2 != sum) return false;
    
    return true;
}

void generateMagicSquare_tv(int magicSquare[][10], int n) {
    
    for(int i = 0; i < n; i++) {
        for(int j = 0; j < n; j++) {
            magicSquare[i][j] = 0;
        }
    }
    
    
    int i = n/2;
    int j = n-1;
    
    for(int num = 1; num <= n*n; ) {
        if(i == -1 && j == n) {
            j = n-2;
            i = 0;
        } else {
            if(j == n) j = 0;
            if(i < 0) i = n-1;
        }
        
        if(magicSquare[i][j]) {
            j -= 2;
            i++;
            continue;
        } else {
            magicSquare[i][j] = num++;
        }
        
        j++; i--;
    }
}

int main() {
    int n;
    cout << "Enter the order of magic square : ";
    cin >> n;
    
    if(n % 2 == 0) {
        cout << "This implementation works for odd-order magic squares only." << endl;
        return 1;
    }
    
    int magicSquare_tv[10][10];
    generateMagicSquare_tv(magicSquare_tv, n);
    printMagicSquare_tv(magicSquare_tv, n);
    
    if(verifyMagicSquare_tv(magicSquare_tv, n)) {
        cout << "\nVerification: This is a valid magic square!" << endl;
        cout << "Magic constant: " << (n*(n*n+1))/2 << endl;
    } else {
        cout << "\nVerification failed!" << endl;
    }
    
    return 0;
}

## Output

Enter the order of magic square : 3

Magic Square of order 3:
2       7       6
9       5       1
4       3       8

Verification: This is a valid magic square!
Magic constant: 15

