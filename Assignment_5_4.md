## Problem Statement
You are given a string containing only parentheses characters: '(', ')', '{', '}', '[', and ']'. Your task is to check whether the parentheses are balanced or not. A string is considered balanced if: Every opening bracket has a corresponding closing bracket of the same type. Brackets are closed in the correct order.

##  Code
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

        cout << "Stack (Top to Bottom): ";
        StackNode_tv* temp = top;
        while (temp != nullptr) {
            cout << temp->data << " ";
            temp = temp->next;
        }
        cout << endl;
    }

    ~CharStack_tv() {
        while (top != nullptr) {
            StackNode_tv* temp = top;
            top = top->next;
            delete temp;
        }
    }
};

class ParenthesesChecker_tv {
private:
   
    bool isOpeningBracket_tv(char ch) {
        return (ch == '(' || ch == '{' || ch == '[');
    }

    
    bool isClosingBracket_tv(char ch) {
        return (ch == ')' || ch == '}' || ch == ']');
    }

    bool isMatchingPair_tv(char opening, char closing) {
        return ((opening == '(' && closing == ')') ||
                (opening == '{' && closing == '}') ||
                (opening == '[' && closing == ']'));
    }

public:
    
    bool isBalanced_tv(const char* expression) {
        CharStack_tv stack_tv;
        int len = strlen(expression);

        cout << "\nChecking expression: " << expression << endl;
        cout << "-----------------------------" << endl;

        for (int i = 0; i < len; i++) {
            char ch = expression[i];
            
          
            if (!isOpeningBracket_tv(ch) && !isClosingBracket_tv(ch)) {
                continue;
            }

            cout << "Step " << (i+1) << ": Processing '" << ch << "'" << endl;

            
            if (isOpeningBracket_tv(ch)) {
                stack_tv.push_tv(ch);
                cout << "  Opening bracket found. Pushed to stack." << endl;
            }
           
            else if (isClosingBracket_tv(ch)) {
                
                if (stack_tv.isEmpty_tv()) {
                    cout << "  Closing bracket found but stack is empty! Unbalanced." << endl;
                    return false;
                }

 
                char popped = stack_tv.pop_tv();
                cout << "  Closing bracket found. Popped '" << popped << "' from stack." << endl;

                if (!isMatchingPair_tv(popped, ch)) {
                    cout << "  Mismatched brackets: '" << popped << "' and '" << ch << "'! Unbalanced." << endl;
                    return false;
                } else {
                    cout << "  Matching pair found: '" << popped << "' and '" << ch << "'" << endl;
                }
            }

            stack_tv.displayStack_tv();
            cout << endl;
        }

        
        if (stack_tv.isEmpty_tv()) {
            cout << "All brackets processed and stack is empty. Expression is balanced!" << endl;
            return true;
        } else {
            cout << "Stack is not empty after processing. Unmatched opening brackets! Unbalanced." << endl;
            stack_tv.displayStack_tv();
            return false;
        }
    }


    void analyzeExpression_tv(const char* expression) {
        int roundCount = 0, curlyCount = 0, squareCount = 0;
        int len = strlen(expression);

        for (int i = 0; i < len; i++) {
            switch (expression[i]) {
                case '(': roundCount++; break;
                case ')': roundCount--; break;
                case '{': curlyCount++; break;
                case '}': curlyCount--; break;
                case '[': squareCount++; break;
                case ']': squareCount--; break;
            }
        }

        cout << "\nExpression Analysis:" << endl;
        cout << "  Round brackets (): " << roundCount << endl;
        cout << "  Curly brackets {}: " << curlyCount << endl;
        cout << "  Square brackets []: " << squareCount << endl;

        if (roundCount == 0 && curlyCount == 0 && squareCount == 0) {
            cout << "  Total count suggests balanced brackets." << endl;
        } else {
            cout << "  Total count suggests unbalanced brackets." << endl;
        }
    }
};

