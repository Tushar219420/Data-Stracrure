## Problem Statement
Pizza parlour accepting maximum n orders. Orders are served on an FCFS basis. Order once placed can't be cancelled. Write C++ program to simulate the system using circular QUEUE.

##  Code
```cpp
#include <iostream>
#include <cstring>
using namespace std;

const int MAX_ORDERS_TV = 10;

struct Order_tv {
    int orderId;
    char customerName[50];
    char pizzaType[30];
    int quantity;
    double price;
};

class PizzaParlor_tv {
private:
    Order_tv orders_tv[MAX_ORDERS_TV];
    int front;
    int rear;
    int count;
    int orderCounter_tv;

public:
    PizzaParlor_tv() {
        front = -1;
        rear = -1;
        count = 0;
        orderCounter_tv = 100; 
    }

    bool isFull_tv() {
        return count == MAX_ORDERS_TV;
    }

    
    bool isEmpty_tv() {
        return count == 0;
    }

   
    void placeOrder_tv(const char* customerName, const char* pizzaType, int quantity, double price) {
        if (isFull_tv()) {
            cout << "Sorry! Maximum order limit reached. Cannot accept more orders." << endl;
            return;
        }

        
        if (isEmpty_tv()) {
            front = 0;
        }
        rear = (rear + 1) % MAX_ORDERS_TV;

        
        orders_tv[rear].orderId = orderCounter_tv++;
        strcpy(orders_tv[rear].customerName, customerName);
        strcpy(orders_tv[rear].pizzaType, pizzaType);
        orders_tv[rear].quantity = quantity;
        orders_tv[rear].price = price;

        count++;
        cout << "Order placed successfully! Order ID: " << orders_tv[rear].orderId << endl;
        cout << "Customer: " << customerName << endl;
        cout << "Pizza: " << pizzaType << " (Qty: " << quantity << ")" << endl;
        cout << "Total Price: Rs. " << (price * quantity) << endl;
        cout << "Position in queue: " << count << endl;
    }

    
    void serveOrder_tv() {
        if (isEmpty_tv()) {
            cout << "No orders to serve! Queue is empty." << endl;
            return;
        }

        Order_tv servedOrder = orders_tv[front];
        cout << "\n=== Order Served ===" << endl;
        cout << "Order ID: " << servedOrder.orderId << endl;
        cout << "Customer: " << servedOrder.customerName << endl;
        cout << "Pizza: " << servedOrder.pizzaType << " (Qty: " << servedOrder.quantity << ")" << endl;
        cout << "Total Price: Rs. " << (servedOrder.price * servedOrder.quantity) << endl;
        cout << "===================" << endl;

        
        if (front == rear) {
          
            front = -1;
            rear = -1;
        } else {
            front = (front + 1) % MAX_ORDERS_TV;
        }

        count--;
    }

   
    void displayPendingOrders_tv() {
        if (isEmpty_tv()) {
            cout << "No pending orders! Queue is empty." << endl;
            return;
        }

        cout << "\n=== Pending Orders ===" << endl;
        cout << "Pos\tOrder ID\tCustomer\tPizza\t\tQty\tPrice\tTotal" << endl;
        cout << "----------------------------------------------------------------" << endl;

        int index = front;
        for (int i = 0; i < count; i++) {
            cout << (i+1) << "\t" << orders_tv[index].orderId << "\t\t" 
                 << orders_tv[index].customerName << "\t\t" << orders_tv[index].pizzaType << "\t\t"
                 << orders_tv[index].quantity << "\t" << orders_tv[index].price << "\t" 
                 << (orders_tv[index].price * orders_tv[index].quantity) << endl;
            
            index = (index + 1) % MAX_ORDERS_TV;
        }
        cout << "======================" << endl;
    }

    
    void displayOrderStats_tv() {
        cout << "\n=== Order Statistics ===" << endl;
        cout << "Pending orders: " << count << endl;
        cout << "Maximum capacity: " << MAX_ORDERS_TV << endl;
        cout << "Available slots: " << (MAX_ORDERS_TV - count) << endl;
        cout << "Queue status: " << (isEmpty_tv() ? "Empty" : (isFull_tv() ? "Full" : "Partially filled")) << endl;
        
        if (!isEmpty_tv()) {
            cout << "Next order to serve: #" << orders_tv[front].orderId << " (" << orders_tv[front].customerName << ")" << endl;
            cout << "Last order placed: #" << orders_tv[rear].orderId << " (" << orders_tv[rear].customerName << ")" << endl;
        }
        cout << "========================" << endl;
    }

    void peekNextOrder_tv() {
        if (isEmpty_tv()) {
            cout << "No orders in queue!" << endl;
            return;
        }

        Order_tv nextOrder = orders_tv[front];
        cout << "\n=== Next Order to Serve ===" << endl;
        cout << "Order ID: " << nextOrder.orderId << endl;
        cout << "Customer: " << nextOrder.customerName << endl;
        cout << "Pizza: " << nextOrder.pizzaType << " (Qty: " << nextOrder.quantity << ")" << endl;
        cout << "Total Price: Rs. " << (nextOrder.price * nextOrder.quantity) << endl;
        cout << "Position: Next in line" << endl;
        cout << "==========================" << endl;
    }

   
    void displayMenu_tv() {
        cout << "\n=== Pizza Menu ===" << endl;
        cout << "1. Margherita\t- Rs. 150" << endl;
        cout << "2. Pepperoni\t- Rs. 180" << endl;
        cout << "3. Veggie\t- Rs. 160" << endl;
        cout << "4. Chicken\t- Rs. 200" << endl;
        cout << "5. Supreme\t- Rs. 220" << endl;
        cout << "==================" << endl;
    }

    
    void getPizzaDetails_tv(int choice, char* pizzaType, double& price) {
        switch (choice) {
            case 1:
                strcpy(pizzaType, "Margherita");
                price = 150.0;
                break;
            case 2:
                strcpy(pizzaType, "Pepperoni");
                price = 180.0;
                break;
            case 3:
                strcpy(pizzaType, "Veggie");
                price = 160.0;
                break;
            case 4:
                strcpy(pizzaType, "Chicken");
                price = 200.0;
                break;
            case 5:
                strcpy(pizzaType, "Supreme");
                price = 220.0;
                break;
            default:
                strcpy(pizzaType, "Custom");
                price = 150.0;  // Default price
                break;
        }
    }
};

int main() {
    PizzaParlor_tv parlor_tv;
    int choice, quantity, pizzaChoice;
    char customerName[50], pizzaType[30];
    double price;

    cout << "=== Pizza Parlor Order Management System ===" << endl;
    cout << "Maximum " << MAX_ORDERS_TV << " orders can be placed at a time" << endl;
    cout << "Orders are served on First-Come, First-Served basis" << endl;

    while (true) {
        cout << "\n1. Place Order\n2. Serve Order\n3. Display Pending Orders\n4. Order Statistics\n5. Peek Next Order\n6. Display Menu\n7. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter customer name (Indian name): ";
                cin >> customerName;
                parlor_tv.displayMenu_tv();
                cout << "Enter pizza choice (1-5): ";
                cin >> pizzaChoice;
                parlor_tv.getPizzaDetails_tv(pizzaChoice, pizzaType, price);
                cout << "Enter quantity: ";
                cin >> quantity;
                parlor_tv.placeOrder_tv(customerName, pizzaType, quantity, price);
                break;

            case 2:
                parlor_tv.serveOrder_tv();
                break;

            case 3:
                parlor_tv.displayPendingOrders_tv();
                break;

            case 4:
                parlor_tv.displayOrderStats_tv();
                break;

            case 5:
                parlor_tv.peekNextOrder_tv();
                break;

            case 6:
                parlor_tv.displayMenu_tv();
                break;

            case 7:
                cout << "Thank you for using the Pizza Parlor Management System!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


##  Output
=== Pizza Parlor Order Management System ===
Maximum 10 orders can be placed at a time
Orders are served on First-Come, First-Served basis

1. Place Order
2. Serve Order
3. Display Pending Orders
4. Order Statistics
5. Peek Next Order
6. Display Menu
7. Exit
Enter your choice: 1
Enter customer name (Indian name): Arjun
Enter pizza choice (1-5): 2
Enter quantity: 2
Order placed successfully! Order ID: 100
Customer: Arjun
Pizza: Pepperoni (Qty: 2)
Total Price: Rs. 360
Position in queue: 1

1. Place Order
2. Serve Order
3. Display Pending Orders
4. Order Statistics
5. Peek Next Order
6. Display Menu
7. Exit
Enter your choice: 1
Enter customer name (Indian name): Priya
Enter pizza choice (1-5): 1
Enter quantity: 1
Order placed successfully! Order ID: 101
Customer: Priya
Pizza: Margherita (Qty: 1)
Total Price: Rs. 150
Position in queue: 2

1. Place Order
2. Serve Order
3. Display Pending Orders
4. Order Statistics
5. Peek Next Order
6. Display Menu
7. Exit
Enter your choice: 1
Enter customer name (Indian name): Rohit
Enter pizza choice (1-5): 4
Enter quantity: 3
Order placed successfully! Order ID: 102
Customer: Rohit
Pizza: Chicken (Qty: 3)
Total Price: Rs. 600
Position in queue: 3

1. Place Order
2. Serve Order
3. Display Pending Orders
4. Order Statistics
5. Peek Next Order
6. Display Menu
7. Exit
Enter your choice: 3

=== Pending Orders ===
Pos	Order ID	Customer	Pizza		Qty	Price	Total
----------------------------------------------------------------
1	100		Arjun		Pepperoni	2	180	360
2	101		Priya		Margherita	1	150	150
3	102		Rohit		Chicken		3	200	600
======================

1. Place Order
2. Serve Order
3. Display Pending Orders
4. Order Statistics
5. Peek Next Order
6. Display Menu
7. Exit
Enter your choice: 4

=== Order Statistics ===
Pending orders: 3
Maximum capacity: 10
Available slots: 7
Queue status: Partially filled
Next order to serve: #100 (Arjun)
Last order placed: #102 (Rohit)
========================

1. Place Order
2. Serve Order
3. Display Pending Orders
4. Order Statistics
5. Peek Next Order
6. Display Menu
7. Exit
Enter your choice: 5

=== Next Order to Serve ===
Order ID: 100
Customer: Arjun
Pizza: Pepperoni (Qty: 2)
Total Price: Rs. 360
Position: Next in line
==========================

1. Place Order
2. Serve Order
3. Display Pending Orders
4. Order Statistics
5. Peek Next Order
6. Display Menu
7. Exit
Enter your choice: 2

=== Order Served ===
Order ID: 100
Customer: Arjun
Pizza: Pepperoni (Qty: 2)
Total Price: Rs. 360
===================

1. Place Order
2. Serve Order
3. Display Pending Orders
4. Order Statistics
5. Peek Next Order
6. Display Menu
7. Exit
Enter your choice: 3

=== Pending Orders ===
Pos	Order ID	Customer	Pizza		Qty	Price	Total
----------------------------------------------------------------
1	101		Priya		Margherita	1	150	150
2	102		Rohit		Chicken		3	200	600
======================

1. Place Order
2. Serve Order
3. Display Pending Orders
4. Order Statistics
5. Peek Next Order
6. Display Menu
7. Exit
Enter your choice: 6

=== Pizza Menu ===
1. Margherita	- Rs. 150
2. Pepperoni	- Rs. 180
3. Veggie	- Rs. 160
4. Chicken	- Rs. 200
5. Supreme	- Rs. 220
==================

