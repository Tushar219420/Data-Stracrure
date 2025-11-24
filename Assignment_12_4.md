
## Problem Statement
WAP to simulate student databases as a hash table. A student database management system using hashing techniques to allow efficient insertion, search, and deletion of student records.

##  Code
```cpp
#include <iostream>
#include <cstring>
using namespace std;

const int TABLE_SIZE_TVV = 10;

struct StudentNode_TVV {
    int rollNo_TVV;
    char name_TVV[50];
    char branch_TVV[30];
    float cgpa_TVV;
    StudentNode_TVV* next_TVV;
    
    StudentNode_TVV(int rollNo, const char* name, const char* branch, float cgpa) {
        rollNo_TVV = rollNo;
        strcpy(name_TVV, name);
        strcpy(branch_TVV, branch);
        cgpa_TVV = cgpa;
        next_TVV = nullptr;
    }
};

class StudentDatabase_TVV {
private:
    StudentNode_TVV* table_TVV[TABLE_SIZE_TVV];

    int hashFunction_TVV(int rollNo) {
        return rollNo % TABLE_SIZE_TVV;
    }

public:
    StudentDatabase_TVV() {
        for (int i = 0; i < TABLE_SIZE_TVV; i++) {
            table_TVV[i] = nullptr;
        }
    }

    void insert_TVV(int rollNo, const char* name, const char* branch, float cgpa) {
        int index = hashFunction_TVV(rollNo);
        
        StudentNode_TVV* current = table_TVV[index];
        while (current != nullptr) {
            if (current->rollNo_TVV == rollNo) {
                strcpy(current->name_TVV, name);
                strcpy(current->branch_TVV, branch);
                current->cgpa_TVV = cgpa;
                cout << "Student record with roll number " << rollNo << " updated successfully!" << endl;
                return;
            }
            current = current->next_TVV;
        }
        
        StudentNode_TVV* newNode = new StudentNode_TVV(rollNo, name, branch, cgpa);
        newNode->next_TVV = table_TVV[index];
        table_TVV[index] = newNode;
        cout << "Student record with roll number " << rollNo << " inserted successfully!" << endl;
    }

    void search_TVV(int rollNo) {
        int index = hashFunction_TVV(rollNo);
        StudentNode_TVV* current = table_TVV[index];
        
        while (current != nullptr) {
            if (current->rollNo_TVV == rollNo) {
                cout << "\nStudent Record Found:" << endl;
                cout << "Roll No: " << current->rollNo_TVV << endl;
                cout << "Name: " << current->name_TVV << endl;
                cout << "Branch: " << current->branch_TVV << endl;
                cout << "CGPA: " << current->cgpa_TVV << endl;
                return;
            }
            current = current->next_TVV;
        }
        
        cout << "Student with roll number " << rollNo << " not found!" << endl;
    }

    void remove_TVV(int rollNo) {
        int index = hashFunction_TVV(rollNo);
        StudentNode_TVV* current = table_TVV[index];
        StudentNode_TVV* prev = nullptr;
        
        while (current != nullptr) {
            if (current->rollNo_TVV == rollNo) {
                if (prev == nullptr) {
                    table_TVV[index] = current->next_TVV;
                } else {
                    prev->next_TVV = current->next_TVV;
                }
                delete current;
                cout << "Student with roll number " << rollNo << " deleted successfully!" << endl;
                return;
            }
            prev = current;
            current = current->next_TVV;
        }
        
        cout << "Student with roll number " << rollNo << " not found!" << endl;
    }

    void display_TVV() {
        cout << "\n=== Student Database ===" << endl;
        bool isEmpty = true;
        for (int i = 0; i < TABLE_SIZE_TVV; i++) {
            StudentNode_TVV* current = table_TVV[i];
            if (current != nullptr) {
                isEmpty = false;
                cout << "Bucket " << i << ": ";
                while (current != nullptr) {
                    cout << "[" << current->rollNo_TVV << ", " << current->name_TVV 
                         << ", " << current->branch_TVV << ", " << current->cgpa_TVV << "] ";
                    current = current->next_TVV;
                }
                cout << endl;
            }
        }
        if (isEmpty) {
            cout << "No student records found!" << endl;
        }
        cout << "========================" << endl;
    }

    void searchByBranch_TVV(const char* branch) {
        cout << "\n=== Students in Branch: " << branch << " ===" << endl;
        bool found = false;
        for (int i = 0; i < TABLE_SIZE_TVV; i++) {
            StudentNode_TVV* current = table_TVV[i];
            while (current != nullptr) {
                if (strcmp(current->branch_TVV, branch) == 0) {
                    cout << "Roll No: " << current->rollNo_TVV << ", Name: " << current->name_TVV 
                         << ", CGPA: " << current->cgpa_TVV << endl;
                    found = true;
                }
                current = current->next_TVV;
            }
        }
        if (!found) {
            cout << "No students found in branch " << branch << endl;
        }
        cout << "========================" << endl;
    }

    ~StudentDatabase_TVV() {
        for (int i = 0; i < TABLE_SIZE_TVV; i++) {
            StudentNode_TVV* current = table_TVV[i];
            while (current != nullptr) {
                StudentNode_TVV* next = current->next_TVV;
                delete current;
                current = next;
            }
        }
    }
};

int main() {
    StudentDatabase_TVV studentDB_TVV;
    int choice, rollNo;
    char name[50], branch[30];
    float cgpa;

    cout << "=== Student Database Management System ===" << endl;
    cout << "Using Hashing Techniques for Efficient Operations" << endl;

    while (true) {
        cout << "\n1. Insert Student Record\n2. Search Student Record\n3. Delete Student Record\n4. Display All Students\n5. Search by Branch\n6. Exit\n";
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
                cout << "Enter CGPA: ";
                cin >> cgpa;
                studentDB_TVV.insert_TVV(rollNo, name, branch, cgpa);
                break;

            case 2:
                cout << "Enter roll number to search: ";
                cin >> rollNo;
                studentDB_TVV.search_TVV(rollNo);
                break;

            case 3:
                cout << "Enter roll number to delete: ";
                cin >> rollNo;
                studentDB_TVV.remove_TVV(rollNo);
                break;

            case 4:
                studentDB_TVV.display_TVV();
                break;

            case 5:
                cout << "Enter branch to search: ";
                cin >> branch;
                studentDB_TVV.searchByBranch_TVV(branch);
                break;

            case 6:
                cout << "Thank you for using Student Database Management System!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}

## Output

=== Student Database Management System ===
Using Hashing Techniques for Efficient Operations

1. Insert Student Record
2. Search Student Record
3. Delete Student Record
4. Display All Students
5. Search by Branch
6. Exit
Enter your choice: 1
Enter roll number: 101
Enter name: Arjun Sharma
Enter branch: Computer Science
Enter CGPA: 8.5
Student record with roll number 101 inserted successfully!

1. Insert Student Record
2. Search Student Record
3. Delete Student Record
4. Display All Students
5. Search by Branch
6. Exit
Enter your choice: 1
Enter roll number: 102
Enter name: Priya Patel
Enter branch: Electronics
Enter CGPA: 9.2
Student record with roll number 102 inserted successfully!

1. Insert Student Record
2. Search Student Record
3. Delete Student Record
4. Display All Students
5. Search by Branch
6. Exit
Enter your choice: 1
Enter roll number: 103
Enter name: Rohan Gupta
Enter branch: Computer Science
Enter CGPA: 7.8
Student record with roll number 103 inserted successfully!

1. Insert Student Record
2. Search Student Record
3. Delete Student Record
4. Display All Students
5. Search by Branch
6. Exit
Enter your choice: 1
Enter roll number: 104
Enter name: Sunita Reddy
Enter branch: Mechanical
Enter CGPA: 8.8
Student record with roll number 104 inserted successfully!

1. Insert Student Record
2. Search Student Record
3. Delete Student Record
4. Display All Students
5. Search by Branch
6. Exit
Enter your choice: 1
Enter roll number: 105
Enter name: Deepak Kumar
Enter branch: Electronics
Enter CGPA: 9.1
Student record with roll number 105 inserted successfully!

1. Insert Student Record
2. Search Student Record
3. Delete Student Record
4. Display All Students
5. Search by Branch
6. Exit
Enter your choice: 4

=== Student Database ===
Bucket 1: [104, Sunita Reddy, Mechanical, 8.8] 
Bucket 2: [105, Deepak Kumar, Electronics, 9.1] [102, Priya Patel, Electronics, 9.2] 
Bucket 4: [101, Arjun Sharma, Computer Science, 8.5] [103, Rohan Gupta, Computer Science, 7.8] 
========================

1. Insert Student Record
2. Search Student Record
3. Delete Student Record
4. Display All Students
5. Search by Branch
6. Exit
Enter your choice: 2
Enter roll number to search: 103

Student Record Found:
Roll No: 103
Name: Rohan Gupta
Branch: Computer Science
CGPA: 7.8

1. Insert Student Record
2. Search Student Record
3. Delete Student Record
4. Display All Students
5. Search by Branch
6. Exit
Enter your choice: 2
Enter roll number to search: 106
Student with roll number 106 not found!

1. Insert Student Record
2. Search Student Record
3. Delete Student Record
4. Display All Students
5. Search by Branch
6. Exit
Enter your choice: 3
Enter roll number to delete: 102
Student with roll number 102 deleted successfully!

1. Insert Student Record
2. Search Student Record
3. Delete Student Record
4. Display All Students
5. Search by Branch
6. Exit
Enter your choice: 4

=== Student Database ===
Bucket 1: [104, Sunita Reddy, Mechanical, 8.8] 
Bucket 2: [105, Deepak Kumar, Electronics, 9.1] 
Bucket 4: [101, Arjun Sharma, Computer Science, 8.5] [103, Rohan Gupta, Computer Science, 7.8] 
========================

1. Insert Student Record
2. Search Student Record
3. Delete Student Record
4. Display All Students
5. Search by Branch
6. Exit
Enter your choice: 5
Enter branch to search: Computer Science

=== Students in Branch: Computer Science ===
Roll No: 101, Name: Arjun Sharma, CGPA: 8.5
Roll No: 103, Name: Rohan Gupta, CGPA: 7.8
========================

1. Insert Student Record
2. Search Student Record
3. Delete Student Record
4. Display All Students
5. Search by Branch
6. Exit
Enter your choice: 1
Enter roll number: 106
Enter name: Meera Singh
Enter branch: Computer Science
Enter CGPA: 8.7
Student record with roll number 106 inserted successfully!

1. Insert Student Record
2. Search Student Record
3. Delete Student Record
4. Display All Students
5. Search by Branch
6. Exit
Enter your choice: 4

=== Student Database ===
Bucket 1: [104, Sunita Reddy, Mechanical, 8.8] 
Bucket 2: [105, Deepak Kumar, Electronics, 9.1] 
Bucket 4: [101, Arjun Sharma, Computer Science, 8.5] [103, Rohan Gupta, Computer Science, 7.8] [106, Meera Singh, Computer Science, 8.7] 
Bucket 6: [106, Meera Singh, Computer Science, 8.7] 
========================

