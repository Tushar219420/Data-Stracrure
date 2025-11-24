## Problem Statement
Write a program using Bubble sort algorithm, assign the roll nos. to the students of your class as per their previous years result. i.e. topper will be roll no. 1 and analyse the sorting algorithm pass by pass.

##  Code
#include <iostream>
#include <cstring>
using namespace std;

struct Student_tv {
    char name[50];
    float previousYearMarks;
    int assignedRollNo;
};

void displayStudents_tv(Student_tv students[], int n, const char* message) {
    cout << "\n" << message << endl;
    cout << "Name\t\tPrev Marks\tAssigned Roll No" << endl;
    cout << "------------------------------------------------" << endl;
    for(int i = 0; i < n; i++) {
        cout << students[i].name << "\t\t" << students[i].previousYearMarks << "\t\t" << students[i].assignedRollNo << endl;
    }
}

void bubbleSort_tv(Student_tv students[], int n) {
    cout << "\n=== Bubble Sort Pass-by-Pass Analysis ===" << endl;
    
    for(int i = 0; i < n-1; i++) {
        bool swapped_tv = false;
        cout << "\nPass " << (i+1) << ":" << endl;
        
        for(int j = 0; j < n-i-1; j++) {
            if(students[j].previousYearMarks < students[j+1].previousYearMarks) {
                
                Student_tv temp = students[j];
                students[j] = students[j+1];
                students[j+1] = temp;
                swapped_tv = true;
                
                cout << "Swapped " << students[j+1].name << " (" << students[j+1].previousYearMarks 
                     << ") with " << students[j].name << " (" << students[j].previousYearMarks << ")" << endl;
            }
        }
        
       
        cout << "Array after pass " << (i+1) << ": ";
        for(int k = 0; k < n; k++) {
            cout << students[k].name << "(" << students[k].previousYearMarks << ") ";
        }
        cout << endl;
        
        
        if(!swapped_tv) {
            cout << "No swaps needed. Array is sorted." << endl;
            break;
        }
    }
}

void assignRollNumbers_tv(Student_tv students[], int n) {
    for(int i = 0; i < n; i++) {
        students[i].assignedRollNo = i + 1;
    }
}

int main() {
    const int MAX_STUDENTS_TV = 30;
    Student_tv students_tv[MAX_STUDENTS_TV];
    int n;
    
    cout << "=== Roll Number Assignment Based on Previous Year Results ===" << endl;
    cout << "Enter number of students in class: ";
    cin >> n;
    
    if(n > MAX_STUDENTS_TV) {
        cout << "Too many students. Maximum allowed: " << MAX_STUDENTS_TV << endl;
        return 1;
    }
    
  
    cout << "\nEnter student details:" << endl;
    for(int i = 0; i < n; i++) {
        cout << "\nStudent " << (i+1) << ":" << endl;
        cout << "Name (Indian name): ";
        cin >> students_tv[i].name;
        cout << "Previous year marks: ";
        cin >> students_tv[i].previousYearMarks;
        students_tv[i].assignedRollNo = 0; 
    }
    
    cout << "\nInitial Student Records:";
    displayStudents_tv(students_tv, n, "Before sorting and roll number assignment");
    
    
    bubbleSort_tv(students_tv, n);
    

    assignRollNumbers_tv(students_tv, n);
    
    cout << "\nFinal Student Records:";
    displayStudents_tv(students_tv, n, "After sorting and roll number assignment");
    
    cout << "\n=== Summary ===" << endl;
    cout << "Topper: " << students_tv[0].name << " with " << students_tv[0].previousYearMarks << " marks (Roll No: 1)" << endl;
    cout << "Lowest: " << students_tv[n-1].name << " with " << students_tv[n-1].previousYearMarks << " marks (Roll No: " << n << ")" << endl;
    
    return 0;
}


## Output

=== Roll Number Assignment Based on Previous Year Results ===
Enter number of students in class: 5

Enter student details:

Student 1:
Name : Arjun
Previous year marks: 85.5

Student 2:
Name : Priya
Previous year marks: 92.0

Student 3:
Name : Rohit
Previous year marks: 78.5

Student 4:
Name : Sneha
Previous year marks: 88.0

Student 5:
Name : Vikram
Previous year marks: 91.5

Initial Student Records:
Before sorting and roll number assignment
Name		Prev Marks	Assigned Roll No
------------------------------------------------
Arjun		85.5		0
Priya		92		    0
Rohit		78.5		0
Sneha		88		    0
Vikram		91.5		0

=== Bubble Sort Pass-by-Pass Analysis ===

Pass 1:
Swapped Arjun (85.5) with Priya (92)
Swapped Rohit (78.5) with Sneha (88)
Swapped Rohit (78.5) with Vikram (91.5)
Array after pass 1: Priya(92) Arjun(85.5) Sneha(88) Vikram(91.5) Rohit(78.5) 

Pass 2:
Swapped Arjun (85.5) with Sneha (88)
Swapped Arjun (85.5) with Vikram (91.5)
Array after pass 2: Priya(92) Sneha(88) Vikram(91.5) Arjun(85.5) Rohit(78.5) 

Pass 3:
Swapped Arjun (85.5) with Vikram (91.5)
Array after pass 3: Priya(92) Sneha(88) Arjun(85.5) Vikram(91.5) Rohit(78.5) 

Pass 4:
Swapped Rohit (78.5) with Vikram (91.5)
Array after pass 4: Priya(92) Sneha(88) Arjun(85.5) Vikram(91.5) Rohit(78.5) 

Final Student Records:
After sorting and roll number assignment
Name		Prev Marks	Assigned Roll No
------------------------------------------------
Priya		92		    1
Sneha		88		    2
Arjun		85.5		3
Vikram		91.5		4
Rohit		78.5		5

=== Summary ===
Topper: Priya with 92 marks (Roll No: 1)
Lowest: Rohit with 78.5 marks (Roll No: 5)

