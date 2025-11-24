## Problem Statement
Write a program to implement multiple queues i.e. two queues using array and perform following operations on it. A. Add Queue, B. Delete from Queue, C. Display Queue

##  Code
```cpp
#include <iostream>
#include <cstring>
using namespace std;

const int MAX_SIZE_TV = 20;
const int NUM_QUEUES_TV = 2;  

struct QueueData_tv {
    int ticketNumber;
    char name[30];
    char serviceType[20];
};

class MultipleQueues_tv {
private:
    QueueData_tv arr_tv[MAX_SIZE_TV];
    int front_tv[NUM_QUEUES_TV];
    int rear_tv[NUM_QUEUES_TV];
    int size_tv[NUM_QUEUES_TV];
    int queueCounter_tv[NUM_QUEUES_TV];


    int getIndex_tv(int queueNum, int position) {
        int base = (MAX_SIZE_TV / NUM_QUEUES_TV) * queueNum;
        return base + position;
    }

public:
    MultipleQueues_tv() {
        
        int segmentSize = MAX_SIZE_TV / NUM_QUEUES_TV;
        
        for (int i = 0; i < NUM_QUEUES_TV; i++) {
            front_tv[i] = -1;
            rear_tv[i] = -1;
            size_tv[i] = segmentSize;
            queueCounter_tv[i] = i * 100;  
        }
    }

   
    bool isFull_tv(int queueNum) {
        if (queueNum < 0 || queueNum >= NUM_QUEUES_TV) {
            cout << "Invalid queue number! Please use 0 to " << (NUM_QUEUES_TV - 1) << endl;
            return false;
        }
        return (rear_tv[queueNum] == size_tv[queueNum] - 1);
    }

    
    bool isEmpty_tv(int queueNum) {
        if (queueNum < 0 || queueNum >= NUM_QUEUES_TV) {
            cout << "Invalid queue number! Please use 0 to " << (NUM_QUEUES_TV - 1) << endl;
            return true;
        }
        return (front_tv[queueNum] == -1);
    }

   
    void addQueue_tv(int queueNum, const char* name, const char* serviceType) {
        if (queueNum < 0 || queueNum >= NUM_QUEUES_TV) {
            cout << "Invalid queue number! Please use 0 to " << (NUM_QUEUES_TV - 1) << endl;
            return;
        }

        if (isFull_tv(queueNum)) {
            cout << "Queue " << queueNum << " is full! Cannot add more elements." << endl;
            return;
        }

        QueueData_tv newData;
        newData.ticketNumber = ++queueCounter_tv[queueNum];
        strcpy(newData.name, name);
        strcpy(newData.serviceType, serviceType);

        if (isEmpty_tv(queueNum)) {
            
            front_tv[queueNum] = 0;
            rear_tv[queueNum] = 0;
        } else {
           
            rear_tv[queueNum]++;
        }

        
        int index = getIndex_tv(queueNum, rear_tv[queueNum]);
        arr_tv[index] = newData;

        cout << "Added to Queue " << queueNum << ": " << name 
             << " (Ticket #" << newData.ticketNumber << ", Service: " << serviceType << ")" << endl;
    }

    
    void deleteFromQueue_tv(int queueNum) {
        if (queueNum < 0 || queueNum >= NUM_QUEUES_TV) {
            cout << "Invalid queue number! Please use 0 to " << (NUM_QUEUES_TV - 1) << endl;
            return;
        }

        if (isEmpty_tv(queueNum)) {
            cout << "Queue " << queueNum << " is empty! Cannot delete elements." << endl;
            return;
        }

        
        int index = getIndex_tv(queueNum, front_tv[queueNum]);
        QueueData_tv deletedData = arr_tv[index];

        cout << "Removed from Queue " << queueNum << ": " << deletedData.name 
             << " (Ticket #" << deletedData.ticketNumber << ", Service: " << deletedData.serviceType << ")" << endl;

        if (front_tv[queueNum] == rear_tv[queueNum]) {
            
            front_tv[queueNum] = -1;
            rear_tv[queueNum] = -1;
        } else {
           
            front_tv[queueNum]++;
        }
    }

    
    void displayQueue_tv(int queueNum) {
        if (queueNum < 0 || queueNum >= NUM_QUEUES_TV) {
            cout << "Invalid queue number! Please use 0 to " << (NUM_QUEUES_TV - 1) << endl;
            return;
        }

        if (isEmpty_tv(queueNum)) {
            cout << "Queue " << queueNum << " is empty!" << endl;
            return;
        }

        cout << "\n=== Queue " << queueNum << " ===" << endl;
        cout << "Pos\tTicket #\tName\t\tService" << endl;
        cout << "----------------------------------------" << endl;

        for (int i = front_tv[queueNum]; i <= rear_tv[queueNum]; i++) {
            int index = getIndex_tv(queueNum, i);
            cout << (i - front_tv[queueNum] + 1) << "\t" << arr_tv[index].ticketNumber 
                 << "\t\t" << arr_tv[index].name << "\t\t" << arr_tv[index].serviceType << endl;
        }
        cout << "==============" << endl;
    }

    
    void displayAllQueues_tv() {
        cout << "\n=== All Queues ===" << endl;
        for (int i = 0; i < NUM_QUEUES_TV; i++) {
            displayQueue_tv(i);
        }
        cout << "==================" << endl;
    }

    
    int getQueueSize_tv(int queueNum) {
        if (queueNum < 0 || queueNum >= NUM_QUEUES_TV) {
            cout << "Invalid queue number!" << endl;
            return -1;
        }
        if (isEmpty_tv(queueNum)) {
            return 0;
        }
        return (rear_tv[queueNum] - front_tv[queueNum] + 1);
    }

    
    void displayQueueStats_tv() {
        cout << "\n=== Queue Statistics ===" << endl;
        for (int i = 0; i < NUM_QUEUES_TV; i++) {
            cout << "Queue " << i << ":" << endl;
            cout << "  Elements: " << getQueueSize_tv(i) << endl;
            cout << "  Status: " << (isEmpty_tv(i) ? "Empty" : (isFull_tv(i) ? "Full" : "Partially filled")) << endl;
            cout << "  Capacity: " << size_tv[i] << endl;
            if (!isEmpty_tv(i)) {
                int frontIndex = getIndex_tv(i, front_tv[i]);
                int rearIndex = getIndex_tv(i, rear_tv[i]);
                cout << "  Next to serve: " << arr_tv[frontIndex].name 
                     << " (Ticket #" << arr_tv[frontIndex].ticketNumber << ")" << endl;
                cout << "  Last added: " << arr_tv[rearIndex].name 
                     << " (Ticket #" << arr_tv[rearIndex].ticketNumber << ")" << endl;
            }
            cout << endl;
        }
        cout << "========================" << endl;
    }

    
    void peekFront_tv(int queueNum) {
        if (queueNum < 0 || queueNum >= NUM_QUEUES_TV) {
            cout << "Invalid queue number!" << endl;
            return;
        }

        if (isEmpty_tv(queueNum)) {
            cout << "Queue " << queueNum << " is empty!" << endl;
            return;
        }

        int index = getIndex_tv(queueNum, front_tv[queueNum]);
        cout << "\n=== Front of Queue " << queueNum << " ===" << endl;
        cout << "Ticket Number: " << arr_tv[index].ticketNumber << endl;
        cout << "Name: " << arr_tv[index].name << endl;
        cout << "Service: " << arr_tv[index].serviceType << endl;
        cout << "Position: Next to be served" << endl;
        cout << "=========================" << endl;
    }
};

int main() {
    MultipleQueues_tv queues_tv;
    int choice, queueNum;
    char name[30], serviceType[20];

    cout << "=== Multiple Queues Implementation ===" << endl;
    cout << "Implemented " << NUM_QUEUES_TV << " queues in an array of size " << MAX_SIZE_TV << endl;
    cout << "Queue 0: General Services" << endl;
    cout << "Queue 1: Special Services" << endl;

    while (true) {
        cout << "\n1. Add to Queue\n2. Delete from Queue\n3. Display Queue\n4. Display All Queues\n5. Queue Statistics\n6. Peek Front\n7. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter queue number (0-" << (NUM_QUEUES_TV - 1) << "): ";
                cin >> queueNum;
                cout << "Enter name (Indian name): ";
                cin >> name;
                cout << "Enter service type: ";
                cin >> serviceType;
                queues_tv.addQueue_tv(queueNum, name, serviceType);
                break;

            case 2:
                cout << "Enter queue number (0-" << (NUM_QUEUES_TV - 1) << "): ";
                cin >> queueNum;
                queues_tv.deleteFromQueue_tv(queueNum);
                break;

            case 3:
                cout << "Enter queue number (0-" << (NUM_QUEUES_TV - 1) << "): ";
                cin >> queueNum;
                queues_tv.displayQueue_tv(queueNum);
                break;

            case 4:
                queues_tv.displayAllQueues_tv();
                break;

            case 5:
                queues_tv.displayQueueStats_tv();
                break;

            case 6:
                cout << "Enter queue number (0-" << (NUM_QUEUES_TV - 1) << "): ";
                cin >> queueNum;
                queues_tv.peekFront_tv(queueNum);
                break;

            case 7:
                cout << "Thank you for using the Multiple Queues Implementation!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


##  Output

=== Multiple Queues Implementation ===
Implemented 2 queues in an array of size 20
Queue 0: General Services
Queue 1: Special Services

1. Add to Queue
2. Delete from Queue
3. Display Queue
4. Display All Queues
5. Queue Statistics
6. Peek Front
7. Exit
Enter your choice: 1
Enter queue number (0-1): 0
Enter name (Indian name): Arjun
Enter service type: Passport
Added to Queue 0: Arjun (Ticket #1, Service: Passport)

1. Add to Queue
2. Delete from Queue
3. Display Queue
4. Display All Queues
5. Queue Statistics
6. Peek Front
7. Exit
Enter your choice: 1
Enter queue number (0-1): 0
Enter name (Indian name): Priya
Enter service type: Visa
Added to Queue 0: Priya (Ticket #2, Service: Visa)

1. Add to Queue
2. Delete from Queue
3. Display Queue
4. Display All Queues
5. Queue Statistics
6. Peek Front
7. Exit
Enter your choice: 1
Enter queue number (0-1): 1
Enter name (Indian name): Rohit
Enter service type: Scholarship
Added to Queue 1: Rohit (Ticket #101, Service: Scholarship)

1. Add to Queue
2. Delete from Queue
3. Display Queue
4. Display All Queues
5. Queue Statistics
6. Peek Front
7. Exit
Enter your choice: 1
Enter queue number (0-1): 1
Enter name (Indian name): Sneha
Enter service type: Loan
Added to Queue 1: Sneha (Ticket #102, Service: Loan)

1. Add to Queue
2. Delete from Queue
3. Display Queue
4. Display All Queues
5. Queue Statistics
6. Peek Front
7. Exit
Enter your choice: 3
Enter queue number (0-1): 0

=== Queue 0 ===
Pos	Ticket #	Name		Service
----------------------------------------
1	1		Arjun		Passport
2	2		Priya		Visa
==============

1. Add to Queue
2. Delete from Queue
3. Display Queue
4. Display All Queues
5. Queue Statistics
6. Peek Front
7. Exit
Enter your choice: 3
Enter queue number (0-1): 1

=== Queue 1 ===
Pos	Ticket #	Name		Service
----------------------------------------
1	101		Rohit		Scholarship
2	102		Sneha		Loan
==============

1. Add to Queue
2. Delete from Queue
3. Display Queue
4. Display All Queues
5. Queue Statistics
6. Peek Front
7. Exit
Enter your choice: 5

=== Queue Statistics ===
Queue 0:
  Elements: 2
  Status: Partially filled
  Capacity: 10
  Next to serve: Arjun (Ticket #1)
  Last added: Priya (Ticket #2)

Queue 1:
  Elements: 2
  Status: Partially filled
  Capacity: 10
  Next to serve: Rohit (Ticket #101)
  Last added: Sneha (Ticket #102)

========================

1. Add to Queue
2. Delete from Queue
3. Display Queue
4. Display All Queues
5. Queue Statistics
6. Peek Front
7. Exit
Enter your choice: 6
Enter queue number (0-1): 0

=== Front of Queue 0 ===
Ticket Number: 1
Name: Arjun
Service: Passport
Position: Next to be served
=========================

1. Add to Queue
2. Delete from Queue
3. Display Queue
4. Display All Queues
5. Queue Statistics
6. Peek Front
7. Exit
Enter your choice: 2
Enter queue number (0-1): 0
Removed from Queue 0: Arjun (Ticket #1, Service: Passport)

1. Add to Queue
2. Delete from Queue
3. Display Queue
4. Display All Queues
5. Queue Statistics
6. Peek Front
7. Exit
Enter your choice: 4

=== All Queues ===

=== Queue 0 ===
Pos	Ticket #	Name		Service
----------------------------------------
1	2		Priya		Visa
==============

=== Queue 1 ===
Pos	Ticket #	Name		Service
----------------------------------------
1	101		Rohit		Scholarship
2	102		Sneha		Loan
==============
