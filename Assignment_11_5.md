## Problem Statement
WAP to simulate a faculty database as a hash table. Search a particular faculty by using MOD as a hash function for linear probing method of collision handling technique. Assume suitable data for faculty record.

##  Code
```cpp
#include <iostream>
#include <cstring>
using namespace std;

const int TABLE_SIZE_TVV = 10;
const int EMPTY_TVV = -1;
const int DELETED_TVV = -2;

struct Faculty_TVV {
    int empId_TVV;
    char name_TVV[50];
    char department_TVV[30];
    char designation_TVV[30];
    float salary_TVV;
};

class FacultyHashTable_TVV {
private:
    Faculty_TVV table_TVV[TABLE_SIZE_TVV];
    int status_TVV[TABLE_SIZE_TVV];

    int hashFunction_TVV(int empId) {
        return empId % TABLE_SIZE_TVV;
    }

public:
    FacultyHashTable_TVV() {
        for (int i = 0; i < TABLE_SIZE_TVV; i++) {
            status_TVV[i] = EMPTY_TVV;
        }
    }

    void insert_TVV(int empId, const char* name, const char* department, const char* designation, float salary) {
        int index = hashFunction_TVV(empId);
        int originalIndex = index;
        
        while (status_TVV[index] != EMPTY_TVV && status_TVV[index] != DELETED_TVV) {
            if (table_TVV[index].empId_TVV == empId) {
                strcpy(table_TVV[index].name_TVV, name);
                strcpy(table_TVV[index].department_TVV, department);
                strcpy(table_TVV[index].designation_TVV, designation);
                table_TVV[index].salary_TVV = salary;
                cout << "Faculty record with employee ID " << empId << " updated successfully!" << endl;
                return;
            }
            
            index = (index + 1) % TABLE_SIZE_TVV;
            
            if (index == originalIndex) {
                cout << "Faculty database is full!" << endl;
                return;
            }
        }
        
        table_TVV[index].empId_TVV = empId;
        strcpy(table_TVV[index].name_TVV, name);
        strcpy(table_TVV[index].department_TVV, department);
        strcpy(table_TVV[index].designation_TVV, designation);
        table_TVV[index].salary_TVV = salary;
        status_TVV[index] = 1;
        cout << "Faculty record with employee ID " << empId << " inserted successfully!" << endl;
    }

    void search_TVV(int empId) {
        int index = hashFunction_TVV(empId);
        int originalIndex = index;
        
        while (status_TVV[index] != EMPTY_TVV) {
            if (status_TVV[index] == 1 && table_TVV[index].empId_TVV == empId) {
                cout << "\nFaculty Found:" << endl;
                cout << "Employee ID: " << table_TVV[index].empId_TVV << endl;
                cout << "Name: " << table_TVV[index].name_TVV << endl;
                cout << "Department: " << table_TVV[index].department_TVV << endl;
                cout << "Designation: " << table_TVV[index].designation_TVV << endl;
                cout << "Salary: " << table_TVV[index].salary_TVV << endl;
                return;
            }
            
            index = (index + 1) % TABLE_SIZE_TVV;
            
            if (index == originalIndex) {
                break;
            }
        }
        
        cout << "Faculty with employee ID " << empId << " not found!" << endl;
    }

    void remove_TVV(int empId) {
        int index = hashFunction_TVV(empId);
        int originalIndex = index;
        
        while (status_TVV[index] != EMPTY_TVV) {
            if (status_TVV[index] == 1 && table_TVV[index].empId_TVV == empId) {
                status_TVV[index] = DELETED_TVV;
                cout << "Faculty with employee ID " << empId << " deleted successfully!" << endl;
                return;
            }
            
            index = (index + 1) % TABLE_SIZE_TVV;
            
            if (index == originalIndex) {
                break;
            }
        }
        
        cout << "Faculty with employee ID " << empId << " not found!" << endl;
    }

    void display_TVV() {
        cout << "\n=== Faculty Database ===" << endl;
        cout << "Index\tEmp ID\tName\t\tDepartment\tDesignation\tSalary\tStatus" << endl;
        cout << "------------------------------------------------------------------------" << endl;
        
        for (int i = 0; i < TABLE_SIZE_TVV; i++) {
            cout << i << "\t";
            if (status_TVV[i] == 1) {
                cout << table_TVV[i].empId_TVV << "\t" << table_TVV[i].name_TVV 
                     << "\t\t" << table_TVV[i].department_TVV << "\t\t" << table_TVV[i].designation_TVV
                     << "\t\t" << table_TVV[i].salary_TVV << "\tOccupied" << endl;
            } else if (status_TVV[i] == DELETED_TVV) {
                cout << "DELETED\tDELETED\t\tDELETED\t\tDELETED\t\tDELETED\t\tDeleted" << endl;
            } else {
                cout << "EMPTY\tEMPTY\t\tEMPTY\t\tEMPTY\t\tEMPTY\t\tEmpty" << endl;
            }
        }
        cout << "========================" << endl;
    }
};

int main() {
    FacultyHashTable_TVV facultyDB_TVV;
    int choice, empId;
    char name[50], department[30], designation[30];
    float salary;

    cout << "=== Faculty Database Hash Table ===" << endl;
    cout << "Using Linear Probing with MOD Hash Function" << endl;

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
Using Linear Probing with MOD Hash Function

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
Index	Emp ID	Name		Department	Designation	Salary	Status
------------------------------------------------------------------------
0	131	Dr. Sunita Reddy		Civil		Professor		72000	Occupied
1	141	Dr. Deepak Kumar		Electrical		Associate Professor		62000	Occupied
2	EMPTY	EMPTY		EMPTY		EMPTY		EMPTY	Empty
3	EMPTY	EMPTY		EMPTY		EMPTY		EMPTY	Empty
4	EMPTY	EMPTY		EMPTY		EMPTY		EMPTY	Empty
5	EMPTY	EMPTY		EMPTY		EMPTY		EMPTY	Empty
6	EMPTY	EMPTY		EMPTY		EMPTY		EMPTY	Empty
7	101	Dr. Arjun Sharma		Computer Science		Professor		75000	Occupied
8	111	Dr. Priya Patel		Electronics		Associate Professor		65000	Occupied
9	121	Dr. Rohan Gupta		Mechanical		Assistant Professor		55000	Occupied
========================

1. Insert Faculty Record
2. Search Faculty Record
3. Delete Faculty Record
4. Display Faculty Database
5. Exit
Enter your choice: 2
Enter employee ID to search: 111

Faculty Found:
Employee ID: 111
Name: Dr. Priya Patel
Department: Electronics
Designation: Associate Professor
Salary: 65000

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
Index	Emp ID	Name		Department	Designation	Salary	Status
------------------------------------------------------------------------
0	131	Dr. Sunita Reddy		Civil		Professor		72000	Occupied
1	141	Dr. Deepak Kumar		Electrical		Associate Professor		62000	Occupied
2	EMPTY	EMPTY		EMPTY		EMPTY		EMPTY	Empty
3	EMPTY	EMPTY		EMPTY		EMPTY		EMPTY	Empty
4	EMPTY	EMPTY		EMPTY		EMPTY		EMPTY	Empty
5	EMPTY	EMPTY		EMPTY		EMPTY		EMPTY	Empty
6	EMPTY	EMPTY		EMPTY		EMPTY		EMPTY	Empty
7	101	Dr. Arjun Sharma		Computer Science		Professor		75000	Occupied
8	111	Dr. Priya Patel		Electronics		Associate Professor		65000	Occupied
9	DELETED	DELETED		DELETED		DELETED		DELETED	Deleted
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
Index	Emp ID	Name		Department	Designation	Salary	Status
------------------------------------------------------------------------
0	131	Dr. Sunita Reddy		Civil		Professor		72000	Occupied
1	141	Dr. Deepak Kumar		Electrical	Associate 		62000	Occupied
2	151	Dr. Meera Singh		Mathematics		Assistant 		52000	Occupied
3	EMPTY	EMPTY		        EMPTY		EMPTY		    EMPTY	Empty
4	EMPTY	EMPTY		        EMPTY		EMPTY		    EMPTY	Empty
5	EMPTY	EMPTY		        EMPTY		EMPTY		    EMPTY	Empty
6	EMPTY	EMPTY		        EMPTY		EMPTY		    EMPTY	Empty
7	101	Dr. Arjun Sharma		Computer    Professor		75000	Occupied
8	111	Dr. Priya Patel		Electronics		 Professor		65000	Occupied
9	DELETED	DELETED		DELETED		DELETED		DELETED	Deleted
========================
