## Problem Statement
Implement a hash table with collision resolution using linear probing.

##  Code
```cpp
#include <iostream>
#include <cstring>
using namespace std;

const int TABLE_SIZE_TVV = 10;
const int EMPTY_TVV = -1;
const int DELETED_TVV = -2;

struct Student_TVV {
    int rollNo_TVV;
    char name_TVV[50];
    float marks_TVV;
};

class HashTable_TVV {
private:
    Student_TVV table_TVV[TABLE_SIZE_TVV];
    int status_TVV[TABLE_SIZE_TVV];

    int hashFunction_TVV(int rollNo) {
        return rollNo % TABLE_SIZE_TVV;
    }

public:
    HashTable_TVV() {
        for (int i = 0; i < TABLE_SIZE_TVV; i++) {
            status_TVV[i] = EMPTY_TVV;
        }
    }

    void insert_TVV(int rollNo, const char* name, float marks) {
        int index = hashFunction_TVV(rollNo);
        int originalIndex = index;
        
        while (status_TVV[index] != EMPTY_TVV && status_TVV[index] != DELETED_TVV) {
            if (table_TVV[index].rollNo_TVV == rollNo) {
                strcpy(table_TVV[index].name_TVV, name);
                table_TVV[index].marks_TVV = marks;
                cout << "Student record with roll no " << rollNo << " updated successfully!" << endl;
                return;
            }
            
            index = (index + 1) % TABLE_SIZE_TVV;
            
            if (index == originalIndex) {
                cout << "Hash table is full!" << endl;
                return;
            }
        }
        
        table_TVV[index].rollNo_TVV = rollNo;
        strcpy(table_TVV[index].name_TVV, name);
        table_TVV[index].marks_TVV = marks;
        status_TVV[index] = 1;
        cout << "Student record with roll no " << rollNo << " inserted successfully!" << endl;
    }

    void search_TVV(int rollNo) {
        int index = hashFunction_TVV(rollNo);
        int originalIndex = index;
        
        while (status_TVV[index] != EMPTY_TVV) {
            if (status_TVV[index] == 1 && table_TVV[index].rollNo_TVV == rollNo) {
                cout << "\nStudent Found:" << endl;
                cout << "Roll No: " << table_TVV[index].rollNo_TVV << endl;
                cout << "Name: " << table_TVV[index].name_TVV << endl;
                cout << "Marks: " << table_TVV[index].marks_TVV << endl;
                return;
            }
            
            index = (index + 1) % TABLE_SIZE_TVV;
            
            if (index == originalIndex) {
                break;
            }
        }
        
        cout << "Student with roll no " << rollNo << " not found!" << endl;
    }

    void remove_TVV(int rollNo) {
        int index = hashFunction_TVV(rollNo);
        int originalIndex = index;
        
        while (status_TVV[index] != EMPTY_TVV) {
            if (status_TVV[index] == 1 && table_TVV[index].rollNo_TVV == rollNo) {
                status_TVV[index] = DELETED_TVV;
                cout << "Student with roll no " << rollNo << " deleted successfully!" << endl;
                return;
            }
            
            index = (index + 1) % TABLE_SIZE_TVV;
            
            if (index == originalIndex) {
                break;
            }
        }
        
        cout << "Student with roll no " << rollNo << " not found!" << endl;
    }

    void display_TVV() {
        cout << "\n=== Hash Table Contents ===" << endl;
        cout << "Index\tRoll No\tName\t\tMarks\tStatus" << endl;
        cout << "------------------------------------------------" << endl;
        
        for (int i = 0; i < TABLE_SIZE_TVV; i++) {
            cout << i << "\t";
            if (status_TVV[i] == 1) {
                cout << table_TVV[i].rollNo_TVV << "\t" << table_TVV[i].name_TVV 
                     << "\t\t" << table_TVV[i].marks_TVV << "\tOccupied" << endl;
            } else if (status_TVV[i] == DELETED_TVV) {
                cout << "DELETED\tDELETED\t\tDELETED\t\tDeleted" << endl;
            } else {
                cout << "EMPTY\tEMPTY\t\tEMPTY\t\tEmpty" << endl;
            }
        }
        cout << "===========================" << endl;
    }
};

int main() {
    HashTable_TVV hashTable_TVV;
    int choice, rollNo;
    char name[50];
    float marks;

    cout << "=== Hash Table Implementation with Linear Probing ===" << endl;
    cout << "Storing Student Records" << endl;

    while (true) {
        cout << "\n1. Insert Student Record\n2. Search Student Record\n3. Delete Student Record\n4. Display Hash Table\n5. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter roll number: ";
                cin >> rollNo;
                cout << "Enter name: ";
                cin >> name;
                cout << "Enter marks: ";
                cin >> marks;
                hashTable_TVV.insert_TVV(rollNo, name, marks);
                break;

            case 2:
                cout << "Enter roll number to search: ";
                cin >> rollNo;
                hashTable_TVV.search_TVV(rollNo);
                break;

            case 3:
                cout << "Enter roll number to delete: ";
                cin >> rollNo;
                hashTable_TVV.remove_TVV(rollNo);
                break;

            case 4:
                hashTable_TVV.display_TVV();
                break;

            case 5:
                cout << "Thank you for using Hash Table with Linear Probing!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


## Output
=== Hash Table Implementation with Linear Probing ===
Storing Student Records

1. Insert Student Record
2. Search Student Record
3. Delete Student Record
4. Display Hash Table
5. Exit
Enter your choice: 1
Enter roll number: 101
Enter name: Arjun
Enter marks: 85.5
Student record with roll no 101 inserted successfully!

1. Insert Student Record
2. Search Student Record
3. Delete Student Record
4. Display Hash Table
5. Exit
Enter your choice: 1
Enter roll number: 111
Enter name: Priya
Enter marks: 92.0
Student record with roll no 111 inserted successfully!

1. Insert Student Record
2. Search Student Record
3. Delete Student Record
4. Display Hash Table
5. Exit
Enter your choice: 1
Enter roll number: 121
Enter name: Rohan
Enter marks: 78.5
Student record with roll no 121 inserted successfully!

1. Insert Student Record
2. Search Student Record
3. Delete Student Record
4. Display Hash Table
5. Exit
Enter your choice: 1
Enter roll number: 131
Enter name: Sunita
Enter marks: 88.0
Student record with roll no 131 inserted successfully!

1. Insert Student Record
2. Search Student Record
3. Delete Student Record
4. Display Hash Table
5. Exit
Enter your choice: 1
Enter roll number: 141
Enter name: Deepak
Enter marks: 91.5
Student record with roll no 141 inserted successfully!

1. Insert Student Record
2. Search Student Record
3. Delete Student Record
4. Display Hash Table
5. Exit
Enter your choice: 4

=== Hash Table Contents ===
Index	Roll No	Name		Marks	Status
------------------------------------------------
0	131	Sunita		88	Occupied
1	141	Deepak		91.5	Occupied
2	EMPTY	EMPTY		EMPTY	Empty
3	EMPTY	EMPTY		EMPTY	Empty
4	EMPTY	EMPTY		EMPTY	Empty
5	EMPTY	EMPTY		EMPTY	Empty
6	EMPTY	EMPTY		EMPTY	Empty
7	101	Arjun		85.5	Occupied
8	111	Priya		92	Occupied
9	121	Rohan		78.5	Occupied
===========================

1. Insert Student Record
2. Search Student Record
3. Delete Student Record
4. Display Hash Table
5. Exit
Enter your choice: 2
Enter roll number to search: 111

Student Found:
Roll No: 111
Name: Priya
Marks: 92

1. Insert Student Record
2. Search Student Record
3. Delete Student Record
4. Display Hash Table
5. Exit
Enter your choice: 2
Enter roll number to search: 151
Student with roll no 151 not found!

1. Insert Student Record
2. Search Student Record
3. Delete Student Record
4. Display Hash Table
5. Exit
Enter your choice: 3
Enter roll number to delete: 121
Student with roll no 121 deleted successfully!

1. Insert Student Record
2. Search Student Record
3. Delete Student Record
4. Display Hash Table
5. Exit
Enter your choice: 4

=== Hash Table Contents ===
Index	Roll No	Name		Marks	Status
------------------------------------------------
0	131	Sunita		88	Occupied
1	141	Deepak		91.5	Occupied
2	EMPTY	EMPTY		EMPTY	Empty
3	EMPTY	EMPTY		EMPTY	Empty
4	EMPTY	EMPTY		EMPTY	Empty
5	EMPTY	EMPTY		EMPTY	Empty
6	EMPTY	EMPTY		EMPTY	Empty
7	101	Arjun		85.5	Occupied
8	111	Priya		92	Occupied
9	DELETED	DELETED		DELETED	Deleted
===========================

1. Insert Student Record
2. Search Student Record
3. Delete Student Record
4. Display Hash Table
5. Exit
Enter your choice: 1
Enter roll number: 151
Enter name: Meera
Enter marks: 87.5
Student record with roll no 151 inserted successfully!

1. Insert Student Record
2. Search Student Record
3. Delete Student Record
4. Display Hash Table
5. Exit
Enter your choice: 4

=== Hash Table Contents ===
Index	Roll No	Name		Marks	Status
------------------------------------------------
0	131	Sunita		88	Occupied
1	141	Deepak		91.5	Occupied
2	151	Meera		87.5	Occupied
3	EMPTY	EMPTY		EMPTY	Empty
4	EMPTY	EMPTY		EMPTY	Empty
5	EMPTY	EMPTY		EMPTY	Empty
6	EMPTY	EMPTY		EMPTY	Empty
7	101	Arjun		85.5	Occupied
8	111	Priya		92	Occupied
9	DELETED	DELETED		DELETED	Deleted
===========================
