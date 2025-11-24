## Problem Statement
WAP to build a simple stock price tracker that keeps a history of daily stock prices entered by the user. To allow users to go back and view or remove the most recent price, implement a stack using a linked list to store these integer prices. Implement the following operations: record(price) – Add a new stock price (an integer) to the stack. remove() – Remove and return the most recent price (top of the stack). latest() – Return the most recent stock price without removing it. isEmpty() – Check if there are no prices recorded.

##  Code
```cpp
#include <iostream>
using namespace std;

struct StackNode_tv {
    int price;
    StackNode_tv* next;
};

class StockPriceTracker_tv {
private:
    StackNode_tv* top;

public:
    StockPriceTracker_tv() {
        top = nullptr;
    }


    void record_tv(int price) {
        StackNode_tv* newNode = new StackNode_tv;
        newNode->price = price;
        newNode->next = top;
        top = newNode;
        cout << "Recorded stock price: " << price << endl;
    }

   
    int remove_tv() {
        if (isEmpty_tv()) {
            cout << "Error: No prices recorded!" << endl;
            return -1;  
        }

        StackNode_tv* temp = top;
        int removedPrice = temp->price;
        top = top->next;
        delete temp;
        cout << "Removed stock price: " << removedPrice << endl;
        return removedPrice;
    }

    int latest_tv() {
        if (isEmpty_tv()) {
            cout << "Error: No prices recorded!" << endl;
            return -1;  
        }

        return top->price;
    }

    
    bool isEmpty_tv() {
        return top == nullptr;
    }


    void displayPrices_tv() {
        if (isEmpty_tv()) {
            cout << "No stock prices recorded!" << endl;
            return;
        }

        cout << "Stock Prices (Most Recent First): ";
        StackNode_tv* temp = top;
        while (temp != nullptr) {
            cout << temp->price << " ";
            temp = temp->next;
        }
        cout << endl;
    }

    
    int countPrices_tv() {
        int count = 0;
        StackNode_tv* temp = top;
        while (temp != nullptr) {
            count++;
            temp = temp->next;
        }
        return count;
    }


    void clear_tv() {
        while (top != nullptr) {
            StackNode_tv* temp = top;
            top = top->next;
            delete temp;
        }
        cout << "All stock prices cleared!" << endl;
    }

    
    double calculateAverage_tv() {
        if (isEmpty_tv()) {
            cout << "No prices to calculate average!" << endl;
            return 0.0;
        }

        int sum = 0;
        int count = 0;
        StackNode_tv* temp = top;
        while (temp != nullptr) {
            sum += temp->price;
            count++;
            temp = temp->next;
        }

        return (double)sum / count;
    }

    ~StockPriceTracker_tv() {
        clear_tv();
    }
};

int main() {
    StockPriceTracker_tv tracker_tv;
    int choice, price;

    cout << "=== Stock Price Tracker using Stack ===" << endl;
    cout << "Implemented with Linked List" << endl;

    while (true) {
        cout << "\n1. Record Price\n2. Remove Latest Price\n3. View Latest Price\n4. Check if Empty\n5. Display All Prices\n6. Count Prices\n7. Calculate Average\n8. Clear All Prices\n9. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter stock price to record: ";
                cin >> price;
                tracker_tv.record_tv(price);
                break;

            case 2: {
                int removedPrice = tracker_tv.remove_tv();
                if (removedPrice != -1) {
                    cout << "Removed price: " << removedPrice << endl;
                }
                break;
            }

            case 3: {
                int latestPrice = tracker_tv.latest_tv();
                if (latestPrice != -1) {
                    cout << "Latest stock price: " << latestPrice << endl;
                }
                break;
            }

            case 4:
                if (tracker_tv.isEmpty_tv()) {
                    cout << "Stock price tracker is empty!" << endl;
                } else {
                    cout << "Stock price tracker has recorded prices." << endl;
                }
                break;

            case 5:
                tracker_tv.displayPrices_tv();
                break;

            case 6:
                cout << "Number of recorded prices: " << tracker_tv.countPrices_tv() << endl;
                break;

            case 7: {
                double average = tracker_tv.calculateAverage_tv();
                if (average > 0) {
                    cout << "Average stock price: " << average << endl;
                }
                break;
            }

            case 8:
                tracker_tv.clear_tv();
                break;

            case 9:
                cout << "Thank you for using the Stock Price Tracker!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


## Output

=== Stock Price Tracker using Stack ===
Implemented with Linked List

1. Record Price
2. Remove Latest Price
3. View Latest Price
4. Check if Empty
5. Display All Prices
6. Count Prices
7. Calculate Average
8. Clear All Prices
9. Exit
Enter your choice: 1
Enter stock price to record: 150
Recorded stock price: 150

1. Record Price
2. Remove Latest Price
3. View Latest Price
4. Check if Empty
5. Display All Prices
6. Count Prices
7. Calculate Average
8. Clear All Prices
9. Exit
Enter your choice: 1
Enter stock price to record: 155
Recorded stock price: 155

1. Record Price
2. Remove Latest Price
3. View Latest Price
4. Check if Empty
5. Display All Prices
6. Count Prices
7. Calculate Average
8. Clear All Prices
9. Exit
Enter your choice: 1
Enter stock price to record: 148
Recorded stock price: 148

1. Record Price
2. Remove Latest Price
3. View Latest Price
4. Check if Empty
5. Display All Prices
6. Count Prices
7. Calculate Average
8. Clear All Prices
9. Exit
Enter your choice: 1
Enter stock price to record: 160
Recorded stock price: 160

1. Record Price
2. Remove Latest Price
3. View Latest Price
4. Check if Empty
5. Display All Prices
6. Count Prices
7. Calculate Average
8. Clear All Prices
9. Exit
Enter your choice: 5
Stock Prices (Most Recent First): 160 148 155 150 

1. Record Price
2. Remove Latest Price
3. View Latest Price
4. Check if Empty
5. Display All Prices
6. Count Prices
7. Calculate Average
8. Clear All Prices
9. Exit
Enter your choice: 3
Latest stock price: 160

1. Record Price
2. Remove Latest Price
3. View Latest Price
4. Check if Empty
5. Display All Prices
6. Count Prices
7. Calculate Average
8. Clear All Prices
9. Exit
Enter your choice: 2
Removed stock price: 160
Removed price: 160

1. Record Price
2. Remove Latest Price
3. View Latest Price
4. Check if Empty
5. Display All Prices
6. Count Prices
7. Calculate Average
8. Clear All Prices
9. Exit
Enter your choice: 5
Stock Prices (Most Recent First): 148 155 150 

1. Record Price
2. Remove Latest Price
3. View Latest Price
4. Check if Empty
5. Display All Prices
6. Count Prices
7. Calculate Average
8. Clear All Prices
9. Exit
Enter your choice: 6
Number of recorded prices: 3

1. Record Price
2. Remove Latest Price
3. View Latest Price
4. Check if Empty
5. Display All Prices
6. Count Prices
7. Calculate Average
8. Clear All Prices
9. Exit
Enter your choice: 7
Average stock price: 151

