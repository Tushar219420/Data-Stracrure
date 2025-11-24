## Problem Statement
Write a program, using trees, to assign the roll nos. to the students of your class as per their previous years result. i.e topper will be roll no. 1

## Code
```cpp
#include <iostream>
#include <cstring>
using namespace std;

struct Student_tv {
    int rollNumber;
    char name[50];
    float previousYearMarks;
    Student_tv* left;
    Student_tv* right;
    
    Student_tv(int roll, const char* studentName, float marks) {
        rollNumber = roll;
        strcpy(name, studentName);
        previousYearMarks = marks;
        left = nullptr;
        right = nullptr;
    }
};

class StudentRollAssignment_tv {
private:
    Student_tv* root;
    int studentCounter_tv;

    
    Student_tv* insertStudentHelper_tv(Student_tv* node, const char* name, float marks) {
        if (node == nullptr) {
            studentCounter_tv++;
            return new Student_tv(studentCounter_tv, name, marks);
        }
        

        if (marks > node->previousYearMarks) {
            node->left = insertStudentHelper_tv(node->left, name, marks);
        } else {
            node->right = insertStudentHelperHelper_tv(node->right, name, marks);
        }
        
        return node;
    }


    void assignRollNumbersHelper_tv(Student_tv* node, int& rollCounter) {
        if (node != nullptr) {
  
            assignRollNumbersHelper_tv(node->left, rollCounter);

            node->rollNumber = rollCounter;
            rollCounter++;

            assignRollNumbersHelper_tv(node->right, rollCounter);
        }
    }


    void displayByRollNumberHelper_tv(Student_tv* node) {
        if (node != nullptr) {
          
            displayByRollNumberHelper_tv(node->left);
            
            
            cout << node->rollNumber << "\t\t" << node->name << "\t\t" << node->previousYearMarks << endl;
            
            
            displayByRollNumberHelper_tv(node->right);
        }
    }

    void displayByMarksHelper_tv(Student_tv* node) {
        if (node != nullptr) {
           
            displayByMarksHelper_tv(node->right);
            
            
            cout << node->name << "\t\t" << node->previousYearMarks << "\t\t" << node->rollNumber << endl;
            
            displayByMarksHelper_tv(node->left);
        }
    }

    Student_tv* searchByNameHelper_tv(Student_tv* node, const char* name) {
        if (node == nullptr) {
            return nullptr;
        }
        
        if (strcmp(node->name, name) == 0) {
            return node;
        }
        
        Student_tv* found = searchByNameHelper_tv(node->left, name);
        if (found != nullptr) {
            return found;
        }
        
        return searchByNameHelper_tv(node->right, name);
    }

  
    int countStudentsHelper_tv(Student_tv* node) {
        if (node == nullptr) {
            return 0;
        }
        return 1 + countStudentsHelper_tv(node->left) + countStudentsHelper_tv(node->right);
    }

   
    Student_tv* findTopperHelper_tv(Student_tv* node) {
        if (node == nullptr) {
            return nullptr;
        }

        if (node->left == nullptr) {
            return node;
        }
        
      
        return findTopperHelper_tv(node->left);
    }

public:
    StudentRollAssignment_tv() {
        root = nullptr;
        studentCounter_tv = 0;
    }

    void createSystem_tv() {
        root = nullptr;
        studentCounter_tv = 0;
        cout << "Student roll assignment system created successfully!" << endl;
    }

    
    void addStudent_tv(const char* name, float marks) {
        root = insertStudentHelper_tv(root, name, marks);
        cout << "Student " << name << " added with marks " << marks << endl;
    }

    void assignRollNumbers_tv() {
        if (root == nullptr) {
            cout << "No students to assign roll numbers!" << endl;
            return;
        }
        
        int rollCounter = 1;
        assignRollNumbersHelper_tv(root, rollCounter);
        cout << "Roll numbers assigned successfully!" << endl;
    }

 
    void displayByRollNumber_tv() {
        if (root == nullptr) {
            cout << "No students in the system!" << endl;
            return;
        }
        
        cout << "\n=== Students by Roll Number ===" << endl;
        cout << "Roll No.\tName\t\tMarks" << endl;
        cout << "----------------------------------------" << endl;
        displayByRollNumberHelper_tv(root);
        cout << "===============================" << endl;
    }

    void displayByMarks_tv() {
        if (root == nullptr) {
            cout << "No students in the system!" << endl;
            return;
        }
        
        cout << "\n=== Students by Marks (Topper First) ===" << endl;
        cout << "Name\t\tMarks\t\tRoll No." << endl;
        cout << "----------------------------------------" << endl;
        displayByMarksHelper_tv(root);
        cout << "========================================" << endl;
    }

 
    void searchStudentByName_tv(const char* name) {
        if (root == nullptr) {
            cout << "No students in the system!" << endl;
            return;
        }
        
        Student_tv* student = searchByNameHelper_tv(root, name);
        if (student != nullptr) {
            cout << "\nStudent Found:" << endl;
            cout << "Name: " << student->name << endl;
            cout << "Marks: " << student->previousYearMarks << endl;
            cout << "Roll Number: " << student->rollNumber << endl;
        } else {
            cout << "Student " << name << " not found!" << endl;
        }
    }

    void displayTopper_tv() {
        if (root == nullptr) {
            cout << "No students in the system!" << endl;
            return;
        }
        
        Student_tv* topper = findTopperHelper_tv(root);
        if (topper != nullptr) {
            cout << "\n=== Class Topper ===" << endl;
            cout << "Name: " << topper->name << endl;
            cout << "Marks: " << topper->previousYearMarks << endl;
            cout << "Roll Number: " << topper->rollNumber << " (Topper gets roll no. 1)" << endl;
            cout << "===================" << endl;
        }
    }


    int countStudents_tv() {
        int count = countStudentsHelper_tv(root);
        cout << "Total students: " << count << endl;
        return count;
    }

    void displayStatistics_tv() {
        cout << "\n=== System Statistics ===" << endl;
        countStudents_tv();
        displayTopper_tv();
        cout << "========================" << endl;
    }


    void inputStudents_tv() {
        int numStudents;
        char name[50];
        float marks;
        
        cout << "Enter number of students: ";
        cin >> numStudents;
        
        for (int i = 0; i < numStudents; i++) {
            cout << "\nStudent " << (i+1) << ":" << endl;
            cout << "Enter name (Indian name): ";
            cin >> name;
            cout << "Enter previous year marks: ";
            cin >> marks;
            addStudent_tv(name, marks);
        }
    }

    ~StudentRollAssignment_tv() {
        
        root = nullptr;
    }
};

int main() {
    StudentRollAssignment_tv system_tv;
    int choice;
    char name[50];
    float marks;

    cout << "=== Student Roll Number Assignment System ===" << endl;
    cout << "Based on previous year marks (Topper gets roll no. 1)" << endl;

    while (true) {
        cout << "\n1. Create System\n2. Add Student\n3. Input Multiple Students\n4. Assign Roll Numbers\n5. Display by Roll Number\n6. Display by Marks\n7. Search Student by Name\n8. Display Topper\n9. Display Statistics\n10. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                system_tv.createSystem_tv();
                break;

            case 2:
                cout << "Enter student name (Indian name): ";
                cin >> name;
                cout << "Enter previous year marks: ";
                cin >> marks;
                system_tv.addStudent_tv(name, marks);
                break;

            case 3:
                system_tv.inputStudents_tv();
                break;

            case 4:
                system_tv.assignRollNumbers_tv();
                break;

            case 5:
                system_tv.displayByRollNumber_tv();
                break;

            case 6:
                system_tv.displayByMarks_tv();
                break;

            case 7:
                cout << "Enter student name to search: ";
                cin >> name;
                system_tv.searchStudentByName_tv(name);
                break;

            case 8:
                system_tv.displayTopper_tv();
                break;

            case 9:
                system_tv.displayStatistics_tv();
                break;

            case 10:
                cout << "Thank you for using the Student Roll Assignment System!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}

##  Output
=== Student Roll Number Assignment System ===
Based on previous year marks (Topper gets roll no. 1)

1. Create System
2. Add Student
3. Input Multiple Students
4. Assign Roll Numbers
5. Display by Roll Number
6. Display by Marks
7. Search Student by Name
8. Display Topper
9. Display Statistics
10. Exit
Enter your choice: 1
Student roll assignment system created successfully!

1. Create System
2. Add Student
3. Input Multiple Students
4. Assign Roll Numbers
5. Display by Roll Number
6. Display by Marks
7. Search Student by Name
8. Display Topper
9. Display Statistics
10. Exit
Enter your choice: 2
Enter student name (Indian name): Arjun
Enter previous year marks: 85.5
Student Arjun added with marks 85.5

1. Create System
2. Add Student
3. Input Multiple Students
4. Assign Roll Numbers
5. Display by Roll Number
6. Display by Marks
7. Search Student by Name
8. Display Topper
9. Display Statistics
10. Exit
Enter your choice: 2
Enter student name (Indian name): Priya
Enter previous year marks: 92.0
Student Priya added with marks 92.0

1. Create System
2. Add Student
3. Input Multiple Students
4. Assign Roll Numbers
5. Display by Roll Number
6. Display by Marks
7. Search Student by Name
8. Display Topper
9. Display Statistics
10. Exit
Enter your choice: 2
Enter student name (Indian name): Rohit
Enter previous year marks: 78.5
Student Rohit added with marks 78.5

1. Create System
2. Add Student
3. Input Multiple Students
4. Assign Roll Numbers
5. Display by Roll Number
6. Display by Marks
7. Search Student by Name
8. Display Topper
9. Display Statistics
10. Exit
Enter your choice: 2
Enter student name (Indian name): Sneha
Enter previous year marks: 88.0
Student Sneha added with marks 88.0

1. Create System
2. Add Student
3. Input Multiple Students
4. Assign Roll Numbers
5. Display by Roll Number
6. Display by Marks
7. Search Student by Name
8. Display Topper
9. Display Statistics
10. Exit
Enter your choice: 2
Enter student name (Indian name): Vikram
Enter previous year marks: 91.5
Student Vikram added with marks 91.5

1. Create System
2. Add Student
3. Input Multiple Students
4. Assign Roll Numbers
5. Display by Roll Number
6. Display by Marks
7. Search Student by Name
8. Display Topper
9. Display Statistics
10. Exit
Enter your choice: 4
Roll numbers assigned successfully!

1. Create System
2. Add Student
3. Input Multiple Students
4. Assign Roll Numbers
5. Display by Roll Number
6. Display by Marks
7. Search Student by Name
8. Display Topper
9. Display Statistics
10. Exit
Enter your choice: 5

=== Students by Roll Number ===
Roll No.	Name		Marks
----------------------------------------
1		Priya		92
2		Vikram		91.5
3		Sneha		88
4		Arjun		85.5
5		Rohit		78.5
===============================

1. Create System
2. Add Student
3. Input Multiple Students
4. Assign Roll Numbers
5. Display by Roll Number
6. Display by Marks
7. Search Student by Name
8. Display Topper
9. Display Statistics
10. Exit
Enter your choice: 6

=== Students by Marks (Topper First) ===
Name		Marks		Roll No.
----------------------------------------
Priya		92		1
Vikram		91.5		2
Sneha		88		3
Arjun		85.5		4
Rohit		78.5		5
========================================

1. Create System
2. Add Student
3. Input Multiple Students
4. Assign Roll Numbers
5. Display by Roll Number
6. Display by Marks
7. Search Student by Name
8. Display Topper
9. Display Statistics
10. Exit
Enter your choice: 8

=== Class Topper ===
Name: Priya
Marks: 92
Roll Number: 1 (Topper gets roll no. 1)
===================

1. Create System
2. Add Student
3. Input Multiple Students
4. Assign Roll Numbers
5. Display by Roll Number
6. Display by Marks
7. Search Student by Name
8. Display Topper
9. Display Statistics
10. Exit
Enter your choice: 7
Enter student name to search: Sneha

Student Found:
Name: Sneha
Marks: 88
Roll Number: 3

1. Create System
2. Add Student
3. Input Multiple Students
4. Assign Roll Numbers
5. Display by Roll Number
6. Display by Marks
7. Search Student by Name
8. Display Topper
9. Display Statistics
10. Exit
Enter your choice: 9

=== System Statistics ===
Total students: 5

=== Class Topper ===
Name: Priya
Marks: 92
Roll Number: 1 (Topper gets roll no. 1)
========================

