## Statement
Write a program to arrange the list of employees as per the average of their height and weight by using Merge and Selection sorting method. Analyse their time complexities and conclude which algorithm will take less time to sort the list.

## Solution Code
#include <iostream>
#include <ctime>
#include <cstring>
using namespace std;

struct Employee_tv {
    char name[50];
    float height;  
    float weight;  
    float avgHeightWeight;
};

void calculateAvg_tv(Employee_tv &emp) {
    emp.avgHeightWeight = (emp.height + emp.weight) / 2;
}

void displayEmployees_tv(Employee_tv employees[], int n, const char* message) {
    cout << "\n" << message << endl;
    cout << "Name\t\tHeight(cm)\tWeight(kg)\tAvg(H+W)/2" << endl;
    cout << "----------------------------------------------------------------" << endl;
    for(int i = 0; i < n; i++) {
        cout << employees[i].name << "\t\t" << employees[i].height << "\t\t" 
             << employees[i].weight << "\t\t" << employees[i].avgHeightWeight << endl;
    }
}

void selectionSort_tv(Employee_tv employees[], int n, long &comparisons_tv) {
    comparisons_tv = 0;
    
    for(int i = 0; i < n-1; i++) {
        int minIndex = i;
        
        for(int j = i+1; j < n; j++) {
            comparisons_tv++;
            if(employees[j].avgHeightWeight < employees[minIndex].avgHeightWeight) {
                minIndex = j;
            }
        }
        
       
        if(minIndex != i) {
            Employee_tv temp = employees[i];
            employees[i] = employees[minIndex];
            employees[minIndex] = temp;
        }
    }
}

void merge_tv(Employee_tv employees[], int left, int mid, int right, long &comparisons_tv) {
    int n1 = mid - left + 1;
    int n2 = right - mid;
    
    
    Employee_tv *leftArr_tv = new Employee_tv[n1];
    Employee_tv *rightArr_tv = new Employee_tv[n2];
    
    
    for(int i = 0; i < n1; i++) {
        leftArr_tv[i] = employees[left + i];
    }
    for(int j = 0; j < n2; j++) {
        rightArr_tv[j] = employees[mid + 1 + j];
    }
    
    
    
    int i = 0, j = 0, k = left;
    
    while(i < n1 && j < n2) {
        comparisons_tv++;
        if(leftArr_tv[i].avgHeightWeight <= rightArr_tv[j].avgHeightWeight) {
            employees[k] = leftArr_tv[i];
            i++;
        } else {
            employees[k] = rightArr_tv[j];
            j++;
        }
        k++;
    }
    
    
    while(i < n1) {
        employees[k] = leftArr_tv[i];
        i++;
        k++;
    }
    
    while(j < n2) {
        employees[k] = rightArr_tv[j];
        j++;
        k++;
    }
    
    delete[] leftArr_tv;
    delete[] rightArr_tv;
}

void mergeSort_tv(Employee_tv employees[], int left, int right, long &comparisons_tv) {
    if(left < right) {
        int mid = left + (right - left) / 2;
        
        mergeSort_tv(employees, left, mid, comparisons_tv);
        mergeSort_tv(employees, mid + 1, right, comparisons_tv);
        merge_tv(employees, left, mid, right, comparisons_tv);
    }
}

