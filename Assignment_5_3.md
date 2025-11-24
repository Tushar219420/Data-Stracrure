## Problem Statement
Write a program to implement multiple stack i.e more than two stack using array and perform following operations on it. A. Push B. Pop C. Stack Overflow D. Stack Underflow E. Display

## Code
```cpp
#include <iostream>
using namespace std;

const int MAX_SIZE_TV = 100;
const int NUM_STACKS_TV = 3;  

class MultipleStacks_tv {
private:
    int arr_tv[MAX_SIZE_TV];
    int top_tv[NUM_STACKS_TV];
    int bottom_tv[NUM_STACKS_TV];
    int size_tv[NUM_STACKS_TV];

public:
    MultipleStacks_tv() {
   
        int segmentSize = MAX_SIZE_TV / NUM_STACKS_TV;
        
        for (int i = 0; i < NUM_STACKS_TV; i++) {
            bottom_tv[i] = i * segmentSize;
            top_tv[i] = bottom_tv[i] - 1;  
            size_tv[i] = segmentSize;
        }
        
      
        size_tv[NUM_STACKS_TV - 1] = MAX_SIZE_TV - bottom_tv[NUM_STACKS_TV - 1];
    }

    
    void push_tv(int stackNum, int value) {
        if (stackNum < 0 || stackNum >= NUM_STACKS_TV) {
            cout << "Invalid stack number! Please use 0 to " << (NUM_STACKS_TV - 1) << endl;
            return;
        }

        if (top_tv[stackNum] >= bottom_tv[stackNum] + size_tv[stackNum] - 1) {
            cout << "Stack Overflow! Cannot push " << value << " to stack " << stackNum << endl;
            return;
        }

        top_tv[stackNum]++;
        arr_tv[top_tv[stackNum]] = value;
        cout << "Pushed " << value << " to stack " << stackNum << endl;
    }

    
    int pop_tv(int stackNum) {
        if (stackNum < 0 || stackNum >= NUM_STACKS_TV) {
            cout << "Invalid stack number! Please use 0 to " << (NUM_STACKS_TV - 1) << endl;
            return -1;
        }

       
        if (top_tv[stackNum] < bottom_tv[stackNum]) {
            cout << "Stack Underflow! Cannot pop from empty stack " << stackNum << endl;
            return -1;
        }

        int poppedValue = arr_tv[top_tv[stackNum]];
        top_tv[stackNum]--;
        cout << "Popped " << poppedValue << " from stack " << stackNum << endl;
        return poppedValue;
    }

    bool isEmpty_tv(int stackNum) {
        if (stackNum < 0 || stackNum >= NUM_STACKS_TV) {
            cout << "Invalid stack number!" << endl;
            return true;
        }
        return top_tv[stackNum] < bottom_tv[stackNum];
    }

    
    bool isFull_tv(int stackNum) {
        if (stackNum < 0 || stackNum >= NUM_STACKS_TV) {
            cout << "Invalid stack number!" << endl;
            return false;
        }
        return top_tv[stackNum] >= bottom_tv[stackNum] + size_tv[stackNum] - 1;
    }


    void displayStack_tv(int stackNum) {
        if (stackNum < 0 || stackNum >= NUM_STACKS_TV) {
            cout << "Invalid stack number! Please use 0 to " << (NUM_STACKS_TV - 1) << endl;
            return;
        }

        cout << "Stack " << stackNum << ": ";
        if (isEmpty_tv(stackNum)) {
            cout << "Empty" << endl;
            return;
        }

        for (int i = bottom_tv[stackNum]; i <= top_tv[stackNum]; i++) {
            cout << arr_tv[i] << " ";
        }
        cout << "(Top to Bottom)" << endl;
    }

  
    void displayAllStacks_tv() {
        cout << "\n=== All Stacks ===" << endl;
        for (int i = 0; i < NUM_STACKS_TV; i++) {
            displayStack_tv(i);
        }
        cout << "==================" << endl;
    }


    int getStackSize_tv(int stackNum) {
        if (stackNum < 0 || stackNum >= NUM_STACKS_TV) {
            cout << "Invalid stack number!" << endl;
            return -1;
        }
        return top_tv[stackNum] - bottom_tv[stackNum] + 1;
    }


    int peek_tv(int stackNum) {
        if (stackNum < 0 || stackNum >= NUM_STACKS_TV) {
            cout << "Invalid stack number!" << endl;
            return -1;
        }

        if (isEmpty_tv(stackNum)) {
            cout << "Stack " << stackNum << " is empty!" << endl;
            return -1;
        }

        return arr_tv[top_tv[stackNum]];
    }

    void displayBoundaries_tv() {
        cout << "\n=== Stack Boundaries ===" << endl;
        for (int i = 0; i < NUM_STACKS_TV; i++) {
            cout << "Stack " << i << ": Bottom=" << bottom_tv[i] 
                 << ", Top=" << top_tv[i] << ", Size=" << size_tv[i] << endl;
        }
        cout << "========================" << endl;
    }
};

int main() {
    MultipleStacks_tv stacks_tv;
    int choice, stackNum, value;

    cout << "=== Multiple Stack Implementation ===" << endl;
    cout << "Implemented " << NUM_STACKS_TV << " stacks in an array of size " << MAX_SIZE_TV << endl;

    while (true) {
        cout << "\n1. Push\n2. Pop\n3. Check Overflow\n4. Check Underflow\n5. Display Stack\n6. Display All Stacks\n7. Peek\n8. Get Stack Size\n9. Display Boundaries\n10. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter stack number (0-" << (NUM_STACKS_TV - 1) << "): ";
                cin >> stackNum;
                cout << "Enter value to push: ";
                cin >> value;
                stacks_tv.push_tv(stackNum, value);
                break;

            case 2:
                cout << "Enter stack number (0-" << (NUM_STACKS_TV - 1) << "): ";
                cin >> stackNum;
                stacks_tv.pop_tv(stackNum);
                break;

            case 3:
                cout << "Enter stack number (0-" << (NUM_STACKS_TV - 1) << "): ";
                cin >> stackNum;
                if (stacks_tv.isFull_tv(stackNum)) {
                    cout << "Stack " << stackNum << " is full (Overflow condition)" << endl;
                } else {
                    cout << "Stack " << stackNum << " is not full" << endl;
                }
                break;

            case 4:
                cout << "Enter stack number (0-" << (NUM_STACKS_TV - 1) << "): ";
                cin >> stackNum;
                if (stacks_tv.isEmpty_tv(stackNum)) {
                    cout << "Stack " << stackNum << " is empty (Underflow condition)" << endl;
                } else {
                    cout << "Stack " << stackNum << " is not empty" << endl;
                }
                break;

            case 5:
                cout << "Enter stack number (0-" << (NUM_STACKS_TV - 1) << "): ";
                cin >> stackNum;
                stacks_tv.displayStack_tv(stackNum);
                break;

            case 6:
                stacks_tv.displayAllStacks_tv();
                break;

            case 7:
                cout << "Enter stack number (0-" << (NUM_STACKS_TV - 1) << "): ";
                cin >> stackNum;
                {
                    int topValue = stacks_tv.peek_tv(stackNum);
                    if (topValue != -1) {
                        cout << "Top element of stack " << stackNum << ": " << topValue << endl;
                    }
                }
                break;

            case 8:
                cout << "Enter stack number (0-" << (NUM_STACKS_TV - 1) << "): ";
                cin >> stackNum;
                cout << "Stack " << stackNum << " has " << stacks_tv.getStackSize_tv(stackNum) << " elements" << endl;
                break;

            case 9:
                stacks_tv.displayBoundaries_tv();
                break;

            case 10:
                cout << "Thank you for using the Multiple Stack Implementation!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


## Output

=== Multiple Stack Implementation ===
Implemented 3 stacks in an array of size 100

1. Push
2. Pop
3. Check Overflow
4. Check Underflow
5. Display Stack
6. Display All Stacks
7. Peek
8. Get Stack Size
9. Display Boundaries
10. Exit
Enter your choice: 1
Enter stack number (0-2): 0
Enter value to push: 10
Pushed 10 to stack 0

1. Push
2. Pop
3. Check Overflow
4. Check Underflow
5. Display Stack
6. Display All Stacks
7. Peek
8. Get Stack Size
9. Display Boundaries
10. Exit
Enter your choice: 1
Enter stack number (0-2): 0
Enter value to push: 20
Pushed 20 to stack 0

1. Push
2. Pop
3. Check Overflow
4. Check Underflow
5. Display Stack
6. Display All Stacks
7. Peek
8. Get Stack Size
9. Display Boundaries
10. Exit
Enter your choice: 1
Enter stack number (0-2): 1
Enter value to push: 30
Pushed 30 to stack 1

1. Push
2. Pop
3. Check Overflow
4. Check Underflow
5. Display Stack
6. Display All Stacks
7. Peek
8. Get Stack Size
9. Display Boundaries
10. Exit
Enter your choice: 1
Enter stack number (0-2): 1
Enter value to push: 40
Pushed 40 to stack 1

1. Push
2. Pop
3. Check Overflow
4. Check Underflow
5. Display Stack
6. Display All Stacks
7. Peek
8. Get Stack Size
9. Display Boundaries
10. Exit
Enter your choice: 6

=== All Stacks ===
Stack 0: 10 20 (Top to Bottom)
Stack 1: 30 40 (Top to Bottom)
Stack 2: Empty
==================

1. Push
2. Pop
3. Check Overflow
4. Check Underflow
5. Display Stack
6. Display All Stacks
7. Peek
8. Get Stack Size
9. Display Boundaries
10. Exit
Enter your choice: 2
Enter stack number (0-2): 0
Popped 20 from stack 0

1. Push
2. Pop
3. Check Overflow
4. Check Underflow
5. Display Stack
6. Display All Stacks
7. Peek
8. Get Stack Size
9. Display Boundaries
10. Exit
Enter your choice: 5
Enter stack number (0-2): 0
Stack 0: 10 (Top to Bottom)

1. Push
2. Pop
3. Check Overflow
4. Check Underflow
5. Display Stack
6. Display All Stacks
7. Peek
8. Get Stack Size
9. Display Boundaries
10. Exit
Enter your choice: 3
Enter stack number (0-2): 0
Stack 0 is not full

1. Push
2. Pop
3. Check Overflow
4. Check Underflow
5. Display Stack
6. Display All Stacks
7. Peek
8. Get Stack Size
9. Display Boundaries
10. Exit
Enter your choice: 4
Enter stack number (0-2): 2
Stack 2 is empty (Underflow condition)

1. Push
2. Pop
3. Check Overflow
4. Check Underflow
5. Display Stack
6. Display All Stacks
7. Peek
8. Get Stack Size
9. Display Boundaries
10. Exit
Enter your choice: 7
Enter stack number (0-2): 1
Top element of stack 1: 40

1. Push
2. Pop
3. Check Overflow
4. Check Underflow
5. Display Stack
6. Display All Stacks
7. Peek
8. Get Stack Size
9. Display Boundaries
10. Exit
Enter your choice: 8
Enter stack number (0-2): 1
Stack 1 has 2 elements

