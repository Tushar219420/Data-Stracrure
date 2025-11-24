## Problem Statement
In Computer Engg. Dept. of VIT there are S.Y., T.Y., and B.Tech. students. Assume that all these students are on ground for a function. We need to identify a student of S.Y. div. (X) whose name is "XYZ" and roll no. is "17". Apply appropriate Searching method to identify the required student.

##  Code
```cpp
#include <iostream>
#include <cstring>
using namespace std;

struct Student_tv {
    char name[50];
    int rollNo;
    char division;
    char year[10];
};

void linearSearch_tv(Student_tv students[], int n, const char* targetName, int targetRollNo) {
    bool found_tv = false;
    cout << "\nSearching for student: " << targetName << " (Roll No: " << targetRollNo << ")" << endl;
    
    for(int i = 0; i < n; i++) {
        if(strcmp(students[i].name, targetName) == 0 && students[i].rollNo == targetRollNo) {
            cout << "Student Found!" << endl;
            cout << "Name: " << students[i].name << endl;
            cout << "Roll No: " << students[i].rollNo << endl;
            cout << "Division: " << students[i].division << endl;
            cout << "Year: " << students[i].year << endl;
            found_tv = true;
            break;
        }
    }
    
    if(!found_tv) {
        cout << "Student not found in the database." << endl;
    }
}

void binarySearch_tv(Student_tv students[], int n, int targetRollNo) {
    
    for(int i = 0; i < n-1; i++) {
        for(int j = 0; j < n-i-1; j++) {
            if(students[j].rollNo > students[j+1].rollNo) {
                Student_tv temp = students[j];
                students[j] = students[j+1];
                students[j+1] = temp;
            }
        }
    }
    
    int left = 0, right = n-1;
    bool found_tv = false;
    
    cout << "\nPerforming Binary Search for Roll No: " << targetRollNo << endl;
    
    while(left <= right) {
        int mid = (left + right) / 2;
        
        if(students[mid].rollNo == targetRollNo) {
            cout << "Student Found!" << endl;
            cout << "Name: " << students[mid].name << endl;
            cout << "Roll No: " << students[mid].rollNo << endl;
            cout << "Division: " << students[mid].division << endl;
            cout << "Year: " << students[mid].year << endl;
            found_tv = true;
            break;
        } else if(students[mid].rollNo < targetRollNo) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    if(!found_tv) {
        cout << "Student with Roll No " << targetRollNo << " not found." << endl;
    }
}

int main() {
    const int MAX_STUDENTS_TV = 100;
    Student_tv students_tv[MAX_STUDENTS_TV];
    int n;
    
    cout << "=== Student Search System (VIT Computer Engineering Dept.) ===" << endl;
    cout << "Enter number of students present at the function: ";
    cin >> n;
    
    
    for(int i = 0; i < n; i++) {
        cout << "\nStudent " << (i+1) << " Details:" << endl;
        cout << "Name (Indian name): ";
        cin >> students_tv[i].name;
        cout << "Roll No: ";
        cin >> students_tv[i].rollNo;
        cout << "Division (e.g., X, Y, Z): ";
        cin >> students_tv[i].division;
        cout << "Year (SY/TY/BTech): ";
        cin >> students_tv[i].year;
    }
    
    
    char searchName_tv[50];
    int searchRollNo_tv;
    cout << "\nEnter name of student to search (e.g., XYZ): ";
    cin >> searchName_tv;
    cout << "Enter roll number of student to search (e.g., 17): ";
    cin >> searchRollNo_tv;
    
    linearSearch_tv(students_tv, n, searchName_tv, searchRollNo_tv);
    

    int rollToSearch_tv;
    cout << "\nEnter roll number to search using Binary Search: ";
    cin >> rollToSearch_tv;
    binarySearch_tv(students_tv, n, rollToSearch_tv);
    
    return 0;
}


## Output

=== Student Search System (VIT Computer Engineering Dept.) ===
Enter number of students present at the function: 3

Student 1 Details:
Name (Indian name): Rajesh
Roll No: 10
Division (e.g., X, Y, Z): X
Year (SY/TY/BTech): SY

Student 2 Details:
Name (Indian name): XYZ
Roll No: 17
Division (e.g., X, Y, Z): X
Year (SY/TY/BTech): SY

Student 3 Details:
Name (Indian name): Priya
Roll No: 25
Division (e.g., X, Y, Z): Y
Year (SY/TY/BTech): TY

Enter name of student to search (e.g., XYZ): XYZ
Enter roll number of student to search (e.g., 17): 17

Searching for student: XYZ (Roll No: 17)
Student Found!
Name: XYZ
Roll No: 17
Division: X
Year: SY

Enter roll number to search using Binary Search: 25

Performing Binary Search for Roll No: 25
Student Found!
Name: Priya
Roll No: 25
Division: Y
Year: TY
