## Problem Statement
Develop a C++ program to store and manage an appointment schedule for a single day. Appointments should be scheduled randomly using a linked list. The system must define the start time, end time, and specify the minimum and maximum duration allowed for each appointment slot. The program should include the following operations: a) Display the list of currently available time slots. b) Book a new appointment within the defined time limits. c) Cancel an existing appointment after validating its time, availability, and correctness. d) Sort the appointment list in order of appointment times. e) Sort the list based on appointment times using pointer manipulation (without swapping data values).

##  Code
#include <iostream>
#include <iomanip>
using namespace std;

struct Appointment_tv {
    int startTime;  
    int endTime;    
    char clientName[50];
    bool isBooked;
    Appointment_tv* next;
};

class AppointmentScheduler_tv {
private:
    Appointment_tv* head;
    int workStartTime;   
    int workEndTime;     
    int minDuration;     
    int maxDuration;     

public:
    AppointmentScheduler_tv() {
        head = nullptr;
        workStartTime = 540;  
        workEndTime = 1080;   
        minDuration = 30; 
        maxDuration = 120;    
        initializeTimeSlots_tv();
    }

    void initializeTimeSlots_tv() {
     
        for (int time = workStartTime; time < workEndTime; time += 30) {
            Appointment_tv* newSlot = new Appointment_tv;
            newSlot->startTime = time;
            newSlot->endTime = time + 30;
            newSlot->clientName[0] = '\0';
            newSlot->isBooked = false;
            newSlot->next = nullptr;

            if (head == nullptr) {
                head = newSlot;
            } else {
                Appointment_tv* temp = head;
                while (temp->next != nullptr) {
                    temp = temp->next;
                }
                temp->next = newSlot;
            }
        }
        cout << "Appointment scheduler initialized with time slots!" << endl;
    }

    void displayTimeSlots_tv() {
        cout << "\n=== Available Appointment Slots ===" << endl;
        cout << "Time\t\tStatus\t\tClient" << endl;
        cout << "----------------------------------------" << endl;

        Appointment_tv* temp = head;
        while (temp != nullptr) {
            cout << formatTime_tv(temp->startTime) << "-" << formatTime_tv(temp->endTime) << "\t";
            if (temp->isBooked) {
                cout << "Booked\t\t" << temp->clientName;
            } else {
                cout << "Available\t-";
            }
            cout << endl;
            temp = temp->next;
        }
    }

    string formatTime_tv(int minutes) {
        int hours = minutes / 60;
        int mins = minutes % 60;
        string timeStr = to_string(hours) + ":" + (mins < 10 ? "0" : "") + to_string(mins);
        return timeStr;
    }

    bool bookAppointment_tv(int startTime, const char* clientName) {
        Appointment_tv* temp = head;
        
       
        while (temp != nullptr) {
            if (temp->startTime == startTime) {
                if (temp->isBooked) {
                    cout << "Slot at " << formatTime_tv(startTime) << " is already booked!" << endl;
                    return false;
                } else {
                    temp->isBooked = true;
                    strcpy(temp->clientName, clientName);
                    cout << "Appointment booked for " << clientName << " at " << formatTime_tv(startTime) << endl;
                    return true;
                }
            }
            temp = temp->next;
        }
        
        cout << "Invalid time slot!" << endl;
        return false;
    }

    bool cancelAppointment_tv(int startTime) {
        Appointment_tv* temp = head;
        
    
        while (temp != nullptr) {
            if (temp->startTime == startTime) {
                if (!temp->isBooked) {
                    cout << "No appointment found at " << formatTime_tv(startTime) << "!" << endl;
                    return false;
                } else {
                    temp->isBooked = false;
                    temp->clientName[0] = '\0';
                    cout << "Appointment at " << formatTime_tv(startTime) << " cancelled successfully!" << endl;
                    return true;
                }
            }
            temp = temp->next;
        }
        
        cout << "Invalid time slot!" << endl;
        return false;
    }

