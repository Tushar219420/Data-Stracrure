## Problem Statement
WAP to simulate employee databases as a hash table. Search a particular faculty by using Mid square method as a hash function for linear probing method of collision handling technique. Assume suitable data for faculty record.

##  Code
```cpp
#include <iostream>
#include <cstring>
using namespace std;

const int TABLE_SIZE_TVV = 10;
const int EMPTY_TVV = -1;

struct Employee_TVV {
    int empId_TVV;
    char name_TVV[50];
    char department_TVV[30];
    char position_TVV[30];
    float salary_TVV;
};

class EmployeeHashTable_TVV {
private:
    Employee_TVV table_TVV[TABLE_SIZE_TVV];
    int status_TVV[TABLE_SIZE_TVV];

    int midSquareHash_TVV(int empId) {
        long long square = (long long)empId * empId;
        int digits = 0;
        long long temp = square;
        
        while (temp > 0) {
            digits++;
            temp /= 10;
        }
        
        if (digits <= 2) {
            return square % TABLE_SIZE_TVV;
        }
        
        int midDigits = digits / 2;
        int divisor = 1;
        for (int i = 0; i < midDigits - 1; i++) {
            divisor *= 10;
        }
        
        int midValue = (square / divisor) % 100;
        return midValue % TABLE_SIZE_TVV;
    }

public:
    EmployeeHashTable_TVV() {
        for (int i = 0; i < TABLE_SIZE_TVV; i++) {
            status_TVV[i] = EMPTY_TVV;
        }
    }

    void insert_TVV(int empId, const char* name, const char* department, const char* position, float salary) {
        int index = midSquareHash_TVV(empId);
        int originalIndex = index;
        
        while (status_TVV[index] != EMPTY_TVV) {
            if (table_TVV[index].empId_TVV == empId) {
                strcpy(table_TVV[index].name_TVV, name);
                strcpy(table_TVV[index].department_TVV, department);
                strcpy(table_TVV[index].position_TVV, position);
                table_TVV[index].salary_TVV = salary;
                cout << "Employee record with employee ID " << empId << " updated successfully!" << endl;
                return;
            }
            
            index = (index + 1) % TABLE_SIZE_TVV;
            
            if (index == originalIndex) {
                cout << "Employee database is full!" << endl;
                return;
            }
        }
        
        table_TVV[index].empId_TVV = empId;
        strcpy(table_TVV[index].name_TVV, name);
        strcpy(table_TVV[index].department_TVV, department);
        strcpy(table_TVV[index].position_TVV, position);
        table_TVV[index].salary_TVV = salary;
        status_TVV[index] = 1;
        cout << "Employee record with employee ID " << empId << " inserted successfully!" << endl;
    }

    void search_TVV(int empId) {
        int index = midSquareHash_TVV(empId);
        int originalIndex = index;
        
        while (status_TVV[index] != EMPTY_TVV) {
            if (status_TVV[index] == 1 && table_TVV[index].empId_TVV == empId) {
                cout << "\nEmployee Found:" << endl;
                cout << "Employee ID: " << table_TVV[index].empId_TVV << endl;
                cout << "Name: " << table_TVV[index].name_TVV << endl;
                cout << "Department: " << table_TVV[index].department_TVV << endl;
                cout << "Position: " << table_TVV[index].position_TVV << endl;
                cout << "Salary: " << table_TVV[index].salary_TVV << endl;
                return;
            }
            
            index = (index + 1) % TABLE_SIZE_TVV;
            
            if (index == originalIndex) {
                break;
            }
        }
        
        cout << "Employee with employee ID " << empId << " not found!" << endl;
    }

    void remove_TVV(int empId) {
        int index = midSquareHash_TVV(empId);
        int originalIndex = index;
        
        while (status_TVV[index] != EMPTY_TVV) {
            if (status_TVV[index] == 1 && table_TVV[index].empId_TVV == empId) {
                status_TVV[index] = EMPTY_TVV;
                cout << "Employee with employee ID " << empId << " deleted successfully!" << endl;
                return;
            }
            
            index = (index + 1) % TABLE_SIZE_TVV;
            
            if (index == originalIndex) {
                break;
            }
        }
        
        cout << "Employee with employee ID " << empId << " not found!" << endl;
    }

    void display_TVV() {
        cout << "\n=== Employee Database ===" << endl;
        cout << "Index\tEmp ID\tName\t\tDepartment\tPosition\tSalary\tStatus" << endl;
        cout << "------------------------------------------------------------------------" << endl;
        
        for (int i = 0; i < TABLE_SIZE_TVV; i++) {
            cout << i << "\t";
            if (status_TVV[i] == 1) {
                cout << table_TVV[i].empId_TVV << "\t" << table_TVV[i].name_TVV 
                     << "\t\t" << table_TVV[i].department_TVV << "\t\t" << table_TVV[i].position_TVV
                     << "\t\t" << table_TVV[i].salary_TVV << "\tOccupied" << endl;
            } else {
                cout << "EMPTY\tEMPTY\t\tEMPTY\t\tEMPTY\t\tEMPTY\t\tEmpty" << endl;
            }
        }
        cout << "========================" << endl;
    }
};

int main() {
    EmployeeHashTable_TVV employeeDB_TVV;
    int choice, empId;
    char name[50], department[30], position[30];
    float salary;

    cout << "=== Employee Database Hash Table ===" << endl;
    cout << "Using Mid-Square Method with Linear Probing" << endl;

    while (true) {
        cout << "\n1. Insert Employee Record\n2. Search Employee Record\n3. Delete Employee Record\n4. Display Employee Database\n5. Exit\n";
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
                cout << "Enter position: ";
                cin >> position;
                cout << "Enter salary: ";
                cin >> salary;
                employeeDB_TVV.insert_TVV(empId, name, department, position, salary);
                break;

            case 2:
                cout << "Enter employee ID to search: ";
                cin >> empId;
                employeeDB_TVV.search_TVV(empId);
                break;

            case 3:
                cout << "Enter employee ID to delete: ";
                cin >> empId;
                employeeDB_TVV.remove_TVV(empId);
                break;

            case 4:
                employeeDB_TVV.display_TVV();
                break;

            case 5:
                cout << "Thank you for using Employee Database Hash Table!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


##  Output

=== Employee Database Hash Table ===
Using Mid-Square Method with Linear Probing

1. Insert Employee Record
2. Search Employee Record
3. Delete Employee Record
4. Display Employee Database
5. Exit
Enter your choice: 1
Enter employee ID: 123
Enter name: Arjun Sharma
Enter department: IT
Enter position: Software Engineer
Enter salary: 45000
Employee record with employee ID 123 inserted successfully!

1. Insert Employee Record
2. Search Employee Record
3. Delete Employee Record
4. Display Employee Database
5. Exit
Enter your choice: 1
Enter employee ID: 234
Enter name: Priya Patel
Enter department: HR
Enter position: HR Manager
Enter salary: 52000
Employee record with employee ID 234 inserted successfully!

1. Insert Employee Record
2. Search Employee Record
3. Delete Employee Record
4. Display Employee Database
5. Exit
Enter your choice: 1
Enter employee ID: 345
Enter name: Rohan Gupta
Enter department: Finance
Enter position: Accountant
Enter salary: 48000
Employee record with employee ID 345 inserted successfully!

1. Insert Employee Record
2. Search Employee Record
3. Delete Employee Record
4. Display Employee Database
5. Exit
Enter your choice: 1
Enter employee ID: 456
Enter name: Sunita Reddy
Enter department: Marketing
Enter position: Marketing Executive
Enter salary: 42000
Employee record with employee ID 456 inserted successfully!

1. Insert Employee Record
2. Search Employee Record
3. Delete Employee Record
4. Display Employee Database
5. Exit
Enter your choice: 1
Enter employee ID: 567
Enter name: Deepak Kumar
Enter department: Operations
Enter position: Operations Manager
Enter salary: 55000
Employee record with employee ID 567 inserted successfully!

1. Insert Employee Record
2. Search Employee Record
3. Delete Employee Record
4. Display Employee Database
5. Exit
Enter your choice: 4

=== Employee Database ===
Index	Emp ID	Name		Department	Position	Salary	Status
------------------------------------------------------------------------
0	EMPTY	EMPTY		EMPTY		EMPTY		EMPTY	Empty
1	234	Priya Patel		HR		HR Manager		52000	Occupied
2	EMPTY	EMPTY		EMPTY		EMPTY		EMPTY	Empty
3	EMPTY	EMPTY		EMPTY		EMPTY		EMPTY	Empty
4	EMPTY	EMPTY		EMPTY		EMPTY		EMPTY	Empty
5	123	Arjun Sharma		IT		Software Engineer		45000	Occupied
6	345	Rohan Gupta		Finance		Accountant		48000	Occupied
7	456	Sunita Reddy		Marketing		Marketing Executive		42000	Occupied
8	567	Deepak Kumar		Operations		Operations Manager		55000	Occupied
9	EMPTY	EMPTY		EMPTY		EMPTY		EMPTY	Empty
========================

1. Insert Employee Record
2. Search Employee Record
3. Delete Employee Record
4. Display Employee Database
5. Exit
Enter your choice: 2
Enter employee ID to search: 345

Employee Found:
Employee ID: 345
Name: Rohan Gupta
Department: Finance
Position: Accountant
Salary: 48000

1. Insert Employee Record
2. Search Employee Record
3. Delete Employee Record
4. Display Employee Database
5. Exit
Enter your choice: 2
Enter employee ID to search: 678
Employee with employee ID 678 not found!

1. Insert Employee Record
2. Search Employee Record
3. Delete Employee Record
4. Display Employee Database
5. Exit
Enter your choice: 3
Enter employee ID to delete: 456
Employee with employee ID 456 deleted successfully!

1. Insert Employee Record
2. Search Employee Record
3. Delete Employee Record
4. Display Employee Database
5. Exit
Enter your choice: 4

=== Employee Database ===
Index	Emp ID	Name		Department	Position	Salary	Status
------------------------------------------------------------------------
0	EMPTY	EMPTY		EMPTY		EMPTY		EMPTY	Empty
1	234	Priya Patel		HR		HR Manager		52000	Occupied
2	EMPTY	EMPTY		EMPTY		EMPTY		EMPTY	Empty
3	EMPTY	EMPTY		EMPTY		EMPTY		EMPTY	Empty
4	EMPTY	EMPTY		EMPTY		EMPTY		EMPTY	Empty
5	123	Arjun Sharma		IT		Software Engineer		45000	Occupied
6	345	Rohan Gupta		Finance		Accountant		48000	Occupied
7	EMPTY	EMPTY		EMPTY		EMPTY		EMPTY	Empty
8	567	Deepak Kumar		Operations		Operations Manager		55000	Occupied
9	EMPTY	EMPTY		EMPTY		EMPTY		EMPTY	Empty
========================

1. Insert Employee Record
2. Search Employee Record
3. Delete Employee Record
4. Display Employee Database
5. Exit
Enter your choice: 1
Enter employee ID: 678
Enter name: Meera Singh
Enter department: IT
Enter position: Senior Software Engineer
Enter salary: 62000
Employee record with employee ID 678 inserted successfully!

1. Insert Employee Record
2. Search Employee Record
3. Delete Employee Record
4. Display Employee Database
5. Exit
Enter your choice: 4

=== Employee Database ===
Index	Emp ID	Name		Department	Position	Salary	Status
------------------------------------------------------------------------
0	678	Meera Singh		IT		Senior Software Engineer		62000	Occupied
1	234	Priya Patel		HR		HR Manager		52000	Occupied
2	EMPTY	EMPTY		EMPTY		EMPTY		EMPTY	Empty
3	EMPTY	EMPTY		EMPTY		EMPTY		EMPTY	Empty
4	EMPTY	EMPTY		EMPTY		EMPTY		EMPTY	Empty
5	123	Arjun Sharma		IT		Software Engineer		45000	Occupied
6	345	Rohan Gupta		Finance		Accountant		48000	Occupied
7	EMPTY	EMPTY		EMPTY		EMPTY		EMPTY	Empty
8	567	Deepak Kumar		Operations		Operations Manager		55000	Occupied
9	EMPTY	EMPTY		EMPTY		EMPTY		EMPTY	Empty
========================

