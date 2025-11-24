## Problem Statement
Write a C++ program to implement a Set using a Generalized Linked List (GLL). For example: Let S = { p, q, {r, s, t, {}, {u, v}, w, x, {y, z}, a1, b1} }. Store this structure using a Generalized Linked List and display the elements in correct set notation format.

##Code
```cpp
#include <iostream>
#include <cstring>
using namespace std;

enum NodeType_tv { ATOM, LIST };

struct GLLNode_tv {
    NodeType_tv type;
    union {
        char atom[20];
        struct {
            GLLNode_tv* head;
            GLLNode_tv* tail;
        } list;
    };
    GLLNode_tv* next;
};

class GeneralizedLinkedList_tv {
private:
    GLLNode_tv* head;

    GLLNode_tv* createAtomNode_tv(const char* atomData) {
        GLLNode_tv* newNode = new GLLNode_tv;
        newNode->type = ATOM;
        strcpy(newNode->atom, atomData);
        newNode->next = nullptr;
        return newNode;
    }

    GLLNode_tv* createListNode_tv() {
        GLLNode_tv* newNode = new GLLNode_tv;
        newNode->type = LIST;
        newNode->list.head = nullptr;
        newNode->list.tail = nullptr;
        newNode->next = nullptr;
        return newNode;
    }

    void destroyGLL_tv(GLLNode_tv* node) {
        if (node == nullptr) return;

        if (node->type == LIST) {
            destroyGLL_tv(node->list.head);
        }
        destroyGLL_tv(node->next);
        delete node;
    }

    void displayGLL_tv(GLLNode_tv* node) {
        if (node == nullptr) return;

        if (node->type == ATOM) {
            cout << node->atom;
        } else {
            cout << "{";
            displayGLL_tv(node->list.head);
            cout << "}";
        }

        if (node->next != nullptr) {
            cout << ", ";
            displayGLL_tv(node->next);
        }
    }

    GLLNode_tv* copyGLL_tv(GLLNode_tv* node) {
        if (node == nullptr) return nullptr;

        GLLNode_tv* newNode = new GLLNode_tv;
        newNode->type = node->type;

        if (node->type == ATOM) {
            strcpy(newNode->atom, node->atom);
        } else {
            newNode->list.head = copyGLL_tv(node->list.head);
            newNode->list.tail = nullptr; // We don't maintain tail in copies
        }

        newNode->next = copyGLL_tv(node->next);
        return newNode;
    }

public:
    GeneralizedLinkedList_tv() {
        head = nullptr;
    }

    void createSetExample_tv() {
        
        destroyGLL_tv(head); 
        head = nullptr;

        
        head = createAtomNode_tv("p");
        head->next = createAtomNode_tv("q");
        head->next->next = createListNode_tv();

        
        GLLNode_tv* nestedList = head->next->next;
        nestedList->list.head = createAtomNode_tv("r");
        nestedList->list.head->next = createAtomNode_tv("s");
        nestedList->list.head->next->next = createAtomNode_tv("t");
        nestedList->list.head->next->next->next = createListNode_tv(); 
        nestedList->list.head->next->next->next->next = createListNode_tv();         nestedList->list.head->next->next->next->next->next = createAtomNode_tv("w");
        nestedList->list.head->next->next->next->next->next->next = createAtomNode_tv("x");
        nestedList->list.head->next->next->next->next->next->next->next = createListNode_tv(); 
        nestedList->list.head->next->next->next->next->next->next->next->next = createAtomNode_tv("a1");
        nestedList->list.head->next->next->next->next->next->next->next->next->next = createAtomNode_tv("b1");

        
        GLLNode_tv* emptySet = nestedList->list.head->next->next->next;
        emptySet->list.head = nullptr;

       
        GLLNode_tv* uvSet = nestedList->list.head->next->next->next->next;
        uvSet->list.head = createAtomNode_tv("u");
        uvSet->list.head->next = createAtomNode_tv("v");

        
        GLLNode_tv* yzSet = nestedList->list.head->next->next->next->next->next->next->next;
        yzSet->list.head = createAtomNode_tv("y");
        yzSet->list.head->next = createAtomNode_tv("z");

        cout << "Generalized Linked List Set created successfully!" << endl;
    }

    void displaySet_tv() {
        cout << "\nSet S = {";
        displayGLL_tv(head);
        cout << "}" << endl;
    }

    bool isMember_tv(const char* element) {
        return isMemberHelper_tv(head, element);
    }

    bool isMemberHelper_tv(GLLNode_tv* node, const char* element) {
        if (node == nullptr) return false;

        if (node->type == ATOM) {
            if (strcmp(node->atom, element) == 0) {
                return true;
            }
        } else {
           
            if (isMemberHelper_tv(node->list.head, element)) {
                return true;
            }
        }

        
        return isMemberHelper_tv(node->next, element);
    }

    int countElements_tv() {
        return countElementsHelper_tv(head);
    }

    int countElementsHelper_tv(GLLNode_tv* node) {
        if (node == nullptr) return 0;

        int count = 0;
        if (node->type == ATOM) {
            count = 1;
        } else {
           
            count = countElementsHelper_tv(node->list.head);
        }

        
        return count + countElementsHelper_tv(node->next);
    }

    ~GeneralizedLinkedList_tv() {
        destroyGLL_tv(head);
    }
};

int main() {
    GeneralizedLinkedList_tv gllSet_tv;
    int choice;
    char element[20];

    cout << "=== Generalized Linked List Set Implementation ===" << endl;

    while (true) {
        cout << "\n1. Create Example Set\n2. Display Set\n3. Check Membership\n4. Count Elements\n5. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                gllSet_tv.createSetExample_tv();
                break;

            case 2:
                gllSet_tv.displaySet_tv();
                break;

            case 3:
                cout << "Enter element to check membership: ";
                cin >> element;
                if (gllSet_tv.isMember_tv(element)) {
                    cout << element << " is a member of the set." << endl;
                } else {
                    cout << element << " is NOT a member of the set." << endl;
                }
                break;

            case 4:
                cout << "Total elements in set: " << gllSet_tv.countElements_tv() << endl;
                break;

            case 5:
                cout << "Thank you for using the GLL Set Implementation!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


## Output

=== Generalized Linked List Set Implementation ===

1. Create Example Set
2. Display Set
3. Check Membership
4. Count Elements
5. Exit
Enter your choice: 1
Generalized Linked List Set created successfully!

1. Create Example Set
2. Display Set
3. Check Membership
4. Count Elements
5. Exit
Enter your choice: 2

Set S = {p, q, {r, s, t, {}, {u, v}, w, x, {y, z}, a1, b1}}

1. Create Example Set
2. Display Set
3. Check Membership
4. Count Elements
5. Exit
Enter your choice: 3
Enter element to check membership: u
u is a member of the set.

1. Create Example Set
2. Display Set
3. Check Membership
4. Count Elements
5. Exit
Enter your choice: 3
Enter element to check membership: z
z is a member of the set.

1. Create Example Set
2. Display Set
3. Check Membership
4. Count Elements
5. Exit
Enter your choice: 3
Enter element to check membership: a
a is NOT a member of the set.

1. Create Example Set
2. Display Set
3. Check Membership
4. Count Elements
5. Exit
Enter your choice: 4
Total elements in set: 12

