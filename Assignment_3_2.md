## Problem Statement
The ticket reservation system for Galaxy Multiplex is to be implemented using a C++ program. The multiplex has 8 rows, with 8 seats in each row. A doubly circular linked list will be used to track the availability of seats in each row. Initially, assume that some seats are randomly booked. An array will store head pointers for each row's linked list. The system should support the following operations: a) Display the current list of available seats. b) Book one or more seats as per customer request. c) Cancel an existing booking when requested.

## Solution Code
```cpp
#include <iostream>
#include <cstdlib>
#include <ctime>
using namespace std;

struct Seat_tv {
    int seatNumber;
    bool isBooked;
    Seat_tv* next;
    Seat_tv* prev;
};

class TicketReservation_tv {
private:
    Seat_tv* rows_tv[8]; 
    int seatsPerRow_tv;

public:
    TicketReservation_tv(int seatsPerRow = 8) {
        seatsPerRow_tv = seatsPerRow;
       
        for (int i = 0; i < 8; i++) {
            rows_tv[i] = nullptr;
            createRow_tv(i);
        }

        srand(time(0));
        randomlyBookSeats_tv();
    }

    void createRow_tv(int row) {
     
        for (int i = 1; i <= seatsPerRow_tv; i++) {
            Seat_tv* newSeat = new Seat_tv;
            newSeat->seatNumber = i;
            newSeat->isBooked = false;

            if (rows_tv[row] == nullptr) {
                
                rows_tv[row] = newSeat;
                newSeat->next = newSeat;
                newSeat->prev = newSeat;
            } else {
               
                Seat_tv* last = rows_tv[row]->prev;
                newSeat->next = rows_tv[row];
                newSeat->prev = last;
                last->next = newSeat;
                rows_tv[row]->prev = newSeat;
            }
        }
    }

    void randomlyBookSeats_tv() {
     
        int seatsToBook = 10 + rand() % 11;
        for (int i = 0; i < seatsToBook; i++) {
            int row = rand() % 8;
            int seat = 1 + rand() % seatsPerRow_tv;
            bookSeat_tv(row, seat);
        }
    }

    bool bookSeat_tv(int row, int seatNumber) {
        if (row < 0 || row >= 8 || seatNumber < 1 || seatNumber > seatsPerRow_tv) {
            cout << "Invalid row or seat number!" << endl;
            return false;
        }

        Seat_tv* current = rows_tv[row];
       
        do {
            if (current->seatNumber == seatNumber) {
                if (current->isBooked) {
                    cout << "Seat " << (row + 1) << "-" << seatNumber << " is already booked!" << endl;
                    return false;
                } else {
                    current->isBooked = true;
                    cout << "Seat " << (row + 1) << "-" << seatNumber << " booked successfully!" << endl;
                    return true;
                }
            }
            current = current->next;
        } while (current != rows_tv[row]);

        cout << "Seat not found!" << endl;
        return false;
    }

    bool cancelBooking_tv(int row, int seatNumber) {
        if (row < 0 || row >= 8 || seatNumber < 1 || seatNumber > seatsPerRow_tv) {
            cout << "Invalid row or seat number!" << endl;
            return false;
        }

        Seat_tv* current = rows_tv[row];
      
        do {
            if (current->seatNumber == seatNumber) {
                if (!current->isBooked) {
                    cout << "Seat " << (row + 1) << "-" << seatNumber << " is not booked!" << endl;
                    return false;
                } else {
                    current->isBooked = false;
                    cout << "Booking for seat " << (row + 1) << "-" << seatNumber << " cancelled successfully!" << endl;
                    return true;
                }
            }
            current = current->next;
        } while (current != rows_tv[row]);

        cout << "Seat not found!" << endl;
        return false;
    }

    void displayAvailableSeats_tv() {
        cout << "\n=== Available Seats in Galaxy Multiplex ===" << endl;
        cout << "Row\tSeats (A-Available, B-Booked)" << endl;
        cout << "----------------------------------------" << endl;

        for (int i = 0; i < 8; i++) {
            cout << "Row " << (i + 1) << ":\t";
            Seat_tv* current = rows_tv[i];
            if (current != nullptr) {
                do {
                    if (current->isBooked) {
                        cout << "B ";
                    } else {
                        cout << "A ";
                    }
                    current = current->next;
                } while (current != rows_tv[i]);
            }
            cout << endl;
        }
    }

    void displayDetailedSeats_tv() {
        cout << "\n=== Detailed Seat Status ===" << endl;
        for (int i = 0; i < 8; i++) {
            cout << "\nRow " << (i + 1) << ":" << endl;
            Seat_tv* current = rows_tv[i];
            if (current != nullptr) {
                do {
                    cout << "Seat " << current->seatNumber << ": " 
                         << (current->isBooked ? "Booked" : "Available") << endl;
                    current = current->next;
                } while (current != rows_tv[i]);
            }
        }
    }

    int countAvailableSeats_tv() {
        int count = 0;
        for (int i = 0; i < 8; i++) {
            Seat_tv* current = rows_tv[i];
            if (current != nullptr) {
                do {
                    if (!current->isBooked) {
                        count++;
                    }
                    current = current->next;
                } while (current != rows_tv[i]);
            }
        }
        return count;
    }

    ~TicketReservation_tv() {
        for (int i = 0; i < 8; i++) {
            if (rows_tv[i] != nullptr) {
                Seat_tv* current = rows_tv[i];
                Seat_tv* next = current->next;
                while (next != rows_tv[i]) {
                    delete current;
                    current = next;
                    next = next->next;
                }
                delete current;
            }
        }
    }
};

int main() {
    TicketReservation_tv multiplex_tv;
    int choice, row, seat;

    cout << "=== Galaxy Multiplex Ticket Reservation System ===" << endl;
    cout << "Total available seats: " << multiplex_tv.countAvailableSeats_tv() << endl;

    while (true) {
        cout << "\n1. Display Available Seats\n2. Book a Seat\n3. Cancel Booking\n4. Display Detailed Status\n5. Count Available Seats\n6. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                multiplex_tv.displayAvailableSeats_tv();
                break;

            case 2:
                multiplex_tv.displayAvailableSeats_tv();
                cout << "\nEnter row number (1-8): ";
                cin >> row;
                cout << "Enter seat number (1-8): ";
                cin >> seat;
                multiplex_tv.bookSeat_tv(row - 1, seat);
                break;

            case 3:
                cout << "\nEnter row number (1-8): ";
                cin >> row;
                cout << "Enter seat number (1-8): ";
                cin >> seat;
                multiplex_tv.cancelBooking_tv(row - 1, seat);
                break;

            case 4:
                multiplex_tv.displayDetailedSeats_tv();
                break;

            case 5:
                cout << "Total available seats: " << multiplex_tv.countAvailableSeats_tv() << endl;
                break;

            case 6:
                cout << "Thank you for using Galaxy Multiplex Reservation System!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


##  Output

=== Galaxy Multiplex Ticket Reservation System ===
Total available seats: 45

1. Display Available Seats
2. Book a Seat
3. Cancel Booking
4. Display Detailed Status
5. Count Available Seats
6. Exit
Enter your choice: 1

=== Available Seats in Galaxy Multiplex ===
Row	Seats (A-Available, B-Booked)
----------------------------------------
Row 1:	A A A B A A A A 
Row 2:	A A A A A B A A 
Row 3:	B A A A A A A B 
Row 4:	A A B A A A A A 
Row 5:	A A A A B A A A 
Row 6:	A B A A A A B A 
Row 7:	A A A A A A A A 
Row 8:	A A B A A A A B 

1. Display Available Seats
2. Book a Seat
3. Cancel Booking
4. Display Detailed Status
5. Count Available Seats
6. Exit
Enter your choice: 2

=== Available Seats in Galaxy Multiplex ===
Row	Seats (A-Available, B-Booked)
----------------------------------------
Row 1:	A A A B A A A A 
Row 2:	A A A A A B A A 
Row 3:	B A A A A A A B 
Row 4:	A A B A A A A A 
Row 5:	A A A A B A A A 
Row 6:	A B A A A A B A 
Row 7:	A A A A A A A A 
Row 8:	A A B A A A A B 

Enter row number (1-8): 1
Enter seat number (1-8): 1
Seat 1-1 booked successfully!

1. Display Available Seats
2. Book a Seat
3. Cancel Booking
4. Display Detailed Status
5. Count Available Seats
6. Exit
Enter your choice: 3

Enter row number (1-8): 1
Enter seat number (1-8): 4
Booking for seat 1-4 cancelled successfully!

1. Display Available Seats
2. Book a Seat
3. Cancel Booking
4. Display Detailed Status
5. Count Available Seats
6. Exit
Enter your choice: 5
Total available seats: 46