    void sortAppointmentsByTime_tv() {
        if (head == nullptr) return;

        bool swapped;
        Appointment_tv* ptr1;
        Appointment_tv* lptr = nullptr;

        do {
            swapped = false;
            ptr1 = head;

            while (ptr1->next != lptr) {
                if (ptr1->startTime > ptr1->next->startTime) {
                    
                    int tempStart = ptr1->startTime;
                    ptr1->startTime = ptr1->next->startTime;
                    ptr1->next->startTime = tempStart;

                    int tempEnd = ptr1->endTime;
                    ptr1->endTime = ptr1->next->endTime;
                    ptr1->next->endTime = tempEnd;

                    bool tempBooked = ptr1->isBooked;
                    ptr1->isBooked = ptr1->next->isBooked;
                    ptr1->next->isBooked = tempBooked;

                    char tempName[50];
                    strcpy(tempName, ptr1->clientName);
                    strcpy(ptr1->clientName, ptr1->next->clientName);
                    strcpy(ptr1->next->clientName, tempName);

                    swapped = true;
                }
                ptr1 = ptr1->next;
            }
            lptr = ptr1;
        } while (swapped);

        cout << "Appointments sorted by time!" << endl;
    }

    void sortAppointmentsByPointer_tv() {
    
        if (head == nullptr || head->next == nullptr) return;

        Appointment_tv* sorted = nullptr;
        Appointment_tv* current = head;

        while (current != nullptr) {
            Appointment_tv* next = current->next;

            if (sorted == nullptr || sorted->startTime >= current->startTime) {
                current->next = sorted;
                sorted = current;
            } else {
                Appointment_tv* temp = sorted;
                while (temp->next != nullptr && temp->next->startTime < current->startTime) {
                    temp = temp->next;
                }
                current->next = temp->next;
                temp->next = current;
            }

            current = next;
        }

        head = sorted;
        cout << "Appointments sorted by pointer manipulation!" << endl;
    }

    int countAvailableSlots_tv() {
        int count = 0;
        Appointment_tv* temp = head;
        while (temp != nullptr) {
            if (!temp->isBooked) {
                count++;
            }
            temp = temp->next;
        }
        return count;
    }

    ~AppointmentScheduler_tv() {
        while (head != nullptr) {
            Appointment_tv* temp = head;
            head = head->next;
            delete temp;
        }
    }
};