int main() {
    ParenthesesChecker_tv checker_tv;
    char expression[100];
    int choice;

    cout << "=== Balanced Parentheses Checker ===" << endl;
    cout << "Checks if parentheses (), curly braces {}, and square brackets [] are balanced." << endl;

    while (true) {
        cout << "\n1. Check Example Expressions\n2. Check Custom Expression\n3. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1: {
                cout << "\n=== Testing Example Expressions ===" << endl;
                
           
                cout << "\nTest 1: {[()]}";
                bool result1 = checker_tv.isBalanced_tv("{[()]}");
                cout << "Result: " << (result1 ? "Balanced" : "Unbalanced") << endl;
                
                
                cout << "\nTest 2: {(})";
                bool result2 = checker_tv.isBalanced_tv("{(})");
                cout << "Result: " << (result2 ? "Balanced" : "Unbalanced") << endl;
                
                
                cout << "\nTest 3: {()]}";
                bool result3 = checker_tv.isBalanced_tv("{()]}");
                cout << "Result: " << (result3 ? "Balanced" : "Unbalanced") << endl;
                
                
                cout << "\nTest 4: {()}[";
                bool result4 = checker_tv.isBalanced_tv("{()}[");
                cout << "Result: " << (result4 ? "Balanced" : "Unbalanced") << endl;
                
                
                cout << "\nTest 5: [{(())}]";
                bool result5 = checker_tv.isBalanced_tv("[{(())}]");
                cout << "Result: " << (result5 ? "Balanced" : "Unbalanced") << endl;
                
                break;
            }

            case 2:
                cout << "Enter expression to check: ";
                cin >> expression;
                checker_tv.analyzeExpression_tv(expression);
                bool result = checker_tv.isBalanced_tv(expression);
                cout << "\nFinal Result: " << (result ? "Balanced" : "Unbalanced") << endl;
                break;

            case 3:
                cout << "Thank you for using the Balanced Parentheses Checker!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


## Output

=== Balanced Parentheses Checker ===
Checks if parentheses (), curly braces {}, and square brackets [] are balanced.

1. Check Example Expressions
2. Check Custom Expression
3. Exit
Enter your choice: 1

=== Testing Example Expressions ===

Test 1: {[()]}

Checking expression: {[()]}
-----------------------------
Step 1: Processing '{'
  Opening bracket found. Pushed to stack.
Stack (Top to Bottom): { 

Step 2: Processing '['
  Opening bracket found. Pushed to stack.
Stack (Top to Bottom): [ { 

Step 3: Processing '('
  Opening bracket found. Pushed to stack.
Stack (Top to Bottom): ( [ { 

Step 4: Processing ')'
  Closing bracket found. Popped '(' from stack.
  Matching pair found: '(' and ')'
Stack (Top to Bottom): [ { 

Step 5: Processing ']'
  Closing bracket found. Popped '[' from stack.
  Matching pair found: '[' and ']'
Stack (Top to Bottom): { 

Step 6: Processing '}'
  Closing bracket found. Popped '{' from stack.
  Matching pair found: '{' and '}'
Stack (Top to Bottom): 

All brackets processed and stack is empty. Expression is balanced!
Result: Balanced

Test 2: {(})

Checking expression: {(})
-----------------------------
Step 1: Processing '{'
  Opening bracket found. Pushed to stack.
Stack (Top to Bottom): { 

Step 2: Processing '('
  Opening bracket found. Pushed to stack.
Stack (Top to Bottom): ( { 

Step 3: Processing '}'
  Closing bracket found. Popped '(' from stack.
  Mismatched brackets: '(' and '}'! Unbalanced.
Result: Unbalanced

Test 3: {()]}
Result: Unbalanced

Test 4: {()}[
Result: Unbalanced

Test 5: [{(())}]
Result: Balanced

1. Check Example Expressions
2. Check Custom Expression
3. Exit
Enter your choice: 2
Enter expression to check: {[()]}()

Expression Analysis:
  Round brackets (): 2
  Curly brackets {}: 1
  Square brackets []: 1
  Total count suggests balanced brackets.

Checking expression: {[()]}()
-----------------------------
Step 1: Processing '{'
  Opening bracket found. Pushed to stack.
Stack (Top to Bottom): { 

Step 2: Processing '['
  Opening bracket found. Pushed to stack.
Stack (Top to Bottom): [ { 

Step 3: Processing '('
  Opening bracket found. Pushed to stack.
Stack (Top to Bottom): ( [ { 

Step 4: Processing ')'
  Closing bracket found. Popped '(' from stack.
  Matching pair found: '(' and ')'
Stack (Top to Bottom): [ { 

Step 5: Processing ']'
  Closing bracket found. Popped '[' from stack.
  Matching pair found: '[' and ']'
Stack (Top to Bottom): { 

Step 6: Processing '}'
  Closing bracket found. Popped '{' from stack.
  Matching pair found: '{' and '}'
Stack (Top to Bottom): 

Step 7: Processing '('
  Opening bracket found. Pushed to stack.
Stack (Top to Bottom): ( 

Step 8: Processing ')'
  Closing bracket found. Popped '(' from stack.
  Matching pair found: '(' and ')'
Stack (Top to Bottom): 

All brackets processed and stack is empty. Expression is balanced!

Final Result: Balanced

