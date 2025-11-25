## Problem Statement
Write a program to efficiently search a particular employee record by using Tree data structure. Also sort the data on emp­id in ascending order.

## Code
#include <iostream>
#include <cstring>
using namespace std;

struct Employee_tv {
    int empId;
    char name[50];
    char department[30];
    float salary;
    Employee_tv* left;
    Employee_tv* right;
    
    Employee_tv(int id, const char* empName, const char* dept, float sal) {
        empId = id;
        strcpy(name, empName);
        strcpy(department, dept);
        salary = sal;
        left = nullptr;
        right = nullptr;
    }
};

class EmployeeRecordManagement_tv {
private:
    Employee_tv* root;

    Employee_tv* insertEmployeeHelper_tv(Employee_tv* node, int id, const char* name, const char* department, float salary) {
        if (node == nullptr) {
            return new Employee_tv(id, name, department, salary);
        }
        
        if (id < node->empId) {
            node->left = insertEmployeeHelper_tv(node->left, id, name, department, salary);
        } else if (id > node->empId) {
            node->right = insertEmployeeHelper_tv(node->right, id, name, department, salary);
        }
        
        
        return node;
    }

    Employee_tv* searchEmployeeHelper_tv(Employee_tv* node, int id) {
        if (node == nullptr || node->empId == id) {
            return node;
        }
        
        if (id < node->empId) {
            return searchEmployeeHelper_tv(node->left, id);
        } else {
            return searchEmployeeHelper_tv(node->right, id);
        }
    }


    Employee_tv* deleteEmployeeHelper_tv(Employee_tv* node, int id) {
        if (node == nullptr) {
            return node;
        }
        
        if (id < node->empId) {
            node->left = deleteEmployeeHelper_tv(node->left, id);
        } else if (id > node->empId) {
            node->right = deleteEmployeeHelper_tv(node->right, id);
        } else {
            
            if (node->left == nullptr && node->right == nullptr) {
                delete node;
                return nullptr;
            }
            
            else if (node->left == nullptr) {
                Employee_tv* temp = node->right;
                delete node;
                return temp;
            } else if (node->right == nullptr) {
                Employee_tv* temp = node->left;
                delete node;
                return temp;
            }
            
            else {
                
                Employee_tv* temp = findMin_tv(node->right);
                node->empId = temp->empId;
                strcpy(node->name, temp->name);
                strcpy(node->department, temp->department);
                node->salary = temp->salary;
                node->right = deleteEmployeeHelper_tv(node->right, temp->empId);
            }
        }
        return node;
    }

    
    Employee_tv* findMin_tv(Employee_tv* node) {
        while (node && node->left != nullptr) {
            node = node->left;
        }
        return node;
    }

    void inorderHelper_tv(Employee_tv* node) {
        if (node != nullptr) {
            inorderHelper_tv(node->left);
            cout << node->empId << "\t\t" << node->name << "\t\t" << node->department << "\t\t" << node->salary << endl;
            inorderHelper_tv(node->right);
        }
    }


    void preorderHelper_tv(Employee_tv* node) {
        if (node != nullptr) {
            cout << node->empId << " ";
            preorderHelper_tv(node->left);
            preorderHelper_tv(node->right);
        }
    }

 
    int countEmployeesHelper_tv(Employee_tv* node) {
        if (node == nullptr) {
            return 0;
        }
        return 1 + countEmployeesHelper_tv(node->left) + countEmployeesHelper_tv(node->right);
    }

    
    Employee_tv* searchByNameHelper_tv(Employee_tv* node, const char* name) {
        if (node == nullptr) {
            return nullptr;
        }
        
        if (strcmp(node->name, name) == 0) {
            return node;
        }
        
        Employee_tv* found = searchByNameHelper_tv(node->left, name);
        if (found != nullptr) {
            return found;
        }
        
        return searchByNameHelper_tv(node->right, name);
    }


    void displayByDepartmentHelper_tv(Employee_tv* node, const char* department) {
        if (node != nullptr) {
            displayByDepartmentHelper_tv(node->left, department);
            if (strcmp(node->department, department) == 0) {
                cout << node->empId << "\t" << node->name << "\t" << node->salary << endl;
            }
            displayByDepartmentHelper_tv(node->right, department);
        }
    }

public:
    EmployeeRecordManagement_tv() {
        root = nullptr;
    }

