## Problem Statement
WAP to simulate a faculty database as a hash table. Search a particular faculty by using 'divide' as a hash function for linear probing with chaining without replacement method of collision handling technique. Assume suitable data for faculty record.

##  Code
```cpp
#include <iostream>
#include <cstring>
using namespace std;

const int TABLE_SIZE_TVV = 10;

struct FacultyNode_TVV {
    int empId_TVV;
    char name_TVV[50];
    char department_TVV[30];
    char designation_TVV[30];
    float salary_TVV;
    FacultyNode_TVV* next_TVV;
    
    FacultyNode_TVV(int empId, const char* name, const char* department, const char* designation, float salary) {
        empId_TVV = empId;
        strcpy(name_TVV, name);
        strcpy(department_TVV, department);
        strcpy(designation_TVV, designation);
        salary_TVV = salary;
        next_TVV = nullptr;
    }
};

class FacultyHashTable_TVV {
private:
    FacultyNode_TVV* table_TVV[TABLE_SIZE_TVV];

    int hashFunction_TVV(int empId) {
        return empId % TABLE_SIZE_TVV;
    }

public:
    FacultyHashTable_TVV() {
        for (int i = 0; i < TABLE_SIZE_TVV; i++) {
            table_TVV[i] = nullptr;
        }
    }

    void insert_TVV(int empId, const char* name, const char* department, const char* designation, float salary) {
        int index = hashFunction_TVV(empId);
        
        FacultyNode_TVV* current = table_TVV[index];
        while (current != nullptr) {
            if (current->empId_TVV == empId) {
                strcpy(current->name_TVV, name);
                strcpy(current->department_TVV, department);
                strcpy(current->designation_TVV, designation);
                current->salary_TVV = salary;
                cout << "Faculty record with employee ID " << empId << " updated successfully!" << endl;
                return;
            }
            current = current->next_TVV;
        }
        
        FacultyNode_TVV* newNode = new FacultyNode_TVV(empId, name, department, designation, salary);
        newNode->next_TVV = table_TVV[index];
        table_TVV[index] = newNode;
        cout << "Faculty record with employee ID " << empId << " inserted successfully!" << endl;
    }

    void search_TVV(int empId) {
        int index = hashFunction_TVV(empId);
        FacultyNode_TVV* current = table_TVV[index];
        
        while (current != nullptr) {
            if (current->empId_TVV == empId) {
                cout << "\nFaculty Found:" << endl;
                cout << "Employee ID: " << current->empId_TVV << endl;
                cout << "Name: " << current->name_TVV << endl;
                cout << "Department: " << current->department_TVV << endl;
                cout << "Designation: " << current->designation_TVV << endl;
                cout << "Salary: " << current->salary_TVV << endl;
                return;
            }
            current = current->next_TVV;
        }
        
        cout << "Faculty with employee ID " << empId << " not found!" << endl;
    }

    void remove_TVV(int empId) {
        int index = hashFunction_TVV(empId);
        FacultyNode_TVV* current = table_TVV[index];
        FacultyNode_TVV* prev = nullptr;
        
        while (current != nullptr) {
            if (current->empId_TVV == empId) {
                if (prev == nullptr) {
                    table_TVV[index] = current->next_TVV;
                } else {
                    prev->next_TVV = current->next_TVV;
                }
                delete current;
                cout << "Faculty with employee ID " << empId << " deleted successfully!" << endl;
                return;
            }
            prev = current;
            current = current->next_TVV;
        }
        
        cout << "Faculty with employee ID " << empId << " not found!" << endl;
    }

    void display_TVV() {
        cout << "\n=== Faculty Database ===" << endl;
        for (int i = 0; i < TABLE_SIZE_TVV; i++) {
            cout << "Bucket " << i << ": ";
            FacultyNode_TVV* current = table_TVV[i];
            if (current == nullptr) {
                cout << "Empty";
            } else {
                while (current != nullptr) {
                    cout << "[" << current->empId_TVV << ", " << current->name_TVV 
                         << ", " << current->department_TVV << ", " << current->designation_TVV
                         << ", " << current->salary_TVV << "] ";
                    current = current->next_TVV;
                }
            }
            cout << endl;
        }
        cout << "========================" << endl;
    }

    ~FacultyHashTable_TVV() {
        for (int i = 0; i < TABLE_SIZE_TVV; i++) {
            FacultyNode_TVV* current = table_TVV[i];
            while (current != nullptr) {
                FacultyNode_TVV* next = current->next_TVV;
                delete current;
                current = next;
            }
        }
    }
};

int main() {
    FacultyHashTable_TVV facultyDB_TVV;
    int choice, empId;
    char name[50], department[30], designation[30];
    float salary;

    cout << "=== Faculty Database Hash Table ===" << endl;
    cout << "Using Chaining Without Replacement Method" << endl;

    while (true) {
        cout << "\n1. Insert Faculty Record\n2. Search Faculty Record\n3. Delete Faculty Record\n4. Display Faculty Database\n5. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter employee ID: ";
                cin >> empId;
                cout << "Enter name: ";
                cin >> name;
                cout << "Enter department: ";
                cin >> department;
                cout << "Enter designation: ";
                cin >> designation;
                cout << "Enter salary: ";
                cin >> salary;
                facultyDB_TVV.insert_TVV(empId, name, department, designation, salary);
                break;

            case 2:
                cout << "Enter employee ID to search: ";
                cin >> empId;
                facultyDB_TVV.search_TVV(empId);
                break;

            case 3:
                cout << "Enter employee ID to delete: ";
                cin >> empId;
                facultyDB_TVV.remove_TVV(empId);
                break;

            case 4:
                facultyDB_TVV.display_TVV();
                break;

            case 5:
                cout << "Thank you for using Faculty Database Hash Table!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}

## Output

=== Faculty Database Hash Table ===
Using Chaining Without Replacement Method

1. Insert Faculty Record
2. Search Faculty Record
3. Delete Faculty Record
4. Display Faculty Database
5. Exit
Enter your choice: 1
Enter employee ID: 101
Enter name: Dr. Arjun Sharma
Enter department: Computer Science
Enter designation: Professor
Enter salary: 75000
Faculty record with employee ID 101 inserted successfully!

1. Insert Faculty Record
2. Search Faculty Record
3. Delete Faculty Record
4. Display Faculty Database
5. Exit
Enter your choice: 1
Enter employee ID: 111
Enter name: Dr. Priya Patel
Enter department: Electronics
Enter designation: Associate Professor
Enter salary: 65000
Faculty record with employee ID 111 inserted successfully!

1. Insert Faculty Record
2. Search Faculty Record
3. Delete Faculty Record
4. Display Faculty Database
5. Exit
Enter your choice: 1
Enter employee ID: 121
Enter name: Dr. Rohan Gupta
Enter department: Mechanical
Enter designation: Assistant Professor
Enter salary: 55000
Faculty record with employee ID 121 inserted successfully!

1. Insert Faculty Record
2. Search Faculty Record
3. Delete Faculty Record
4. Display Faculty Database
5. Exit
Enter your choice: 1
Enter employee ID: 131
Enter name: Dr. Sunita Reddy
Enter department: Civil
Enter designation: Professor
Enter salary: 72000
Faculty record with employee ID 131 inserted successfully!

1. Insert Faculty Record
2. Search Faculty Record
3. Delete Faculty Record
4. Display Faculty Database
5. Exit
Enter your choice: 1
Enter employee ID: 141
Enter name: Dr. Deepak Kumar
Enter department: Electrical
Enter designation: Associate Professor
Enter salary: 62000
Faculty record with employee ID 141 inserted successfully!

1. Insert Faculty Record
2. Search Faculty Record
3. Delete Faculty Record
4. Display Faculty Database
5. Exit
Enter your choice: 4

=== Faculty Database ===
Bucket 0: Empty
Bucket 1: [141, Dr. Deepak Kumar, Electrical, Associate Professor, 62000] 
Bucket 2: Empty
Bucket 3: Empty
Bucket 4: Empty
Bucket 5: Empty
Bucket 6: Empty
Bucket 7: [101, Dr. Arjun Sharma, Computer Science, Professor, 75000] 
Bucket 8: [111, Dr. Priya Patel, Electronics, Associate Professor, 65000] 
Bucket 9: [131, Dr. Sunita Reddy, Civil, Professor, 72000] [121, Dr. Rohan Gupta, Mechanical, Assistant Professor, 55000] 
========================

1. Insert Faculty Record
2. Search Faculty Record
3. Delete Faculty Record
4. Display Faculty Database
5. Exit
Enter your choice: 2
Enter employee ID to search: 121

Faculty Found:
Employee ID: 121
Name: Dr. Rohan Gupta
Department: Mechanical
Designation: Assistant Professor
Salary: 55000

1. Insert Faculty Record
2. Search Faculty Record
3. Delete Faculty Record
4. Display Faculty Database
5. Exit
Enter your choice: 2
Enter employee ID to search: 151
Faculty with employee ID 151 not found!

1. Insert Faculty Record
2. Search Faculty Record
3. Delete Faculty Record
4. Display Faculty Database
5. Exit
Enter your choice: 3
Enter employee ID to delete: 121
Faculty with employee ID 121 deleted successfully!

1. Insert Faculty Record
2. Search Faculty Record
3. Delete Faculty Record
4. Display Faculty Database
5. Exit
Enter your choice: 4

=== Faculty Database ===
Bucket 0: Empty
Bucket 1: [141, Dr. Deepak Kumar, Electrical, Associate Professor, 62000] 
Bucket 2: Empty
Bucket 3: Empty
Bucket 4: Empty
Bucket 5: Empty
Bucket 6: Empty
Bucket 7: [101, Dr. Arjun Sharma, Computer Science, Professor, 75000] 
Bucket 8: [111, Dr. Priya Patel, Electronics, Associate Professor, 65000] 
Bucket 9: [131, Dr. Sunita Reddy, Civil, Professor, 72000] 
========================

1. Insert Faculty Record
2. Search Faculty Record
3. Delete Faculty Record
4. Display Faculty Database
5. Exit
Enter your choice: 1
Enter employee ID: 151
Enter name: Dr. Meera Singh
Enter department: Mathematics
Enter designation: Assistant Professor
Enter salary: 52000
Faculty record with employee ID 151 inserted successfully!

1. Insert Faculty Record
2. Search Faculty Record
3. Delete Faculty Record
4. Display Faculty Database
5. Exit
Enter your choice: 4

=== Faculty Database ===
Bucket 0: Empty
Bucket 1: [141, Dr. Deepak Kumar, Electrical, Associate Professor, 62000] [151, Dr. Meera Singh, Mathematics, Assistant Professor, 52000] 
Bucket 2: Empty
Bucket 3: Empty
Bucket 4: Empty
Bucket 5: Empty
Bucket 6: Empty
Bucket 7: [101, Dr. Arjun Sharma, Computer Science, Professor, 75000] 
Bucket 8: [111, Dr. Priya Patel, Electronics, Associate Professor, 65000] 
Bucket 9: [131, Dr. Sunita Reddy, Civil, Professor, 72000] 
========================
