## Problem Statement
In a call center, customer calls are handled on a first-come, first-served basis. Implement a queue system using Linked list where: Each customer call is enqueued as it arrives. Customer service agents dequeue calls to assist customers. If there are no calls, the system waits.

##  Code
```cpp
#include <iostream>
#include <cstring>
#include <ctime>
using namespace std;

struct Call_tv {
    int callId;
    char customerName[50];
    char phoneNumber[15];
    char issue[100];
    time_t arrivalTime;
    Call_tv* next;
};

class CallCenter_tv {
private:
    Call_tv* front;
    Call_tv* rear;
    int callCounter_tv;
    int totalCallsHandled_tv;

public:
    CallCenter_tv() {
        front = nullptr;
        rear = nullptr;
        callCounter_tv = 1000;  
        totalCallsHandled_tv = 0;
    }


    void enqueueCall_tv(const char* customerName, const char* phoneNumber, const char* issue) {
        Call_tv* newCall = new Call_tv;
        newCall->callId = callCounter_tv++;
        strcpy(newCall->customerName, customerName);
        strcpy(newCall->phoneNumber, phoneNumber);
        strcpy(newCall->issue, issue);
        newCall->arrivalTime = time(nullptr);
        newCall->next = nullptr;

        if (rear == nullptr) {
            
            front = rear = newCall;
        } else {
            
            rear->next = newCall;
            rear = newCall;
        }

        cout << "\n=== New Call Received ===" << endl;
        cout << "Call ID: " << newCall->callId << endl;
        cout << "Customer: " << customerName << endl;
        cout << "Phone: " << phoneNumber << endl;
        cout << "Issue: " << issue << endl;
        cout << "Position in queue: " << getQueueSize_tv() << endl;
        cout << "========================" << endl;
    }

    
    void dequeueCall_tv() {
        if (front == nullptr) {
            cout << "No calls in queue. Customer service agent is waiting for calls." << endl;
            return;
        }

        Call_tv* servedCall = front;
        cout << "\n=== Call Being Handled ===" << endl;
        cout << "Call ID: " << servedCall->callId << endl;
        cout << "Customer: " << servedCall->customerName << endl;
        cout << "Phone: " << servedCall->phoneNumber << endl;
        cout << "Issue: " << servedCall->issue << endl;
        
       
        time_t currentTime = time(nullptr);
        double waitTime = difftime(currentTime, servedCall->arrivalTime);
        cout << "Wait Time: " << waitTime << " seconds" << endl;
        cout << "=========================" << endl;

        
        if (front == rear) {
            
            front = rear = nullptr;
        } else {
            front = front->next;
        }

        totalCallsHandled_tv++;
        delete servedCall;
    }

    
    void displayPendingCalls_tv() {
        if (front == nullptr) {
            cout << "No pending calls in queue." << endl;
            return;
        }

        cout << "\n=== Pending Calls Queue ===" << endl;
        cout << "Pos\tCall ID\tCustomer\t\tPhone\t\tIssue" << endl;
        cout << "----------------------------------------------------------------" << endl;

        Call_tv* temp = front;
        int position = 1;
        while (temp != nullptr) {
            cout << position << "\t" << temp->callId << "\t" << temp->customerName 
                 << "\t\t" << temp->phoneNumber << "\t\t" << temp->issue << endl;
            temp = temp->next;
            position++;
        }
        cout << "===========================" << endl;
    }

    
    int getQueueSize_tv() {
        int count = 0;
        Call_tv* temp = front;
        while (temp != nullptr) {
            count++;
            temp = temp->next;
        }
        return count;
    }

    
    bool isQueueEmpty_tv() {
        return front == nullptr;
    }

    
    void displayCallStats_tv() {
        cout << "\n=== Call Center Statistics ===" << endl;
        cout << "Pending calls: " << getQueueSize_tv() << endl;
        cout << "Total calls handled: " << totalCallsHandled_tv << endl;
        cout << "Queue status: " << (isQueueEmpty_tv() ? "Empty" : "Has pending calls") << endl;
        
        if (!isQueueEmpty_tv()) {
            cout << "Next call to handle: #" << front->callId << " (" << front->customerName << ")" << endl;
            cout << "Last call received: #" << rear->callId << " (" << rear->customerName << ")" << endl;
            
            
            time_t currentTime = time(nullptr);
            double waitTime = difftime(currentTime, front->arrivalTime);
            cout << "Current wait time for next call: " << waitTime << " seconds" << endl;
        }
        cout << "=============================" << endl;
    }

    
    void peekNextCall_tv() {
        if (front == nullptr) {
            cout << "No calls in queue." << endl;
            return;
        }

        cout << "\n=== Next Call to Handle ===" << endl;
        cout << "Call ID: " << front->callId << endl;
        cout << "Customer: " << front->customerName << endl;
        cout << "Phone: " << front->phoneNumber << endl;
        cout << "Issue: " << front->issue << endl;
        
        
        time_t currentTime = time(nullptr);
        double waitTime = difftime(currentTime, front->arrivalTime);
        cout << "Wait Time: " << waitTime << " seconds" << endl;
        cout << "Position: Next in line" << endl;
        cout << "=========================" << endl;
    }

    
    void simulateCallArrival_tv() {
        const char* names[] = {"Arjun", "Priya", "Rohit", "Sneha", "Vikram", "Anjali", "Rajesh", "Meera"};
        const char* phones[] = {"9876543210", "9876543211", "9876543212", "9876543213", "9876543214", "9876543215", "9876543216", "9876543217"};
        const char* issues[] = {"Internet not working", "Billing issue", "Service complaint", "New connection", "Technical support", "Account query", "Complaint", "Information"};

        int nameIndex = rand() % 8;
        int phoneIndex = rand() % 8;
        int issueIndex = rand() % 8;

        enqueueCall_tv(names[nameIndex], phones[phoneIndex], issues[issueIndex]);
    }

    
    void clearQueue_tv() {
        while (front != nullptr) {
            Call_tv* temp = front;
            front = front->next;
            delete temp;
        }
        rear = nullptr;
        cout << "All calls cleared from queue!" << endl;
    }

    ~CallCenter_tv() {
        clearQueue_tv();
    }
};

int main() {
    CallCenter_tv callCenter_tv;
    int choice;
    char customerName[50], phoneNumber[15], issue[100];

    cout << "=== Call Center Queue Management System ===" << endl;
    cout << "Customer calls are handled on First-Come, First-Served basis" << endl;

    
    srand(time(nullptr));

    while (true) {
        cout << "\n1. Receive New Call\n2. Handle Call (Agent)\n3. Display Pending Calls\n4. Call Statistics\n5. Peek Next Call\n6. Simulate Random Call\n7. Clear Queue\n8. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter customer name (Indian name): ";
                cin >> customerName;
                cout << "Enter phone number: ";
                cin >> phoneNumber;
                cout << "Enter issue description: ";
                cin.ignore();  
                cin.getline(issue, 100);
                callCenter_tv.enqueueCall_tv(customerName, phoneNumber, issue);
                break;

            case 2:
                callCenter_tv.dequeueCall_tv();
                break;

            case 3:
                callCenter_tv.displayPendingCalls_tv();
                break;

            case 4:
                callCenter_tv.displayCallStats_tv();
                break;

            case 5:
                callCenter_tv.peekNextCall_tv();
                break;

            case 6:
                callCenter_tv.simulateCallArrival_tv();
                break;

            case 7:
                callCenter_tv.clearQueue_tv();
                break;

            case 8:
                cout << "Thank you for using the Call Center Management System!" << endl;
                cout << "Final Statistics:" << endl;
                callCenter_tv.displayCallStats_tv();
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


##  Output

=== Call Center Queue Management System ===
Customer calls are handled on First-Come, First-Served basis

1. Receive New Call
2. Handle Call (Agent)
3. Display Pending Calls
4. Call Statistics
5. Peek Next Call
6. Simulate Random Call
7. Clear Queue
8. Exit
Enter your choice: 1
Enter customer name (Indian name): Arjun
Enter phone number: 9876543210
Enter issue description: Internet not working

=== New Call Received ===
Call ID: 1000
Customer: Arjun
Phone: 9876543210
Issue: Internet not working
Position in queue: 1
========================

1. Receive New Call
2. Handle Call (Agent)
3. Display Pending Calls
4. Call Statistics
5. Peek Next Call
6. Simulate Random Call
7. Clear Queue
8. Exit
Enter your choice: 1
Enter customer name (Indian name): Priya
Enter phone number: 9876543211
Enter issue description: Billing issue

=== New Call Received ===
Call ID: 1001
Customer: Priya
Phone: 9876543211
Issue: Billing issue
Position in queue: 2
========================

1. Receive New Call
2. Handle Call (Agent)
3. Display Pending Calls
4. Call Statistics
5. Peek Next Call
6. Simulate Random Call
7. Clear Queue
8. Exit
Enter your choice: 1
Enter customer name (Indian name): Rohit
Enter phone number: 9876543212
Enter issue description: Service complaint

=== New Call Received ===
Call ID: 1002
Customer: Rohit
Phone: 9876543212
Issue: Service complaint
Position in queue: 3
========================

1. Receive New Call
2. Handle Call (Agent)
3. Display Pending Calls
4. Call Statistics
5. Peek Next Call
6. Simulate Random Call
7. Clear Queue
8. Exit
Enter your choice: 3

=== Pending Calls Queue ===
Pos	Call ID	Customer		Phone		Issue
----------------------------------------------------------------
1	1000	Arjun		9876543210		Internet not working
2	1001	Priya		9876543211		Billing issue
3	1002	Rohit		9876543212		Service complaint
===========================

1. Receive New Call
2. Handle Call (Agent)
3. Display Pending Calls
4. Call Statistics
5. Peek Next Call
6. Simulate Random Call
7. Clear Queue
8. Exit
Enter your choice: 4

=== Call Center Statistics ===
Pending calls: 3
Total calls handled: 0
Queue status: Has pending calls
Next call to handle: #1000 (Arjun)
Last call received: #1002 (Rohit)
Current wait time for next call: 15 seconds
=============================

1. Receive New Call
2. Handle Call (Agent)
3. Display Pending Calls
4. Call Statistics
5. Peek Next Call
6. Simulate Random Call
7. Clear Queue
8. Exit
Enter your choice: 5

=== Next Call to Handle ===
Call ID: 1000
Customer: Arjun
Phone: 9876543210
Issue: Internet not working
Wait Time: 20 seconds
Position: Next in line
=========================

1. Receive New Call
2. Handle Call (Agent)
3. Display Pending Calls
4. Call Statistics
5. Peek Next Call
6. Simulate Random Call
7. Clear Queue
8. Exit
Enter your choice: 2

=== Call Being Handled ===
Call ID: 1000
Customer: Arjun
Phone: 9876543210
Issue: Internet not working
Wait Time: 25 seconds
=========================

1. Receive New Call
2. Handle Call (Agent)
3. Display Pending Calls
4. Call Statistics
5. Peek Next Call
6. Simulate Random Call
7. Clear Queue
8. Exit
Enter your choice: 6
=== New Call Received ===
Call ID: 1003
Customer: Sneha
Phone: 9876543213
Issue: New connection
Position in queue: 3
========================

1. Receive New Call
2. Handle Call (Agent)
3. Display Pending Calls
4. Call Statistics
5. Peek Next Call
6. Simulate Random Call
7. Clear Queue
8. Exit
Enter your choice: 3

=== Pending Calls Queue ===
Pos	Call ID	Customer		Phone		Issue
----------------------------------------------------------------
1	1001	Priya		9876543211		Billing issue
2	1002	Rohit		9876543212		Service complaint
3	1003	Sneha		9876543213		New connection
===========================

1. Receive New Call
2. Handle Call (Agent)
3. Display Pending Calls
4. Call Statistics
5. Peek Next Call
6. Simulate Random Call
7. Clear Queue
8. Exit
Enter your choice: 7
All calls cleared from queue!

Final Statistics:

=== Call Center Statistics ===
Pending calls: 0
Total calls handled: 1
Queue status: Empty
=============================

