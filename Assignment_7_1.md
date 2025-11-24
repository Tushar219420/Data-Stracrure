## Problem Statement
Write a program to perform Binary Search Tree (BST) operations (Create, Insert, Delete, Levelwise display)

## Code
```cpp
#include <iostream>
#include <queue>
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


    TreeNode_tv* findMin_tv(TreeNode_tv* node) {
        while (node && node->left != nullptr) {
            node = node->left;
        }
        return node;
    }

    
    TreeNode_tv* deleteHelper_tv(TreeNode_tv* node, int value) {
        if (node == nullptr) {
            return node;
        }
        
        if (value < node->data) {
            node->left = deleteHelper_tv(node->left, value);
        } else if (value > node->data) {
            node->right = deleteHelper_tv(node->right, value);
        } else {
            
            if (node->left == nullptr && node->right == nullptr) {
                delete node;
                return nullptr;
            }
            
            else if (node->left == nullptr) {
                TreeNode_tv* temp = node->right;
                delete node;
                return temp;
            } else if (node->right == nullptr) {
                TreeNode_tv* temp = node->left;
                delete node;
                return temp;
            }
            
            else {
                TreeNode_tv* temp = findMin_tv(node->right);
                node->data = temp->data;
                node->right = deleteHelper_tv(node->right, temp->data);
            }
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

   
    int findHeightHelper_tv(TreeNode_tv* node) {
        if (node == nullptr) {
            return -1;  
        }
        
        int leftHeight = findHeightHelper_tv(node->left);
        int rightHeight = findHeightHelper_tv(node->right);
        
        return 1 + max(leftHeight, rightHeight);
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

    
    void delete_tv(int value) {
        if (searchHelper_tv(root, value)) {
            root = deleteHelper_tv(root, value);
            cout << "Value " << value << " deleted from BST." << endl;
        } else {
            cout << "Value " << value << " not found in BST!" << endl;
        }
    }

    
    void levelwiseDisplay_tv() {
        if (root == nullptr) {
            cout << "BST is empty!" << endl;
            return;
        }
        
        cout << "\n=== Level-wise Display (BFS) ===" << endl;
        queue<TreeNode_tv*> q;
        q.push(root);
        int level = 0;
        
        while (!q.empty()) {
            int levelSize = q.size();
            cout << "Level " << level << ": ";
            
            for (int i = 0; i < levelSize; i++) {
                TreeNode_tv* current = q.front();
                q.pop();
                
                cout << current->data << " ";
                
                if (current->left != nullptr) {
                    q.push(current->left);
                }
                if (current->right != nullptr) {
                    q.push(current->right);
                }
            }
            cout << endl;
            level++;
        }
        cout << "===============================" << endl;
    }

    
    void inorderTraversal_tv() {
        cout << "Inorder Traversal: ";
        inorderHelper_tv(root);
        cout << endl;
    }

    
    void preorderTraversal_tv() {
        cout << "Preorder Traversal: ";
        preorderHelper_tv(root);
        cout << endl;
    }

    
    void postorderTraversal_tv() {
        cout << "Postorder Traversal: ";
        postorderHelper_tv(root);
        cout << endl;
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

    
    int findHeight_tv() {
        int height = findHeightHelper_tv(root);
        cout << "Height of BST: " << height << endl;
        return height;
    }

    
    void displayTreeStructure_tv() {
        if (root == nullptr) {
            cout << "BST is empty!" << endl;
            return;
        }
        
        cout << "\n=== BST Structure ===" << endl;
        inorderTraversal_tv();
        preorderTraversal_tv();
        postorderTraversal_tv();
        cout << "====================" << endl;
    }

    ~BinarySearchTree_tv() {
        
        root = nullptr;
    }
};

int main() {
    BinarySearchTree_tv bst_tv;
    int choice, value;

    cout << "=== Binary Search Tree Operations ===" << endl;
    cout << "Supports Create, Insert, Delete, and Level-wise Display" << endl;

    while (true) {
        cout << "\n1. Create BST\n2. Insert Value\n3. Delete Value\n4. Level-wise Display\n5. Inorder Traversal\n6. Preorder Traversal\n7. Postorder Traversal\n8. Search Value\n9. Count Nodes\n10. Find Height\n11. Display Tree Structure\n12. Exit\n";
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
                cout << "Enter value to delete: ";
                cin >> value;
                bst_tv.delete_tv(value);
                break;

            case 4:
                bst_tv.levelwiseDisplay_tv();
                break;

            case 5:
                bst_tv.inorderTraversal_tv();
                break;

            case 6:
                bst_tv.preorderTraversal_tv();
                break;

            case 7:
                bst_tv.postorderTraversal_tv();
                break;

            case 8:
                cout << "Enter value to search: ";
                cin >> value;
                bst_tv.search_tv(value);
                break;

            case 9:
                bst_tv.countNodes_tv();
                break;

            case 10:
                bst_tv.findHeight_tv();
                break;

            case 11:
                bst_tv.displayTreeStructure_tv();
                break;

            case 12:
                cout << "Thank you for using the Binary Search Tree Operations!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}

##  Output

=== Binary Search Tree Operations ===
Supports Create, Insert, Delete, and Level-wise Display

1. Create BST
2. Insert Value
3. Delete Value
4. Level-wise Display
5. Inorder Traversal
6. Preorder Traversal
7. Postorder Traversal
8. Search Value
9. Count Nodes
10. Find Height
11. Display Tree Structure
12. Exit
Enter your choice: 1
BST created successfully!

1. Create BST
2. Insert Value
3. Delete Value
4. Level-wise Display
5. Inorder Traversal
6. Preorder Traversal
7. Postorder Traversal
8. Search Value
9. Count Nodes
10. Find Height
11. Display Tree Structure
12. Exit
Enter your choice: 2
Enter value to insert: 50
Value 50 inserted into BST.

1. Create BST
2. Insert Value
3. Delete Value
4. Level-wise Display
5. Inorder Traversal
6. Preorder Traversal
7. Postorder Traversal
8. Search Value
9. Count Nodes
10. Find Height
11. Display Tree Structure
12. Exit
Enter your choice: 2
Enter value to insert: 30
Value 30 inserted into BST.

1. Create BST
2. Insert Value
3. Delete Value
4. Level-wise Display
5. Inorder Traversal
6. Preorder Traversal
7. Postorder Traversal
8. Search Value
9. Count Nodes
10. Find Height
11. Display Tree Structure
12. Exit
Enter your choice: 2
Enter value to insert: 70
Value 70 inserted into BST.

1. Create BST
2. Insert Value
3. Delete Value
4. Level-wise Display
5. Inorder Traversal
6. Preorder Traversal
7. Postorder Traversal
8. Search Value
9. Count Nodes
10. Find Height
11. Display Tree Structure
12. Exit
Enter your choice: 2
Enter value to insert: 20
Value 20 inserted into BST.

1. Create BST
2. Insert Value
3. Delete Value
4. Level-wise Display
5. Inorder Traversal
6. Preorder Traversal
7. Postorder Traversal
8. Search Value
9. Count Nodes
10. Find Height
11. Display Tree Structure
12. Exit
Enter your choice: 2
Enter value to insert: 40
Value 40 inserted into BST.

1. Create BST
2. Insert Value
3. Delete Value
4. Level-wise Display
5. Inorder Traversal
6. Preorder Traversal
7. Postorder Traversal
8. Search Value
9. Count Nodes
10. Find Height
11. Display Tree Structure
12. Exit
Enter your choice: 2
Enter value to insert: 60
Value 60 inserted into BST.

1. Create BST
2. Insert Value
3. Delete Value
4. Level-wise Display
5. Inorder Traversal
6. Preorder Traversal
7. Postorder Traversal
8. Search Value
9. Count Nodes
10. Find Height
11. Display Tree Structure
12. Exit
Enter your choice: 2
Enter value to insert: 80
Value 80 inserted into BST.

1. Create BST
2. Insert Value
3. Delete Value
4. Level-wise Display
5. Inorder Traversal
6. Preorder Traversal
7. Postorder Traversal
8. Search Value
9. Count Nodes
10. Find Height
11. Display Tree Structure
12. Exit
Enter your choice: 4

=== Level-wise Display (BFS) ===
Level 0: 50 
Level 1: 30 70 
Level 2: 20 40 60 80 
===============================

1. Create BST
2. Insert Value
3. Delete Value
4. Level-wise Display
5. Inorder Traversal
6. Preorder Traversal
7. Postorder Traversal
8. Search Value
9. Count Nodes
10. Find Height
11. Display Tree Structure
12. Exit
Enter your choice: 5
Inorder Traversal: 20 30 40 50 60 70 80 

1. Create BST
2. Insert Value
3. Delete Value
4. Level-wise Display
5. Inorder Traversal
6. Preorder Traversal
7. Postorder Traversal
8. Search Value
9. Count Nodes
10. Find Height
11. Display Tree Structure
12. Exit
Enter your choice: 8
Enter value to search: 40
Value 40 found in BST.

1. Create BST
2. Insert Value
3. Delete Value
4. Level-wise Display
5. Inorder Traversal
6. Preorder Traversal
7. Postorder Traversal
8. Search Value
9. Count Nodes
10. Find Height
11. Display Tree Structure
12. Exit
Enter your choice: 3
Enter value to delete: 30
Value 30 deleted from BST.

1. Create BST
2. Insert Value
3. Delete Value
4. Level-wise Display
5. Inorder Traversal
6. Preorder Traversal
7. Postorder Traversal
8. Search Value
9. Count Nodes
10. Find Height
11. Display Tree Structure
12. Exit
Enter your choice: 4

=== Level-wise Display (BFS) ===
Level 0: 50 
Level 1: 40 70 
Level 2: 20 60 80 
===============================

1. Create BST
2. Insert Value
3. Delete Value
4. Level-wise Display
5. Inorder Traversal
6. Preorder Traversal
7. Postorder Traversal
8. Search Value
9. Count Nodes
10. Find Height
11. Display Tree Structure
12. Exit
Enter your choice: 9
Total nodes in BST: 6

1. Create BST
2. Insert Value
3. Delete Value
4. Level-wise Display
5. Inorder Traversal
6. Preorder Traversal
7. Postorder Traversal
8. Search Value
9. Count Nodes
10. Find Height
11. Display Tree Structure
12. Exit
Enter your choice: 10
Height of BST: 2
