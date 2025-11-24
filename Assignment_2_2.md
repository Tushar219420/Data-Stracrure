## Problem Statement
WAP to implement Bubble sort and Quick Sort on a 1D array of Student structure (contains student_name, student_roll_no, total_marks), with key as student_roll_no. And count the number of swap performed by each method.

##  Code
```cpp
#include <iostream>
#include <cstring>
using namespace std;

struct Student_tv {
    char name[50];
    int rollNo;
    float marks;
};

void displayStudents_tv(Student_tv students[], int n) {
    cout << "\nStudent Records:" << endl;
    cout << "Name\t\tRoll No\tMarks" << endl;
    cout << "--------------------------------" << endl;
    for(int i = 0; i < n; i++) {
        cout << students[i].name << "\t\t" << students[i].rollNo << "\t" << students[i].marks << endl;
    }
}

int bubbleSort_tv(Student_tv students[], int n) {
    int swapCount_tv = 0;
    
    for(int i = 0; i < n-1; i++) {
        bool swapped_tv = false;
        for(int j = 0; j < n-i-1; j++) {
            if(students[j].rollNo > students[j+1].rollNo) {
                
                Student_tv temp = students[j];
                students[j] = students[j+1];
                students[j+1] = temp;
                swapCount_tv++;
                swapped_tv = true;
            }
        }
        
        if(!swapped_tv) break;
    }
    
    return swapCount_tv;
}

int partition_tv(Student_tv students[], int low, int high, int &swapCount_tv) {
    int pivot = students[high].rollNo;
    int i = low - 1;
    
    for(int j = low; j < high; j++) {
        if(students[j].rollNo < pivot) {
            i++;
            // Swap students
            Student_tv temp = students[i];
            students[i] = students[j];
            students[j] = temp;
            swapCount_tv++;
        }
    }
    
    
    Student_tv temp = students[i+1];
    students[i+1] = students[high];
    students[high] = temp;
    swapCount_tv++;
    
    return i+1;
}

void quickSort_tv(Student_tv students[], int low, int high, int &swapCount_tv) {
    if(low < high) {
        int pi = partition_tv(students, low, high, swapCount_tv);
        quickSort_tv(students, low, pi-1, swapCount_tv);
        quickSort_tv(students, pi+1, high, swapCount_tv);
    }
}

int main() {
    const int MAX_STUDENTS_TV = 20;
    Student_tv studentsBubble_tv[MAX_STUDENTS_TV], studentsQuick_tv[MAX_STUDENTS_TV];
    int n;
    
    cout << "=== Sorting Comparison: Bubble Sort vs Quick Sort ===" << endl;
    cout << "Enter number of students: ";
    cin >> n;
    
    if(n > MAX_STUDENTS_TV) {
        cout << "Too many students. Maximum allowed: " << MAX_STUDENTS_TV << endl;
        return 1;
    }
    
    
    for(int i = 0; i < n; i++) {
        cout << "\nStudent " << (i+1) << " Details:" << endl;
        cout << "Name (Indian name): ";
        cin >> studentsBubble_tv[i].name;
        cout << "Roll No: ";
        cin >> studentsBubble_tv[i].rollNo;
        cout << "Total Marks: ";
        cin >> studentsBubble_tv[i].marks;
        
        
        strcpy(studentsQuick_tv[i].name, studentsBubble_tv[i].name);
        studentsQuick_tv[i].rollNo = studentsBubble_tv[i].rollNo;
        studentsQuick_tv[i].marks = studentsBubble_tv[i].marks;
    }
    
    cout << "\nOriginal Student Records:";
    displayStudents_tv(studentsBubble_tv, n);
    
    
    int bubbleSwaps_tv = bubbleSort_tv(studentsBubble_tv, n);
    cout << "\nAfter Bubble Sort:";
    displayStudents_tv(studentsBubble_tv, n);
    cout << "Number of swaps in Bubble Sort: " << bubbleSwaps_tv << endl;
    
    
    int quickSwaps_tv = 0;
    quickSort_tv(studentsQuick_tv, 0, n-1, quickSwaps_tv);
    cout << "\nAfter Quick Sort:";
    displayStudents_tv(studentsQuick_tv, n);
    cout << "Number of swaps in Quick Sort: " << quickSwaps_tv << endl;
    
    cout << "\nComparison Results:" << endl;
    cout << "Bubble Sort Swaps: " << bubbleSwaps_tv << endl;
    cout << "Quick Sort Swaps: " << quickSwaps_tv << endl;
    
    if(bubbleSwaps_tv > quickSwaps_tv) {
        cout << "Quick Sort is more efficient with " << (bubbleSwaps_tv - quickSwaps_tv) << " fewer swaps!" << endl;
    } else if(bubbleSwaps_tv < quickSwaps_tv) {
        cout << "Bubble Sort is more efficient with " << (quickSwaps_tv - bubbleSwaps_tv) << " fewer swaps!" << endl;
    } else {
        cout << "Both algorithms performed the same number of swaps." << endl;
    }
    
    return 0;
}

##  Output

=== Sorting Comparison: Bubble Sort vs Quick Sort ===
Enter number of students: 5

Student 1 Details:
Name (Indian name): Arjun
Roll No: 25
Total Marks: 85.5

Student 2 Details:
Name (Indian name): Priya
Roll No: 12
Total Marks: 92.0

Student 3 Details:
Name (Indian name): Rohit
Roll No: 30
Total Marks: 78.5

Student 4 Details:
Name (Indian name): Sneha
Roll No: 8
Total Marks: 88.0

Student 5 Details:
Name (Indian name): Vikram
Roll No: 17
Total Marks: 91.5

Original Student Records:
Student Records:
Name		Roll No	Marks
--------------------------------
Arjun		25	85.5
Priya		12	92
Rohit		30	78.5
Sneha		8	88
Vikram		17	91.5

After Bubble Sort:
Student Records:
Name		Roll No	Marks
--------------------------------
Sneha		8	88
Priya		12	92
Vikram		17	91.5
Arjun		25	85.5
Rohit		30	78.5
Number of swaps in Bubble Sort: 7

After Quick Sort:
Student Records:
Name		Roll No	Marks
--------------------------------
Sneha		8	88
Priya		12	92
Vikram		17	91.5
Arjun		25	85.5
Rohit		30	78.5
Number of swaps in Quick Sort: 4

Comparison Results:
Bubble Sort Swaps: 7
Quick Sort Swaps: 4
Quick Sort is more efficient with 3 fewer swaps!
