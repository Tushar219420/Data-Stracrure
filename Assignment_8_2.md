## Problem Statement
Write a program to illustrate operations on a BST holding numeric keys. The menu must include: • Insert • Delete • Find • Show

## Code
```cpp
#include <iostream>
#include <queue>
using namespace std;

struct TreeNode_tv {
    int key;
    TreeNode_tv* left;
    TreeNode_tv* right;
    
    TreeNode_tv(int value) {
        key = value;
        left = nullptr;
        right = nullptr;
    }
};

class BSTOperations_tv {
private:
    TreeNode_tv* root;

    
    TreeNode_tv* insertHelper_tv(TreeNode_tv* node, int key) {
        if (node == nullptr) {
            return new TreeNode_tv(key);
        }
        
        if (key < node->key) {
            node->left = insertHelper_tv(node->left, key);
        } else if (key > node->key) {
            node->right = insertHelper_tv(node->right, key);
        }
        
        
        return node;
    }

    
    TreeNode_tv* deleteHelper_tv(TreeNode_tv* node, int key) {
        if (node == nullptr) {
            return node;
        }
        
        if (key < node->key) {
            node->left = deleteHelper_tv(node->left, key);
        } else if (key > node->key) {
            node->right = deleteHelper_tv(node->right, key);
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
                node->key = temp->key;
                node->right = deleteHelper_tv(node->right, temp->key);
            }
        }
        return node;
    }

    TreeNode_tv* findMin_tv(TreeNode_tv* node) {
        while (node && node->left != nullptr) {
            node = node->left;
        }
        return node;
    }

    TreeNode_tv* findMax_tv(TreeNode_tv* node) {
        while (node && node->right != nullptr) {
            node = node->right;
        }
        return node;
    }

    bool searchHelper_tv(TreeNode_tv* node, int key) {
        if (node == nullptr) {
            return false;
        }
        
        if (key == node->key) {
            return true;
        } else if (key < node->key) {
            return searchHelper_tv(node->left, key);
        } else {
            return searchHelper_tv(node->right, key);
        }
    }

    void inorderHelper_tv(TreeNode_tv* node) {
        if (node != nullptr) {
            inorderHelper_tv(node->left);
            cout << node->key << " ";
            inorderHelper_tv(node->right);
        }
    }

    void preorderHelper_tv(TreeNode_tv* node) {
        if (node != nullptr) {
            cout << node->key << " ";
            preorderHelper_tv(node->left);
            preorderHelper_tv(node->right);
        }
    }

    void postorderHelper_tv(TreeNode_tv* node) {
        if (node != nullptr) {
            postorderHelper_tv(node->left);
            postorderHelper_tv(node->right);
            cout << node->key << " ";
        }
    }

    void levelwiseDisplayHelper_tv() {
        if (root == nullptr) {
            cout << "BST is empty!" << endl;
            return;
        }
        
        cout << "\n=== Level-wise Display ===" << endl;
        queue<TreeNode_tv*> q;
        q.push(root);
        int level = 0;
        
        while (!q.empty()) {
            int levelSize = q.size();
            cout << "Level " << level << ": ";
            
            for (int i = 0; i < levelSize; i++) {
                TreeNode_tv* current = q.front();
                q.pop();
                
                cout << current->key << " ";
                
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
        cout << "========================" << endl;
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
    BSTOperations_tv() {
        root = nullptr;
    }

  
    void createBST_tv() {
        root = nullptr;
        cout << "BST created successfully!" << endl;
    }

    
    void insert_tv(int key) {
        root = insertHelper_tv(root, key);
        cout << "Key " << key << " inserted into BST." << endl;
    }

   
    void delete_tv(int key) {
        if (searchHelper_tv(root, key)) {
            root = deleteHelper_tv(root, key);
            cout << "Key " << key << " deleted from BST." << endl;
        } else {
            cout << "Key " << key << " not found in BST!" << endl;
        }
    }

    void find_tv(int key) {
        bool found = searchHelper_tv(root, key);
        if (found) {
            cout << "Key " << key << " found in BST." << endl;
        } else {
            cout << "Key " << key << " not found in BST." << endl;
        }
    }


    void show_tv() {
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
        levelwiseDisplayHelper_tv();
        cout << "==================" << endl;
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

   
    int findMinKey_tv() {
        if (root == nullptr) {
            cout << "BST is empty!" << endl;
            return -1;
        }
        
        TreeNode_tv* minNode = findMin_tv(root);
        cout << "Minimum key in BST: " << minNode->key << endl;
        return minNode->key;
    }

   
    int findMaxKey_tv() {
        if (root == nullptr) {
            cout << "BST is empty!" << endl;
            return -1;
        }
        
        TreeNode_tv* maxNode = findMax_tv(root);
        cout << "Maximum key in BST: " << maxNode->key << endl;
        return maxNode->key;
    }

    bool isEmpty_tv() {
        return root == nullptr;
    }

    void displayStatistics_tv() {
        if (root == nullptr) {
            cout << "BST is empty!" << endl;
            return;
        }
        
        cout << "\n=== BST Statistics ===" << endl;
        countNodes_tv();
        findHeight_tv();
        findMinKey_tv();
        findMaxKey_tv();
        cout << "=====================" << endl;
    }

    void inputKeys_tv() {
        int numKeys, key;
        cout << "Enter number of keys to insert: ";
        cin >> numKeys;
        
        for (int i = 0; i < numKeys; i++) {
            cout << "Enter key " << (i+1) << ": ";
            cin >> key;
            insert_tv(key);
        }
    }

    ~BSTOperations_tv() {
      
        root = nullptr;
    }
};

int main() {
    BSTOperations_tv bst_tv;
    int choice, key;

    cout << "=== Binary Search Tree Operations ===" << endl;
    cout << "Menu: Insert, Delete, Find, Show" << endl;

    while (true) {
        cout << "\n1. Create BST\n2. Insert\n3. Delete\n4. Find\n5. Show\n6. Input Multiple Keys\n7. Count Nodes\n8. Find Height\n9. Find Min Key\n10. Find Max Key\n11. Display Statistics\n12. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                bst_tv.createBST_tv();
                break;

            case 2:
                cout << "Enter key to insert: ";
                cin >> key;
                bst_tv.insert_tv(key);
                break;

            case 3:
                cout << "Enter key to delete: ";
                cin >> key;
                bst_tv.delete_tv(key);
                break;

            case 4:
                cout << "Enter key to find: ";
                cin >> key;
                bst_tv.find_tv(key);
                break;

            case 5:
                bst_tv.show_tv();
                break;

            case 6:
                bst_tv.inputKeys_tv();
                break;

            case 7:
                bst_tv.countNodes_tv();
                break;

            case 8:
                bst_tv.findHeight_tv();
                break;

            case 9:
                bst_tv.findMinKey_tv();
                break;

            case 10:
                bst_tv.findMaxKey_tv();
                break;

            case 11:
                bst_tv.displayStatistics_tv();
                break;

            case 12:
                cout << "Thank you for using the BST Operations Menu!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


##  Output

=== Binary Search Tree Operations ===
Menu: Insert, Delete, Find, Show

1. Create BST
2. Insert
3. Delete
4. Find
5. Show
6. Input Multiple Keys
7. Count Nodes
8. Find Height
9. Find Min Key
10. Find Max Key
11. Display Statistics
12. Exit
Enter your choice: 1
BST created successfully!

1. Create BST
2. Insert
3. Delete
4. Find
5. Show
6. Input Multiple Keys
7. Count Nodes
8. Find Height
9. Find Min Key
10. Find Max Key
11. Display Statistics
12. Exit
Enter your choice: 2
Enter key to insert: 50
Key 50 inserted into BST.

1. Create BST
2. Insert
3. Delete
4. Find
5. Show
6. Input Multiple Keys
7. Count Nodes
8. Find Height
9. Find Min Key
10. Find Max Key
11. Display Statistics
12. Exit
Enter your choice: 2
Enter key to insert: 30
Key 30 inserted into BST.

1. Create BST
2. Insert
3. Delete
4. Find
5. Show
6. Input Multiple Keys
7. Count Nodes
8. Find Height
9. Find Min Key
10. Find Max Key
11. Display Statistics
12. Exit
Enter your choice: 2
Enter key to insert: 70
Key 70 inserted into BST.

1. Create BST
2. Insert
3. Delete
4. Find
5. Show
6. Input Multiple Keys
7. Count Nodes
8. Find Height
9. Find Min Key
10. Find Max Key
11. Display Statistics
12. Exit
Enter your choice: 2
Enter key to insert: 20
Key 20 inserted into BST.

1. Create BST
2. Insert
3. Delete
4. Find
5. Show
6. Input Multiple Keys
7. Count Nodes
8. Find Height
9. Find Min Key
10. Find Max Key
11. Display Statistics
12. Exit
Enter your choice: 2
Enter key to insert: 40
Key 40 inserted into BST.

1. Create BST
2. Insert
3. Delete
4. Find
5. Show
6. Input Multiple Keys
7. Count Nodes
8. Find Height
9. Find Min Key
10. Find Max Key
11. Display Statistics
12. Exit
Enter your choice: 2
Enter key to insert: 60
Key 60 inserted into BST.

1. Create BST
2. Insert
3. Delete
4. Find
5. Show
6. Input Multiple Keys
7. Count Nodes
8. Find Height
9. Find Min Key
10. Find Max Key
11. Display Statistics
12. Exit
Enter your choice: 2
Enter key to insert: 80
Key 80 inserted into BST.

1. Create BST
2. Insert
3. Delete
4. Find
5. Show
6. Input Multiple Keys
7. Count Nodes
8. Find Height
9. Find Min Key
10. Find Max Key
11. Display Statistics
12. Exit
Enter your choice: 5

=== BST Display ===
Inorder (sorted): 20 30 40 50 60 70 80 
Preorder: 50 30 20 40 70 60 80 
Postorder: 20 40 30 60 80 70 50 

=== Level-wise Display ===
Level 0: 50 
Level 1: 30 70 
Level 2: 20 40 60 80 
==================
==================

1. Create BST
2. Insert
3. Delete
4. Find
5. Show
6. Input Multiple Keys
7. Count Nodes
8. Find Height
9. Find Min Key
10. Find Max Key
11. Display Statistics
12. Exit
Enter your choice: 4
Enter key to find: 40
Key 40 found in BST.

1. Create BST
2. Insert
3. Delete
4. Find
5. Show
6. Input Multiple Keys
7. Count Nodes
8. Find Height
9. Find Min Key
10. Find Max Key
11. Display Statistics
12. Exit
Enter your choice: 3
Enter key to delete: 30
Key 30 deleted from BST.

1. Create BST
2. Insert
3. Delete
4. Find
5. Show
6. Input Multiple Keys
7. Count Nodes
8. Find Height
9. Find Min Key
10. Find Max Key
11. Display Statistics
12. Exit
Enter your choice: 5

=== BST Display ===
Inorder (sorted): 20 40 50 60 70 80 
Preorder: 50 40 20 70 60 80 
Postorder: 20 40 60 80 70 50 

=== Level-wise Display ===
Level 0: 50 
Level 1: 40 70 
Level 2: 20 60 80 
==================
==================

1. Create BST
2. Insert
3. Delete
4. Find
5. Show
6. Input Multiple Keys
7. Count Nodes
8. Find Height
9. Find Min Key
10. Find Max Key
11. Display Statistics
12. Exit
Enter your choice: 11

=== BST Statistics ===
Total nodes in BST: 6
Height of BST: 2
Minimum key in BST: 20
Maximum key in BST: 80
=====================

1. Create BST
2. Insert
3. Delete
4. Find
5. Show
6. Input Multiple Keys
7. Count Nodes
8. Find Height
9. Find Min Key
10. Find Max Key
11. Display Statistics
12. Exit
Enter your choice: 6
Enter number of keys to insert: 3
Enter key 1: 10
Key 10 inserted into BST.
Enter key 2: 90
Key 90 inserted into BST.
Enter key 3: 25
Key 25 inserted into BST.

1. Create BST
2. Insert
3. Delete
4. Find
5. Show
6. Input Multiple Keys
7. Count Nodes
8. Find Height
9. Find Min Key
10. Find Max Key
11. Display Statistics
12. Exit
Enter your choice: 5

=== BST Display ===
Inorder (sorted): 10 20 25 40 50 60 70 80 90 
Preorder: 50 40 20 10 25 70 60 80 90 
Postorder: 10 25 20 40 60 90 80 70 50 

=== Level-wise Display ===
Level 0: 50 
Level 1: 40 70 
Level 2: 20 60 80 
Level 3: 10 25 90 
==================
==================

