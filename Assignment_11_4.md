
## Problem Statement
Store and retrieve student records using roll numbers.

##  Code
```cpp
#include <iostream>
#include <cstring>
using namespace std;

const int MAX_STUDENTS_TVV = 100;

struct Student_TVV {
    int rollNo_TVV;
    char name_TVV[50];
    char branch_TVV[30];
    float marks_TVV;
};

class StudentRecord_TVV {
private:
    Student_TVV students_TVV[MAX_STUDENTS_TVV];
    int count_TVV;

public:
    StudentRecord_TVV() {
        count_TVV = 0;
    }

    void addStudent_TVV(int rollNo, const char* name, const char* branch, float marks) {
        if (count_TVV >= MAX_STUDENTS_TVV) {
            cout << "Student record limit reached!" << endl;
            return;
        }

        students_TVV[count_TVV].rollNo_TVV = rollNo;
        strcpy(students_TVV[count_TVV].name_TVV, name);
        strcpy(students_TVV[count_TVV].branch_TVV, branch);
        students_TVV[count_TVV].marks_TVV = marks;
        count_TVV++;

        cout << "Student record added successfully!" << endl;
    }

    void searchStudent_TVV(int rollNo) {
        for (int i = 0; i < count_TVV; i++) {
            if (students_TVV[i].rollNo_TVV == rollNo) {
                cout << "\nStudent Record Found:" << endl;
                cout << "Roll No: " << students_TVV[i].rollNo_TVV << endl;
                cout << "Name: " << students_TVV[i].name_TVV << endl;
                cout << "Branch: " << students_TVV[i].branch_TVV << endl;
                cout << "Marks: " << students_TVV[i].marks_TVV << endl;
                return;
            }
        }
        cout << "Student with roll number " << rollNo << " not found!" << endl;
    }

    void updateStudent_TVV(int rollNo, const char* name, const char* branch, float marks) {
        for (int i = 0; i < count_TVV; i++) {
            if (students_TVV[i].rollNo_TVV == rollNo) {
                strcpy(students_TVV[i].name_TVV, name);
                strcpy(students_TVV[i].branch_TVV, branch);
                students_TVV[i].marks_TVV = marks;
                cout << "Student record updated successfully!" << endl;
                return;
            }
        }
        cout << "Student with roll number " << rollNo << " not found!" << endl;
    }

    void deleteStudent_TVV(int rollNo) {
        for (int i = 0; i < count_TVV; i++) {
            if (students_TVV[i].rollNo_TVV == rollNo) {
                for (int j = i; j < count_TVV - 1; j++) {
                    students_TVV[j] = students_TVV[j + 1];
                }
                count_TVV--;
                cout << "Student record deleted successfully!" << endl;
                return;
            }
        }
        cout << "Student with roll number " << rollNo << " not found!" << endl;
    }

    void displayAllStudents_TVV() {
        if (count_TVV == 0) {
            cout << "No student records found!" << endl;
            return;
        }

        cout << "\n=== All Student Records ===" << endl;
        cout << "Roll No\tName\t\tBranch\t\tMarks" << endl;
        cout << "----------------------------------------" << endl;
        for (int i = 0; i < count_TVV; i++) {
            cout << students_TVV[i].rollNo_TVV << "\t" 
                 << students_TVV[i].name_TVV << "\t\t"
                 << students_TVV[i].branch_TVV << "\t\t"
                 << students_TVV[i].marks_TVV << endl;
        }
        cout << "===========================" << endl;
    }

    void displayStudentCount_TVV() {
        cout << "Total number of student records: " << count_TVV << endl;
    }
};

int main() {
    StudentRecord_TVV record_TVV;
    int choice, rollNo;
    char name[50], branch[30];
    float marks;

    cout << "=== Student Records Management System ===" << endl;
    cout << "Storing and Retrieving Student Records Using Roll Numbers" << endl;

    while (true) {
        cout << "\n1. Add Student Record\n2. Search Student Record\n3. Update Student Record\n4. Delete Student Record\n5. Display All Students\n6. Display Student Count\n7. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter roll number: ";
                cin >> rollNo;
                cout << "Enter name: ";
                cin >> name;
                cout << "Enter branch: ";
                cin >> branch;
                cout << "Enter marks: ";
                cin >> marks;
                record_TVV.addStudent_TVV(rollNo, name, branch, marks);
                break;

            case 2:
                cout << "Enter roll number to search: ";
                cin >> rollNo;
                record_TVV.searchStudent_TVV(rollNo);
                break;

            case 3:
                cout << "Enter roll number to update: ";
                cin >> rollNo;
                cout << "Enter new name: ";
                cin >> name;
                cout << "Enter new branch: ";
                cin >> branch;
                cout << "Enter new marks: ";
                cin >> marks;
                record_TVV.updateStudent_TVV(rollNo, name, branch, marks);
                break;

            case 4:
                cout << "Enter roll number to delete: ";
                cin >> rollNo;
                record_TVV.deleteStudent_TVV(rollNo);
                break;

            case 5:
                record_TVV.displayAllStudents_TVV();
                break;

            case 6:
                record_TVV.displayStudentCount_TVV();
                break;

            case 7:
                cout << "Thank you for using Student Records Management System!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


## Output

=== Student Records Management System ===
Storing and Retrieving Student Records Using Roll Numbers

1. Add Student Record
2. Search Student Record
3. Update Student Record
4. Delete Student Record
5. Display All Students
6. Display Student Count
7. Exit
Enter your choice: 1
Enter roll number: 101
Enter name: Arjun
Enter branch: Computer Science
Enter marks: 85.5
Student record added successfully!

1. Add Student Record
2. Search Student Record
3. Update Student Record
4. Delete Student Record
5. Display All Students
6. Display Student Count
7. Exit
Enter your choice: 1
Enter roll number: 102
Enter name: Priya
Enter branch: Electronics
Enter marks: 92.0
Student record added successfully!

1. Add Student Record
2. Search Student Record
3. Update Student Record
4. Delete Student Record
5. Display All Students
6. Display Student Count
7. Exit
Enter your choice: 1
Enter roll number: 103
Enter name: Rohan
Enter branch: Mechanical
Enter marks: 78.5
Student record added successfully!

1. Add Student Record
2. Search Student Record
3. Update Student Record
4. Delete Student Record
5. Display All Students
6. Display Student Count
7. Exit
Enter your choice: 1
Enter roll number: 104
Enter name: Sunita
Enter branch: Civil
Enter marks: 88.0
Student record added successfully!

1. Add Student Record
2. Search Student Record
3. Update Student Record
4. Delete Student Record
5. Display All Students
6. Display Student Count
7. Exit
Enter your choice: 1
Enter roll number: 105
Enter name: Deepak
Enter branch: Electrical
Enter marks: 91.5
Student record added successfully!

1. Add Student Record
2. Search Student Record
3. Update Student Record
4. Delete Student Record
5. Display All Students
6. Display Student Count
7. Exit
Enter your choice: 5

=== All Student Records ===
Roll No	Name		Branch		Marks
----------------------------------------
101	Arjun		Computer Science		85.5
102	Priya		Electronics		92
103	Rohan		Mechanical		78.5
104	Sunita		Civil		88
105	Deepak		Electrical		91.5
===========================

1. Add Student Record
2. Search Student Record
3. Update Student Record
4. Delete Student Record
5. Display All Students
6. Display Student Count
7. Exit
Enter your choice: 2
Enter roll number to search: 103

Student Record Found:
Roll No: 103
Name: Rohan
Branch: Mechanical
Marks: 78.5

1. Add Student Record
2. Search Student Record
3. Update Student Record
4. Delete Student Record
5. Display All Students
6. Display Student Count
7. Exit
Enter your choice: 2
Enter roll number to search: 106
Student with roll number 106 not found!

1. Add Student Record
2. Search Student Record
3. Update Student Record
4. Delete Student Record
5. Display All Students
6. Display Student Count
7. Exit
Enter your choice: 3
Enter roll number to update: 103
Enter new name: Rohan Kumar
Enter new branch: Mechanical Engineering
Enter new marks: 80.0
Student record updated successfully!

1. Add Student Record
2. Search Student Record
3. Update Student Record
4. Delete Student Record
5. Display All Students
6. Display Student Count
7. Exit
Enter your choice: 2
Enter roll number to search: 103

Student Record Found:
Roll No: 103
Name: Rohan Kumar
Branch: Mechanical Engineering
Marks: 80

1. Add Student Record
2. Search Student Record
3. Update Student Record
4. Delete Student Record
5. Display All Students
6. Display Student Count
7. Exit
Enter your choice: 4
Enter roll number to delete: 105
Student record deleted successfully!

1. Add Student Record
2. Search Student Record
3. Update Student Record
4. Delete Student Record
5. Display All Students
6. Display Student Count
7. Exit
Enter your choice: 5

=== All Student Records ===
Roll No	Name		Branch		Marks
----------------------------------------
101	Arjun		Computer Science		85.5
102	Priya		Electronics		92
103	Rohan Kumar		Mechanical Engineering		80
104	Sunita		Civil		88
===========================

1. Add Student Record
2. Search Student Record
3. Update Student Record
4. Delete Student Record
5. Display All Students
6. Display Student Count
7. Exit
Enter your choice: 6
Total number of student records: 4

1. Add Student Record
2. Search Student Record
3. Update Student Record
4. Delete Student Record
5. Display All Students
6. Display Student Count
7. Exit
Enter your choice: 1
Enter roll number: 106
Enter name: Meera
Enter branch: Chemical
Enter marks: 87.5
Student record added successfully!

1. Add Student Record
2. Search Student Record
3. Update Student Record
4. Delete Student Record
5. Display All Students
6. Display Student Count
7. Exit
Enter your choice: 5

=== All Student Records ===
Roll No	Name		Branch		Marks
----------------------------------------
101	Arjun		Computer Science		85.5
102	Priya		Electronics		92
103	Rohan Kumar		Mechanical Engineering		80
104	Sunita		Civil		88
106	Meera		Chemical		87.5
===========================