    void createSystem_tv() {
        root = nullptr;
        cout << "Employee record management system created successfully!" << endl;
    }

  
    void addEmployee_tv(int id, const char* name, const char* department, float salary) {
        root = insertEmployeeHelper_tv(root, id, name, department, salary);
        cout << "Employee " << name << " (ID: " << id << ") added successfully!" << endl;
    }

  
    void searchEmployeeById_tv(int id) {
        if (root == nullptr) {
            cout << "No employees in the system!" << endl;
            return;
        }
        
        Employee_tv* employee = searchEmployeeHelper_tv(root, id);
        if (employee != nullptr) {
            cout << "\n=== Employee Record Found ===" << endl;
            cout << "Employee ID: " << employee->empId << endl;
            cout << "Name: " << employee->name << endl;
            cout << "Department: " << employee->department << endl;
            cout << "Salary: " << employee->salary << endl;
            cout << "=============================" << endl;
        } else {
            cout << "Employee with ID " << id << " not found!" << endl;
        }
    }

    void deleteEmployee_tv(int id) {
        Employee_tv* employee = searchEmployeeHelper_tv(root, id);
        if (employee != nullptr) {
            root = deleteEmployeeHelper_tv(root, id);
            cout << "Employee with ID " << id << " deleted successfully!" << endl;
        } else {
            cout << "Employee with ID " << id << " not found!" << endl;
        }
    }

    void displayAllEmployeesSorted_tv() {
        if (root == nullptr) {
            cout << "No employees in the system!" << endl;
            return;
        }
        
        cout << "\n=== All Employees (Sorted by Employee ID) ===" << endl;
        cout << "Emp ID\t\tName\t\tDepartment\t\tSalary" << endl;
        cout << "----------------------------------------------------------------" << endl;
        inorderHelper_tv(root);
        cout << "=============================================" << endl;
    }


    void displayEmployeesByDepartment_tv(const char* department) {
        if (root == nullptr) {
            cout << "No employees in the system!" << endl;
            return;
        }
        
        cout << "\n=== Employees in " << department << " Department ===" << endl;
        cout << "Emp ID\tName\t\tSalary" << endl;
        cout << "--------------------------------" << endl;
        displayByDepartmentHelper_tv(root, department);
        cout << "==================================" << endl;
    }

  
    void searchEmployeeByName_tv(const char* name) {
        if (root == nullptr) {
            cout << "No employees in the system!" << endl;
            return;
        }
        
        Employee_tv* employee = searchByNameHelper_tv(root, name);
        if (employee != nullptr) {
            cout << "\n=== Employee Record Found ===" << endl;
            cout << "Employee ID: " << employee->empId << endl;
            cout << "Name: " << employee->name << endl;
            cout << "Department: " << employee->department << endl;
            cout << "Salary: " << employee->salary << endl;
            cout << "=============================" << endl;
        } else {
            cout << "Employee with name " << name << " not found!" << endl;
        }
    }


    int countEmployees_tv() {
        int count = countEmployeesHelper_tv(root);
        cout << "Total employees: " << count << endl;
        return count;
    }

    void displayStatistics_tv() {
        cout << "\n=== System Statistics ===" << endl;
        countEmployees_tv();
        cout << "Preorder traversal: ";
        preorderHelper_tv(root);
        cout << endl;
        cout << "========================" << endl;
    }

    void inputEmployees_tv() {
        int numEmployees, id;
        char name[50], department[30];
        float salary;
        
        cout << "Enter number of employees to add: ";
        cin >> numEmployees;
        
        for (int i = 0; i < numEmployees; i++) {
            cout << "\nEmployee " << (i+1) << ":" << endl;
            cout << "Enter employee ID: ";
            cin >> id;
            cout << "Enter name (Indian name): ";
            cin >> name;
            cout << "Enter department: ";
            cin >> department;
            cout << "Enter salary: ";
            cin >> salary;
            addEmployee_tv(id, name, department, salary);
        }
    }

    void displayEmployeesBySalary_tv(float minSalary) {
        if (root == nullptr) {
            cout << "No employees in the system!" << endl;
            return;
        }
        
        cout << "\n=== Employees with Salary > " << minSalary << " ===" << endl;
        cout << "Emp ID\tName\t\tDepartment\tSalary" << endl;
        cout << "----------------------------------------" << endl;
        displayHighSalaryEmployeesHelper_tv(root, minSalary);
        cout << "========================================" << endl;
    }

