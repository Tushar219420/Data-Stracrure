## Problem Statement
Write a program to keep track of patients as they checked into a medical clinic, assigning patients to doctors on a first-come, first-served basis.

## Code
```cpp
#include <iostream>
#include <cstring>
using namespace std;

const int MAX_DOCTORS_TV = 5;

struct Patient_tv {
    int patientId;
    char name[50];
    int age;
    char symptoms[100];
    Patient_tv* next;
};

struct Doctor_tv {
    int doctorId;
    char name[50];
    char specialization[50];
    int patientsAssigned;
};

class PatientQueue_tv {
private:
    Patient_tv* front;
    Patient_tv* rear;
    Doctor_tv doctors_tv[MAX_DOCTORS_TV];
    int doctorCount_tv;
    int patientCounter_tv;

public:
    PatientQueue_tv() {
        front = nullptr;
        rear = nullptr;
        doctorCount_tv = 0;
        patientCounter_tv = 1000;  
        
        initializeDoctors_tv();
    }

    void initializeDoctors_tv() {
    
        addDoctor_tv(1, "Dr. Sharma", "General Physician");
        addDoctor_tv(2, "Dr. Patel", "Cardiologist");
        addDoctor_tv(3, "Dr. Gupta", "Dermatologist");
        addDoctor_tv(4, "Dr. Kumar", "Pediatrician");
        addDoctor_tv(5, "Dr. Singh", "Orthopedic");
    }

    void addDoctor_tv(int id, const char* name, const char* specialization) {
        if (doctorCount_tv < MAX_DOCTORS_TV) {
            doctors_tv[doctorCount_tv].doctorId = id;
            strcpy(doctors_tv[doctorCount_tv].name, name);
            strcpy(doctors_tv[doctorCount_tv].specialization, specialization);
            doctors_tv[doctorCount_tv].patientsAssigned = 0;
            doctorCount_tv++;
            cout << "Doctor " << name << " added successfully!" << endl;
        } else {
            cout << "Cannot add more doctors. Maximum limit reached!" << endl;
        }
    }

    void registerPatient_tv(const char* name, int age, const char* symptoms) {
        Patient_tv* newPatient = new Patient_tv;
        newPatient->patientId = patientCounter_tv++;
        strcpy(newPatient->name, name);
        newPatient->age = age;
        strcpy(newPatient->symptoms, symptoms);
        newPatient->next = nullptr;

        if (rear == nullptr) {
            
            front = rear = newPatient;
        } else {
           
            rear->next = newPatient;
            rear = newPatient;
        }

        cout << "Patient " << name << " (ID: " << newPatient->patientId 
             << ") registered successfully!" << endl;
        cout << "Position in queue: " << getQueueSize_tv() << endl;
    }

    void assignPatientToDoctor_tv() {
        if (front == nullptr) {
            cout << "No patients in queue!" << endl;
            return;
        }

        int minIndex = 0;
        for (int i = 1; i < doctorCount_tv; i++) {
            if (doctors_tv[i].patientsAssigned < doctors_tv[minIndex].patientsAssigned) {
                minIndex = i;
            }
        }

        
        Patient_tv* patient = front;
        front = front->next;
        
        if (front == nullptr) {
            
            rear = nullptr;
        }

        doctors_tv[minIndex].patientsAssigned++;
        
        cout << "\n=== Patient Assignment ===" << endl;
        cout << "Patient ID: " << patient->patientId << endl;
        cout << "Patient Name: " << patient->name << endl;
        cout << "Age: " << patient->age << endl;
        cout << "Symptoms: " << patient->symptoms << endl;
        cout << "Assigned Doctor: " << doctors_tv[minIndex].name << endl;
        cout << "Specialization: " << doctors_tv[minIndex].specialization << endl;
        cout << "=========================" << endl;

        delete patient;
    }

    void displayQueue_tv() {
        if (front == nullptr) {
            cout << "No patients in queue!" << endl;
            return;
        }

        cout << "\n=== Patient Queue ===" << endl;
        cout << "Position\tPatient ID\tName\t\tAge\tSymptoms" << endl;
        cout << "------------------------------------------------------------" << endl;

        Patient_tv* temp = front;
        int position = 1;
        while (temp != nullptr) {
            cout << position << "\t\t" << temp->patientId << "\t\t" 
                 << temp->name << "\t\t" << temp->age << "\t" << temp->symptoms << endl;
            temp = temp->next;
            position++;
        }
        cout << "=====================" << endl;
    }

    void displayDoctors_tv() {
        cout << "\n=== Available Doctors ===" << endl;
        cout << "ID\tName\t\tSpecialization\t\tPatients Assigned" << endl;
        cout << "------------------------------------------------------------" << endl;
        
        for (int i = 0; i < doctorCount_tv; i++) {
            cout << doctors_tv[i].doctorId << "\t" << doctors_tv[i].name << "\t\t" 
                 << doctors_tv[i].specialization << "\t\t" << doctors_tv[i].patientsAssigned << endl;
        }
        cout << "=========================" << endl;
    }

    int getQueueSize_tv() {
        int count = 0;
        Patient_tv* temp = front;
        while (temp != nullptr) {
            count++;
            temp = temp->next;
        }
        return count;
    }

    bool isQueueEmpty_tv() {
        return front == nullptr;
    }

    void displayQueueStats_tv() {
        cout << "\n=== Queue Statistics ===" << endl;
        cout << "Patients in queue: " << getQueueSize_tv() << endl;
        cout << "Queue status: " << (isQueueEmpty_tv() ? "Empty" : "Has patients") << endl;
        
        if (!isQueueEmpty_tv()) {
            cout << "Next patient to be served: " << front->name << " (ID: " << front->patientId << ")" << endl;
        }
        cout << "========================" << endl;
    }

    void clearQueue_tv() {
        while (front != nullptr) {
            Patient_tv* temp = front;
            front = front->next;
            delete temp;
        }
        rear = nullptr;
        cout << "Patient queue cleared!" << endl;
    }

    ~PatientQueue_tv() {
        clearQueue_tv();
    }
};

int main() {
    PatientQueue_tv clinic_tv;
    int choice;
    char name[50], symptoms[100];
    int age;

    cout << "=== Medical Clinic Patient Queue Management System ===" << endl;
    cout << "Patients are assigned to doctors on first-come, first-served basis" << endl;

    while (true) {
        cout << "\n1. Register Patient\n2. Assign Patient to Doctor\n3. Display Patient Queue\n4. Display Doctors\n5. Queue Statistics\n6. Clear Queue\n7. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter patient name : ";
                cin >> name;
                cout << "Enter patient age: ";
                cin >> age;
                cout << "Enter symptoms: ";
                cin.ignore();  
                cin.getline(symptoms, 100);
                clinic_tv.registerPatient_tv(name, age, symptoms);
                break;

            case 2:
                clinic_tv.assignPatientToDoctor_tv();
                break;

            case 3:
                clinic_tv.displayQueue_tv();
                break;

            case 4:
                clinic_tv.displayDoctors_tv();
                break;

            case 5:
                clinic_tv.displayQueueStats_tv();
                break;

            case 6:
                clinic_tv.clearQueue_tv();
                break;

            case 7:
                cout << "Thank you for using the Patient Queue Management System!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


##  Output

=== Medical Clinic Patient Queue Management System ===
Patients are assigned to doctors on first-come, first-served basis

1. Register Patient
2. Assign Patient to Doctor
3. Display Patient Queue
4. Display Doctors
5. Queue Statistics
6. Clear Queue
7. Exit
Enter your choice: 1
Enter patient name : Arjun
Enter patient age: 25
Enter symptoms: Fever and headache
Patient Arjun (ID: 1000) registered successfully!
Position in queue: 1

1. Register Patient
2. Assign Patient to Doctor
3. Display Patient Queue
4. Display Doctors
5. Queue Statistics
6. Clear Queue
7. Exit
Enter your choice: 1
Enter patient name : Priya
Enter patient age: 30
Enter symptoms: Stomach ache
Patient Priya (ID: 1001) registered successfully!
Position in queue: 2

1. Register Patient
2. Assign Patient to Doctor
3. Display Patient Queue
4. Display Doctors
5. Queue Statistics
6. Clear Queue
7. Exit
Enter your choice: 1
Enter patient name ): Rohit
Enter patient age: 35
Enter symptoms: Back pain
Patient Rohit (ID: 1002) registered successfully!
Position in queue: 3

1. Register Patient
2. Assign Patient to Doctor
3. Display Patient Queue
4. Display Doctors
5. Queue Statistics
6. Clear Queue
7. Exit
Enter your choice: 3

=== Patient Queue ===
Position	Patient ID	Name		Age	Symptoms
------------------------------------------------------------
1		1000		Arjun		25	Fever and headache
2		1001		Priya		30	Stomach ache
3		1002		Rohit		35	Back pain
=====================

1. Register Patient
2. Assign Patient to Doctor
3. Display Patient Queue
4. Display Doctors
5. Queue Statistics
6. Clear Queue
7. Exit
Enter your choice: 4

=== Available Doctors ===
ID	Name		Specialization		Patients Assigned
------------------------------------------------------------
1	Dr. Sharma	General Physician	0
2	Dr. Patel	Cardiologist		0
3	Dr. Gupta	Dermatologist		0
4	Dr. Kumar	Pediatrician		0
5	Dr. Singh	Orthopedic		    0
=========================

1. Register Patient
2. Assign Patient to Doctor
3. Display Patient Queue
4. Display Doctors
5. Queue Statistics
6. Clear Queue
7. Exit
Enter your choice: 2

=== Patient Assignment ===
Patient ID: 1000
Patient Name: Arjun
Age: 25
Symptoms: Fever and headache
Assigned Doctor: Dr. Sharma
Specialization: General Physician
=========================

1. Register Patient
2. Assign Patient to Doctor
3. Display Patient Queue
4. Display Doctors
5. Queue Statistics
6. Clear Queue
7. Exit
Enter your choice: 5

=== Queue Statistics ===
Patients in queue: 2
Queue status: Has patients
Next patient to be served: Priya (ID: 1001)
========================

1. Register Patient
2. Assign Patient to Doctor
3. Display Patient Queue
4. Display Doctors
5. Queue Statistics
6. Clear Queue
7. Exit
Enter your choice: 2

=== Patient Assignment ===
Patient ID: 1001
Patient Name: Priya
Age: 30
Symptoms: Stomach ache
Assigned Doctor: Dr. Patel
Specialization: Cardiologist
=========================

1. Register Patient
2. Assign Patient to Doctor
3. Display Patient Queue
4. Display Doctors
5. Queue Statistics
6. Clear Queue
7. Exit
Enter your choice: 4

=== Available Doctors ===
ID	Name		Specialization		Patients Assigned
------------------------------------------------------------
1	Dr. Sharma	General Physician	1
2	Dr. Patel	Cardiologist		1
3	Dr. Gupta	Dermatologist		0
4	Dr. Kumar	Pediatrician		0
5	Dr. Singh	Orthopedic		    0
=========================


