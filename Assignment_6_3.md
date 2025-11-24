## Problem Statement
Write a program that maintains a queue of passengers waiting to see a ticket agent. The program user should be able to insert a new passenger at the rear of the queue, Display the passenger at the front of the Queue, or remove the passenger at the front of the queue. The program will display the number of passengers left in the queue just before it terminates.

##  Code
```cpp
#include <iostream>
#include <cstring>
using namespace std;

struct Passenger_tv {
    int ticketNumber;
    char name[50];
    char destination[50];
    char travelDate[20];
    Passenger_tv* next;
};

class PassengerQueue_tv {
private:
    Passenger_tv* front;
    Passenger_tv* rear;
    int passengerCounter_tv;

public:
    PassengerQueue_tv() {
        front = nullptr;
        rear = nullptr;
        passengerCounter_tv = 1000;  
    }

    
    void insertPassenger_tv(const char* name, const char* destination, const char* travelDate) {
        Passenger_tv* newPassenger = new Passenger_tv;
        newPassenger->ticketNumber = passengerCounter_tv++;
        strcpy(newPassenger->name, name);
        strcpy(newPassenger->destination, destination);
        strcpy(newPassenger->travelDate, travelDate);
        newPassenger->next = nullptr;

        if (rear == nullptr) {
            
            front = rear = newPassenger;
        } else {
            
            rear->next = newPassenger;
            rear = newPassenger;
        }

        cout << "Passenger " << name << " added to queue with ticket #" << newPassenger->ticketNumber << endl;
    }

    
    void displayFrontPassenger_tv() {
        if (front == nullptr) {
            cout << "Queue is empty! No passengers waiting." << endl;
            return;
        }

        cout << "\n=== Front Passenger Details ===" << endl;
        cout << "Ticket Number: " << front->ticketNumber << endl;
        cout << "Name: " << front->name << endl;
        cout << "Destination: " << front->destination << endl;
        cout << "Travel Date: " << front->travelDate << endl;
        cout << "Position: Next to be served" << endl;
        cout << "=============================" << endl;
    }

    /
    void removeFrontPassenger_tv() {
        if (front == nullptr) {
            cout << "Queue is empty! No passengers to remove." << endl;
            return;
        }

        Passenger_tv* temp = front;
        cout << "Removing passenger: " << temp->name << " (Ticket #" << temp->ticketNumber << ")" << endl;

        if (front == rear) {
            
            front = rear = nullptr;
        } else {
            
            front = front->next;
        }

        delete temp;
        cout << "Passenger served and removed from queue." << endl;
    }

   
    void displayAllPassengers_tv() {
        if (front == nullptr) {
            cout << "Queue is empty! No passengers waiting." << endl;
            return;
        }

        cout << "\n=== All Passengers in Queue ===" << endl;
        cout << "Pos\tTicket #\tName\t\tDestination\t\tTravel Date" << endl;
        cout << "----------------------------------------------------------------" << endl;

        Passenger_tv* temp = front;
        int position = 1;
        while (temp != nullptr) {
            cout << position << "\t" << temp->ticketNumber << "\t\t" 
                 << temp->name << "\t\t" << temp->destination << "\t\t" << temp->travelDate << endl;
            temp = temp->next;
            position++;
        }
        cout << "==============================" << endl;
    }

    
    int getPassengerCount_tv() {
        int count = 0;
        Passenger_tv* temp = front;
        while (temp != nullptr) {
            count++;
            temp = temp->next;
        }
        return count;
    }

    
    bool isEmpty_tv() {
        return front == nullptr;
    }

    
    void displayQueueStats_tv() {
        cout << "\n=== Queue Statistics ===" << endl;
        cout << "Passengers in queue: " << getPassengerCount_tv() << endl;
        cout << "Queue status: " << (isEmpty_tv() ? "Empty" : "Has passengers") << endl;
        
        if (!isEmpty_tv()) {
            cout << "Next passenger: " << front->name << " (Ticket #" << front->ticketNumber << ")" << endl;
            cout << "Last passenger: " << rear->name << " (Ticket #" << rear->ticketNumber << ")" << endl;
        }
        cout << "========================" << endl;
    }

    
    void clearQueue_tv() {
        while (front != nullptr) {
            Passenger_tv* temp = front;
            front = front->next;
            delete temp;
        }
        rear = nullptr;
        cout << "All passengers cleared from queue!" << endl;
    }

    ~PassengerQueue_tv() {
        cout << "\nTermination: " << getPassengerCount_tv() << " passengers left in queue." << endl;
        clearQueue_tv();
    }
};

int main() {
    PassengerQueue_tv queue_tv;
    int choice;
    char name[50], destination[50], travelDate[20];

    cout << "=== Passenger Queue Management for Ticket Agent ===" << endl;
    cout << "Maintains a queue of passengers waiting to see a ticket agent" << endl;

    while (true) {
        cout << "\n1. Insert Passenger\n2. Display Front Passenger\n3. Remove Front Passenger\n4. Display All Passengers\n5. Queue Statistics\n6. Clear Queue\n7. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter passenger name (Indian name): ";
                cin >> name;
                cout << "Enter destination: ";
                cin >> destination;
                cout << "Enter travel date (DD/MM/YYYY): ";
                cin >> travelDate;
                queue_tv.insertPassenger_tv(name, destination, travelDate);
                break;

            case 2:
                queue_tv.displayFrontPassenger_tv();
                break;

            case 3:
                queue_tv.removeFrontPassenger_tv();
                break;

            case 4:
                queue_tv.displayAllPassengers_tv();
                break;

            case 5:
                queue_tv.displayQueueStats_tv();
                break;

            case 6:
                queue_tv.clearQueue_tv();
                break;

            case 7:
                cout << "Thank you for using the Passenger Queue Management System!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


##  Output

=== Passenger Queue Management for Ticket Agent ===
Maintains a queue of passengers waiting to see a ticket agent

1. Insert Passenger
2. Display Front Passenger
3. Remove Front Passenger
4. Display All Passengers
5. Queue Statistics
6. Clear Queue
7. Exit
Enter your choice: 1
Enter passenger name (Indian name): Arjun
Enter destination: Mumbai
Enter travel date (DD/MM/YYYY): 15/12/2023
Passenger Arjun added to queue with ticket #1000

1. Insert Passenger
2. Display Front Passenger
3. Remove Front Passenger
4. Display All Passengers
5. Queue Statistics
6. Clear Queue
7. Exit
Enter your choice: 1
Enter passenger name (Indian name): Priya
Enter destination: Delhi
Enter travel date (DD/MM/YYYY): 16/12/2023
Passenger Priya added to queue with ticket #1001

1. Insert Passenger
2. Display Front Passenger
3. Remove Front Passenger
4. Display All Passengers
5. Queue Statistics
6. Clear Queue
7. Exit
Enter your choice: 1
Enter passenger name (Indian name): Rohit
Enter destination: Bangalore
Enter travel date (DD/MM/YYYY): 17/12/2023
Passenger Rohit added to queue with ticket #1002

1. Insert Passenger
2. Display Front Passenger
3. Remove Front Passenger
4. Display All Passengers
5. Queue Statistics
6. Clear Queue
7. Exit
Enter your choice: 4

=== All Passengers in Queue ===
Pos	Ticket #	Name		Destination		Travel Date
----------------------------------------------------------------
1	1000		Arjun		Mumbai		15/12/2023
2	1001		Priya		Delhi		16/12/2023
3	1002		Rohit		Bangalore		17/12/2023
==============================

1. Insert Passenger
2. Display Front Passenger
3. Remove Front Passenger
4. Display All Passengers
5. Queue Statistics
6. Clear Queue
7. Exit
Enter your choice: 2

=== Front Passenger Details ===
Ticket Number: 1000
Name: Arjun
Destination: Mumbai
Travel Date: 15/12/2023
Position: Next to be served
=============================

1. Insert Passenger
2. Display Front Passenger
3. Remove Front Passenger
4. Display All Passengers
5. Queue Statistics
6. Clear Queue
7. Exit
Enter your choice: 5

=== Queue Statistics ===
Passengers in queue: 3
Queue status: Has passengers
Next passenger: Arjun (Ticket #1000)
Last passenger: Rohit (Ticket #1002)
========================

1. Insert Passenger
2. Display Front Passenger
3. Remove Front Passenger
4. Display All Passengers
5. Queue Statistics
6. Clear Queue
7. Exit
Enter your choice: 3
Removing passenger: Arjun (Ticket #1000)
Passenger served and removed from queue.

1. Insert Passenger
2. Display Front Passenger
3. Remove Front Passenger
4. Display All Passengers
5. Queue Statistics
6. Clear Queue
7. Exit
Enter your choice: 2

=== Front Passenger Details ===
Ticket Number: 1001
Name: Priya
Destination: Delhi
Travel Date: 16/12/2023
Position: Next to be served
=============================

1. Insert Passenger
2. Display Front Passenger
3. Remove Front Passenger
4. Display All Passengers
5. Queue Statistics
6. Clear Queue
7. Exit
Enter your choice: 4

=== All Passengers in Queue ===
Pos	Ticket #	Name		Destination		Travel Date
----------------------------------------------------------------
1	1001		Priya		Delhi		16/12/2023
2	1002		Rohit		Bangalore		17/12/2023
==============================

1. Insert Passenger
2. Display Front Passenger
3. Remove Front Passenger
4. Display All Passengers
5. Queue Statistics
6. Clear Queue
7. Exit
Enter your choice: 6
All passengers cleared from queue!

