## Problem Statement
In the Second Year Computer Engineering class, there are two groups of students based on their favorite sports: Set A includes students who like Cricket. Set B includes students who like Football. Write a C++ program to represent these two sets using linked lists and perform the following operations: a) Find and display the set of students who like both Cricket and Football. b) Find and display the set of students who like either Cricket or Football, but not both. c) Display the number of students who like neither Cricket nor Football.

##  Code
```cpp
#include <iostream>
#include <cstring>
using namespace std;

struct Student_tv {
    char name[50];
    Student_tv* next;
};

class SportsSet_tv {
private:
    Student_tv* head;

public:
    SportsSet_tv() {
        head = nullptr;
    }

    void addStudent_tv(const char* name) {
        Student_tv* newStudent = new Student_tv;
        strcpy(newStudent->name, name);
        newStudent->next = head;
        head = newStudent;
    }

    bool isStudentInSet_tv(const char* name) {
        Student_tv* temp = head;
        while (temp != nullptr) {
            if (strcmp(temp->name, name) == 0) {
                return true;
            }
            temp = temp->next;
        }
        return false;
    }

    void displaySet_tv(const char* setName) {
        cout << "\nStudents who like " << setName << ":" << endl;
        if (head == nullptr) {
            cout << "No students in this set." << endl;
            return;
        }

        Student_tv* temp = head;
        int count = 1;
        while (temp != nullptr) {
            cout << count << ". " << temp->name << endl;
            temp = temp->next;
            count++;
        }
    }

    int countStudents_tv() {
        int count = 0;
        Student_tv* temp = head;
        while (temp != nullptr) {
            count++;
            temp = temp->next;
        }
        return count;
    }

    Student_tv* getHead_tv() {
        return head;
    }

    ~SportsSet_tv() {
        while (head != nullptr) {
            Student_tv* temp = head;
            head = head->next;
            delete temp;
        }
    }
};


SportsSet_tv* findIntersection_tv(SportsSet_tv& cricket, SportsSet_tv& football) {
    SportsSet_tv* intersection = new SportsSet_tv();
    
    Student_tv* temp = cricket.getHead_tv();
    while (temp != nullptr) {
        if (football.isStudentInSet_tv(temp->name)) {
            intersection->addStudent_tv(temp->name);
        }
        temp = temp->next;
    }
    
    return intersection;
}


SportsSet_tv* findSymmetricDifference_tv(SportsSet_tv& cricket, SportsSet_tv& football) {
    SportsSet_tv* symmetricDiff = new SportsSet_tv();
    
    Student_tv* temp = cricket.getHead_tv();
    while (temp != nullptr) {
        if (!football.isStudentInSet_tv(temp->name)) {
            symmetricDiff->addStudent_tv(temp->name);
        }
        temp = temp->next;
    }
    
    temp = football.getHead_tv();
    while (temp != nullptr) {
        if (!cricket.isStudentInSet_tv(temp->name)) {
            symmetricDiff->addStudent_tv(temp->name);
        }
        temp = temp->next;
    }
    
    return symmetricDiff;
}


SportsSet_tv* findUnion_tv(SportsSet_tv& cricket, SportsSet_tv& football) {
    SportsSet_tv* unionSet = new SportsSet_tv();
    

    Student_tv* temp = cricket.getHead_tv();
    while (temp != nullptr) {
        unionSet->addStudent_tv(temp->name);
        temp = temp->next;
    }
    
    temp = football.getHead_tv();
    while (temp != nullptr) {
        if (!unionSet->isStudentInSet_tv(temp->name)) {
            unionSet->addStudent_tv(temp->name);
        }
        temp = temp->next;
    }
    
    return unionSet;
}

int main() {
    SportsSet_tv cricket_tv, football_tv;
    int choice, totalStudents;
    char studentName[50];

    cout << "=== Sports Preference Management System ===" << endl;
    
 
    cout << "\nEnter number of students who like Cricket: ";
    cin >> choice;
    cout << "Enter names of students who like Cricket:" << endl;
    for (int i = 0; i < choice; i++) {
        cout << "Student " << (i+1) << " (Indian name): ";
        cin >> studentName;
        cricket_tv.addStudent_tv(studentName);
    }

    
    cout << "\nEnter number of students who like Football: ";
    cin >> choice;
    cout << "Enter names of students who like Football:" << endl;
    for (int i = 0; i < choice; i++) {
        cout << "Student " << (i+1) << " (Indian name): ";
        cin >> studentName;
        football_tv.addStudent_tv(studentName);
    }

    cout << "\nEnter total number of students in class: ";
    cin >> totalStudents;

    while (true) {
        cout << "\n1. Display Cricket Set\n2. Display Football Set\n3. Find students who like both\n4. Find students who like either but not both\n5. Count students who like neither\n6. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cricket_tv.displaySet_tv("Cricket");
                break;

            case 2:
                football_tv.displaySet_tv("Football");
                break;

            case 3: {
                SportsSet_tv* intersection = findIntersection_tv(cricket_tv, football_tv);
                intersection->displaySet_tv("both Cricket and Football");
                delete intersection;
                break;
            }

            case 4: {
                SportsSet_tv* symDiff = findSymmetricDifference_tv(cricket_tv, football_tv);
                symDiff->displaySet_tv("either Cricket or Football, but not both");
                delete symDiff;
                break;
            }

            case 5: {
                SportsSet_tv* unionSet = findUnion_tv(cricket_tv, football_tv);
                int unionCount = unionSet->countStudents_tv();
                int neitherCount = totalStudents - unionCount;
                cout << "\nNumber of students who like neither Cricket nor Football: " << neitherCount << endl;
                delete unionSet;
                break;
            }

            case 6:
                cout << "Thank you for using the Sports Preference Management System!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


## Output

=== Sports Preference Management System ===

Enter number of students who like Cricket: 5
Enter names of students who like Cricket:
Student 1 ( name): Arjun
Student 2 ( name): Priya
Student 3 ( name): Rohit
Student 4 ( name): Sneha
Student 5 ( name): Vikram

Enter number of students who like Football: 4
Enter names of students who like Football:
Student 1 ( name): Priya
Student 2 ( name): Sneha
Student 3 ( name): Anjali
Student 4 ( name): Rajesh

Enter total number of students in class: 10

1. Display Cricket Set
2. Display Football Set
3. Find students who like both
4. Find students who like either but not both
5. Count students who like neither
6. Exit
Enter your choice: 1

Students who like Cricket:
1. Vikram
2. Sneha
3. Rohit
4. Priya
5. Arjun

1. Display Cricket Set
2. Display Football Set
3. Find students who like both
4. Find students who like either but not both
5. Count students who like neither
6. Exit
Enter your choice: 2

Students who like Football:
1. Rajesh
2. Anjali
3. Sneha
4. Priya

1. Display Cricket Set
2. Display Football Set
3. Find students who like both
4. Find students who like either but not both
5. Count students who like neither
6. Exit
Enter your choice: 3

Students who like both Cricket and Football:
1. Sneha
2. Priya

1. Display Cricket Set
2. Display Football Set
3. Find students who like both
4. Find students who like either but not both
5. Count students who like neither
6. Exit
Enter your choice: 4

Students who like either Cricket or Football, but not both:
1. Rajesh
2. Anjali
3. Vikram
4. Rohit
5. Arjun

1. Display Cricket Set
2. Display Football Set
3. Find students who like both
4. Find students who like either but not both
5. Count students who like neither
6. Exit
Enter your choice: 5

Number of students who like neither Cricket nor Football: 3

