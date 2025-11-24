## Problem Statement
Write a C++ program to store a binary number using a doubly linked list. Implement the following functions: a) Calculate and display the 1's complement and 2's complement of the stored binary number. b) Perform addition of two binary numbers represented using doubly linked lists and display the result.

## Code
```cpp
#include <iostream>
#include <cstring>
using namespace std;

struct BinaryDigit_tv {
    int digit;  // 0 or 1
    BinaryDigit_tv* next;
    BinaryDigit_tv* prev;
};

class BinaryNumber_tv {
private:
    BinaryDigit_tv* head;
    BinaryDigit_tv* tail;

public:
    BinaryNumber_tv() {
        head = nullptr;
        tail = nullptr;
    }

    void insertDigit_tv(int digit) {
        if (digit != 0 && digit != 1) {
            cout << "Invalid binary digit! Only 0 or 1 allowed." << endl;
            return;
        }

        BinaryDigit_tv* newDigit = new BinaryDigit_tv;
        newDigit->digit = digit;
        newDigit->next = nullptr;
        newDigit->prev = nullptr;

        if (head == nullptr) {
            head = tail = newDigit;
        } else {
            tail->next = newDigit;
            newDigit->prev = tail;
            tail = newDigit;
        }
    }

    void inputBinary_tv(const char* binaryStr) {
       
        clear_tv();

       
        for (int i = 0; i < strlen(binaryStr); i++) {
            if (binaryStr[i] == '0' || binaryStr[i] == '1') {
                insertDigit_tv(binaryStr[i] - '0');
            }
        }
    }

    void displayBinary_tv(const char* label = "") {
        if (head == nullptr) {
            cout << label << "Empty binary number" << endl;
            return;
        }

        cout << label;
        BinaryDigit_tv* temp = head;
        while (temp != nullptr) {
            cout << temp->digit;
            temp = temp->next;
        }
        cout << endl;
    }

    BinaryNumber_tv* onesComplement_tv() {
        BinaryNumber_tv* complement = new BinaryNumber_tv();
        
        BinaryDigit_tv* temp = head;
        while (temp != nullptr) {
            complement->insertDigit_tv(1 - temp->digit);  // Flip the bit
            temp = temp->next;
        }
        
        return complement;
    }

    BinaryNumber_tv* twosComplement_tv() {
     
        BinaryNumber_tv* onesComp = onesComplement_tv();
        
       
        BinaryNumber_tv* twosComp = new BinaryNumber_tv();
        
       
        BinaryDigit_tv* temp = onesComp->head;
        while (temp != nullptr) {
            twosComp->insertDigit_tv(temp->digit);
            temp = temp->next;
        }
        
       
        twosComp->addOne_tv();
        
        delete onesComp;
        return twosComp;
    }

    void addOne_tv() {
        if (head == nullptr) return;
        
        BinaryDigit_tv* temp = tail;
        int carry = 1;  
        
        while (temp != nullptr && carry) {
            int sum = temp->digit + carry;
            temp->digit = sum % 2;
            carry = sum / 2;
            temp = temp->prev;
        }
        
        
        if (carry) {
            BinaryDigit_tv* newDigit = new BinaryDigit_tv;
            newDigit->digit = carry;
            newDigit->next = head;
            newDigit->prev = nullptr;
            if (head != nullptr) {
                head->prev = newDigit;
            }
            head = newDigit;
            if (tail == nullptr) {
                tail = head;
            }
        }
    }

    static BinaryNumber_tv* addBinary_tv(BinaryNumber_tv& num1, BinaryNumber_tv& num2) {
        BinaryNumber_tv* result = new BinaryNumber_tv();
        
        BinaryDigit_tv* ptr1 = num1.tail;
        BinaryDigit_tv* ptr2 = num2.tail;
        int carry = 0;
        
        
        while (ptr1 != nullptr || ptr2 != nullptr || carry) {
            int sum = carry;
            
            if (ptr1 != nullptr) {
                sum += ptr1->digit;
                ptr1 = ptr1->prev;
            }
            
            if (ptr2 != nullptr) {
                sum += ptr2->digit;
                ptr2 = ptr2->prev;
            }
            
          
            BinaryDigit_tv* newDigit = new BinaryDigit_tv;
            newDigit->digit = sum % 2;
            newDigit->next = result->head;
            newDigit->prev = nullptr;
            
            if (result->head != nullptr) {
                result->head->prev = newDigit;
            }
            result->head = newDigit;
            
            if (result->tail == nullptr) {
                result->tail = result->head;
            }
            
            carry = sum / 2;
        }
        
        return result;
    }

    void clear_tv() {
        while (head != nullptr) {
            BinaryDigit_tv* temp = head;
            head = head->next;
            delete temp;
        }
        tail = nullptr;
    }

    ~BinaryNumber_tv() {
        clear_tv();
    }
};

int main() {
    BinaryNumber_tv binary1_tv, binary2_tv;
    char binaryStr[100];
    int choice;

    cout << "=== Binary Number Operations using Doubly Linked List ===" << endl;

    while (true) {
        cout << "\n1. Input First Binary Number\n2. Input Second Binary Number\n3. Display Numbers\n4. 1's Complement\n5. 2's Complement\n6. Add Binary Numbers\n7. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter first binary number: ";
                cin >> binaryStr;
                binary1_tv.inputBinary_tv(binaryStr);
                cout << "First binary number stored successfully!" << endl;
                break;

            case 2:
                cout << "Enter second binary number: ";
                cin >> binaryStr;
                binary2_tv.inputBinary_tv(binaryStr);
                cout << "Second binary number stored successfully!" << endl;
                break;

            case 3:
                binary1_tv.displayBinary_tv("First Binary Number: ");
                binary2_tv.displayBinary_tv("Second Binary Number: ");
                break;

            case 4: {
                cout << "\n--- 1's Complement ---" << endl;
                binary1_tv.displayBinary_tv("Original: ");
                BinaryNumber_tv* onesComp1 = binary1_tv.onesComplement_tv();
                onesComp1->displayBinary_tv("1's Complement: ");
                delete onesComp1;

                binary2_tv.displayBinary_tv("Original: ");
                BinaryNumber_tv* onesComp2 = binary2_tv.onesComplement_tv();
                onesComp2->displayBinary_tv("1's Complement: ");
                delete onesComp2;
                break;
            }

            case 5: {
                cout << "\n--- 2's Complement ---" << endl;
                binary1_tv.displayBinary_tv("Original: ");
                BinaryNumber_tv* twosComp1 = binary1_tv.twosComplement_tv();
                twosComp1->displayBinary_tv("2's Complement: ");
                delete twosComp1;

                binary2_tv.displayBinary_tv("Original: ");
                BinaryNumber_tv* twosComp2 = binary2_tv.twosComplement_tv();
                twosComp2->displayBinary_tv("2's Complement: ");
                delete twosComp2;
                break;
            }

            case 6: {
                cout << "\n--- Binary Addition ---" << endl;
                binary1_tv.displayBinary_tv("First Number: ");
                binary2_tv.displayBinary_tv("Second Number: ");
                
                BinaryNumber_tv* sum = BinaryNumber_tv::addBinary_tv(binary1_tv, binary2_tv);
                sum->displayBinary_tv("Sum: ");
                delete sum;
                break;
            }

            case 7:
                cout << "Thank you for using the Binary Number Operations System!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}

##  Output

=== Binary Number Operations using Doubly Linked List ===

1. Input First Binary Number
2. Input Second Binary Number
3. Display Numbers
4. 1's Complement
5. 2's Complement
6. Add Binary Numbers
7. Exit
Enter your choice: 1
Enter first binary number: 1011
First binary number stored successfully!

1. Input First Binary Number
2. Input Second Binary Number
3. Display Numbers
4. 1's Complement
5. 2's Complement
6. Add Binary Numbers
7. Exit
Enter your choice: 2
Enter second binary number: 1101
Second binary number stored successfully!

1. Input First Binary Number
2. Input Second Binary Number
3. Display Numbers
4. 1's Complement
5. 2's Complement
6. Add Binary Numbers
7. Exit
Enter your choice: 3
First Binary Number: 1011
Second Binary Number: 1101

1. Input First Binary Number
2. Input Second Binary Number
3. Display Numbers
4. 1's Complement
5. 2's Complement
6. Add Binary Numbers
7. Exit
Enter your choice: 4

--- 1's Complement ---
Original: 1011
1's Complement: 0100
Original: 1101
1's Complement: 0010

1. Input First Binary Number
2. Input Second Binary Number
3. Display Numbers
4. 1's Complement
5. 2's Complement
6. Add Binary Numbers
7. Exit
Enter your choice: 5

--- 2's Complement ---
Original: 1011
2's Complement: 0101
Original: 1101
2's Complement: 0011

1. Input First Binary Number
2. Input Second Binary Number
3. Display Numbers
4. 1's Complement
5. 2's Complement
6. Add Binary Numbers
7. Exit
Enter your choice: 6

--- Binary Addition ---
First Number: 1011
Second Number: 1101
Sum: 11000