int main() {
    AppointmentScheduler_tv scheduler_tv;
    int choice, timeSlot;
    char clientName[50];

    cout << "=== Daily Appointment Scheduler ===" << endl;
    cout << "Working hours: 09:00 AM - 06:00 PM" << endl;
    cout << "Available slots: " << scheduler_tv.countAvailableSlots_tv() << endl;

    while (true) {
        cout << "\n1. Display Time Slots\n2. Book Appointment\n3. Cancel Appointment\n4. Sort by Time (Data Swap)\n5. Sort by Time (Pointer)\n6. Count Available Slots\n7. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                scheduler_tv.displayTimeSlots_tv();
                break;

            case 2:
                scheduler_tv.displayTimeSlots_tv();
                cout << "\nEnter start time (in 24-hour format, e.g., 540 for 09:00): ";
                cin >> timeSlot;
                cout << "Enter client name (Indian name): ";
                cin >> clientName;
                scheduler_tv.bookAppointment_tv(timeSlot, clientName);
                break;

            case 3:
                scheduler_tv.displayTimeSlots_tv();
                cout << "\nEnter start time to cancel (e.g., 540 for 09:00): ";
                cin >> timeSlot;
                scheduler_tv.cancelAppointment_tv(timeSlot);
                break;

            case 4:
                scheduler_tv.sortAppointmentsByTime_tv();
                break;

            case 5:
                scheduler_tv.sortAppointmentsByPointer_tv();
                break;

            case 6:
                cout << "Available slots: " << scheduler_tv.countAvailableSlots_tv() << endl;
                break;

            case 7:
                cout << "Thank you for using the Appointment Scheduler!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


##  Output

=== Daily Appointment Scheduler ===
Working hours: 09:00 AM - 06:00 PM
Available slots: 18
Appointment scheduler initialized with time slots!

1. Display Time Slots
2. Book Appointment
3. Cancel Appointment
4. Sort by Time (Data Swap)
5. Sort by Time (Pointer)
6. Count Available Slots
7. Exit
Enter your choice: 1

=== Available Appointment Slots ===
Time		Status		Client
----------------------------------------
9:0-9:30	Available	-
9:30-10:0	Available	-
10:0-10:30	Available	-
10:30-11:0	Available	-
11:0-11:30	Available	-
11:30-12:0	Available	-
12:0-12:30	Available	-
12:30-13:0	Available	-
13:0-13:30	Available	-
13:30-14:0	Available	-
14:0-14:30	Available	-
14:30-15:0	Available	-
15:0-15:30	Available	-
15:30-16:0	Available	-
16:0-16:30	Available	-
16:30-17:0	Available	-
17:0-17:30	Available	-
17:30-18:0	Available	-

1. Display Time Slots
2. Book Appointment
3. Cancel Appointment
4. Sort by Time (Data Swap)
5. Sort by Time (Pointer)
6. Count Available Slots
7. Exit
Enter your choice: 2

=== Available Appointment Slots ===
Time		Status		Client
----------------------------------------
9:0-9:30	Available	-
9:30-10:0	Available	-
10:0-10:30	Available	-
10:30-11:0	Available	-
11:0-11:30	Available	-
11:30-12:0	Available	-
12:0-12:30	Available	-
12:30-13:0	Available	-
13:0-13:30	Available	-
13:30-14:0	Available	-
14:0-14:30	Available	-
14:30-15:0	Available	-
15:0-15:30	Available	-
15:30-16:0	Available	-
16:0-16:30	Available	-
16:30-17:0	Available	-
17:0-17:30	Available	-
17:30-18:0	Available	-

Enter start time (in 24-hour format, e.g., 540 for 09:00): 540
Enter client name : Arjun
Appointment booked for Arjun at 9:0

1. Display Time Slots
2. Book Appointment
3. Cancel Appointment
4. Sort by Time (Data Swap)
5. Sort by Time (Pointer)
6. Count Available Slots
7. Exit
Enter your choice: 2

Enter start time (in 24-hour format, e.g., 540 for 09:00): 600
Enter client name : Priya
Appointment booked for Priya at 10:0

1. Display Time Slots
2. Book Appointment
3. Cancel Appointment
4. Sort by Time (Data Swap)
5. Sort by Time (Pointer)
6. Count Available Slots
7. Exit
Enter your choice: 1

=== Available Appointment Slots ===
Time		Status		Client
----------------------------------------
9:0-9:30	Booked		Arjun
9:30-10:0	Available	-
10:0-10:30	Booked		Priya
10:30-11:0	Available	-
11:0-11:30	Available	-
11:30-12:0	Available	-
12:0-12:30	Available	-
12:30-13:0	Available	-
13:0-13:30	Available	-
13:30-14:0	Available	-
14:0-14:30	Available	-
14:30-15:0	Available	-
15:0-15:30	Available	-
15:30-16:0	Available	-
16:0-16:30	Available	-
16:30-17:0	Available	-
17:0-17:30	Available	-
17:30-18:0	Available	-

1. Display Time Slots
2. Book Appointment
3. Cancel Appointment
4. Sort by Time (Data Swap)
5. Sort by Time (Pointer)
6. Count Available Slots
7. Exit
Enter your choice: 3

=== Available Appointment Slots ===
Time		Status		Client
----------------------------------------
9:0-9:30	Booked		Arjun
9:30-10:0	Available	-
10:0-10:30	Booked		Priya
10:30-11:0	Available	-
11:0-11:30	Available	-
11:30-12:0	Available	-
12:0-12:30	Available	-
12:30-13:0	Available	-
13:0-13:30	Available	-
13:30-14:0	Available	-
14:0-14:30	Available	-
14:30-15:0	Available	-
15:0-15:30	Available	-
15:30-16:0	Available	-
16:0-16:30	Available	-
16:30-17:0	Available	-
17:0-17:30	Available	-
17:30-18:0	Available	-

Enter start time to cancel (e.g., 540 for 09:00): 540
Appointment at 9:0 cancelled successfully!

1. Display Time Slots
2. Book Appointment
3. Cancel Appointment
4. Sort by Time (Data Swap)
5. Sort by Time (Pointer)
6. Count Available Slots
7. Exit
Enter your choice: 6
Available slots: 17


