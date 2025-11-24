## Problem Statement
Implement Bubble sort using Doubly Linked List

##  Code
```cpp
#include <iostream>
#include <cstdlib>
#include <ctime>
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

    void insertAtEnd_tv(int value) {
        Node_tv* newNode = new Node_tv;
        newNode->data = value;
        newNode->next = nullptr;
        newNode->prev = nullptr;

        if (head == nullptr) {
            head = tail = newNode;
        } else {
            tail->next = newNode;
            newNode->prev = tail;
            tail = newNode;
        }
    }

    void generateRandomData_tv(int count) {
        srand(time(0));
        clear_tv();
        
        cout << "Generated random data: ";
        for (int i = 0; i < count; i++) {
            int randomValue = rand() % 100 + 1;  
            insertAtEnd_tv(randomValue);
            cout << randomValue << " ";
        }
        cout << endl;
    }

    void inputData_tv() {
        int count, value;
        cout << "Enter number of elements: ";
        cin >> count;
        
        clear_tv();
        cout << "Enter " << count << " elements:" << endl;
        for (int i = 0; i < count; i++) {
            cout << "Element " << (i+1) << ": ";
            cin >> value;
            insertAtEnd_tv(value);
        }
    }

    void displayList_tv(const char* message = "") {
        cout << message;
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

    void bubbleSort_tv() {
        if (head == nullptr || head->next == nullptr) {
            return; 
        }

        bool swapped;
        Node_tv* ptr1;
        Node_tv* lptr = nullptr;

        cout << "\n--- Bubble Sort Pass-by-Pass ---" << endl;
        int pass = 1;

        do {
            swapped = false;
            ptr1 = head;
            cout << "Pass " << pass << ": ";

            while (ptr1->next != lptr) {
                if (ptr1->data > ptr1->next->data) {
                    
                    int temp = ptr1->data;
                    ptr1->data = ptr1->next->data;
                    ptr1->next->data = temp;
                    swapped = true;
                }
                ptr1 = ptr1->next;
            }
            
            
            Node_tv* temp = head;
            while (temp != nullptr) {
                cout << temp->data << " ";
                temp = temp->next;
            }
            cout << endl;

            lptr = ptr1;
            pass++;
        } while (swapped);
    }

    void bubbleSortByPointer_tv() {
        if (head == nullptr || head->next == nullptr) {
            return; 
        }

        bool swapped;
        Node_tv* ptr1;
        Node_tv* lptr = nullptr;

        cout << "\n--- Bubble Sort by Pointer Manipulation ---" << endl;
        int pass = 1;

        do {
            swapped = false;
            ptr1 = head;
            cout << "Pass " << pass << ": ";

            while (ptr1->next != lptr) {
                if (ptr1->data > ptr1->next->data) {
                  
                    Node_tv* temp = ptr1->next;
                    
                  
                    if (ptr1->prev != nullptr) {
                        ptr1->prev->next = temp;
                    } else {
                        head = temp;  
                    }
                    
                    
                    if (temp->next != nullptr) {
                        temp->next->prev = ptr1;
                    } else {
                        tail = ptr1;  
                    }
                    
                    
                    ptr1->next = temp->next;
                    temp->prev = ptr1->prev;
                    ptr1->prev = temp;
                    temp->next = ptr1;
                    
                    swapped = true;
                }
                
                if (ptr1->next != nullptr) {
                    ptr1 = ptr1->next;
                }
            }
            
  
            Node_tv* temp = head;
            while (temp != nullptr) {
                cout << temp->data << " ";
                temp = temp->next;
            }
            cout << endl;

            
            lptr = ptr1;
            pass++;
        } while (swapped);
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
    }

    ~DoublyLinkedList_tv() {
        clear_tv();
    }
};

int main() {
    DoublyLinkedList_tv dll_tv;
    int choice;

    cout << "=== Bubble Sort using Doubly Linked List ===" << endl;

    while (true) {
        cout << "\n1. Generate Random Data\n2. Input Data Manually\n3. Display List\n4. Bubble Sort (Data Swap)\n5. Bubble Sort (Pointer Manipulation)\n6. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1: {
                int count;
                cout << "Enter number of elements to generate: ";
                cin >> count;
                dll_tv.generateRandomData_tv(count);
                break;
            }

            case 2:
                dll_tv.inputData_tv();
                break;

            case 3:
                dll_tv.displayList_tv("Current List: ");
                break;

            case 4:
                dll_tv.displayList_tv("Before Sorting: ");
                dll_tv.bubbleSort_tv();
                dll_tv.displayList_tv("After Sorting: ");
                break;

            case 5:
                dll_tv.displayList_tv("Before Sorting: ");
                dll_tv.bubbleSortByPointer_tv();
                dll_tv.displayList_tv("After Sorting: ");
                break;

            case 6:
                cout << "Thank you for using the Doubly Linked List Bubble Sort!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}

##  Output
```
=== Bubble Sort using Doubly Linked List ===

1. Generate Random Data
2. Input Data Manually
3. Display List
4. Bubble Sort (Data Swap)
5. Bubble Sort (Pointer Manipulation)
6. Exit
Enter your choice: 1
Enter number of elements to generate: 6
Generated random data: 42 68 35 1 70 25 

1. Generate Random Data
2. Input Data Manually
3. Display List
4. Bubble Sort (Data Swap)
5. Bubble Sort (Pointer Manipulation)
6. Exit
Enter your choice: 3
Current List: 42 68 35 1 70 25 

1. Generate Random Data
2. Input Data Manually
3. Display List
4. Bubble Sort (Data Swap)
5. Bubble Sort (Pointer Manipulation)
6. Exit
Enter your choice: 4
Before Sorting: 42 68 35 1 70 25 

--- Bubble Sort Pass-by-Pass ---
Pass 1: 42 35 1 68 25 70 
Pass 2: 35 1 42 25 68 70 
Pass 3: 1 35 25 42 68 70 
Pass 4: 1 25 35 42 68 70 
Pass 5: 1 25 35 42 68 70 
After Sorting: 1 25 35 42 68 70 

1. Generate Random Data
2. Input Data Manually
3. Display List
4. Bubble Sort (Data Swap)
5. Bubble Sort (Pointer Manipulation)
6. Exit
Enter your choice: 1
Enter number of elements to generate: 5
Generated random data: 87 93 12 56 34 

1. Generate Random Data
2. Input Data Manually
3. Display List
4. Bubble Sort (Data Swap)
5. Bubble Sort (Pointer Manipulation)
6. Exit
Enter your choice: 5
Before Sorting: 87 93 12 56 34 

--- Bubble Sort by Pointer Manipulation ---
Pass 1: 87 12 56 34 93 
Pass 2: 12 56 34 87 93 
Pass 3: 12 34 56 87 93 
Pass 4: 12 34 56 87 93 
After Sorting: 12 34 56 87 93 

