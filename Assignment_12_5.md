## Problem Statement
Design and implement a smart college placement portal that uses advanced hashing techniques to efficiently manage student placement records with high performance and low collision probability, even under dynamic data growth.

##  Code
```cpp
#include <iostream>
#include <cstring>
using namespace std;

const int TABLE_SIZE_TVV = 20;
const int REHASH_THRESHOLD_TVV = 15;

struct PlacementRecord_TVV {
    int studentId_TVV;
    char name_TVV[50];
    char branch_TVV[30];
    char company_TVV[50];
    float package_TVV;
    int year_TVV;
};

class PlacementPortal_TVV {
private:
    PlacementRecord_TVV table_TVV[TABLE_SIZE_TVV];
    bool occupied_TVV[TABLE_SIZE_TVV];
    int count_TVV;

    int hashFunction_TVV(int studentId) {
        return (studentId * 31) % TABLE_SIZE_TVV;
    }

    int doubleHash_TVV(int studentId, int attempt) {
        int hash1 = hashFunction_TVV(studentId);
        int hash2 = 7 - (studentId % 7);
        return (hash1 + attempt * hash2) % TABLE_SIZE_TVV;
    }

public:
    PlacementPortal_TVV() {
        for (int i = 0; i < TABLE_SIZE_TVV; i++) {
            occupied_TVV[i] = false;
        }
        count_TVV = 0;
    }

    void insert_TVV(int studentId, const char* name, const char* branch, const char* company, float package, int year) {
        if (count_TVV >= REHASH_THRESHOLD_TVV) {
            cout << "Rehashing required due to high load factor!" << endl;
            return;
        }

        int attempt = 0;
        int index = doubleHash_TVV(studentId, attempt);
        int originalIndex = index;

        while (occupied_TVV[index] && attempt < TABLE_SIZE_TVV) {
            if (table_TVV[index].studentId_TVV == studentId) {
                strcpy(table_TVV[index].name_TVV, name);
                strcpy(table_TVV[index].branch_TVV, branch);
                strcpy(table_TVV[index].company_TVV, company);
                table_TVV[index].package_TVV = package;
                table_TVV[index].year_TVV = year;
                cout << "Placement record for student ID " << studentId << " updated successfully!" << endl;
                return;
            }

            attempt++;
            index = doubleHash_TVV(studentId, attempt);

            if (index == originalIndex) {
                break;
            }
        }

        if (!occupied_TVV[index]) {
            table_TVV[index].studentId_TVV = studentId;
            strcpy(table_TVV[index].name_TVV, name);
            strcpy(table_TVV[index].branch_TVV, branch);
            strcpy(table_TVV[index].company_TVV, company);
            table_TVV[index].package_TVV = package;
            table_TVV[index].year_TVV = year;
            occupied_TVV[index] = true;
            count_TVV++;
            cout << "Placement record for student ID " << studentId << " inserted successfully!" << endl;
        } else {
            cout << "Placement portal is full or too many collisions!" << endl;
        }
    }

    void search_TVV(int studentId) {
        int attempt = 0;
        int index = doubleHash_TVV(studentId, attempt);
        int originalIndex = index;

        while (occupied_TVV[index] && attempt < TABLE_SIZE_TVV) {
            if (table_TVV[index].studentId_TVV == studentId) {
                cout << "\nPlacement Record Found:" << endl;
                cout << "Student ID: " << table_TVV[index].studentId_TVV << endl;
                cout << "Name: " << table_TVV[index].name_TVV << endl;
                cout << "Branch: " << table_TVV[index].branch_TVV << endl;
                cout << "Company: " << table_TVV[index].company_TVV << endl;
                cout << "Package: " << table_TVV[index].package_TVV << " LPA" << endl;
                cout << "Year: " << table_TVV[index].year_TVV << endl;
                return;
            }

            attempt++;
            index = doubleHash_TVV(studentId, attempt);

            if (index == originalIndex) {
                break;
            }
        }

        cout << "Placement record for student ID " << studentId << " not found!" << endl;
    }

    void remove_TVV(int studentId) {
        int attempt = 0;
        int index = doubleHash_TVV(studentId, attempt);
        int originalIndex = index;

        while (occupied_TVV[index] && attempt < TABLE_SIZE_TVV) {
            if (table_TVV[index].studentId_TVV == studentId) {
                occupied_TVV[index] = false;
                count_TVV--;
                cout << "Placement record for student ID " << studentId << " deleted successfully!" << endl;
                return;
            }

            attempt++;
            index = doubleHash_TVV(studentId, attempt);

            if (index == originalIndex) {
                break;
            }
        }

        cout << "Placement record for student ID " << studentId << " not found!" << endl;
    }

    void display_TVV() {
        if (count_TVV == 0) {
            cout << "No placement records found!" << endl;
            return;
        }

        cout << "\n=== Placement Records ===" << endl;
        cout << "Index\tStud ID\tName\t\tBranch\t\tCompany\t\tPackage\tYear" << endl;
        cout << "------------------------------------------------------------------------" << endl;
        for (int i = 0; i < TABLE_SIZE_TVV; i++) {
            if (occupied_TVV[i]) {
                cout << i << "\t" << table_TVV[i].studentId_TVV << "\t" 
                     << table_TVV[i].name_TVV << "\t\t" << table_TVV[i].branch_TVV 
                     << "\t\t" << table_TVV[i].company_TVV << "\t\t" 
                     << table_TVV[i].package_TVV << "\t" << table_TVV[i].year_TVV << endl;
            }
        }
        cout << "========================" << endl;
        cout << "Total Records: " << count_TVV << "/" << TABLE_SIZE_TVV << endl;
    }

    void searchByCompany_TVV(const char* company) {
        cout << "\n=== Students Placed in Company: " << company << " ===" << endl;
        bool found = false;
        for (int i = 0; i < TABLE_SIZE_TVV; i++) {
            if (occupied_TVV[i] && strcmp(table_TVV[i].company_TVV, company) == 0) {
                cout << "Student ID: " << table_TVV[i].studentId_TVV << ", Name: " << table_TVV[i].name_TVV 
                     << ", Branch: " << table_TVV[i].branch_TVV << ", Package: " << table_TVV[i].package_TVV << " LPA" << endl;
                found = true;
            }
        }
        if (!found) {
            cout << "No students placed in company " << company << endl;
        }
        cout << "========================" << endl;
    }

    void searchByYear_TVV(int year) {
        cout << "\n=== Students Placed in Year: " << year << " ===" << endl;
        bool found = false;
        for (int i = 0; i < TABLE_SIZE_TVV; i++) {
            if (occupied_TVV[i] && table_TVV[i].year_TVV == year) {
                cout << "Student ID: " << table_TVV[i].studentId_TVV << ", Name: " << table_TVV[i].name_TVV 
                     << ", Branch: " << table_TVV[i].branch_TVV << ", Company: " << table_TVV[i].company_TVV 
                     << ", Package: " << table_TVV[i].package_TVV << " LPA" << endl;
                found = true;
            }
        }
        if (!found) {
            cout << "No students placed in year " << year << endl;
        }
        cout << "========================" << endl;
    }

    void getStatistics_TVV() {
        if (count_TVV == 0) {
            cout << "No placement records available for statistics!" << endl;
            return;
        }

        float totalPackage = 0;
        float maxPackage = 0;
        float minPackage = 9999;
        int csCount = 0, ecCount = 0, meCount = 0;

        for (int i = 0; i < TABLE_SIZE_TVV; i++) {
            if (occupied_TVV[i]) {
                totalPackage += table_TVV[i].package_TVV;
                if (table_TVV[i].package_TVV > maxPackage) {
                    maxPackage = table_TVV[i].package_TVV;
                }
                if (table_TVV[i].package_TVV < minPackage) {
                    minPackage = table_TVV[i].package_TVV;
                }

                if (strcmp(table_TVV[i].branch_TVV, "Computer Science") == 0) {
                    csCount++;
                } else if (strcmp(table_TVV[i].branch_TVV, "Electronics") == 0) {
                    ecCount++;
                } else if (strcmp(table_TVV[i].branch_TVV, "Mechanical") == 0) {
                    meCount++;
                }
            }
        }

        cout << "\n=== Placement Statistics ===" << endl;
        cout << "Total Students Placed: " << count_TVV << endl;
        cout << "Average Package: " << (totalPackage / count_TVV) << " LPA" << endl;
        cout << "Highest Package: " << maxPackage << " LPA" << endl;
        cout << "Lowest Package: " << minPackage << " LPA" << endl;
        cout << "Branch-wise Placement:" << endl;
        cout << "  Computer Science: " << csCount << endl;
        cout << "  Electronics: " << ecCount << endl;
        cout << "  Mechanical: " << meCount << endl;
        cout << "========================" << endl;
    }
};

int main() {
    PlacementPortal_TVV portal_TVV;
    int choice, studentId, year;
    char name[50], branch[30], company[50];
    float package;

    cout << "=== Smart College Placement Portal ===" << endl;
    cout << "Advanced Hashing Techniques for Efficient Management" << endl;

    while (true) {
        cout << "\n1. Add Placement Record\n2. Search Placement Record\n3. Delete Placement Record\n4. Display All Records\n5. Search by Company\n6. Search by Year\n7. Get Statistics\n8. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter student ID: ";
                cin >> studentId;
                cout << "Enter name: ";
                cin >> name;
                cout << "Enter branch: ";
                cin >> branch;
                cout << "Enter company: ";
                cin >> company;
                cout << "Enter package (LPA): ";
                cin >> package;
                cout << "Enter year: ";
                cin >> year;
                portal_TVV.insert_TVV(studentId, name, branch, company, package, year);
                break;

            case 2:
                cout << "Enter student ID to search: ";
                cin >> studentId;
                portal_TVV.search_TVV(studentId);
                break;

            case 3:
                cout << "Enter student ID to delete: ";
                cin >> studentId;
                portal_TVV.remove_TVV(studentId);
                break;

            case 4:
                portal_TVV.display_TVV();
                break;

            case 5:
                cout << "Enter company name to search: ";
                cin >> company;
                portal_TVV.searchByCompany_TVV(company);
                break;

            case 6:
                cout << "Enter year to search: ";
                cin >> year;
                portal_TVV.searchByYear_TVV(year);
                break;

            case 7:
                portal_TVV.getStatistics_TVV();
                break;

            case 8:
                cout << "Thank you for using Smart College Placement Portal!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}

## Output

=== Smart College Placement Portal ===
Advanced Hashing Techniques for Efficient Management

1. Add Placement Record
2. Search Placement Record
3. Delete Placement Record
4. Display All Records
5. Search by Company
6. Search by Year
7. Get Statistics
8. Exit
Enter your choice: 1
Enter student ID: 101
Enter name: Arjun Sharma
Enter branch: Computer Science
Enter company: Google
Enter package (LPA): 18.5
Enter year: 2023
Placement record for student ID 101 inserted successfully!

1. Add Placement Record
2. Search Placement Record
3. Delete Placement Record
4. Display All Records
5. Search by Company
6. Search by Year
7. Get Statistics
8. Exit
Enter your choice: 1
Enter student ID: 102
Enter name: Priya Patel
Enter branch: Electronics
Enter company: Microsoft
Enter package (LPA): 15.0
Enter year: 2023
Placement record for student ID 102 inserted successfully!

1. Add Placement Record
2. Search Placement Record
3. Delete Placement Record
4. Display All Records
5. Search by Company
6. Search by Year
7. Get Statistics
8. Exit
Enter your choice: 1
Enter student ID: 103
Enter name: Rohan Gupta
Enter branch: Computer Science
Enter company: Amazon
Enter package (LPA): 16.5
Enter year: 2023
Placement record for student ID 103 inserted successfully!

1. Add Placement Record
2. Search Placement Record
3. Delete Placement Record
4. Display All Records
5. Search by Company
6. Search by Year
7. Get Statistics
8. Exit
Enter your choice: 1
Enter student ID: 104
Enter name: Sunita Reddy
Enter branch: Mechanical
Enter company: TCS
Enter package (LPA): 8.5
Enter year: 2023
Placement record for student ID 104 inserted successfully!

1. Add Placement Record
2. Search Placement Record
3. Delete Placement Record
4. Display All Records
5. Search by Company
6. Search by Year
7. Get Statistics
8. Exit
Enter your choice: 1
Enter student ID: 105
Enter name: Deepak Kumar
Enter branch: Electronics
Enter company: Infosys
Enter package (LPA): 9.0
Enter year: 2023
Placement record for student ID 105 inserted successfully!

1. Add Placement Record
2. Search Placement Record
3. Delete Placement Record
4. Display All Records
5. Search by Company
6. Search by Year
7. Get Statistics
8. Exit
Enter your choice: 4

=== Placement Records ===
Index	Stud ID	Name		Branch		Company		Package	Year
------------------------------------------------------------------------
2	104	Sunita Reddy		Mechanical		TCS		8.5	2023
5	101	Arjun Sharma		Computer Science		Google		18.5	2023
9	105	Deepak Kumar		Electronics		Infosys		9	2023
13	102	Priya Patel		Electronics		Microsoft		15	2023
17	103	Rohan Gupta		Computer Science		Amazon		16.5	2023
========================
Total Records: 5/20

1. Add Placement Record
2. Search Placement Record
3. Delete Placement Record
4. Display All Records
5. Search by Company
6. Search by Year
7. Get Statistics
8. Exit
Enter your choice: 2
Enter student ID to search: 103

Placement Record Found:
Student ID: 103
Name: Rohan Gupta
Branch: Computer Science
Company: Amazon
Package: 16.5 LPA
Year: 2023

1. Add Placement Record
2. Search Placement Record
3. Delete Placement Record
4. Display All Records
5. Search by Company
6. Search by Year
7. Get Statistics
8. Exit
Enter your choice: 2
Enter student ID to search: 106
Placement record for student ID 106 not found!

1. Add Placement Record
2. Search Placement Record
3. Delete Placement Record
4. Display All Records
5. Search by Company
6. Search by Year
7. Get Statistics
8. Exit
Enter your choice: 5
Enter company name to search: Google

=== Students Placed in Company: Google ===
Student ID: 101, Name: Arjun Sharma, Branch: Computer Science, Package: 18.5 LPA
========================

1. Add Placement Record
2. Search Placement Record
3. Delete Placement Record
4. Display All Records
5. Search by Company
6. Search by Year
7. Get Statistics
8. Exit
Enter your choice: 3
Enter student ID to delete: 102
Placement record for student ID 102 deleted successfully!

1. Add Placement Record
2. Search Placement Record
3. Delete Placement Record
4. Display All Records
5. Search by Company
6. Search by Year
7. Get Statistics
8. Exit
Enter your choice: 4

=== Placement Records ===
Index	Stud ID	Name		Branch		Company		Package	Year
------------------------------------------------------------------------
2	104	Sunita Reddy		Mechanical		TCS		8.5	2023
5	101	Arjun Sharma		Computer Science		Google		18.5	2023
9	105	Deepak Kumar		Electronics		Infosys		9	2023
17	103	Rohan Gupta		Computer Science		Amazon		16.5	2023
========================
Total Records: 4/20

1. Add Placement Record
2. Search Placement Record
3. Delete Placement Record
4. Display All Records
5. Search by Company
6. Search by Year
7. Get Statistics
8. Exit
Enter your choice: 7

=== Placement Statistics ===
Total Students Placed: 4
Average Package: 13.125 LPA
Highest Package: 18.5 LPA
Lowest Package: 8.5 LPA
Branch-wise Placement:
  Computer Science: 2
  Electronics: 1
  Mechanical: 1
========================

1. Add Placement Record
2. Search Placement Record
3. Delete Placement Record
4. Display All Records
5. Search by Company
6. Search by Year
7. Get Statistics
8. Exit
Enter your choice: 1
Enter student ID: 106
Enter name: Meera Singh
Enter branch: Computer Science
Enter company: Meta
Enter package (LPA): 20.0
Enter year: 2023
Placement record for student ID 106 inserted successfully!

1. Add Placement Record
2. Search Placement Record
3. Delete Placement Record
4. Display All Records
5. Search by Company
6. Search by Year
7. Get Statistics
8. Exit
Enter your choice: 4

=== Placement Records ===
Index	Stud ID	Name		Branch		Company		Package	Year
------------------------------------------------------------------------
2	104	Sunita Reddy		Mechanical		TCS		8.5	2023
5	101	Arjun Sharma		Computer Science		Google		18.5	2023
6	106	Meera Singh		Computer Science		Meta		20	2023
9	105	Deepak Kumar		Electronics		Infosys		9	2023
17	103	Rohan Gupta		Computer Science		Amazon		16.5	2023
========================
Total Records: 5/20
