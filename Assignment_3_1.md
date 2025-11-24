## Problem Statement
Implementation of Singly Linked List to Manage 'Vertex Club' Membership Records. The Department of Computer Engineering has a student club named 'Vertex Club' for second, third, and final year students. The first member is the President and the last member is the Secretary. Write a C++ program to: Add/delete members (including President/Secretary), Count members, Display members, Concatenate two division lists. Also implement: reverse, search by PRN, and sort by PRN operations.

## Code
```cpp
#include <iostream>
#include <cstring>
using namespace std;

struct Member_tv {
    char name[50];
    int prn;
    char position[20]; 
    Member_tv* next;
};

class VertexClub_tv {
private:
    Member_tv* head;

public:
    VertexClub_tv() {
        head = nullptr;
    }

    void addMember_tv(const char* name, int prn, const char* position) {
        Member_tv* newMember = new Member_tv;
        strcpy(newMember->name, name);
        newMember->prn = prn;
        strcpy(newMember->position, position);
        newMember->next = nullptr;

        if (head == nullptr) {
            head = newMember;
        } else {
            Member_tv* temp = head;
            while (temp->next != nullptr) {
                temp = temp->next;
            }
            temp->next = newMember;
        }
        cout << "Member " << name << " added successfully!" << endl;
    }

    void addPresident_tv(const char* name, int prn) {
        Member_tv* newPresident = new Member_tv;
        strcpy(newPresident->name, name);
        newPresident->prn = prn;
        strcpy(newPresident->position, "President");
        newPresident->next = head;
        head = newPresident;
        cout << "President " << name << " added successfully!" << endl;
    }

    void addSecretary_tv(const char* name, int prn) {
        Member_tv* newSecretary = new Member_tv;
        strcpy(newSecretary->name, name);
        newSecretary->prn = prn;
        strcpy(newSecretary->position, "Secretary");
        newSecretary->next = nullptr;

        if (head == nullptr) {
            head = newSecretary;
        } else {
            Member_tv* temp = head;
            while (temp->next != nullptr) {
                temp = temp->next;
            }
            temp->next = newSecretary;
        }
        cout << "Secretary " << name << " added successfully!" << endl;
    }

    void deleteMember_tv(int prn) {
        if (head == nullptr) {
            cout << "Club is empty!" << endl;
            return;
        }

        if (head->prn == prn) {
            Member_tv* temp = head;
            head = head->next;
            cout << "Member " << temp->name << " deleted successfully!" << endl;
            delete temp;
            return;
        }

        Member_tv* temp = head;
        while (temp->next != nullptr && temp->next->prn != prn) {
            temp = temp->next;
        }

       
        if (temp->next == nullptr) {
            cout << "Member with PRN " << prn << " not found!" << endl;
            return;
        }

      
        Member_tv* nodeToDelete = temp->next;
        temp->next = temp->next->next;
        cout << "Member " << nodeToDelete->name << " deleted successfully!" << endl;
        delete nodeToDelete;
    }

    void displayMembers_tv() {
        if (head == nullptr) {
            cout << "Club has no members!" << endl;
            return;
        }

        cout << "\n=== Vertex Club Members ===" << endl;
        cout << "Name\t\tPRN\tPosition" << endl;
        cout << "--------------------------------" << endl;
        Member_tv* temp = head;
        while (temp != nullptr) {
            cout << temp->name << "\t\t" << temp->prn << "\t" << temp->position << endl;
            temp = temp->next;
        }
    }

    int countMembers_tv() {
        int count = 0;
        Member_tv* temp = head;
        while (temp != nullptr) {
            count++;
            temp = temp->next;
        }
        return count;
    }

    void reverseList_tv() {
        Member_tv* prev = nullptr;
        Member_tv* current = head;
        Member_tv* next = nullptr;

        while (current != nullptr) {
            next = current->next;
            current->next = prev;
            prev = current;
            current = next;
        }
        head = prev;
        cout << "List reversed successfully!" << endl;
    }

    Member_tv* searchByPRN_tv(int prn) {
        Member_tv* temp = head;
        while (temp != nullptr) {
            if (temp->prn == prn) {
                return temp;
            }
            temp = temp->next;
        }
        return nullptr;
    }

    void sortByPRN_tv() {
        if (head == nullptr) return;

        Member_tv* i, *j;
        for (i = head; i->next != nullptr; i = i->next) {
            for (j = i->next; j != nullptr; j = j->next) {
                if (i->prn > j->prn) {
                    // Swap data
                    char tempName[50];
                    strcpy(tempName, i->name);
                    strcpy(i->name, j->name);
                    strcpy(j->name, tempName);

                    int tempPRN = i->prn;
                    i->prn = j->prn;
                    j->prn = tempPRN;

                    char tempPosition[20];
                    strcpy(tempPosition, i->position);
                    strcpy(i->position, j->position);
                    strcpy(j->position, tempPosition);
                }
            }
        }
        cout << "List sorted by PRN successfully!" << endl;
    }

    void concatenate_tv(VertexClub_tv& otherClub) {
        if (head == nullptr) {
            head = otherClub.head;
        } else {
            Member_tv* temp = head;
            while (temp->next != nullptr) {
                temp = temp->next;
            }
            temp->next = otherClub.head;
        }
        otherClub.head = nullptr; 
        cout << "Clubs concatenated successfully!" << endl;
    }

    ~VertexClub_tv() {
        while (head != nullptr) {
            Member_tv* temp = head;
            head = head->next;
            delete temp;
        }
    }
};

int main() {
    VertexClub_tv clubA_tv, clubB_tv;
    int choice, prn;
    char name[50], position[20];

    cout << "=== Vertex Club Management System ===" << endl;

    while (true) {
        cout << "\n1. Add President\n2. Add Member\n3. Add Secretary\n4. Delete Member\n5. Display Members\n6. Count Members\n7. Reverse List\n8. Search by PRN\n9. Sort by PRN\n10. Concatenate Clubs\n11. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter President name (Indian name): ";
                cin >> name;
                cout << "Enter PRN: ";
                cin >> prn;
                clubA_tv.addPresident_tv(name, prn);
                break;

            case 2:
                cout << "Enter Member name (Indian name): ";
                cin >> name;
                cout << "Enter PRN: ";
                cin >> prn;
                clubA_tv.addMember_tv(name, prn, "Member");
                break;

            case 3:
                cout << "Enter Secretary name (Indian name): ";
                cin >> name;
                cout << "Enter PRN: ";
                cin >> prn;
                clubA_tv.addSecretary_tv(name, prn);
                break;

            case 4:
                cout << "Enter PRN of member to delete: ";
                cin >> prn;
                clubA_tv.deleteMember_tv(prn);
                break;

            case 5:
                clubA_tv.displayMembers_tv();
                break;

            case 6:
                cout << "Total members: " << clubA_tv.countMembers_tv() << endl;
                break;

            case 7:
                clubA_tv.reverseList_tv();
                break;

            case 8:
                cout << "Enter PRN to search: ";
                cin >> prn;
                {
                    Member_tv* member = clubA_tv.searchByPRN_tv(prn);
                    if (member != nullptr) {
                        cout << "Member found: " << member->name << " (" << member->position << ")" << endl;
                    } else {
                        cout << "Member with PRN " << prn << " not found!" << endl;
                    }
                }
                break;

            case 9:
                clubA_tv.sortByPRN_tv();
                break;

            case 10:
                cout << "Adding members to Club B:" << endl;
                clubB_tv.addPresident_tv("Rajesh", 101);
                clubB_tv.addMember_tv("Sneha", 102, "Member");
                clubB_tv.addSecretary_tv("Priya", 103);
                clubA_tv.concatenate_tv(clubB_tv);
                break;

            case 11:
                cout << "Exiting program..." << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


## Output
=== Vertex Club Management System ===

1. Add President
2. Add Member
3. Add Secretary
4. Delete Member
5. Display Members
6. Count Members
7. Reverse List
8. Search by PRN
9. Sort by PRN
10. Concatenate Clubs
11. Exit
Enter your choice: 1
Enter President name (Indian name): Arjun
Enter PRN: 100
President Arjun added successfully!

1. Add President
2. Add Member
3. Add Secretary
4. Delete Member
5. Display Members
6. Count Members
7. Reverse List
8. Search by PRN
9. Sort by PRN
10. Concatenate Clubs
11. Exit
Enter your choice: 2
Enter Member name (Indian name): Vikram
Enter PRN: 105
Member Vikram added successfully!

1. Add President
2. Add Member
3. Add Secretary
4. Delete Member
5. Display Members
6. Count Members
7. Reverse List
8. Search by PRN
9. Sort by PRN
10. Concatenate Clubs
11. Exit
Enter your choice: 3
Enter Secretary name (Indian name): Priya
Enter PRN: 110
Secretary Priya added successfully!

1. Add President
2. Add Member
3. Add Secretary
4. Delete Member
5. Display Members
6. Count Members
7. Reverse List
8. Search by PRN
9. Sort by PRN
10. Concatenate Clubs
11. Exit
Enter your choice: 5

=== Vertex Club Members ===
Name		PRN	Position
--------------------------------
Arjun		100	President
Vikram		105	Member
Priya		110	Secretary

1. Add President
2. Add Member
3. Add Secretary
4. Delete Member
5. Display Members
6. Count Members
7. Reverse List
8. Search by PRN
9. Sort by PRN
10. Concatenate Clubs
11. Exit
Enter your choice: 6
Total members: 3

1. Add President
2. Add Member
3. Add Secretary
4. Delete Member
5. Display Members
6. Count Members
7. Reverse List
8. Search by PRN
9. Sort by PRN
10. Concatenate Clubs
11. Exit
Enter your choice: 8
Enter PRN to search: 105
Member found: Vikram (Member)
