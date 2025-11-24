## Problem Statement
Given a list, split it into two sublists — one for the front half, and one for the back half. If the number of elements is odd, the extra element should go in the front list. So FrontBackSplit() on the list {2, 3, 5, 7, 11} should yield the two lists {2, 3, 5} and {7, 11}. Getting this right for all the cases is harder than it looks. You should check your solution against a few cases (length = 2, length = 3, length = 4) to make sure that the list gets split correctly near the shortlist boundary conditions. If it works right for length=4, it probably works right for length=1000. You will probably need special case code to deal with the (length <2) cases.

##  Code
```cpp
#include <iostream>
using namespace std;

struct Node_tv {
    int data;
    Node_tv* next;
};

class LinkedList_tv {
private:
    Node_tv* head;

public:
    LinkedList_tv() {
        head = nullptr;
    }

    void insertAtEnd_tv(int value) {
        Node_tv* newNode = new Node_tv;
        newNode->data = value;
        newNode->next = nullptr;

        if (head == nullptr) {
            head = newNode;
        } else {
            Node_tv* temp = head;
            while (temp->next != nullptr) {
                temp = temp->next;
            }
            temp->next = newNode;
        }
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

    int countNodes_tv() {
        int count = 0;
        Node_tv* temp = head;
        while (temp != nullptr) {
            count++;
            temp = temp->next;
        }
        return count;
    }

    
    void frontBackSplit_tv(LinkedList_tv& front, LinkedList_tv& back) {
      
        front.clear_tv();
        back.clear_tv();
        
       
        if (head == nullptr) {
            return;  
        }
        
        if (head->next == nullptr) {
            
            front.insertAtEnd_tv(head->data);
            return;
        }
        
        
        Node_tv* slow = head;
        Node_tv* fast = head;
        Node_tv* slowPrev = nullptr;
        
        while (fast != nullptr && fast->next != nullptr) {
            slowPrev = slow;
            slow = slow->next;
            fast = fast->next->next;
        }
        
        
        Node_tv* temp = head;
        while (temp != slow) {
            front.insertAtEnd_tv(temp->data);
            temp = temp->next;
        }
        front.insertAtEnd_tv(temp->data);  
        
        
        temp = slow->next;
        while (temp != nullptr) {
            back.insertAtEnd_tv(temp->data);
            temp = temp->next;
        }
    }
    
    
    void frontBackSplitByCount_tv(LinkedList_tv& front, LinkedList_tv& back) {
       
        front.clear_tv();
        back.clear_tv();
        
        int length = countNodes_tv();
        
        
        if (length == 0) {
            return;  
        }
        
        if (length == 1) {
            
            front.insertAtEnd_tv(head->data);
            return;
        }
        
        
        int frontLength = (length + 1) / 2;         
        
        Node_tv* temp = head;
        for (int i = 0; i < frontLength && temp != nullptr; i++) {
            front.insertAtEnd_tv(temp->data);
            temp = temp->next;
        }
        
        
        while (temp != nullptr) {
            back.insertAtEnd_tv(temp->data);
            temp = temp->next;
        }
    }

    void clear_tv() {
        while (head != nullptr) {
            Node_tv* temp = head;
            head = head->next;
            delete temp;
        }
    }

    ~LinkedList_tv() {
        clear_tv();
    }
};

int main() {
    LinkedList_tv originalList_tv, frontList_tv, backList_tv;
    int choice;

    cout << "=== FrontBackSplit of Linked List ===" << endl;
    cout << "Splits a list into front and back halves." << endl;
    cout << "For odd length, extra element goes to front list." << endl;

    while (true) {
        cout << "\n1. Input Data Manually\n2. Generate Random Data\n3. Display Original List\n4. FrontBackSplit (Fast/Slow Pointer)\n5. FrontBackSplit (Count Method)\n6. Test Boundary Cases\n7. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                originalList_tv.inputData_tv();
                break;

            case 2: {
                int count;
                cout << "Enter number of elements to generate: ";
                cin >> count;
                originalList_tv.generateRandomData_tv(count);
                break;
            }

            case 3:
                originalList_tv.displayList_tv("Original List: ");
                cout << "Length: " << originalList_tv.countNodes_tv() << endl;
                break;

            case 4:
                originalList_tv.displayList_tv("Original List: ");
                originalList_tv.frontBackSplit_tv(frontList_tv, backList_tv);
                frontList_tv.displayList_tv("Front List: ");
                backList_tv.displayList_tv("Back List: ");
                break;

            case 5:
                originalList_tv.displayList_tv("Original List: ");
                originalList_tv.frontBackSplitByCount_tv(frontList_tv, backList_tv);
                frontList_tv.displayList_tv("Front List: ");
                backList_tv.displayList_tv("Back List: ");
                break;

            case 6: {
                cout << "\n--- Testing Boundary Cases ---" << endl;

                cout << "\nTest Case 1: Empty List" << endl;
                LinkedList_tv emptyList_tv;
                emptyList_tv.frontBackSplit_tv(frontList_tv, backList_tv);
                emptyList_tv.displayList_tv("Original: ");
                frontList_tv.displayList_tv("Front: ");
                backList_tv.displayList_tv("Back: ");
                
             
                cout << "\nTest Case 2: Length = 1" << endl;
                LinkedList_tv singleList_tv;
                singleList_tv.insertAtEnd_tv(42);
                singleList_tv.frontBackSplit_tv(frontList_tv, backList_tv);
                singleList_tv.displayList_tv("Original: ");
                frontList_tv.displayList_tv("Front: ");
                backList_tv.displayList_tv("Back: ");
               
                cout << "\nTest Case 3: Length = 2" << endl;
                LinkedList_tv twoList_tv;
                twoList_tv.insertAtEnd_tv(10);
                twoList_tv.insertAtEnd_tv(20);
                twoList_tv.frontBackSplit_tv(frontList_tv, backList_tv);
                twoList_tv.displayList_tv("Original: ");
                frontList_tv.displayList_tv("Front: ");
                backList_tv.displayList_tv("Back: ");
                
            
                cout << "\nTest Case 4: Length = 3" << endl;
                LinkedList_tv threeList_tv;
                threeList_tv.insertAtEnd_tv(10);
                threeList_tv.insertAtEnd_tv(20);
                threeList_tv.insertAtEnd_tv(30);
                threeList_tv.frontBackSplit_tv(frontList_tv, backList_tv);
                threeList_tv.displayList_tv("Original: ");
                frontList_tv.displayList_tv("Front: ");
                backList_tv.displayList_tv("Back: ");
                
                cout << "\nTest Case 5: Length = 4" << endl;
                LinkedList_tv fourList_tv;
                fourList_tv.insertAtEnd_tv(10);
                fourList_tv.insertAtEnd_tv(20);
                fourList_tv.insertAtEnd_tv(30);
                fourList_tv.insertAtEnd_tv(40);
                fourList_tv.frontBackSplit_tv(frontList_tv, backList_tv);
                fourList_tv.displayList_tv("Original: ");
                frontList_tv.displayList_tv("Front: ");
                backList_tv.displayList_tv("Back: ");
                
                break;
            }

            case 7:
                cout << "Thank you for using the FrontBackSplit Implementation!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


##  Output

=== FrontBackSplit of Linked List ===
Splits a list into front and back halves.
For odd length, extra element goes to front list.

1. Input Data Manually
2. Generate Random Data
3. Display Original List
4. FrontBackSplit (Fast/Slow Pointer)
5. FrontBackSplit (Count Method)
6. Test Boundary Cases
7. Exit
Enter your choice: 2
Enter number of elements to generate: 5
Generated random data: 42 68 35 1 70 

1. Input Data Manually
2. Generate Random Data
3. Display Original List
4. FrontBackSplit (Fast/Slow Pointer)
5. FrontBackSplit (Count Method)
6. Test Boundary Cases
7. Exit
Enter your choice: 3
Original List: 42 68 35 1 70 
Length: 5

1. Input Data Manually
2. Generate Random Data
3. Display Original List
4. FrontBackSplit (Fast/Slow Pointer)
5. FrontBackSplit (Count Method)
6. Test Boundary Cases
7. Exit
Enter your choice: 4
Original List: 42 68 35 1 70 
Front List: 42 68 35 
Back List: 1 70 

1. Input Data Manually
2. Generate Random Data
3. Display Original List
4. FrontBackSplit (Fast/Slow Pointer)
5. FrontBackSplit (Count Method)
6. Test Boundary Cases
7. Exit
Enter your choice: 6

--- Testing Boundary Cases ---

Test Case 1: Empty List
Original: List is empty!
Front: List is empty!
Back: List is empty!

Test Case 2: Length = 1
Original: 42 
Front: 42 
Back: List is empty!

Test Case 3: Length = 2
Original: 10 20 
Front: 10 
Back: 20 

Test Case 4: Length = 3
Original: 10 20 30 
Front: 10 20 
Back: 30 

Test Case 5: Length = 4
Original: 10 20 30 40 
Front: 10 20 
Back: 30 40 