int main() {
    const int MAX_EMPLOYEES_TV = 50;
    Employee_tv employeesSelection_tv[MAX_EMPLOYEES_TV], employeesMerge_tv[MAX_EMPLOYEES_TV];
    int n;
    
    cout << "=== Employee Sorting Comparison: Selection Sort vs Merge Sort ===" << endl;
    cout << "Enter number of employees: ";
    cin >> n;
    
    if(n > MAX_EMPLOYEES_TV) {
        cout << "Too many employees. Maximum allowed: " << MAX_EMPLOYEES_TV << endl;
        return 1;
    }
    
    
    cout << "\nEnter employee details:" << endl;
    for(int i = 0; i < n; i++) {
        cout << "\nEmployee " << (i+1) << ":" << endl;
        cout << "Name (Indian name): ";
        cin >> employeesSelection_tv[i].name;
        cout << "Height (cm): ";
        cin >> employeesSelection_tv[i].height;
        cout << "Weight (kg): ";
        cin >> employeesSelection_tv[i].weight;
        calculateAvg_tv(employeesSelection_tv[i]);
        
        
        strcpy(employeesMerge_tv[i].name, employeesSelection_tv[i].name);
        employeesMerge_tv[i].height = employeesSelection_tv[i].height;
        employeesMerge_tv[i].weight = employeesSelection_tv[i].weight;
        employeesMerge_tv[i].avgHeightWeight = employeesSelection_tv[i].avgHeightWeight;
    }
    
    cout << "\nOriginal Employee Records:";
    displayEmployees_tv(employeesSelection_tv, n, "Before sorting");
    
    
    long selectionComparisons_tv = 0;
    clock_t selectionStart_tv = clock();
    selectionSort_tv(employeesSelection_tv, n, selectionComparisons_tv);
    clock_t selectionEnd_tv = clock();
    double selectionTime_tv = ((double)(selectionEnd_tv - selectionStart_tv)) / CLOCKS_PER_SEC;
    
    cout << "\nAfter Selection Sort:";
    displayEmployees_tv(employeesSelection_tv, n, "Sorted by average of height and weight");
    cout << "Selection Sort Comparisons: " << selectionComparisons_tv << endl;
    cout << "Selection Sort Time: " << selectionTime_tv << " seconds" << endl;
    
   
    long mergeComparisons_tv = 0;
    clock_t mergeStart_tv = clock();
    mergeSort_tv(employeesMerge_tv, 0, n-1, mergeComparisons_tv);
    clock_t mergeEnd_tv = clock();
    double mergeTime_tv = ((double)(mergeEnd_tv - mergeStart_tv)) / CLOCKS_PER_SEC;
    
    cout << "\nAfter Merge Sort:";
    displayEmployees_tv(employeesMerge_tv, n, "Sorted by average of height and weight");
    cout << "Merge Sort Comparisons: " << mergeComparisons_tv << endl;
    cout << "Merge Sort Time: " << mergeTime_tv << " seconds" << endl;
    
    cout << "\n=== Comparison Results ===" << endl;
    cout << "Selection Sort - Comparisons: " << selectionComparisons_tv << ", Time: " << selectionTime_tv << "s" << endl;
    cout << "Merge Sort - Comparisons: " << mergeComparisons_tv << ", Time: " << mergeTime_tv << "s" << endl;
    
    if(selectionComparisons_tv > mergeComparisons_tv) {
        cout << "Merge Sort is more efficient with " << (selectionComparisons_tv - mergeComparisons_tv) << " fewer comparisons!" << endl;
    } else if(selectionComparisons_tv < mergeComparisons_tv) {
        cout << "Selection Sort is more efficient with " << (mergeComparisons_tv - selectionComparisons_tv) << " fewer comparisons!" << endl;
    } else {
        cout << "Both algorithms performed the same number of comparisons." << endl;
    }
    
    if(selectionTime_tv > mergeTime_tv) {
        cout << "Merge Sort is faster by " << (selectionTime_tv - mergeTime_tv) << " seconds!" << endl;
    } else if(selectionTime_tv < mergeTime_tv) {
        cout << "Selection Sort is faster by " << (mergeTime_tv - selectionTime_tv) << " seconds!" << endl;
    } else {
        cout << "Both algorithms took the same time." << endl;
    }
    
    cout << "\nTime Complexity Analysis:" << endl;
    cout << "Selection Sort: O(n²) - Better for small datasets" << endl;
    cout << "Merge Sort: O(n log n) - Better for large datasets" << endl;
    
    return 0;
}


## Output

=== Employee Sorting Comparison: Selection Sort vs Merge Sort ===
Enter number of employees: 5

Enter employee details:

Employee 1:
Name : Arjun
Height (cm): 175
Weight (kg): 70

Employee 2:
Name ): Priya
Height (cm): 160
Weight (kg): 55

Employee 3:
Name : Rohit
Height (cm): 180
Weight (kg): 80

Employee 4:
Name : Sneha
Height (cm): 165
Weight (kg): 60

Employee 5:
Name : Vikram
Height (cm): 170
Weight (kg): 75

Original Employee Records:
Before sorting
Name		Height(cm)	Weight(kg)	Avg(H+W)/2
----------------------------------------------------------------
Arjun		175		70		122.5
Priya		160		55		107.5
Rohit		180		80		130
Sneha		165		60		112.5
Vikram		170		75		122.5

After Selection Sort:
Sorted by average of height and weight
Name		Height(cm)	Weight(kg)	Avg(H+W)/2
----------------------------------------------------------------
Priya		160		55		107.5
Sneha		165		60		112.5
Arjun		175		70		122.5
Vikram		170		75		122.5
Rohit		180		80		130
Selection Sort Comparisons: 10
Selection Sort Time: 0.000005 seconds

After Merge Sort:
Sorted by average of height and weight
Name		Height(cm)	Weight(kg)	Avg(H+W)/2
----------------------------------------------------------------
Priya		160		55		107.5
Sneha		165		60		112.5
Arjun		175		70		122.5
Vikram		170		75		122.5
Rohit		180		80		130
Merge Sort Comparisons: 8
Merge Sort Time: 0.000003 seconds

== Comparison Results ==
Selection Sort - Comparisons: 10, Time: 5e-06s
Merge Sort - Comparisons: 8, Time: 3e-06s
Merge Sort is more efficient with 2 fewer comparisons!
Merge Sort is faster by 2e-06 seconds!

Time Complexity Analysis:
Selection Sort: O(n²) - Better for small datasets
Merge Sort: O(n log n) - Better for large datasets