    void displayHighSalaryEmployeesHelper_tv(Employee_tv* node, float minSalary) {
        if (node != nullptr) {
            displayHighSalaryEmployeesHelper_tv(node->left, minSalary);
            if (node->salary > minSalary) {
                cout << node->empId << "\t" << node->name << "\t\t" << node->department << "\t\t" << node->salary << endl;
            }
            displayHighSalaryEmployeesHelper_tv(node->right, minSalary);
        }
    }

    ~EmployeeRecordManagement_tv() {
   
        root = nullptr;
    }
};

int main() {
    EmployeeRecordManagement_tv system_tv;
    int choice, id;
    char name[50], department[30];
    float salary, minSalary;

    cout << "=== Employee Record Management System ===" << endl;
    cout << "Efficient search using Tree data structure" << endl;
    cout << "Data sorted by empId in ascending order" << endl;

    while (true) {
        cout << "\n1. Create System\n2. Add Employee\n3. Input Multiple Employees\n4. Search Employee by ID\n5. Search Employee by Name\n6. Delete Employee\n7. Display All Employees (Sorted)\n8. Display by Department\n9. Display by Salary\n10. Count Employees\n11. Display Statistics\n12. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                system_tv.createSystem_tv();
                break;

            case 2:
                cout << "Enter employee ID: ";
                cin >> id;
                cout << "Enter name : ";
                cin >> name;
                cout << "Enter department: ";
                cin >> department;
                cout << "Enter salary: ";
                cin >> salary;
                system_tv.addEmployee_tv(id, name, department, salary);
                break;

            case 3:
                system_tv.inputEmployees_tv();
                break;

            case 4:
                cout << "Enter employee ID to search: ";
                cin >> id;
                system_tv.searchEmployeeById_tv(id);
                break;

            case 5:
                cout << "Enter employee name to search: ";
                cin >> name;
                system_tv.searchEmployeeByName_tv(name);
                break;

            case 6:
                cout << "Enter employee ID to delete: ";
                cin >> id;
                system_tv.deleteEmployee_tv(id);
                break;

            case 7:
                system_tv.displayAllEmployeesSorted_tv();
                break;

            case 8:
                cout << "Enter department name: ";
                cin >> department;
                system_tv.displayEmployeesByDepartment_tv(department);
                break;

            case 9:
                cout << "Enter minimum salary threshold: ";
                cin >> minSalary;
                system_tv.displayEmployeesBySalary_tv(minSalary);
                break;

            case 10:
                system_tv.countEmployees_tv();
                break;

            case 11:
                system_tv.displayStatistics_tv();
                break;

            case 12:
                cout << "Thank you for using the Employee Record Management System!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


## Output

=== Employee Record Management System ===
Efficient search using Tree data structure
Data sorted by empId in ascending order

1. Create System
2. Add Employee
3. Input Multiple Employees
4. Search Employee by ID
5. Search Employee by Name
6. Delete Employee
7. Display All Employees (Sorted)
8. Display by Department
9. Display by Salary
10. Count Employees
11. Display Statistics
12. Exit
Enter your choice: 1
Employee record management system created successfully!

1. Create System
2. Add Employee
3. Input Multiple Employees
4. Search Employee by ID
5. Search Employee by Name
6. Delete Employee
7. Display All Employees (Sorted)
8. Display by Department
9. Display by Salary
10. Count Employees
11. Display Statistics
12. Exit
Enter your choice: 2
Enter employee ID: 101
Enter name : Arjun
Enter department: IT
Enter salary: 50000
Employee Arjun (ID: 101) added successfully!

1. Create System
2. Add Employee
3. Input Multiple Employees
4. Search Employee by ID
5. Search Employee by Name
6. Delete Employee
7. Display All Employees (Sorted)
8. Display by Department
9. Display by Salary
10. Count Employees
11. Display Statistics
12. Exit
Enter your choice: 2
Enter employee ID: 103
Enter name : Priya
Enter department: HR
Enter salary: 45000
Employee Priya (ID: 103) added successfully!

1. Create System
2. Add Employee
3. Input Multiple Employees
4. Search Employee by ID
5. Search Employee by Name
6. Delete Employee
7. Display All Employees (Sorted)
8. Display by Department
9. Display by Salary
10. Count Employees
11. Display Statistics
12. Exit
Enter your choice: 2
Enter employee ID: 102
Enter name : Rohit
Enter department: IT
Enter salary: 55000
Employee Rohit (ID: 102) added successfully!

1. Create System
2. Add Employee
3. Input Multiple Employees
4. Search Employee by ID
5. Search Employee by Name
6. Delete Employee
7. Display All Employees (Sorted)
8. Display by Department
9. Display by Salary
10. Count Employees
11. Display Statistics
12. Exit
Enter your choice: 2
Enter employee ID: 105
Enter name (Indian name): Sneha
Enter department: Finance
Enter salary: 48000
Employee Sneha (ID: 105) added successfully!

1. Create System
2. Add Employee
3. Input Multiple Employees
4. Search Employee by ID
5. Search Employee by Name
6. Delete Employee
7. Display All Employees (Sorted)
8. Display by Department
9. Display by Salary
10. Count Employees
11. Display Statistics
12. Exit
Enter your choice: 2
Enter employee ID: 104
Enter name : Vikram
Enter department: HR
Enter salary: 47000
Employee Vikram (ID: 104) added successfully!

1. Create System
2. Add Employee
3. Input Multiple Employees
4. Search Employee by ID
5. Search Employee by Name
6. Delete Employee
7. Display All Employees (Sorted)
8. Display by Department
9. Display by Salary
10. Count Employees
11. Display Statistics
12. Exit
Enter your choice: 7

=== All Employees (Sorted by Employee ID) ===
Emp ID		Name		Department		Salary
----------------------------------------------------------------
101		Arjun		IT		50000
102		Rohit		IT		55000
103		Priya		HR		45000
104		Vikram		HR		47000
105		Sneha		Finance		48000
=============================================

1. Create System
2. Add Employee
3. Input Multiple Employees
4. Search Employee by ID
5. Search Employee by Name
6. Delete Employee
7. Display All Employees (Sorted)
8. Display by Department
9. Display by Salary
10. Count Employees
11. Display Statistics
12. Exit
Enter your choice: 4
Enter employee ID to search: 102

=== Employee Record Found ===
Employee ID: 102
Name: Rohit
Department: IT
Salary: 55000
=============================

1. Create System
2. Add Employee
3. Input Multiple Employees
4. Search Employee by ID
5. Search Employee by Name
6. Delete Employee
7. Display All Employees (Sorted)
8. Display by Department
9. Display by Salary
10. Count Employees
11. Display Statistics
12. Exit
Enter your choice: 8
Enter department name: IT

=== Employees in IT Department ===
Emp ID	Name		Salary
--------------------------------
101	Arjun	50000
102	Rohit	55000
==================================

1. Create System
2. Add Employee
3. Input Multiple Employees
4. Search Employee by ID
5. Search Employee by Name
6. Delete Employee
7. Display All Employees (Sorted)
8. Display by Department
9. Display by Salary
10. Count Employees
11. Display Statistics
12. Exit
Enter your choice: 9
Enter minimum salary threshold: 48000

=== Employees with Salary > 48000 ===
Emp ID	Name		Department	Salary
----------------------------------------
101	Arjun		IT		50000
102	Rohit		IT		55000
105	Sneha		Finance		48000
========================================

1. Create System
2. Add Employee
3. Input Multiple Employees
4. Search Employee by ID
5. Search Employee by Name
6. Delete Employee
7. Display All Employees (Sorted)
8. Display by Department
9. Display by Salary
10. Count Employees
11. Display Statistics
12. Exit
Enter your choice: 10
Total employees: 5

1. Create System
2. Add Employee
3. Input Multiple Employees
4. Search Employee by ID
5. Search Employee by Name
6. Delete Employee
7. Display All Employees (Sorted)
8. Display by Department
9. Display by Salary
10. Count Employees
11. Display Statistics
12. Exit
Enter your choice: 6
Enter employee ID to delete: 103
Employee with ID 103 deleted successfully!

1. Create System
2. Add Employee
3. Input Multiple Employees
4. Search Employee by ID
5. Search Employee by Name
6. Delete Employee
7. Display All Employees (Sorted)
8. Display by Department
9. Display by Salary
10. Count Employees
11. Display Statistics
12. Exit
Enter your choice: 7

=== All Employees (Sorted by Employee ID) ===
Emp ID		Name		Department		Salary
----------------------------------------------------------------
101		Arjun		IT		50000
102		Rohit		IT		55000
104		Vikram		HR		47000
105		Sneha		Finance		48000
=============================================

1. Create System
2. Add Employee
3. Input Multiple Employees
4. Search Employee by ID
5. Search Employee by Name
6. Delete Employee
7. Display All Employees (Sorted)
8. Display by Department
9. Display by Salary
10. Count Employees
11. Display Statistics
12. Exit
Enter your choice: 11

=== System Statistics ===
Total employees: 4
Preorder traversal: 101 102 104 105 
========================


