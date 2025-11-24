## Problem Statement
You are given a postfix expression (also known as Reverse Polish Notation) consisting of single-digit operands and binary operators (+, -, *, /). Your task is to evaluate the expression using stack and return its result.

## Code
```cpp
#include <iostream>
#include <cstring>
#include <cmath>
using namespace std;

struct StackNode_tv {
    int data;
    StackNode_tv* next;
};

class IntStack_tv {
private:
    StackNode_tv* top;

public:
    IntStack_tv() {
        top = nullptr;
    }

    void push_tv(int value) {
        StackNode_tv* newNode = new StackNode_tv;
        newNode->data = value;
        newNode->next = top;
        top = newNode;
    }

    int pop_tv() {
        if (isEmpty_tv()) {
            cout << "Error: Stack underflow!" << endl;
            return 0;
        }

        StackNode_tv* temp = top;
        int poppedValue = temp->data;
        top = top->next;
        delete temp;
        return poppedValue;
    }

    int peek_tv() {
        if (isEmpty_tv()) {
            cout << "Error: Stack is empty!" << endl;
            return 0;
        }
        return top->data;
    }

    bool isEmpty_tv() {
        return top == nullptr;
    }

    int size_tv() {
        int count = 0;
        StackNode_tv* temp = top;
        while (temp != nullptr) {
            count++;
            temp = temp->next;
        }
        return count;
    }

    void displayStack_tv() {
        if (isEmpty_tv()) {
            cout << "Stack is empty";
            return;
        }

        cout << "Stack (Top to Bottom): ";
        StackNode_tv* temp = top;
        while (temp != nullptr) {
            cout << temp->data << " ";
            temp = temp->next;
        }
        cout << endl;
    }

    ~IntStack_tv() {
        while (top != nullptr) {
            StackNode_tv* temp = top;
            top = top->next;
            delete temp;
        }
    }
};

class PostfixEvaluator_tv {
private:

    bool isOperand_tv(char ch) {
        return (ch >= '0' && ch <= '9');
    }

   
    bool isOperator_tv(char ch) {
        return (ch == '+' || ch == '-' || ch == '*' || ch == '/' || ch == '^');
    }

   
    int performOperation_tv(int operand2, int operand1, char op) {
        switch (op) {
            case '+':
                return operand1 + operand2;
            case '-':
                return operand1 - operand2;
            case '*':
                return operand1 * operand2;
            case '/':
                if (operand2 == 0) {
                    cout << "Error: Division by zero!" << endl;
                    return 0;
                }
                return operand1 / operand2;
            case '^':
                return pow(operand1, operand2);
            default:
                cout << "Error: Invalid operator!" << endl;
                return 0;
        }
    }

public:
    
    int evaluatePostfix_tv(const char* expression) {
        IntStack_tv stack_tv;
        int len = strlen(expression);

        cout << "\nEvaluating postfix expression: " << expression << endl;
        cout << "----------------------------------------" << endl;

        for (int i = 0; i < len; i++) {
            char ch = expression[i];

            cout << "Step " << (i+1) << ": Processing '" << ch << "'" << endl;

            
            if (isOperand_tv(ch)) {
                int value = ch - '0';  
                stack_tv.push_tv(value);
                cout << "  Operand found. Pushed " << value << " to stack." << endl;
            }
            
            else if (isOperator_tv(ch)) {
                
                if (stack_tv.size_tv() < 2) {
                    cout << "  Error: Not enough operands for operator '" << ch << "'!" << endl;
                    return -1;
                }

                int operand2 = stack_tv.pop_tv();
                int operand1 = stack_tv.pop_tv();

                cout << "  Operator found. Popped operands: " << operand1 << " and " << operand2 << endl;

                
                int result = performOperation_tv(operand2, operand1, ch);
                stack_tv.push_tv(result);

                cout << "  Performed " << operand1 << " " << ch << " " << operand2 << " = " << result << endl;
                cout << "  Pushed result " << result << " to stack." << endl;
            } else {
                cout << "  Ignoring non-operand, non-operator character: '" << ch << "'" << endl;
            }

            stack_tv.displayStack_tv();
            cout << endl;
        }

        
        if (stack_tv.size_tv() == 1) {
            int finalResult = stack_tv.pop_tv();
            cout << "Evaluation complete. Final result: " << finalResult << endl;
            return finalResult;
        } else {
            cout << "Error: Invalid postfix expression! Stack should contain exactly one value." << endl;
            cout << "Stack size: " << stack_tv.size_tv() << endl;
            stack_tv.displayStack_tv();
            return -1;
        }
    }


    bool isValidPostfix_tv(const char* expression) {
        IntStack_tv validationStack_tv;
        int len = strlen(expression);
        int operandCount = 0, operatorCount = 0;

        for (int i = 0; i < len; i++) {
            char ch = expression[i];

            if (isOperand_tv(ch)) {
                validationStack_tv.push_tv(1);  
                operandCount++;
            } else if (isOperator_tv(ch)) {
                operatorCount++;
   
                if (validationStack_tv.size_tv() < 2) {
                    return false;
                }
                validationStack_tv.pop_tv();  
                validationStack_tv.pop_tv();
                validationStack_tv.push_tv(1);  
            }
        }

        
        return (validationStack_tv.size_tv() == 1) && (operatorCount == operandCount - 1);
    }
};

int main() {
    PostfixEvaluator_tv evaluator_tv;
    char expression[100];
    int choice;

    cout << "=== Postfix Expression Evaluator ===" << endl;
    cout << "Evaluates expressions in Reverse Polish Notation (RPN)." << endl;
    cout << "Supported operators: +, -, *, /, ^" << endl;
    cout << "Operands: Single digits (0-9)" << endl;

    while (true) {
        cout << "\n1. Evaluate Example Expressions\n2. Evaluate Custom Expression\n3. Validate Expression\n4. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1: {
                cout << "\n=== Evaluating Example Expressions ===" << endl;
                
                
                cout << "\nTest 1: 23+";
                int result1 = evaluator_tv.evaluatePostfix_tv("23+");
                if (result1 != -1) {
                    cout << "Result: " << result1 << endl;
                }
                
                
                cout << "\nTest 2: 234*+";
                int result2 = evaluator_tv.evaluatePostfix_tv("234*+");
                if (result2 != -1) {
                    cout << "Result: " << result2 << endl;
                }
                
                
                cout << "\nTest 3: 123*+4-";
                int result3 = evaluator_tv.evaluatePostfix_tv("123*+4-");
                if (result3 != -1) {
                    cout << "Result: " << result3 << endl;
                }
                
                
                cout << "\nTest 4: 23^";
                int result4 = evaluator_tv.evaluatePostfix_tv("23^");
                if (result4 != -1) {
                    cout << "Result: " << result4 << endl;
                }
                
                
                cout << "\nTest 5: 152*+8/";
                int result5 = evaluator_tv.evaluatePostfix_tv("152*+8/");
                if (result5 != -1) {
                    cout << "Result: " << result5 << endl;
                }
                
                break;
            }

            case 2:
                cout << "Enter postfix expression: ";
                cin >> expression;
                {
                    int result = evaluator_tv.evaluatePostfix_tv(expression);
                    if (result != -1) {
                        cout << "\nFinal Result: " << result << endl;
                    }
                }
                break;

            case 3:
                cout << "Enter postfix expression to validate: ";
                cin >> expression;
                if (evaluator_tv.isValidPostfix_tv(expression)) {
                    cout << "Expression is a valid postfix expression." << endl;
                } else {
                    cout << "Expression is NOT a valid postfix expression." << endl;
                }
                break;

            case 4:
                cout << "Thank you for using the Postfix Expression Evaluator!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


##  Output

=== Postfix Expression Evaluator ===
Evaluates expressions in Reverse Polish Notation (RPN).
Supported operators: +, -, *, /, ^
Operands: Single digits (0-9)

1. Evaluate Example Expressions
2. Evaluate Custom Expression
3. Validate Expression
4. Exit
Enter your choice: 1

=== Evaluating Example Expressions ===

Test 1: 23+

Evaluating postfix expression: 23+
----------------------------------------
Step 1: Processing '2'
  Operand found. Pushed 2 to stack.
Stack (Top to Bottom): 2 

Step 2: Processing '3'
  Operand found. Pushed 3 to stack.
Stack (Top to Bottom): 3 2 

Step 3: Processing '+'
  Operator found. Popped operands: 2 and 3
  Performed 2 + 3 = 5
  Pushed result 5 to stack.
Stack (Top to Bottom): 5 

Evaluation complete. Final result: 5
Result: 5

Test 2: 234*+

Evaluating postfix expression: 234*+
----------------------------------------
Step 1: Processing '2'
  Operand found. Pushed 2 to stack.
Stack (Top to Bottom): 2 

Step 2: Processing '3'
  Operand found. Pushed 3 to stack.
Stack (Top to Bottom): 3 2 

Step 3: Processing '4'
  Operand found. Pushed 4 to stack.
Stack (Top to Bottom): 4 3 2 

Step 4: Processing '*'
  Operator found. Popped operands: 3 and 4
  Performed 3 * 4 = 12
  Pushed result 12 to stack.
Stack (Top to Bottom): 12 2 

Step 5: Processing '+'
  Operator found. Popped operands: 2 and 12
  Performed 2 + 12 = 14
  Pushed result 14 to stack.
Stack (Top to Bottom): 14 

Evaluation complete. Final result: 14
Result: 14

Test 3: 123*+4-

Evaluating postfix expression: 123*+4-
----------------------------------------
Step 1: Processing '1'
  Operand found. Pushed 1 to stack.
Stack (Top to Bottom): 1 

Step 2: Processing '2'
  Operand found. Pushed 2 to stack.
Stack (Top to Bottom): 2 1 

Step 3: Processing '3'
  Operand found. Pushed 3 to stack.
Stack (Top to Bottom): 3 2 1 

Step 4: Processing '*'
  Operator found. Popped operands: 2 and 3
  Performed 2 * 3 = 6
  Pushed result 6 to stack.
Stack (Top to Bottom): 6 1 

Step 5: Processing '+'
  Operator found. Popped operands: 1 and 6
  Performed 1 + 6 = 7
  Pushed result 7 to stack.
Stack (Top to Bottom): 7 

Step 6: Processing '4'
  Operand found. Pushed 4 to stack.
Stack (Top to Bottom): 4 7 

Step 7: Processing '-'
  Operator found. Popped operands: 7 and 4
  Performed 7 - 4 = 3
  Pushed result 3 to stack.
Stack (Top to Bottom): 3 

Evaluation complete. Final result: 3
Result: 3

Test 4: 23^
Result: 8

Test 5: 152*+8/
Result: 1

1. Evaluate Example Expressions
2. Evaluate Custom Expression
3. Validate Expression
4. Exit
Enter your choice: 2
Enter postfix expression: 234*+

Evaluating postfix expression: 234*+
----------------------------------------
Step 1: Processing '2'
  Operand found. Pushed 2 to stack.
Stack (Top to Bottom): 2 

Step 2: Processing '3'
  Operand found. Pushed 3 to stack.
Stack (Top to Bottom): 3 2 

Step 3: Processing '4'
  Operand found. Pushed 4 to stack.
Stack (Top to Bottom): 4 3 2 

Step 4: Processing '*'
  Operator found. Popped operands: 3 and 4
  Performed 3 * 4 = 12
  Pushed result 12 to stack.
Stack (Top to Bottom): 12 2 

Step 5: Processing '+'
  Operator found. Popped operands: 2 and 12
  Performed 2 + 12 = 14
  Pushed result 14 to stack.
Stack (Top to Bottom): 14 

Evaluation complete. Final result: 14

Final Result: 14


