## Problem Statement
Write a Program to create a Binary Tree and perform following Nonrecursive operations on it. a. Inorder Traversal b. Preorder Traversal c. Display Number of Leaf Nodes d. Mirror Image

## Code
```cpp
#include <iostream>
#include <stack>
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

class BinaryTree_tv {
private:
    TreeNode_tv* root;

    
    void insertLevelWise_tv(int value) {
        TreeNode_tv* newNode = new TreeNode_tv(value);
        
        if (root == nullptr) {
            root = newNode;
            cout << "Root node " << value << " inserted." << endl;
            return;
        }
        
        queue<TreeNode_tv*> q;
        q.push(root);
        
        while (!q.empty()) {
            TreeNode_tv* current = q.front();
            q.pop();
            
            if (current->left == nullptr) {
                current->left = newNode;
                cout << "Node " << value << " inserted as left child of " << current->data << "." << endl;
                return;
            } else if (current->right == nullptr) {
                current->right = newNode;
                cout << "Node " << value << " inserted as right child of " << current->data << "." << endl;
                return;
            }
            
            q.push(current->left);
            q.push(current->right);
        }
    }

    
    int countLeafNodesRecursive_tv(TreeNode_tv* node) {
        if (node == nullptr) {
            return 0;
        }
        
        if (node->left == nullptr && node->right == nullptr) {
            return 1;
        }
        
        return countLeafNodesRecursive_tv(node->left) + countLeafNodesRecursive_tv(node->right);
    }

public:
    BinaryTree_tv() {
        root = nullptr;
    }

   
    void createBinaryTree_tv() {
        root = nullptr;
        cout << "Binary tree created successfully!" << endl;
    }

    void insertNode_tv(int value) {
        insertLevelWise_tv(value);
    }

    void inorderTraversalNonRecursive_tv() {
        if (root == nullptr) {
            cout << "Tree is empty!" << endl;
            return;
        }
        
        cout << "Inorder Traversal (Non-recursive): ";
        stack<TreeNode_tv*> s;
        TreeNode_tv* current = root;
        
        while (current != nullptr || !s.empty()) {
            
            while (current != nullptr) {
                s.push(current);
                current = current->left;
            }
            
           
            current = s.top();
            s.pop();
            cout << current->data << " ";
            
            
            current = current->right;
        }
        cout << endl;
    }

    void preorderTraversalNonRecursive_tv() {
        if (root == nullptr) {
            cout << "Tree is empty!" << endl;
            return;
        }
        
        cout << "Preorder Traversal (Non-recursive): ";
        stack<TreeNode_tv*> s;
        s.push(root);
        
        while (!s.empty()) {
            TreeNode_tv* current = s.top();
            s.pop();
            cout << current->data << " ";
            
            if (current->right != nullptr) {
                s.push(current->right);
            }
            if (current->left != nullptr) {
                s.push(current->left);
            }
        }
        cout << endl;
    }

    void postorderTraversalNonRecursive_tv() {
        if (root == nullptr) {
            cout << "Tree is empty!" << endl;
            return;
        }
        
        cout << "Postorder Traversal (Non-recursive): ";
        stack<TreeNode_tv*> s1, s2;
        s1.push(root);
        
        
        while (!s1.empty()) {
            TreeNode_tv* current = s1.top();
            s1.pop();
            s2.push(current);
            
            if (current->left != nullptr) {
                s1.push(current->left);
            }
            if (current->right != nullptr) {
                s1.push(current->right);
            }
        }
        
        
        while (!s2.empty()) {
            cout << s2.top()->data << " ";
            s2.pop();
        }
        cout << endl;
    }

  
    void displayLeafNodesCount_tv() {
        if (root == nullptr) {
            cout << "Tree is empty! Number of leaf nodes: 0" << endl;
            return;
        }
        
        int leafCount = 0;
        queue<TreeNode_tv*> q;
        q.push(root);
        
        while (!q.empty()) {
            TreeNode_tv* current = q.front();
            q.pop();
            
            
            if (current->left == nullptr && current->right == nullptr) {
                leafCount++;
            }
            
            if (current->left != nullptr) {
                q.push(current->left);
            }
            if (current->right != nullptr) {
                q.push(current->right);
            }
        }
        
        cout << "Number of leaf nodes: " << leafCount << endl;
        
        
        int recursiveCount = countLeafNodesRecursive_tv(root);
        cout << "Verification (recursive count): " << recursiveCount << endl;
    }

    
    void createMirrorImage_tv() {
        if (root == nullptr) {
            cout << "Tree is empty! Cannot create mirror image." << endl;
            return;
        }
        
        queue<TreeNode_tv*> q;
        q.push(root);
        
        while (!q.empty()) {
            TreeNode_tv* current = q.front();
            q.pop();
            
            
            TreeNode_tv* temp = current->left;
            current->left = current->right;
            current->right = temp;
            
            if (current->left != nullptr) {
                q.push(current->left);
            }
            if (current->right != nullptr) {
                q.push(current->right);
            }
        }
        
        cout << "Mirror image of tree created successfully!" << endl;
    }


    void levelwiseDisplay_tv() {
        if (root == nullptr) {
            cout << "Tree is empty!" << endl;
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

    void displayTree_tv() {
        if (root == nullptr) {
            cout << "Tree is empty!" << endl;
            return;
        }
        
        cout << "\n=== Binary Tree Structure ===" << endl;
        inorderTraversalNonRecursive_tv();
        preorderTraversalNonRecursive_tv();
        postorderTraversalNonRecursive_tv();
        levelwiseDisplay_tv();
        displayLeafNodesCount_tv();
        cout << "============================" << endl;
    }

   
    bool isEmpty_tv() {
        return root == nullptr;
    }

 
    int getRootValue_tv() {
        if (root == nullptr) {
            cout << "Tree is empty!" << endl;
            return -1;
        }
        return root->data;
    }

    int countTotalNodes_tv() {
        if (root == nullptr) {
            return 0;
        }
        
        int count = 0;
        queue<TreeNode_tv*> q;
        q.push(root);
        
        while (!q.empty()) {
            TreeNode_tv* current = q.front();
            q.pop();
            count++;
            
            if (current->left != nullptr) {
                q.push(current->left);
            }
            if (current->right != nullptr) {
                q.push(current->right);
            }
        }
        
        cout << "Total number of nodes: " << count << endl;
        return count;
    }

    ~BinaryTree_tv() {
        
        root = nullptr;
    }
};

int main() {
    BinaryTree_tv tree_tv;
    int choice, value;

    cout << "=== Binary Tree Non-Recursive Operations ===" << endl;
    cout << "Supports Inorder, Preorder, Leaf Count, and Mirror Image" << endl;

    while (true) {
        cout << "\n1. Create Binary Tree\n2. Insert Node\n3. Inorder Traversal (Non-recursive)\n4. Preorder Traversal (Non-recursive)\n5. Postorder Traversal (Non-recursive)\n6. Display Leaf Nodes Count\n7. Create Mirror Image\n8. Display Tree\n9. Level-wise Display\n10. Count Total Nodes\n11. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                tree_tv.createBinaryTree_tv();
                break;

            case 2:
                cout << "Enter value to insert: ";
                cin >> value;
                tree_tv.insertNode_tv(value);
                break;

            case 3:
                tree_tv.inorderTraversalNonRecursive_tv();
                break;

            case 4:
                tree_tv.preorderTraversalNonRecursive_tv();
                break;

            case 5:
                tree_tv.postorderTraversalNonRecursive_tv();
                break;

            case 6:
                tree_tv.displayLeafNodesCount_tv();
                break;

            case 7:
                tree_tv.createMirrorImage_tv();
                break;

            case 8:
                tree_tv.displayTree_tv();
                break;

            case 9:
                tree_tv.levelwiseDisplay_tv();
                break;

            case 10:
                tree_tv.countTotalNodes_tv();
                break;

            case 11:
                cout << "Thank you for using the Binary Tree Operations!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}

## Output

=== Binary Tree Non-Recursive Operations ===
Supports Inorder, Preorder, Leaf Count, and Mirror Image

1. Create Binary Tree
2. Insert Node
3. Inorder Traversal (Non-recursive)
4. Preorder Traversal (Non-recursive)
5. Postorder Traversal (Non-recursive)
6. Display Leaf Nodes Count
7. Create Mirror Image
8. Display Tree
9. Level-wise Display
10. Count Total Nodes
11. Exit
Enter your choice: 1
Binary tree created successfully!

1. Create Binary Tree
2. Insert Node
3. Inorder Traversal (Non-recursive)
4. Preorder Traversal (Non-recursive)
5. Postorder Traversal (Non-recursive)
6. Display Leaf Nodes Count
7. Create Mirror Image
8. Display Tree
9. Level-wise Display
10. Count Total Nodes
11. Exit
Enter your choice: 2
Enter value to insert: 1
Root node 1 inserted.

1. Create Binary Tree
2. Insert Node
3. Inorder Traversal (Non-recursive)
4. Preorder Traversal (Non-recursive)
5. Postorder Traversal (Non-recursive)
6. Display Leaf Nodes Count
7. Create Mirror Image
8. Display Tree
9. Level-wise Display
10. Count Total Nodes
11. Exit
Enter your choice: 2
Enter value to insert: 2
Node 2 inserted as left child of 1.

1. Create Binary Tree
2. Insert Node
3. Inorder Traversal (Non-recursive)
4. Preorder Traversal (Non-recursive)
5. Postorder Traversal (Non-recursive)
6. Display Leaf Nodes Count
7. Create Mirror Image
8. Display Tree
9. Level-wise Display
10. Count Total Nodes
11. Exit
Enter your choice: 2
Enter value to insert: 3
Node 3 inserted as right child of 1.

1. Create Binary Tree
2. Insert Node
3. Inorder Traversal (Non-recursive)
4. Preorder Traversal (Non-recursive)
5. Postorder Traversal (Non-recursive)
6. Display Leaf Nodes Count
7. Create Mirror Image
8. Display Tree
9. Level-wise Display
10. Count Total Nodes
11. Exit
Enter your choice: 2
Enter value to insert: 4
Node 4 inserted as left child of 2.

1. Create Binary Tree
2. Insert Node
3. Inorder Traversal (Non-recursive)
4. Preorder Traversal (Non-recursive)
5. Postorder Traversal (Non-recursive)
6. Display Leaf Nodes Count
7. Create Mirror Image
8. Display Tree
9. Level-wise Display
10. Count Total Nodes
11. Exit
Enter your choice: 2
Enter value to insert: 5
Node 5 inserted as right child of 2.

1. Create Binary Tree
2. Insert Node
3. Inorder Traversal (Non-recursive)
4. Preorder Traversal (Non-recursive)
5. Postorder Traversal (Non-recursive)
6. Display Leaf Nodes Count
7. Create Mirror Image
8. Display Tree
9. Level-wise Display
10. Count Total Nodes
11. Exit
Enter your choice: 2
Enter value to insert: 6
Node 6 inserted as left child of 3.

1. Create Binary Tree
2. Insert Node
3. Inorder Traversal (Non-recursive)
4. Preorder Traversal (Non-recursive)
5. Postorder Traversal (Non-recursive)
6. Display Leaf Nodes Count
7. Create Mirror Image
8. Display Tree
9. Level-wise Display
10. Count Total Nodes
11. Exit
Enter your choice: 2
Enter value to insert: 7
Node 7 inserted as right child of 3.

1. Create Binary Tree
2. Insert Node
3. Inorder Traversal (Non-recursive)
4. Preorder Traversal (Non-recursive)
5. Postorder Traversal (Non-recursive)
6. Display Leaf Nodes Count
7. Create Mirror Image
8. Display Tree
9. Level-wise Display
10. Count Total Nodes
11. Exit
Enter your choice: 3
Inorder Traversal (Non-recursive): 4 2 5 1 6 3 7 

1. Create Binary Tree
2. Insert Node
3. Inorder Traversal (Non-recursive)
4. Preorder Traversal (Non-recursive)
5. Postorder Traversal (Non-recursive)
6. Display Leaf Nodes Count
7. Create Mirror Image
8. Display Tree
9. Level-wise Display
10. Count Total Nodes
11. Exit
Enter your choice: 4
Preorder Traversal (Non-recursive): 1 2 4 5 3 6 7 

1. Create Binary Tree
2. Insert Node
3. Inorder Traversal (Non-recursive)
4. Preorder Traversal (Non-recursive)
5. Postorder Traversal (Non-recursive)
6. Display Leaf Nodes Count
7. Create Mirror Image
8. Display Tree
9. Level-wise Display
10. Count Total Nodes
11. Exit
Enter your choice: 5
Postorder Traversal (Non-recursive): 4 5 2 6 7 3 1 

1. Create Binary Tree
2. Insert Node
3. Inorder Traversal (Non-recursive)
4. Preorder Traversal (Non-recursive)
5. Postorder Traversal (Non-recursive)
6. Display Leaf Nodes Count
7. Create Mirror Image
8. Display Tree
9. Level-wise Display
10. Count Total Nodes
11. Exit
Enter your choice: 6
Number of leaf nodes: 4
Verification (recursive count): 4

1. Create Binary Tree
2. Insert Node
3. Inorder Traversal (Non-recursive)
4. Preorder Traversal (Non-recursive)
5. Postorder Traversal (Non-recursive)
6. Display Leaf Nodes Count
7. Create Mirror Image
8. Display Tree
9. Level-wise Display
10. Count Total Nodes
11. Exit
Enter your choice: 8

=== Binary Tree Structure ===
Inorder Traversal (Non-recursive): 4 2 5 1 6 3 7 
Preorder Traversal (Non-recursive): 1 2 4 5 3 6 7 
Postorder Traversal (Non-recursive): 4 5 2 6 7 3 1 

=== Level-wise Display ===
Level 0: 1 
Level 1: 2 3 
Level 2: 4 5 6 7 
========================
Number of leaf nodes: 4
Verification (recursive count): 4
============================

1. Create Binary Tree
2. Insert Node
3. Inorder Traversal (Non-recursive)
4. Preorder Traversal (Non-recursive)
5. Postorder Traversal (Non-recursive)
6. Display Leaf Nodes Count
7. Create Mirror Image
8. Display Tree
9. Level-wise Display
10. Count Total Nodes
11. Exit
Enter your choice: 7
Mirror image of tree created successfully!

1. Create Binary Tree
2. Insert Node
3. Inorder Traversal (Non-recursive)
4. Preorder Traversal (Non-recursive)
5. Postorder Traversal (Non-recursive)
6. Display Leaf Nodes Count
7. Create Mirror Image
8. Display Tree
9. Level-wise Display
10. Count Total Nodes
11. Exit
Enter your choice: 3
Inorder Traversal (Non-recursive): 7 3 6 1 5 2 4 
