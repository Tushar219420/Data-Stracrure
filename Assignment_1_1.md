## Problem Statement
Implement basic string operations such as length calculation, copy, reverse, and concatenation using character single dimensional arrays without using built-in string library functions.

##  Code
```cpp
#include <iostream>
#include <cstring>
using namespace std;

int stringLength_tv(char str[]) {
    int len = 0;
    while(str[len] != '\0') {
        len++;
    }
    return len;
}

void stringCopy_tv(char dest[], char src[]) {
    int i = 0;
    while(src[i] != '\0') {
        dest[i] = src[i];
        i++;
    }
    dest[i] = '\0';
}

void stringReverse_tv(char str[]) {
    int len = stringLength_tv(str);
    for(int i = 0; i < len/2; i++) {
        char temp = str[i];
        str[i] = str[len-i-1];
        str[len-i-1] = temp;
    }
}

void stringConcat_tv(char str1[], char str2[]) {
    int len1 = stringLength_tv(str1);
    int i = 0;
    while(str2[i] != '\0') {
        str1[len1+i] = str2[i];
        i++;
    }
    str1[len1+i] = '\0';
}

int main() {
    char name1_tv[100], name2_tv[100];
    cout << "Enter first name : ";
    cin >> name1_tv;
    cout << "Enter second name : ";
    cin >> name2_tv;
    
    cout << "\n=== String Operations Demo ===" << endl;
    cout << "Original Names: " << name1_tv << ", " << name2_tv << endl;
    
    cout << "Length of " << name1_tv << ": " << stringLength_tv(name1_tv) << endl;
    cout << "Length of " << name2_tv << ": " << stringLength_tv(name2_tv) << endl;
    
    char copyName_tv[100];
    stringCopy_tv(copyName_tv, name1_tv);
    cout << "Copy of " << name1_tv << ": " << copyName_tv << endl;
    
    stringReverse_tv(name1_tv);
    cout << "Reverse of first name: " << name1_tv << endl;
    
    stringConcat_tv(copyName_tv, name2_tv);
    cout << "Concatenation: " << copyName_tv << endl;
    
    return 0;
}


##  Output
Enter first name : Arjun
Enter second name : Patel

=== String Operations Demo ===
Original Names: Arjun, Patel
Length of Arjun: 5
Length of Patel: 5
Copy of Arjun: Arjun
Reverse of first name: nujrA
Concatenation: ArjunPatel

