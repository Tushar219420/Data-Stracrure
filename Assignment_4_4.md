## Problem Statement
WAP to create a doubly linked list and perform following operations on it. A) Insert (all cases) 2. Delete (all cases).

##  Code
```cpp
#include <iostream>
using namespace std;

struct Node_tv {
    int data;
    Node_tv* next;
    Node_tv* prev;
};

class DoublyLinkedList_tv {
private:
    Node_tv* head;
    Node_tv* tail;

public:
    DoublyLinkedList_tv() {
        head = nullptr;
        tail = nullptr;
    }

    
    void insertAtBeginning_tv(int value) {
        Node_tv* newNode = new Node_tv;
        newNode->data = value;
        newNode->next = head;
        newNode->prev = nullptr;

        if (head != nullptr) {
            head->prev = newNode;
        } else {
            
            tail = newNode;
        }
        head = newNode;
        cout << "Inserted " << value << " at the beginning." << endl;
    }

    
    void insertAtEnd_tv(int value) {
        Node_tv* newNode = new Node_tv;
        newNode->data = value;
        newNode->next = nullptr;
        newNode->prev = tail;

        if (tail != nullptr) {
            tail->next = newNode;
        } else {
            
            head = newNode;
        }
        tail = newNode;
        cout << "Inserted " << value << " at the end." << endl;
    }

    
    void insertAtPosition_tv(int value, int position) {
        if (position < 1) {
            cout << "Invalid position!" << endl;
            return;
        }

        if (position == 1) {
            insertAtBeginning_tv(value);
            return;
        }

        Node_tv* newNode = new Node_tv;
        newNode->data = value;

        Node_tv* temp = head;
        for (int i = 1; i < position - 1 && temp != nullptr; i++) {
            temp = temp->next;
        }

        if (temp == nullptr) {
            cout << "Position out of bounds!" << endl;
            delete newNode;
            return;
        }

        newNode->next = temp->next;
        newNode->prev = temp;
        
        if (temp->next != nullptr) {
            temp->next->prev = newNode;
        } else {
           
            tail = newNode;
        }
        
        temp->next = newNode;
        cout << "Inserted " << value << " at position " << position << "." << endl;
    }

   
    void deleteFromBeginning_tv() {
        if (head == nullptr) {
            cout << "List is empty! Cannot delete." << endl;
            return;
        }

        Node_tv* temp = head;
        int value = temp->data;
        
        if (head == tail) {
            
            head = tail = nullptr;
        } else {
            head = head->next;
            head->prev = nullptr;
        }
        
        delete temp;
        cout << "Deleted " << value << " from the beginning." << endl;
    }

    
    void deleteFromEnd_tv() {
        if (tail == nullptr) {
            cout << "List is empty! Cannot delete." << endl;
            return;
        }

        Node_tv* temp = tail;
        int value = temp->data;
        
        if (head == tail) {
            
            head = tail = nullptr;
        } else {
            tail = tail->prev;
            tail->next = nullptr;
        }
        
        delete temp;
        cout << "Deleted " << value << " from the end." << endl;
    }

    
    void deleteByValue_tv(int value) {
        if (head == nullptr) {
            cout << "List is empty! Cannot delete." << endl;
            return;
        }

       
        if (head->data == value) {
            Node_tv* temp = head;
            if (head == tail) {
             
                head = tail = nullptr;
            } else {
                head = head->next;
                head->prev = nullptr;
            }
            delete temp;
            cout << "Deleted " << value << " from the list." << endl;
            return;
        }

        
        Node_tv* temp = head;
        while (temp != nullptr && temp->data != value) {
            temp = temp->next;
        }

   
        if (temp == nullptr) {
            cout << "Value " << value << " not found in the list!" << endl;
            return;
        }

       
        if (temp == tail) {
            tail = tail->prev;
            tail->next = nullptr;
        } else {
          
            temp->prev->next = temp->next;
            temp->next->prev = temp->prev;
        }

        delete temp;
        cout << "Deleted " << value << " from the list." << endl;
    }

   
    void deleteAtPosition_tv(int position) {
        if (position < 1 || head == nullptr) {
            cout << "Invalid position or list is empty!" << endl;
            return;
        }

        if (position == 1) {
            deleteFromBeginning_tv();
            return;
        }

        Node_tv* temp = head;
        for (int i = 1; i < position && temp != nullptr; i++) {
            temp = temp->next;
        }

        if (temp == nullptr) {
            cout << "Position out of bounds!" << endl;
            return;
        }

    
        if (temp == tail) {
            tail = tail->prev;
            tail->next = nullptr;
        } else {
        
            temp->prev->next = temp->next;
            temp->next->prev = temp->prev;
        }

        int value = temp->data;
        delete temp;
        cout << "Deleted " << value << " from position " << position << "." << endl;
    }

    void displayForward_tv() {
        cout << "List (Forward): ";
        if (head == nullptr) {
            cout << "List is empty!" << endl;
            return;
        }

        Node_tv* temp = head;
        while (temp != nullptr) {
            cout << temp->data << " ";
            temp = temp->next;
        }
        cout << endl;
    }

    void displayBackward_tv() {
        cout << "List (Backward): ";
        if (tail == nullptr) {
            cout << "List is empty!" << endl;
            return;
        }

        Node_tv* temp = tail;
        while (temp != nullptr) {
            cout << temp->data << " ";
            temp = temp->prev;
        }
        cout << endl;
    }

    bool isEmpty_tv() {
        return head == nullptr;
    }

    int countNodes_tv() {
        int count = 0;
        Node_tv* temp = head;
        while (temp != nullptr) {
            count++;
            temp = temp->next;
        }
        return count;
    }

    void clear_tv() {
        while (head != nullptr) {
            Node_tv* temp = head;
            head = head->next;
            delete temp;
        }
        tail = nullptr;
        cout << "List cleared!" << endl;
    }

    ~DoublyLinkedList_tv() {
        clear_tv();
    }
};

int main() {
    DoublyLinkedList_tv dll_tv;
    int choice, value, position;

    cout << "=== Doubly Linked List Operations ===" << endl;

    while (true) {
        cout << "\n1. Insert at Beginning\n2. Insert at End\n3. Insert at Position\n4. Delete from Beginning\n5. Delete from End\n6. Delete by Value\n7. Delete at Position\n8. Display Forward\n9. Display Backward\n10. Check if Empty\n11. Count Nodes\n12. Clear List\n13. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter value to insert: ";
                cin >> value;
                dll_tv.insertAtBeginning_tv(value);
                break;

            case 2:
                cout << "Enter value to insert: ";
                cin >> value;
                dll_tv.insertAtEnd_tv(value);
                break;

            case 3:
                cout << "Enter value to insert: ";
                cin >> value;
                cout << "Enter position: ";
                cin >> position;
                dll_tv.insertAtPosition_tv(value, position);
                break;

            case 4:
                dll_tv.deleteFromBeginning_tv();
                break;

            case 5:
                dll_tv.deleteFromEnd_tv();
                break;

            case 6:
                cout << "Enter value to delete: ";
                cin >> value;
                dll_tv.deleteByValue_tv(value);
                break;

            case 7:
                cout << "Enter position to delete: ";
                cin >> position;
                dll_tv.deleteAtPosition_tv(position);
                break;

            case 8:
                dll_tv.displayForward_tv();
                break;

            case 9:
                dll_tv.displayBackward_tv();
                break;

            case 10:
                if (dll_tv.isEmpty_tv()) {
                    cout << "List is empty!" << endl;
                } else {
                    cout << "List is not empty." << endl;
                }
                break;

            case 11:
                cout << "Number of nodes in list: " << dll_tv.countNodes_tv() << endl;
                break;

            case 12:
                dll_tv.clear_tv();
                break;

            case 13:
                cout << "Thank you for using the Doubly Linked List Operations!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}

## Output

=== Doubly Linked List Operations ===

1. Insert at Beginning
2. Insert at End
3. Insert at Position
4. Delete from Beginning
5. Delete from End
6. Delete by Value
7. Delete at Position
8. Display Forward
9. Display Backward
10. Check if Empty
11. Count Nodes
12. Clear List
13. Exit
Enter your choice: 1
Enter value to insert: 10
Inserted 10 at the beginning.

1. Insert at Beginning
2. Insert at End
3. Insert at Position
4. Delete from Beginning
5. Delete from End
6. Delete by Value
7. Delete at Position
8. Display Forward
9. Display Backward
10. Check if Empty
11. Count Nodes
12. Clear List
13. Exit
Enter your choice: 2
Enter value to insert: 20
Inserted 20 at the end.

1. Insert at Beginning
2. Insert at End
3. Insert at Position
4. Delete from Beginning
5. Delete from End
6. Delete by Value
7. Delete at Position
8. Display Forward
9. Display Backward
10. Check if Empty
11. Count Nodes
12. Clear List
13. Exit
Enter your choice: 2
Enter value to insert: 30
Inserted 30 at the end.

1. Insert at Beginning
2. Insert at End
3. Insert at Position
4. Delete from Beginning
5. Delete from End
6. Delete by Value
7. Delete at Position
8. Display Forward
9. Display Backward
10. Check if Empty
11. Count Nodes
12. Clear List
13. Exit
Enter your choice: 3
Enter value to insert: 15
Enter position: 2
Inserted 15 at position 2.

1. Insert at Beginning
2. Insert at End
3. Insert at Position
4. Delete from Beginning
5. Delete from End
6. Delete by Value
7. Delete at Position
8. Display Forward
9. Display Backward
10. Check if Empty
11. Count Nodes
12. Clear List
13. Exit
Enter your choice: 8
List (Forward): 10 15 20 30 

1. Insert at Beginning
2. Insert at End
3. Insert at Position
4. Delete from Beginning
5. Delete from End
6. Delete by Value
7. Delete at Position
8. Display Forward
9. Display Backward
10. Check if Empty
11. Count Nodes
12. Clear List
13. Exit
Enter your choice: 9
List (Backward): 30 20 15 10 

1. Insert at Beginning
2. Insert at End
3. Insert at Position
4. Delete from Beginning
5. Delete from End
6. Delete by Value
7. Delete at Position
8. Display Forward
9. Display Backward
10. Check if Empty
11. Count Nodes
12. Clear List
13. Exit
Enter your choice: 6
Enter value to delete: 15
Deleted 15 from the list.

1. Insert at Beginning
2. Insert at End
3. Insert at Position
4. Delete from Beginning
5. Delete from End
6. Delete by Value
7. Delete at Position
8. Display Forward
9. Display Backward
10. Check if Empty
11. Count Nodes
12. Clear List
13. Exit
Enter your choice: 8
List (Forward): 10 20 30 

1. Insert at Beginning
2. Insert at End
3. Insert at Position
4. Delete from Beginning
5. Delete from End
6. Delete by Value
7. Delete at Position
8. Display Forward
9. Display Backward
10. Check if Empty
11. Count Nodes
12. Clear List
13. Exit
Enter your choice: 4
Deleted 10 from the beginning.

1. Insert at Beginning
2. Insert at End
3. Insert at Position
4. Delete from Beginning
5. Delete from End
6. Delete by Value
7. Delete at Position
8. Display Forward
9. Display Backward
10. Check if Empty
11. Count Nodes
12. Clear List
13. Exit
Enter your choice: 8
List (Forward): 20 30 

1. Insert at Beginning
2. Insert at End
3. Insert at Position
4. Delete from Beginning
5. Delete from End
6. Delete by Value
7. Delete at Position
8. Display Forward
9. Display Backward
10. Check if Empty
11. Count Nodes
12. Clear List
13. Exit
Enter your choice: 11
Number of nodes in list: 2

