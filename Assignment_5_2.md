## Problem Statement
Convert given infix expression Eg. a-b*c-d/e+f into postfix form using stack and show the operations step by step.

## Code
```cpp
#include <iostream>
#include <cstring>
using namespace std;

struct StackNode_tv {
    char data;
    StackNode_tv* next;
};

class CharStack_tv {
private:
    StackNode_tv* top;

public:
    CharStack_tv() {
        top = nullptr;
    }

    void push_tv(char ch) {
        StackNode_tv* newNode = new StackNode_tv;
        newNode->data = ch;
        newNode->next = top;
        top = newNode;
    }

    char pop_tv() {
        if (isEmpty_tv()) {
            return '\0';  
        }

        StackNode_tv* temp = top;
        char poppedChar = temp->data;
        top = top->next;
        delete temp;
        return poppedChar;
    }

    char peek_tv() {
        if (isEmpty_tv()) {
            return '\0';  
        }
        return top->data;
    }

    bool isEmpty_tv() {
        return top == nullptr;
    }

    void displayStack_tv() {
        if (isEmpty_tv()) {
            cout << "Stack is empty";
            return;
        }

        StackNode_tv* temp = top;
        while (temp != nullptr) {
            cout << temp->data << " ";
            temp = temp->next;
        }
    }

    ~CharStack_tv() {
        while (top != nullptr) {
            StackNode_tv* temp = top;
            top = top->next;
            delete temp;
        }
    }
};

class InfixToPostfix_tv {
private:

    bool isOperand_tv(char ch) {
        return (ch >= 'a' && ch <= 'z') || (ch >= 'A' && ch <= 'Z') || (ch >= '0' && ch <= '9');
    }

    int getPrecedence_tv(char op) {
        switch (op) {
            case '+':
            case '-':
                return 1;
            case '*':
            case '/':
            case '%':
                return 2;
            case '^':
                return 3;
            default:
                return 0;
        }
    }

    bool isOperator_tv(char ch) {
        return (ch == '+' || ch == '-' || ch == '*' || ch == '/' || ch == '%' || ch == '^');
    }

public:

    void convertToPostfix_tv(const char* infix) {
        CharStack_tv stack_tv;
        int infixLen = strlen(infix);
        char postfix[100];
        int postfixIndex = 0;

        cout << "\nStep-by-Step Conversion Process:" << endl;
        cout << "Infix: " << infix << endl;
        cout << "------------------------------" << endl;

        for (int i = 0; i < infixLen; i++) {
            char ch = infix[i];

            cout << "Step " << (i+1) << ": Processing '" << ch << "'" << endl;

            if (isOperand_tv(ch)) {
                postfix[postfixIndex++] = ch;
                postfix[postfixIndex] = '\0';
                cout << "  Operand found. Added to postfix: " << postfix << endl;
            }
          
            else if (ch == '(') {
                stack_tv.push_tv(ch);
                cout << "  '(' found. Pushed to stack." << endl;
            }
            
            else if (ch == ')') {
                cout << "  ')' found. Popping until '(':" << endl;
                while (!stack_tv.isEmpty_tv() && stack_tv.peek_tv() != '(') {
                    char popped = stack_tv.pop_tv();
                    postfix[postfixIndex++] = popped;
                    postfix[postfixIndex] = '\0';
                    cout << "    Popped '" << popped << "', Postfix: " << postfix << endl;
                }
                if (!stack_tv.isEmpty_tv()) {
                    stack_tv.pop_tv();  
                    cout << "    Removed '(' from stack" << endl;
                }
            }
           
            else if (isOperator_tv(ch)) {
                cout << "  Operator '" << ch << "' found:" << endl;
                
                while (!stack_tv.isEmpty_tv() && 
                       stack_tv.peek_tv() != '(' && 
                       getPrecedence_tv(stack_tv.peek_tv()) >= getPrecedence_tv(ch)) {
                    char popped = stack_tv.pop_tv();
                    postfix[postfixIndex++] = popped;
                    postfix[postfixIndex] = '\0';
                    cout << "    Popped '" << popped << "' (higher/equal precedence), Postfix: " << postfix << endl;
                }
                stack_tv.push_tv(ch);
                cout << "    Pushed '" << ch << "' to stack" << endl;
            }

            cout << "  Current Stack: ";
            stack_tv.displayStack_tv();
            cout << endl << "  Current Postfix: " << postfix << endl << endl;
        }

        
        cout << "Final Step: Popping remaining operators from stack:" << endl;
        while (!stack_tv.isEmpty_tv()) {
            char popped = stack_tv.pop_tv();
            if (popped != '(') {  
                postfix[postfixIndex++] = popped;
                postfix[postfixIndex] = '\0';
                cout << "  Popped '" << popped << "', Postfix: " << postfix << endl;
            }
        }

        postfix[postfixIndex] = '\0';
        cout << "\nFinal Postfix Expression: " << postfix << endl;
    }
};

int main() {
    InfixToPostfix_tv converter_tv;
    char infixExpression[100];
    int choice;

    cout << "=== Infix to Postfix Conversion using Stack ===" << endl;
    cout << "Operators supported: +, -, *, /, %, ^" << endl;
    cout << "Operands: Single characters (a-z, A-Z) or digits (0-9)" << endl;

    while (true) {
        cout << "\n1. Convert Example Expression (a-b*c-d/e+f)\n2. Convert Custom Expression\n3. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "\nConverting example expression: a-b*c-d/e+f" << endl;
                converter_tv.convertToPostfix_tv("a-b*c-d/e+f");
                break;

            case 2:
                cout << "Enter infix expression: ";
                cin >> infixExpression;
                converter_tv.convertToPostfix_tv(infixExpression);
                break;

            case 3:
                cout << "Thank you for using the Infix to Postfix Converter!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


##  Output

=== Infix to Postfix Conversion using Stack ===
Operators supported: +, -, *, /, %, ^
Operands: Single characters (a-z, A-Z) or digits (0-9)

1. Convert Example Expression (a-b*c-d/e+f)
2. Convert Custom Expression
3. Exit
Enter your choice: 1

Converting example expression: a-b*c-d/e+f

Step-by-Step Conversion Process:
Infix: a-b*c-d/e+f
------------------------------
Step 1: Processing 'a'
  Operand found. Added to postfix: a
  Current Stack: 
  Current Postfix: a

Step 2: Processing '-'
  Operator '-' found:
    Pushed '-' to stack
  Current Stack: - 
  Current Postfix: a

Step 3: Processing 'b'
  Operand found. Added to postfix: ab
  Current Stack: - 
  Current Postfix: ab

Step 4: Processing '*'
  Operator '*' found:
    Pushed '*' to stack
  Current Stack: * - 
  Current Postfix: ab

Step 5: Processing 'c'
  Operand found. Added to postfix: abc
  Current Stack: * - 
  Current Postfix: abc

Step 6: Processing '-'
  Operator '-' found:
    Popped '*' (higher/equal precedence), Postfix: abc*
    Pushed '-' to stack
  Current Stack: - - 
  Current Postfix: abc*

Step 7: Processing 'd'
  Operand found. Added to postfix: abc*d
  Current Stack: - - 
  Current Postfix: abc*d

Step 8: Processing '/'
  Operator '/' found:
    Pushed '/' to stack
  Current Stack: / - - 
  Current Postfix: abc*d

Step 9: Processing 'e'
  Operand found. Added to postfix: abc*de
  Current Stack: / - - 
  Current Postfix: abc*de

Step 10: Processing '+'
  Operator '+' found:
    Popped '/' (higher/equal precedence), Postfix: abc*de/
    Popped '-' (higher/equal precedence), Postfix: abc*de/-
    Pushed '+' to stack
  Current Stack: + - 
  Current Postfix: abc*de/-

Step 11: Processing 'f'
  Operand found. Added to postfix: abc*de/-f
  Current Stack: + - 
  Current Postfix: abc*de/-f

Final Step: Popping remaining operators from stack:
  Popped '+' (higher/equal precedence), Postfix: abc*de/-f+
  Popped '-' (higher/equal precedence), Postfix: abc*de/-f+-

Final Postfix Expression: abc*de/-f+-

1. Convert Example Expression (a-b*c-d/e+f)
2. Convert Custom Expression
3. Exit
Enter your choice: 2
Enter infix expression: a+b*c
```