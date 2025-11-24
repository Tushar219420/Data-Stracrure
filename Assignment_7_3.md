## Problem Statement
Write a Program to create a Binary Tree Search and Find Minimum/Maximum in BST

## Code
```cpp
#include <iostream>
#include <climits>
using namespace std;

struct TreeNode_tv {
    int data;
    TreeNode_tv* left;
    TreeNode_tv* right;
    
    TreeNode_tv(int value) {
        data = value;
        left = nullptr;
        right = nullptr;
    }
};

class BinarySearchTree_tv {
private:
    TreeNode_tv* root;

  
    TreeNode_tv* insertHelper_tv(TreeNode_tv* node, int value) {
        if (node == nullptr) {
            return new TreeNode_tv(value);
        }
        
        if (value < node->data) {
            node->left = insertHelper_tv(node->left, value);
        } else if (value > node->data) {
            node->right = insertHelper_tv(node->right, value);
        }
        
        
        return node;
    }

    
    void inorderHelper_tv(TreeNode_tv* node) {
        if (node != nullptr) {
            inorderHelper_tv(node->left);
            cout << node->data << " ";
            inorderHelper_tv(node->right);
        }
    }

    
    void preorderHelper_tv(TreeNode_tv* node) {
        if (node != nullptr) {
            cout << node->data << " ";
            preorderHelper_tv(node->left);
            preorderHelper_tv(node->right);
        }
    }

    
    void postorderHelper_tv(TreeNode_tv* node) {
        if (node != nullptr) {
            postorderHelper_tv(node->left);
            postorderHelper_tv(node->right);
            cout << node->data << " ";
        }
    }

    
    int findMinRecursive_tv(TreeNode_tv* node) {
        if (node == nullptr) {
            cout << "Tree is empty!" << endl;
            return INT_MAX;
        }
        
        if (node->left == nullptr) {
            return node->data;
        }
        
        return findMinRecursive_tv(node->left);
    }

    
    int findMaxRecursive_tv(TreeNode_tv* node) {
        if (node == nullptr) {
            cout << "Tree is empty!" << endl;
            return INT_MIN;
        }
        
        if (node->right == nullptr) {
            return node->data;
        }
        
        return findMaxRecursive_tv(node->right);
    }

    
    int findMinIterative_tv() {
        if (root == nullptr) {
            cout << "Tree is empty!" << endl;
            return INT_MAX;
        }
        
        TreeNode_tv* current = root;
        while (current->left != nullptr) {
            current = current->left;
        }
        return current->data;
    }

    
    int findMaxIterative_tv() {
        if (root == nullptr) {
            cout << "Tree is empty!" << endl;
            return INT_MIN;
        }
        
        TreeNode_tv* current = root;
        while (current->right != nullptr) {
            current = current->right;
        }
        return current->data;
    }

    bool searchHelper_tv(TreeNode_tv* node, int value) {
        if (node == nullptr) {
            return false;
        }
        
        if (value == node->data) {
            return true;
        } else if (value < node->data) {
            return searchHelper_tv(node->left, value);
        } else {
            return searchHelper_tv(node->right, value);
        }
    }

    
    int countNodesHelper_tv(TreeNode_tv* node) {
        if (node == nullptr) {
            return 0;
        }
        return 1 + countNodesHelper_tv(node->left) + countNodesHelper_tv(node->right);
    }

public:
    BinarySearchTree_tv() {
        root = nullptr;
    }

    
    void createBST_tv() {
        root = nullptr;
        cout << "BST created successfully!" << endl;
    }

    
    void insert_tv(int value) {
        root = insertHelper_tv(root, value);
        cout << "Value " << value << " inserted into BST." << endl;
    }

    
    void createBSTFromArray_tv(int arr[], int size) {
        createBST_tv();  
        cout << "Creating BST from array: ";
        for (int i = 0; i < size; i++) {
            cout << arr[i] << " ";
            insert_tv(arr[i]);
        }
        cout << endl;
    }

    
    int findMinRecursive_tv() {
        int minVal = findMinRecursive_tv(root);
        if (minVal != INT_MAX) {
            cout << "Minimum value (recursive): " << minVal << endl;
        }
        return minVal;
    }

    
    int findMaxRecursive_tv() {
        int maxVal = findMaxRecursive_tv(root);
        if (maxVal != INT_MIN) {
            cout << "Maximum value (recursive): " << maxVal << endl;
        }
        return maxVal;
    }

    int findMinIterative_tv() {
        int minVal = findMinIterative_tv();
        if (minVal != INT_MAX) {
            cout << "Minimum value (iterative): " << minVal << endl;
        }
        return minVal;
    }

   
    int findMaxIterative_tv() {
        int maxVal = findMaxIterative_tv();
        if (maxVal != INT_MIN) {
            cout << "Maximum value (iterative): " << maxVal << endl;
        }
        return maxVal;
    }

    void displayBST_tv() {
        if (root == nullptr) {
            cout << "BST is empty!" << endl;
            return;
        }
        
        cout << "\n=== BST Display ===" << endl;
        cout << "Inorder (sorted): ";
        inorderHelper_tv(root);
        cout << endl;
        cout << "Preorder: ";
        preorderHelper_tv(root);
        cout << endl;
        cout << "Postorder: ";
        postorderHelper_tv(root);
        cout << endl;
        cout << "==================" << endl;
    }

    bool search_tv(int value) {
        bool found = searchHelper_tv(root, value);
        if (found) {
            cout << "Value " << value << " found in BST." << endl;
        } else {
            cout << "Value " << value << " not found in BST." << endl;
        }
        return found;
    }

    
    int countNodes_tv() {
        int count = countNodesHelper_tv(root);
        cout << "Total nodes in BST: " << count << endl;
        return count;
    }

    
    bool isEmpty_tv() {
        return root == nullptr;
    }

    int getRootValue_tv() {
        if (root == nullptr) {
            cout << "BST is empty!" << endl;
            return -1;
        }
        return root->data;
    }

    void displayStatistics_tv() {
        if (root == nullptr) {
            cout << "BST is empty!" << endl;
            return;
        }
        
        cout << "\n=== BST Statistics ===" << endl;
        cout << "Root value: " << getRootValue_tv() << endl;
        cout << "Total nodes: " << countNodes_tv() << endl;
        cout << "Minimum value (recursive): " << findMinRecursive_tv() << endl;
        cout << "Maximum value (recursive): " << findMaxRecursive_tv() << endl;
        cout << "Minimum value (iterative): " << findMinIterative_tv() << endl;
        cout << "Maximum value (iterative): " << findMaxIterative_tv() << endl;
        cout << "=====================" << endl;
    }

    ~BinarySearchTree_tv() {
        
        root = nullptr;
    }
};

int main() {
    BinarySearchTree_tv bst_tv;
    int choice, value, arr[20], size;

    cout << "=== Binary Search Tree - Creation and Min/Max Finding ===" << endl;
    cout << "Create BST and find minimum/maximum values" << endl;

    while (true) {
        cout << "\n1. Create BST\n2. Insert Value\n3. Create BST from Array\n4. Find Min (Recursive)\n5. Find Max (Recursive)\n6. Find Min (Iterative)\n7. Find Max (Iterative)\n8. Display BST\n9. Search Value\n10. Display Statistics\n11. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                bst_tv.createBST_tv();
                break;

            case 2:
                cout << "Enter value to insert: ";
                cin >> value;
                bst_tv.insert_tv(value);
                break;

            case 3:
                cout << "Enter number of elements: ";
                cin >> size;
                cout << "Enter " << size << " elements: ";
                for (int i = 0; i < size; i++) {
                    cin >> arr[i];
                }
                bst_tv.createBSTFromArray_tv(arr, size);
                break;

            case 4:
                bst_tv.findMinRecursive_tv();
                break;

            case 5:
                bst_tv.findMaxRecursive_tv();
                break;

            case 6:
                bst_tv.findMinIterative_tv();
                break;

            case 7:
                bst_tv.findMaxIterative_tv();
                break;

            case 8:
                bst_tv.displayBST_tv();
                break;

            case 9:
                cout << "Enter value to search: ";
                cin >> value;
                bst_tv.search_tv(value);
                break;

            case 10:
                bst_tv.displayStatistics_tv();
                break;

            case 11:
                cout << "Thank you for using the BST Min/Max Finder!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


## Output

=== Binary Search Tree - Creation and Min/Max Finding ===
Create BST and find minimum/maximum values

1. Create BST
2. Insert Value
3. Create BST from Array
4. Find Min (Recursive)
5. Find Max (Recursive)
6. Find Min (Iterative)
7. Find Max (Iterative)
8. Display BST
9. Search Value
10. Display Statistics
11. Exit
Enter your choice: 1
BST created successfully!

1. Create BST
2. Insert Value
3. Create BST from Array
4. Find Min (Recursive)
5. Find Max (Recursive)
6. Find Min (Iterative)
7. Find Max (Iterative)
8. Display BST
9. Search Value
10. Display Statistics
11. Exit
Enter your choice: 2
Enter value to insert: 50
Value 50 inserted into BST.

1. Create BST
2. Insert Value
3. Create BST from Array
4. Find Min (Recursive)
5. Find Max (Recursive)
6. Find Min (Iterative)
7. Find Max (Iterative)
8. Display BST
9. Search Value
10. Display Statistics
11. Exit
Enter your choice: 2
Enter value to insert: 30
Value 30 inserted into BST.

1. Create BST
2. Insert Value
3. Create BST from Array
4. Find Min (Recursive)
5. Find Max (Recursive)
6. Find Min (Iterative)
7. Find Max (Iterative)
8. Display BST
9. Search Value
10. Display Statistics
11. Exit
Enter your choice: 2
Enter value to insert: 70
Value 70 inserted into BST.

1. Create BST
2. Insert Value
3. Create BST from Array
4. Find Min (Recursive)
5. Find Max (Recursive)
6. Find Min (Iterative)
7. Find Max (Iterative)
8. Display BST
9. Search Value
10. Display Statistics
11. Exit
Enter your choice: 2
Enter value to insert: 20
Value 20 inserted into BST.

1. Create BST
2. Insert Value
3. Create BST from Array
4. Find Min (Recursive)
5. Find Max (Recursive)
6. Find Min (Iterative)
7. Find Max (Iterative)
8. Display BST
9. Search Value
10. Display Statistics
11. Exit
Enter your choice: 2
Enter value to insert: 40
Value 40 inserted into BST.

1. Create BST
2. Insert Value
3. Create BST from Array
4. Find Min (Recursive)
5. Find Max (Recursive)
6. Find Min (Iterative)
7. Find Max (Iterative)
8. Display BST
9. Search Value
10. Display Statistics
11. Exit
Enter your choice: 2
Enter value to insert: 60
Value 60 inserted into BST.

1. Create BST
2. Insert Value
3. Create BST from Array
4. Find Min (Recursive)
5. Find Max (Recursive)
6. Find Min (Iterative)
7. Find Max (Iterative)
8. Display BST
9. Search Value
10. Display Statistics
11. Exit
Enter your choice: 2
Enter value to insert: 80
Value 80 inserted into BST.

1. Create BST
2. Insert Value
3. Create BST from Array
4. Find Min (Recursive)
5. Find Max (Recursive)
6. Find Min (Iterative)
7. Find Max (Iterative)
8. Display BST
9. Search Value
10. Display Statistics
11. Exit
Enter your choice: 8

=== BST Display ===
Inorder (sorted): 20 30 40 50 60 70 80 
Preorder: 50 30 20 40 70 60 80 
Postorder: 20 40 30 60 80 70 50 
==================

1. Create BST
2. Insert Value
3. Create BST from Array
4. Find Min (Recursive)
5. Find Max (Recursive)
6. Find Min (Iterative)
7. Find Max (Iterative)
8. Display BST
9. Search Value
10. Display Statistics
11. Exit
Enter your choice: 4
Minimum value (recursive): 20

1. Create BST
2. Insert Value
3. Create BST from Array
4. Find Min (Recursive)
5. Find Max (Recursive)
6. Find Min (Iterative)
7. Find Max (Iterative)
8. Display BST
9. Search Value
10. Display Statistics
11. Exit
Enter your choice: 5
Maximum value (recursive): 80

1. Create BST
2. Insert Value
3. Create BST from Array
4. Find Min (Recursive)
5. Find Max (Recursive)
6. Find Min (Iterative)
7. Find Max (Iterative)
8. Display BST
9. Search Value
10. Display Statistics
11. Exit
Enter your choice: 6
Minimum value (iterative): 20

1. Create BST
2. Insert Value
3. Create BST from Array
4. Find Min (Recursive)
5. Find Max (Recursive)
6. Find Min (Iterative)
7. Find Max (Iterative)
8. Display BST
9. Search Value
10. Display Statistics
11. Exit
Enter your choice: 7
Maximum value (iterative): 80

1. Create BST
2. Insert Value
3. Create BST from Array
4. Find Min (Recursive)
5. Find Max (Recursive)
6. Find Min (Iterative)
7. Find Max (Iterative)
8. Display BST
9. Search Value
10. Display Statistics
11. Exit
Enter your choice: 3
Enter number of elements: 5
Enter 5 elements: 15 35 55 75 95
Creating BST from array: 15 35 55 75 95 
Value 15 inserted into BST.
BST created successfully!
Value 15 inserted into BST.
Value 35 inserted into BST.
Value 55 inserted into BST.
Value 75 inserted into BST.
Value 95 inserted into BST.

1. Create BST
2. Insert Value
3. Create BST from Array
4. Find Min (Recursive)
5. Find Max (Recursive)
6. Find Min (Iterative)
7. Find Max (Iterative)
8. Display BST
9. Search Value
10. Display Statistics
11. Exit
Enter your choice: 10

=== BST Statistics ===
Root value: 15
Total nodes: 5
Minimum value (recursive): 15
Maximum value (recursive): 95
Minimum value (iterative): 15
Maximum value (iterative): 95
=====================

1. Create BST
2. Insert Value
3. Create BST from Array
4. Find Min (Recursive)
5. Find Max (Recursive)
6. Find Min (Iterative)
7. Find Max (Iterative)
8. Display BST
9. Search Value
10. Display Statistics
11. Exit
Enter your choice: 9
Enter value to search: 55
Value 55 found in BST.

