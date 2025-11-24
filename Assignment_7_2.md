
## Problem Statement
Write a program to perform Binary Search Tree (BST) operations (Count the total number of nodes, Compute the height of the BST, Mirror Image)

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

    
    TreeNode_tv* mirrorHelper_tv(TreeNode_tv* node) {
        if (node == nullptr) {
            return nullptr;
        }
        
        
        TreeNode_tv* temp = node->left;
        node->left = node->right;
        node->right = temp;
        
        
        mirrorHelper_tv(node->left);
        mirrorHelper_tv(node->right);
        
        return node;
    }

    
    bool isIdentical_tv(TreeNode_tv* root1, TreeNode_tv* root2) {
        if (root1 == nullptr && root2 == nullptr) {
            return true;
        }
        
        if (root1 != nullptr && root2 != nullptr) {
            return (root1->data == root2->data) &&
                   isIdentical_tv(root1->left, root2->left) &&
                   isIdentical_tv(root1->right, root2->right);
        }
        
        return false;
    }

    
    TreeNode_tv* copyTree_tv(TreeNode_tv* node) {
        if (node == nullptr) {
            return nullptr;
        }
        
        TreeNode_tv* newNode = new TreeNode_tv(node->data);
        newNode->left = copyTree_tv(node->left);
        newNode->right = copyTree_tv(node->right);
        
        return newNode;
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
        cout << "========================" << endl;
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

    
    int countTotalNodes_tv() {
        int count = countNodesHelper_tv(root);
        cout << "Total number of nodes in BST: " << count << endl;
        return count;
    }

    
    int computeHeight_tv() {
        int height = findHeightHelper_tv(root);
        cout << "Height of BST: " << height << endl;
        return height;
    }

    
    void createMirrorImage_tv() {
        if (root == nullptr) {
            cout << "BST is empty! Cannot create mirror image." << endl;
            return;
        }
        
        
        TreeNode_tv* originalTree = copyTree_tv(root);
        
        
        root = mirrorHelper_tv(root);
        
        cout << "Mirror image of BST created successfully!" << endl;
        cout << "Original BST (inorder): ";
        inorderHelper_tv(originalTree);
        cout << endl;
        cout << "Mirror BST (inorder): ";
        inorderHelper_tv(root);
        cout << endl;
        
        
    }

   
    void displayTree_tv() {
        if (root == nullptr) {
            cout << "BST is empty!" << endl;
            return;
        }
        
        cout << "\n=== BST Structure ===" << endl;
        cout << "Inorder Traversal: ";
        inorderHelper_tv(root);
        cout << endl;
        cout << "Preorder Traversal: ";
        preorderHelper_tv(root);
        cout << endl;
        levelwiseDisplayHelper_tv();
        cout << "====================" << endl;
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

    
    int findMinValue_tv() {
        if (root == nullptr) {
            cout << "BST is empty!" << endl;
            return -1;
        }
        
        TreeNode_tv* current = root;
        while (current->left != nullptr) {
            current = current->left;
        }
        return current->data;
    }

   
    int findMaxValue_tv() {
        if (root == nullptr) {
            cout << "BST is empty!" << endl;
            return -1;
        }
        
        TreeNode_tv* current = root;
        while (current->right != nullptr) {
            current = current->right;
        }
        return current->data;
    }

    ~BinarySearchTree_tv() {
        
        root = nullptr;
    }
};

int main() {
    BinarySearchTree_tv bst_tv;
    int choice, value;

    cout << "=== Binary Search Tree Operations - Part 2 ===" << endl;
    cout << "Count nodes, Compute height, Create mirror image" << endl;

    while (true) {
        cout << "\n1. Create BST\n2. Insert Value\n3. Count Total Nodes\n4. Compute Height\n5. Create Mirror Image\n6. Display Tree\n7. Check if Empty\n8. Get Root Value\n9. Find Min Value\n10. Find Max Value\n11. Exit\n";
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
                bst_tv.countTotalNodes_tv();
                break;

            case 4:
                bst_tv.computeHeight_tv();
                break;

            case 5:
                bst_tv.createMirrorImage_tv();
                break;

            case 6:
                bst_tv.displayTree_tv();
                break;

            case 7:
                if (bst_tv.isEmpty_tv()) {
                    cout << "BST is empty." << endl;
                } else {
                    cout << "BST is not empty." << endl;
                }
                break;

            case 8:
                cout << "Root value: " << bst_tv.getRootValue_tv() << endl;
                break;

            case 9:
                cout << "Minimum value: " << bst_tv.findMinValue_tv() << endl;
                break;

            case 10:
                cout << "Maximum value: " << bst_tv.findMaxValue_tv() << endl;
                break;

            case 11:
                cout << "Thank you for using the Binary Search Tree Operations!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}
## Output

=== Binary Search Tree Operations - Part 2 ===
Count nodes, Compute height, Create mirror image

1. Create BST
2. Insert Value
3. Count Total Nodes
4. Compute Height
5. Create Mirror Image
6. Display Tree
7. Check if Empty
8. Get Root Value
9. Find Min Value
10. Find Max Value
11. Exit
Enter your choice: 1
BST created successfully!

1. Create BST
2. Insert Value
3. Count Total Nodes
4. Compute Height
5. Create Mirror Image
6. Display Tree
7. Check if Empty
8. Get Root Value
9. Find Min Value
10. Find Max Value
11. Exit
Enter your choice: 2
Enter value to insert: 50
Value 50 inserted into BST.

1. Create BST
2. Insert Value
3. Count Total Nodes
4. Compute Height
5. Create Mirror Image
6. Display Tree
7. Check if Empty
8. Get Root Value
9. Find Min Value
10. Find Max Value
11. Exit
Enter your choice: 2
Enter value to insert: 30
Value 30 inserted into BST.

1. Create BST
2. Insert Value
3. Count Total Nodes
4. Compute Height
5. Create Mirror Image
6. Display Tree
7. Check if Empty
8. Get Root Value
9. Find Min Value
10. Find Max Value
11. Exit
Enter your choice: 2
Enter value to insert: 70
Value 70 inserted into BST.

1. Create BST
2. Insert Value
3. Count Total Nodes
4. Compute Height
5. Create Mirror Image
6. Display Tree
7. Check if Empty
8. Get Root Value
9. Find Min Value
10. Find Max Value
11. Exit
Enter your choice: 2
Enter value to insert: 20
Value 20 inserted into BST.

1. Create BST
2. Insert Value
3. Count Total Nodes
4. Compute Height
5. Create Mirror Image
6. Display Tree
7. Check if Empty
8. Get Root Value
9. Find Min Value
10. Find Max Value
11. Exit
Enter your choice: 2
Enter value to insert: 40
Value 40 inserted into BST.

1. Create BST
2. Insert Value
3. Count Total Nodes
4. Compute Height
5. Create Mirror Image
6. Display Tree
7. Check if Empty
8. Get Root Value
9. Find Min Value
10. Find Max Value
11. Exit
Enter your choice: 2
Enter value to insert: 60
Value 60 inserted into BST.

1. Create BST
2. Insert Value
3. Count Total Nodes
4. Compute Height
5. Create Mirror Image
6. Display Tree
7. Check if Empty
8. Get Root Value
9. Find Min Value
10. Find Max Value
11. Exit
Enter your choice: 2
Enter value to insert: 80
Value 80 inserted into BST.

1. Create BST
2. Insert Value
3. Count Total Nodes
4. Compute Height
5. Create Mirror Image
6. Display Tree
7. Check if Empty
8. Get Root Value
9. Find Min Value
10. Find Max Value
11. Exit
Enter your choice: 3
Total number of nodes in BST: 7

1. Create BST
2. Insert Value
3. Count Total Nodes
4. Compute Height
5. Create Mirror Image
6. Display Tree
7. Check if Empty
8. Get Root Value
9. Find Min Value
10. Find Max Value
11. Exit
Enter your choice: 4
Height of BST: 2

1. Create BST
2. Insert Value
3. Count Total Nodes
4. Compute Height
5. Create Mirror Image
6. Display Tree
7. Check if Empty
8. Get Root Value
9. Find Min Value
10. Find Max Value
11. Exit
Enter your choice: 6

=== BST Structure ===
Inorder Traversal: 20 30 40 50 60 70 80 
Preorder Traversal: 50 30 20 40 70 60 80 

=== Level-wise Display ===
Level 0: 50 
Level 1: 30 70 
Level 2: 20 40 60 80 
========================

1. Create BST
2. Insert Value
3. Count Total Nodes
4. Compute Height
5. Create Mirror Image
6. Display Tree
7. Check if Empty
8. Get Root Value
9. Find Min Value
10. Find Max Value
11. Exit
Enter your choice: 9
Minimum value: 20

1. Create BST
2. Insert Value
3. Count Total Nodes
4. Compute Height
5. Create Mirror Image
6. Display Tree
7. Check if Empty
8. Get Root Value
9. Find Min Value
10. Find Max Value
11. Exit
Enter your choice: 10
Maximum value: 80

1. Create BST
2. Insert Value
3. Count Total Nodes
4. Compute Height
5. Create Mirror Image
6. Display Tree
7. Check if Empty
8. Get Root Value
9. Find Min Value
10. Find Max Value
11. Exit
Enter your choice: 5
Mirror image of BST created successfully!
Original BST (inorder): 20 30 40 50 60 70 80 
Mirror BST (inorder): 80 70 60 50 40 30 20 

1. Create BST
2. Insert Value
3. Count Total Nodes
4. Compute Height
5. Create Mirror Image
6. Display Tree
7. Check if Empty
8. Get Root Value
9. Find Min Value
10. Find Max Value
11. Exit
Enter your choice: 6

=== BST Structure ===
Inorder Traversal: 80 70 60 50 40 30 20 
Preorder Traversal: 50 70 80 60 30 40 20 

=== Level-wise Display ===
Level 0: 50 
Level 1: 70 30 
Level 2: 80 60 40 20 
========================
