## Problem Statement
WAP to perform addition of two polynomials using singly linked list.

##  Code
```cpp
#include <iostream>
using namespace std;

struct Term_tv {
    int coefficient;
    int exponent;
    Term_tv* next;
};

class Polynomial_tv {
private:
    Term_tv* head;

public:
    Polynomial_tv() {
        head = nullptr;
    }

    void insertTerm_tv(int coeff, int exp) {
        
        Term_tv* newTerm = new Term_tv;
        newTerm->coefficient = coeff;
        newTerm->exponent = exp;
        newTerm->next = nullptr;

        
        if (head == nullptr || head->exponent < exp) {
            newTerm->next = head;
            head = newTerm;
            return;
        }

        
        if (head->exponent == exp) {
            head->coefficient += coeff;
            
            if (head->coefficient == 0) {
                Term_tv* temp = head;
                head = head->next;
                delete temp;
            }
            delete newTerm;
            return;
        }

        
        Term_tv* temp = head;
        while (temp->next != nullptr && temp->next->exponent > exp) {
            temp = temp->next;
        }

        
        if (temp->next != nullptr && temp->next->exponent == exp) {
            temp->next->coefficient += coeff;
            
            if (temp->next->coefficient == 0) {
                Term_tv* toDelete = temp->next;
                temp->next = temp->next->next;
                delete toDelete;
            }
            delete newTerm;
            return;
        }

       
        newTerm->next = temp->next;
        temp->next = newTerm;
    }

    void inputPolynomial_tv(const char* polyName) {
        int numTerms;
        cout << "Enter number of terms in " << polyName << ": ";
        cin >> numTerms;

        for (int i = 0; i < numTerms; i++) {
            int coeff, exp;
            cout << "Enter coefficient and exponent for term " << (i+1) << ": ";
            cin >> coeff >> exp;
            insertTerm_tv(coeff, exp);
        }
    }

    void displayPolynomial_tv(const char* polyName) {
        cout << polyName << " = ";
        if (head == nullptr) {
            cout << "0" << endl;
            return;
        }

        Term_tv* temp = head;
        bool first = true;
        while (temp != nullptr) {
            if (!first && temp->coefficient > 0) {
                cout << " + ";
            } else if (temp->coefficient < 0) {
                if (!first) cout << " ";
                cout << "- ";
                if (first) first = false;
            }

            int absCoeff = abs(temp->coefficient);
            if (temp->exponent == 0) {
                cout << absCoeff;
            } else if (temp->exponent == 1) {
                if (absCoeff == 1) {
                    cout << "x";
                } else {
                    cout << absCoeff << "x";
                }
            } else {
                if (absCoeff == 1) {
                    cout << "x^" << temp->exponent;
                } else {
                    cout << absCoeff << "x^" << temp->exponent;
                }
            }

            if (temp->coefficient >= 0 && !first) {
                first = false;
            } else if (temp->coefficient < 0) {
                first = false;
            }
            temp = temp->next;
        }
        cout << endl;
    }

    static Polynomial_tv* addPolynomials_tv(Polynomial_tv& poly1, Polynomial_tv& poly2) {
        Polynomial_tv* result = new Polynomial_tv();

        Term_tv* ptr1 = poly1.head;
        Term_tv* ptr2 = poly2.head;

      
        while (ptr1 != nullptr && ptr2 != nullptr) {
            if (ptr1->exponent > ptr2->exponent) {
                result->insertTerm_tv(ptr1->coefficient, ptr1->exponent);
                ptr1 = ptr1->next;
            } else if (ptr1->exponent < ptr2->exponent) {
                result->insertTerm_tv(ptr2->coefficient, ptr2->exponent);
                ptr2 = ptr2->next;
            } else {
                
                int sumCoeff = ptr1->coefficient + ptr2->coefficient;
                if (sumCoeff != 0) {
                    result->insertTerm_tv(sumCoeff, ptr1->exponent);
                }
                ptr1 = ptr1->next;
                ptr2 = ptr2->next;
            }
        }

      
        while (ptr1 != nullptr) {
            result->insertTerm_tv(ptr1->coefficient, ptr1->exponent);
            ptr1 = ptr1->next;
        }

        
        while (ptr2 != nullptr) {
            result->insertTerm_tv(ptr2->coefficient, ptr2->exponent);
            ptr2 = ptr2->next;
        }

        return result;
    }

    int evaluate_tv(int x) {
        int result = 0;
        Term_tv* temp = head;
        while (temp != nullptr) {
            int termValue = temp->coefficient;
            for (int i = 0; i < temp->exponent; i++) {
                termValue *= x;
            }
            result += termValue;
            temp = temp->next;
        }
        return result;
    }

    void clear_tv() {
        while (head != nullptr) {
            Term_tv* temp = head;
            head = head->next;
            delete temp;
        }
    }

    ~Polynomial_tv() {
        clear_tv();
    }
};

int main() {
    Polynomial_tv poly1_tv, poly2_tv;
    int choice, x;

    cout << "=== Polynomial Addition using Singly Linked List ===" << endl;

    while (true) {
        cout << "\n1. Input First Polynomial\n2. Input Second Polynomial\n3. Display Polynomials\n4. Add Polynomials\n5. Evaluate Polynomial\n6. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                poly1_tv.clear_tv();
                poly1_tv.inputPolynomial_tv("First Polynomial");
                cout << "First polynomial stored successfully!" << endl;
                break;

            case 2:
                poly2_tv.clear_tv();
                poly2_tv.inputPolynomial_tv("Second Polynomial");
                cout << "Second polynomial stored successfully!" << endl;
                break;

            case 3:
                poly1_tv.displayPolynomial_tv("First Polynomial");
                poly2_tv.displayPolynomial_tv("Second Polynomial");
                break;

            case 4: {
                cout << "\n--- Polynomial Addition ---" << endl;
                poly1_tv.displayPolynomial_tv("First Polynomial");
                poly2_tv.displayPolynomial_tv("Second Polynomial");
                
                Polynomial_tv* sum = Polynomial_tv::addPolynomials_tv(poly1_tv, poly2_tv);
                sum->displayPolynomial_tv("Sum");
                delete sum;
                break;
            }

            case 5:
                cout << "Enter value of x for evaluation: ";
                cin >> x;
                cout << "\nFirst Polynomial at x=" << x << " : " << poly1_tv.evaluate_tv(x) << endl;
                cout << "Second Polynomial at x=" << x << " : " << poly2_tv.evaluate_tv(x) << endl;
                break;

            case 6:
                cout << "Thank you for using the Polynomial Operations System!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


##  Output

=== Polynomial Addition using Singly Linked List ===

1. Input First Polynomial
2. Input Second Polynomial
3. Display Polynomials
4. Add Polynomials
5. Evaluate Polynomial
6. Exit
Enter your choice: 1
Enter number of terms in First Polynomial: 3
Enter coefficient and exponent for term 1: 3 2
Enter coefficient and exponent for term 2: 2 1
Enter coefficient and exponent for term 3: 1 0
First polynomial stored successfully!

1. Input First Polynomial
2. Input Second Polynomial
3. Display Polynomials
4. Add Polynomials
5. Evaluate Polynomial
6. Exit
Enter your choice: 2
Enter number of terms in Second Polynomial: 2
Enter coefficient and exponent for term 1: 2 2
Enter coefficient and exponent for term 2: -1 0
Second polynomial stored successfully!

1. Input First Polynomial
2. Input Second Polynomial
3. Display Polynomials
4. Add Polynomials
5. Evaluate Polynomial
6. Exit
Enter your choice: 3
First Polynomial = 3x^2 + 2x + 1
Second Polynomial = 2x^2 - 1

1. Input First Polynomial
2. Input Second Polynomial
3. Display Polynomials
4. Add Polynomials
5. Evaluate Polynomial
6. Exit
Enter your choice: 4

--- Polynomial Addition ---
First Polynomial = 3x^2 + 2x + 1
Second Polynomial = 2x^2 - 1
Sum = 5x^2 + 2x

1. Input First Polynomial
2. Input Second Polynomial
3. Display Polynomials
4. Add Polynomials
5. Evaluate Polynomial
6. Exit
Enter your choice: 5
Enter value of x for evaluation: 2

First Polynomial at x=2 : 17
Second Polynomial at x=2 : 7

