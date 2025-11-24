## Problem Statement
Implement collision resolution using linked lists.

##  Code
```cpp
#include <iostream>
#include <cstring>
using namespace std;

const int TABLE_SIZE_TVV = 10;

struct Node_TVV {
    int rollNo_TVV;
    char name_TVV[50];
    float marks_TVV;
    Node_TVV* next_TVV;
    
    Node_TVV(int rollNo, const char* name, float marks) {
        rollNo_TVV = rollNo;
        strcpy(name_TVV, name);
        marks_TVV = marks;
        next_TVV = nullptr;
    }
};

class HashTable_TVV {
private:
    Node_TVV* table_TVV[TABLE_SIZE_TVV];

    int hashFunction_TVV(int rollNo) {
        return rollNo % TABLE_SIZE_TVV;
    }

public:
    HashTable_TVV() {
        for (int i = 0; i < TABLE_SIZE_TVV; i++) {
            table_TVV[i] = nullptr;
        }
    }

    void insert_TVV(int rollNo, const char* name, float marks) {
        int index = hashFunction_TVV(rollNo);
        
        Node_TVV* current = table_TVV[index];
        while (current != nullptr) {
            if (current->rollNo_TVV == rollNo) {
                strcpy(current->name_TVV, name);
                current->marks_TVV = marks;
                cout << "Student record with roll no " << rollNo << " updated successfully!" << endl;
                return;
            }
            current = current->next_TVV;
        }
        
        Node_TVV* newNode = new Node_TVV(rollNo, name, marks);
        newNode->next_TVV = table_TVV[index];
        table_TVV[index] = newNode;
        cout << "Student record with roll no " << rollNo << " inserted successfully!" << endl;
    }

    void search_TVV(int rollNo) {
        int index = hashFunction_TVV(rollNo);
        Node_TVV* current = table_TVV[index];
        
        while (current != nullptr) {
            if (current->rollNo_TVV == rollNo) {
                cout << "\nStudent Found:" << endl;
                cout << "Roll No: " << current->rollNo_TVV << endl;
                cout << "Name: " << current->name_TVV << endl;
                cout << "Marks: " << current->marks_TVV << endl;
                return;
            }
            current = current->next_TVV;
        }
        
        cout << "Student with roll no " << rollNo << " not found!" << endl;
    }

    void remove_TVV(int rollNo) {
        int index = hashFunction_TVV(rollNo);
        Node_TVV* current = table_TVV[index];
        Node_TVV* prev = nullptr;
        
        while (current != nullptr) {
            if (current->rollNo_TVV == rollNo) {
                if (prev == nullptr) {
                    table_TVV[index] = current->next_TVV;
                } else {
                    prev->next_TVV = current->next_TVV;
                }
                delete current;
                cout << "Student with roll no " << rollNo << " deleted successfully!" << endl;
                return;
            }
            prev = current;
            current = current->next_TVV;
        }
        
        cout << "Student with roll no " << rollNo << " not found!" << endl;
    }

    void display_TVV() {
        cout << "\n=== Hash Table Contents ===" << endl;
        for (int i = 0; i < TABLE_SIZE_TVV; i++) {
            cout << "Index " << i << ": ";
            Node_TVV* current = table_TVV[i];
            if (current == nullptr) {
                cout << "Empty";
            } else {
                while (current != nullptr) {
                    cout << "[" << current->rollNo_TVV << ", " << current->name_TVV 
                         << ", " << current->marks_TVV << "] -> ";
                    current = current->next_TVV;
                }
                cout << "NULL";
            }
            cout << endl;
        }
        cout << "==========================" << endl;
    }

    ~HashTable_TVV() {
        for (int i = 0; i < TABLE_SIZE_TVV; i++) {
            Node_TVV* current = table_TVV[i];
            while (current != nullptr) {
                Node_TVV* next = current->next_TVV;
                delete current;
                current = next;
            }
        }
    }
};

int main() {
    HashTable_TVV hashTable_TVV;
    int choice, rollNo;
    char name[50];
    float marks;

    cout << "=== Hash Table Implementation with Linked Lists ===" << endl;
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
                cout << "Thank you for using Hash Table with Linked Lists!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}

## Output

=== Hash Table Implementation with Linked Lists ===
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
Index 0: Empty
Index 1: [141, Deepak, 91.5] -> NULL
Index 2: Empty
Index 3: Empty
Index 4: Empty
Index 5: Empty
Index 6: Empty
Index 7: [101, Arjun, 85.5] -> NULL
Index 8: [111, Priya, 92] -> NULL
Index 9: [131, Sunita, 88] -> [121, Rohan, 78.5] -> NULL
==========================

1. Insert Student Record
2. Search Student Record
3. Delete Student Record
4. Display Hash Table
5. Exit
Enter your choice: 2
Enter roll number to search: 121

Student Found:
Roll No: 121
Name: Rohan
Marks: 78.5

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
Index 0: Empty
Index 1: [141, Deepak, 91.5] -> NULL
Index 2: Empty
Index 3: Empty
Index 4: Empty
Index 5: Empty
Index 6: Empty
Index 7: [101, Arjun, 85.5] -> NULL
Index 8: [111, Priya, 92] -> NULL
Index 9: [131, Sunita, 88] -> NULL
==========================

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
Index 0: Empty
Index 1: [141, Deepak, 91.5] -> [151, Meera, 87.5] -> NULL
Index 2: Empty
Index 3: Empty
Index 4: Empty
Index 5: Empty
Index 6: Empty
Index 7: [101, Arjun, 85.5] -> NULL
Index 8: [111, Priya, 92] -> NULL
Index 9: [131, Sunita, 88] -> NULL
==========================
