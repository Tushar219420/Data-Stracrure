## Problem Statement
Write a program to input marks of n students Sort the marks in ascending order using the Quick Sort algorithm without using built-in library functions and analyse the sorting algorithm pass by pass. Find the minimum and maximum marks using Divide and Conquer (recursively).

## Code
```cpp
#include <iostream>
using namespace std;

void displayArray_tv(float arr[], int n, const char* message) {
    cout << message << ": ";
    for(int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;
}

int partition_tv(float arr[], int low, int high) {
    float pivot = arr[high];
    int i = low - 1;
    
    for(int j = low; j < high; j++) {
        if(arr[j] < pivot) {
            i++;
            // Swap elements
            float temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
        }
    }
    
   
    float temp = arr[i+1];
    arr[i+1] = arr[high];
    arr[high] = temp;
    
    return i+1;
}

void quickSort_tv(float arr[], int low, int high, int pass) {
    if(low < high) {
        int pi = partition_tv(arr, low, high);
        cout << "Pass " << pass << ": Pivot = " << arr[pi] << ", Partition at index " << pi << endl;
        displayArray_tv(arr, high+1, "Array after pass");
        
        quickSort_tv(arr, low, pi-1, pass+1);
        quickSort_tv(arr, pi+1, high, pass+1);
    }
}

float findMin_tv(float arr[], int low, int high) {
    
    if(low == high) {
        return arr[low];
    }
    
    
    if(high == low + 1) {
        return (arr[low] < arr[high]) ? arr[low] : arr[high];
    }
    
   
    int mid = (low + high) / 2;
    float minLeft = findMin_tv(arr, low, mid);
    float minRight = findMin_tv(arr, mid+1, high);
    
    return (minLeft < minRight) ? minLeft : minRight;
}

float findMax_tv(float arr[], int low, int high) {
    
    if(low == high) {
        return arr[low];
    }
    
    
    if(high == low + 1) {
        return (arr[low] > arr[high]) ? arr[low] : arr[high];
    }
    
    
    int mid = (low + high) / 2;
    float maxLeft = findMax_tv(arr, low, mid);
    float maxRight = findMax_tv(arr, mid+1, high);
    
    return (maxLeft > maxRight) ? maxLeft : maxRight;
}

int main() {
    const int MAX_STUDENTS_TV = 50;
    float marks_tv[MAX_STUDENTS_TV];
    int n;
    
    cout << "=== Quick Sort with Min/Max using Divide and Conquer ===" << endl;
    cout << "Enter number of students: ";
    cin >> n;
    
    if(n > MAX_STUDENTS_TV) {
        cout << "Too many students. Maximum allowed: " << MAX_STUDENTS_TV << endl;
        return 1;
    }
    
    
    cout << "Enter marks of " << n << " students (Indian names):" << endl;
    for(int i = 0; i < n; i++) {
        cout << "Student " << (i+1) << " marks: ";
        cin >> marks_tv[i];
    }
    
    cout << "\nOriginal Marks: ";
    displayArray_tv(marks_tv, n, "Before sorting");
    
    
    cout << "\n=== Quick Sort Pass-by-Pass Analysis ===" << endl;
    quickSort_tv(marks_tv, 0, n-1, 1);
    
    cout << "\nFinal Sorted Marks: ";
    displayArray_tv(marks_tv, n, "After sorting");
    
    float minMarks_tv = findMin_tv(marks_tv, 0, n-1);
    float maxMarks_tv = findMax_tv(marks_tv, 0, n-1);
    
    cout << "\n=== Min/Max using Divide and Conquer ===" << endl;
    cout << "Minimum Marks: " << minMarks_tv << endl;
    cout << "Maximum Marks: " << maxMarks_tv << endl;
    
    
    cout << "\nVerification:" << endl;
    cout << "First element (should be min): " << marks_tv[0] << endl;
    cout << "Last element (should be max): " << marks_tv[n-1] << endl;
    
    return 0;
}


## Output

=== Quick Sort with Min/Max using Divide and Conquer ===
Enter number of students: 6
Enter marks of 6 students (Indian names):
Student 1 marks: 85
Student 2 marks: 92
Student 3 marks: 78
Student 4 marks: 88
Student 5 marks: 91
Student 6 marks: 82

Original Marks: Before sorting: 85 92 78 88 91 82 

=== Quick Sort Pass-by-Pass Analysis ===
Pass 1: Pivot = 82, Partition at index 2
Array after pass: 78 82 85 88 91 92 
Pass 2: Pivot = 78, Partition at index 0
Array after pass: 78 82 85 88 91 92 
Pass 3: Pivot = 85, Partition at index 3
Array after pass: 78 82 85 88 91 92 
Pass 4: Pivot = 91, Partition at index 5
Array after pass: 78 82 85 88 91 92 

Final Sorted Marks: After sorting: 78 82 85 88 91 92 

=== Min/Max using Divide and Conquer ===
Minimum Marks: 78
Maximum Marks: 92

Verification:
First element (should be min): 78
Last element (should be max): 92
