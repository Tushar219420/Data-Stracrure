## Problem Statement
Write a program to implement a product inventory management system for a shop using a search tree data structure. Each product must store the following information: Unique Product Code, Product Name, Price, Quantity in Stock, Date Received, Expiration Date. Implement the following operations: Insert a product into the tree (organized by product name). Display all items in the inventory using inorder traversal. List expired items in prefix (preorder) order of their names.

##  Code
```cpp
#include <iostream>
#include <cstring>
using namespace std;

struct Date_tv {
    int day;
    int month;
    int year;
};

struct Product_tv {
    int productCode;
    char productName[50];
    float price;
    int quantity;
    Date_tv dateReceived;
    Date_tv expirationDate;
    Product_tv* left;
    Product_tv* right;
    
    Product_tv(int code, const char* name, float pr, int qty, Date_tv received, Date_tv expiry) {
        productCode = code;
        strcpy(productName, name);
        price = pr;
        quantity = qty;
        dateReceived = received;
        expirationDate = expiry;
        left = nullptr;
        right = nullptr;
    }
};

class ProductInventory_tv {
private:
    Product_tv* root;

    Product_tv* insertProductHelper_tv(Product_tv* node, int code, const char* name, float price, int quantity, Date_tv received, Date_tv expiry) {
        if (node == nullptr) {
            return new Product_tv(code, name, price, quantity, received, expiry);
        }

        if (strcmp(name, node->productName) < 0) {
            node->left = insertProductHelper_tv(node->left, code, name, price, quantity, received, expiry);
        } else if (strcmp(name, node->productName) > 0) {
            node->right = insertProductHelper_tv(node->right, code, name, price, quantity, received, expiry);
        }
        
        
        return node;
    }

    
    void inorderHelper_tv(Product_tv* node) {
        if (node != nullptr) {
            inorderHelper_tv(node->left);
            cout << node->productCode << "\t" << node->productName << "\t\t" << node->price << "\t\t" 
                 << node->quantity << "\t\t" << node->dateReceived.day << "/" << node->dateReceived.month << "/" << node->dateReceived.year 
                 << "\t" << node->expirationDate.day << "/" << node->expirationDate.month << "/" << node->expirationDate.year << endl;
            inorderHelper_tv(node->right);
        }
    }

    void preorderHelper_tv(Product_tv* node) {
        if (node != nullptr) {
            cout << node->productCode << "\t" << node->productName << "\t\t" << node->price << "\t\t" 
                 << node->quantity << "\t\t" << node->dateReceived.day << "/" << node->dateReceived.month << "/" << node->dateReceived.year 
                 << "\t" << node->expirationDate.day << "/" << node->expirationDate.month << "/" << node->expirationDate.year << endl;
            preorderHelper_tv(node->left);
            preorderHelper_tv(node->right);
        }
    }


    Product_tv* searchProductHelper_tv(Product_tv* node, const char* name) {
        if (node == nullptr || strcmp(node->productName, name) == 0) {
            return node;
        }
        
        if (strcmp(name, node->productName) < 0) {
            return searchProductHelper_tv(node->left, name);
        } else {
            return searchProductHelper_tv(node->right, name);
        }
    }

    Product_tv* deleteProductHelper_tv(Product_tv* node, const char* name) {
        if (node == nullptr) {
            return node;
        }
        
        if (strcmp(name, node->productName) < 0) {
            node->left = deleteProductHelper_tv(node->left, name);
        } else if (strcmp(name, node->productName) > 0) {
            node->right = deleteProductHelper_tv(node->right, name);
        } else {
            
            if (node->left == nullptr && node->right == nullptr) {
                delete node;
                return nullptr;
            }
          
            else if (node->left == nullptr) {
                Product_tv* temp = node->right;
                delete node;
                return temp;
            } else if (node->right == nullptr) {
                Product_tv* temp = node->left;
                delete node;
                return temp;
            }
            
            else {
                
                Product_tv* temp = findMin_tv(node->right);
                node->productCode = temp->productCode;
                strcpy(node->productName, temp->productName);
                node->price = temp->price;
                node->quantity = temp->quantity;
                node->dateReceived = temp->dateReceived;
                node->expirationDate = temp->expirationDate;
                node->right = deleteProductHelper_tv(node->right, temp->productName);
            }
        }
        return node;
    }

 
    Product_tv* findMin_tv(Product_tv* node) {
        while (node && node->left != nullptr) {
            node = node->left;
        }
        return node;
    }


    int countProductsHelper_tv(Product_tv* node) {
        if (node == nullptr) {
            return 0;
        }
        return 1 + countProductsHelper_tv(node->left) + countProductsHelper_tv(node->right);
    }


    bool isExpired_tv(Date_tv expiryDate, Date_tv currentDate) {
        if (expiryDate.year < currentDate.year) {
            return true;
        } else if (expiryDate.year == currentDate.year) {
            if (expiryDate.month < currentDate.month) {
                return true;
            } else if (expiryDate.month == currentDate.month) {
                if (expiryDate.day < currentDate.day) {
                    return true;
                }
            }
        }
        return false;
    }

    void displayLowStockHelper_tv(Product_tv* node, int threshold) {
        if (node != nullptr) {
            displayLowStockHelper_tv(node->left, threshold);
            if (node->quantity < threshold) {
                cout << node->productCode << "\t" << node->productName << "\t\t" << node->quantity << endl;
            }
            displayLowStockHelper_tv(node->right, threshold);
        }
    }

public:
    ProductInventory_tv() {
        root = nullptr;
    }

   
    void createInventory_tv() {
        root = nullptr;
        cout << "Product inventory management system created successfully!" << endl;
    }

    void insertProduct_tv(int code, const char* name, float price, int quantity, Date_tv received, Date_tv expiry) {
        root = insertProductHelper_tv(root, code, name, price, quantity, received, expiry);
        cout << "Product '" << name << "' (Code: " << code << ") inserted successfully!" << endl;
    }

    void displayAllItems_tv() {
        if (root == nullptr) {
            cout << "Inventory is empty!" << endl;
            return;
        }
        
        cout << "\n=== Complete Inventory (Sorted by Product Name) ===" << endl;
        cout << "Code\tName\t\tPrice\t\tQuantity\tReceived\tExpiry" << endl;
        cout << "------------------------------------------------------------------------" << endl;
        inorderHelper_tv(root);
        cout << "===================================================" << endl;
    }

    void listExpiredItems_tv(Date_tv currentDate) {
        if (root == nullptr) {
            cout << "Inventory is empty!" << endl;
            return;
        }
        
        cout << "\n=== Expired Items (Preorder Traversal) ===" << endl;
        cout << "Code\tName\t\tPrice\t\tQuantity\tReceived\tExpiry" << endl;
        cout << "------------------------------------------------------------------------" << endl;
        listExpiredItemsHelper_tv(root, currentDate);
        cout << "=========================================" << endl;
    }


    void listExpiredItemsHelper_tv(Product_tv* node, Date_tv currentDate) {
        if (node != nullptr) {
            if (isExpired_tv(node->expirationDate, currentDate)) {
                cout << node->productCode << "\t" << node->productName << "\t\t" << node->price << "\t\t" 
                     << node->quantity << "\t\t" << node->dateReceived.day << "/" << node->dateReceived.month << "/" << node->dateReceived.year 
                     << "\t" << node->expirationDate.day << "/" << node->expirationDate.month << "/" << node->expirationDate.year << endl;
            }
            listExpiredItemsHelper_tv(node->left, currentDate);
            listExpiredItemsHelper_tv(node->right, currentDate);
        }
    }


    void searchProduct_tv(const char* name) {
        if (root == nullptr) {
            cout << "Inventory is empty!" << endl;
            return;
        }
        
        Product_tv* product = searchProductHelper_tv(root, name);
        if (product != nullptr) {
            cout << "\n=== Product Found ===" << endl;
            cout << "Product Code: " << product->productCode << endl;
            cout << "Product Name: " << product->productName << endl;
            cout << "Price: " << product->price << endl;
            cout << "Quantity: " << product->quantity << endl;
            cout << "Date Received: " << product->dateReceived.day << "/" << product->dateReceived.month << "/" << product->dateReceived.year << endl;
            cout << "Expiration Date: " << product->expirationDate.day << "/" << product->expirationDate.month << "/" << product->expirationDate.year << endl;
            cout << "====================" << endl;
        } else {
            cout << "Product '" << name << "' not found in inventory!" << endl;
        }
    }


    void deleteProduct_tv(const char* name) {
        Product_tv* product = searchProductHelper_tv(root, name);
        if (product != nullptr) {
            root = deleteProductHelper_tv(root, name);
            cout << "Product '" << name << "' deleted successfully!" << endl;
        } else {
            cout << "Product '" << name << "' not found in inventory!" << endl;
        }
    }


    void updateQuantity_tv(const char* name, int newQuantity) {
        Product_tv* product = searchProductHelper_tv(root, name);
        if (product != nullptr) {
            product->quantity = newQuantity;
            cout << "Quantity of '" << name << "' updated to " << newQuantity << endl;
        } else {
            cout << "Product '" << name << "' not found in inventory!" << endl;
        }
    }


    int countProducts_tv() {
        int count = countProductsHelper_tv(root);
        cout << "Total products in inventory: " << count << endl;
        return count;
    }


    void displayLowStockItems_tv(int threshold) {
        if (root == nullptr) {
            cout << "Inventory is empty!" << endl;
            return;
        }
        
        cout << "\n=== Low Stock Items (Quantity < " << threshold << ") ===" << endl;
        cout << "Code\tName\t\tQuantity" << endl;
        cout << "------------------------" << endl;
        displayLowStockHelper_tv(root, threshold);
        cout << "========================" << endl;
    }

    void inputProducts_tv() {
        int numProducts, code, quantity;
        char name[50];
        float price;
        Date_tv received, expiry;
        
        cout << "Enter number of products to add: ";
        cin >> numProducts;
        
        for (int i = 0; i < numProducts; i++) {
            cout << "\nProduct " << (i+1) << ":" << endl;
            cout << "Enter product code: ";
            cin >> code;
            cout << "Enter product name: ";
            cin >> name;
            cout << "Enter price: ";
            cin >> price;
            cout << "Enter quantity: ";
            cin >> quantity;
            cout << "Enter date received (DD MM YYYY): ";
            cin >> received.day >> received.month >> received.year;
            cout << "Enter expiration date (DD MM YYYY): ";
            cin >> expiry.day >> expiry.month >> expiry.year;
            insertProduct_tv(code, name, price, quantity, received, expiry);
        }
    }

    ~ProductInventory_tv() {
  
        root = nullptr;
    }
};

int main() {
    ProductInventory_tv inventory_tv;
    int choice, code, quantity, threshold;
    char name[50];
    float price;
    Date_tv received, expiry, currentDate;

    cout << "=== Product Inventory Management System ===" << endl;
    cout << "Using Search Tree Data Structure" << endl;
    cout << "Organized by Product Name" << endl;

    while (true) {
        cout << "\n1. Create Inventory\n2. Insert Product\n3. Input Multiple Products\n4. Display All Items\n5. List Expired Items\n6. Search Product\n7. Delete Product\n8. Update Quantity\n9. Display Low Stock Items\n10. Count Products\n11. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                inventory_tv.createInventory_tv();
                break;

            case 2:
                cout << "Enter product code: ";
                cin >> code;
                cout << "Enter product name: ";
                cin >> name;
                cout << "Enter price: ";
                cin >> price;
                cout << "Enter quantity: ";
                cin >> quantity;
                cout << "Enter date received (DD MM YYYY): ";
                cin >> received.day >> received.month >> received.year;
                cout << "Enter expiration date (DD MM YYYY): ";
                cin >> expiry.day >> expiry.month >> expiry.year;
                inventory_tv.insertProduct_tv(code, name, price, quantity, received, expiry);
                break;

            case 3:
                inventory_tv.inputProducts_tv();
                break;

            case 4:
                inventory_tv.displayAllItems_tv();
                break;

            case 5:
                cout << "Enter current date (DD MM YYYY): ";
                cin >> currentDate.day >> currentDate.month >> currentDate.year;
                inventory_tv.listExpiredItems_tv(currentDate);
                break;

            case 6:
                cout << "Enter product name to search: ";
                cin >> name;
                inventory_tv.searchProduct_tv(name);
                break;

            case 7:
                cout << "Enter product name to delete: ";
                cin >> name;
                inventory_tv.deleteProduct_tv(name);
                break;

            case 8:
                cout << "Enter product name to update: ";
                cin >> name;
                cout << "Enter new quantity: ";
                cin >> quantity;
                inventory_tv.updateQuantity_tv(name, quantity);
                break;

            case 9:
                cout << "Enter stock threshold: ";
                cin >> threshold;
                inventory_tv.displayLowStockItems_tv(threshold);
                break;

            case 10:
                inventory_tv.countProducts_tv();
                break;

            case 11:
                cout << "Thank you for using the Product Inventory Management System!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}

## Output

=== Product Inventory Management System ===
Using Search Tree Data Structure
Organized by Product Name

1. Create Inventory
2. Insert Product
3. Input Multiple Products
4. Display All Items
5. List Expired Items
6. Search Product
7. Delete Product
8. Update Quantity
9. Display Low Stock Items
10. Count Products
11. Exit
Enter your choice: 1
Product inventory management system created successfully!

1. Create Inventory
2. Insert Product
3. Input Multiple Products
4. Display All Items
5. List Expired Items
6. Search Product
7. Delete Product
8. Update Quantity
9. Display Low Stock Items
10. Count Products
11. Exit
Enter your choice: 2
Enter product code: 101
Enter product name: Milk
Enter price: 25.50
Enter quantity: 50
Enter date received (DD MM YYYY): 1 11 2023
Enter expiration date (DD MM YYYY): 1 12 2023
Product 'Milk' (Code: 101) inserted successfully!

1. Create Inventory
2. Insert Product
3. Input Multiple Products
4. Display All Items
5. List Expired Items
6. Search Product
7. Delete Product
8. Update Quantity
9. Display Low Stock Items
10. Count Products
11. Exit
Enter your choice: 2
Enter product code: 102
Enter product name: Bread
Enter price: 30.00
Enter quantity: 30
Enter date received (DD MM YYYY): 5 11 2023
Enter expiration date (DD MM YYYY): 10 11 2023
Product 'Bread' (Code: 102) inserted successfully!

1. Create Inventory
2. Insert Product
3. Input Multiple Products
4. Display All Items
5. List Expired Items
6. Search Product
7. Delete Product
8. Update Quantity
9. Display Low Stock Items
10. Count Products
11. Exit
Enter your choice: 2
Enter product code: 103
Enter product name: Cheese
Enter price: 80.75
Enter quantity: 20
Enter date received (DD MM YYYY): 3 11 2023
Enter expiration date (DD MM YYYY): 15 12 2023
Product 'Cheese' (Code: 103) inserted successfully!

1. Create Inventory
2. Insert Product
3. Input Multiple Products
4. Display All Items
5. List Expired Items
6. Search Product
7. Delete Product
8. Update Quantity
9. Display Low Stock Items
10. Count Products
11. Exit
Enter your choice: 2
Enter product code: 104
Enter product name: Apple
Enter price: 150.00
Enter quantity: 100
Enter date received (DD MM YYYY): 1 11 2023
Enter expiration date (DD MM YYYY): 20 11 2023
Product 'Apple' (Code: 104) inserted successfully!

1. Create Inventory
2. Insert Product
3. Input Multiple Products
4. Display All Items
5. List Expired Items
6. Search Product
7. Delete Product
8. Update Quantity
9. Display Low Stock Items
10. Count Products
11. Exit
Enter your choice: 2
Enter product code: 105
Enter product name: Banana
Enter price: 60.25
Enter quantity: 80
Enter date received (DD MM YYYY): 2 11 2023
Enter expiration date (DD MM YYYY): 25 11 2023
Product 'Banana' (Code: 105) inserted successfully!

1. Create Inventory
2. Insert Product
3. Input Multiple Products
4. Display All Items
5. List Expired Items
6. Search Product
7. Delete Product
8. Update Quantity
9. Display Low Stock Items
10. Count Products
11. Exit
Enter your choice: 4

=== Complete Inventory (Sorted by Product Name) ===
Code	Name		Price		Quantity	Received	Expiry
------------------------------------------------------------------------
104	Apple		150		100		1/11/2023	20/11/2023
105	Banana		60.25		80		2/11/2023	25/11/2023
102	Bread		30		30		5/11/2023	10/11/2023
103	Cheese		80.75		20		3/11/2023	15/12/2023
101	Milk		25.5		50		1/11/2023	1/12/2023
===================================================

1. Create Inventory
2. Insert Product
3. Input Multiple Products
4. Display All Items
5. List Expired Items
6. Search Product
7. Delete Product
8. Update Quantity
9. Display Low Stock Items
10. Count Products
11. Exit
Enter your choice: 5
Enter current date (DD MM YYYY): 15 11 2023

=== Expired Items (Preorder Traversal) ===
Code	Name		Price		Quantity	Received	Expiry
------------------------------------------------------------------------
102	Bread		30		30		5/11/2023	10/11/2023
104	Apple		150		100		1/11/2023	20/11/2023
=========================================

1. Create Inventory
2. Insert Product
3. Input Multiple Products
4. Display All Items
5. List Expired Items
6. Search Product
7. Delete Product
8. Update Quantity
9. Display Low Stock Items
10. Count Products
11. Exit
Enter your choice: 6
Enter product name to search: Cheese

=== Product Found ===
Product Code: 103
Product Name: Cheese
Price: 80.75
Quantity: 20
Date Received: 3/11/2023
Expiration Date: 15/12/2023
====================

1. Create Inventory
2. Insert Product
3. Input Multiple Products
4. Display All Items
5. List Expired Items
6. Search Product
7. Delete Product
8. Update Quantity
9. Display Low Stock Items
10. Count Products
11. Exit
Enter your choice: 9
Enter stock threshold: 25

=== Low Stock Items (Quantity < 25) ===
Code	Name		Quantity
------------------------
103	Cheese		20
========================

1. Create Inventory
2. Insert Product
3. Input Multiple Products
4. Display All Items
5. List Expired Items
6. Search Product
7. Delete Product
8. Update Quantity
9. Display Low Stock Items
10. Count Products
11. Exit
Enter your choice: 8
Enter product name to update: Milk
Enter new quantity: 10
Quantity of 'Milk' updated to 10

1. Create Inventory
2. Insert Product
3. Input Multiple Products
4. Display All Items
5. List Expired Items
6. Search Product
7. Delete Product
8. Update Quantity
9. Display Low Stock Items
10. Count Products
11. Exit
Enter your choice: 10
Total products in inventory: 5

1. Create Inventory
2. Insert Product
3. Input Multiple Products
4. Display All Items
5. List Expired Items
6. Search Product
7. Delete Product
8. Update Quantity
9. Display Low Stock Items
10. Count Products
11. Exit
Enter your choice: 7
Enter product name to delete: Bread
Product 'Bread' deleted successfully!

1. Create Inventory
2. Insert Product
3. Input Multiple Products
4. Display All Items
5. List Expired Items
6. Search Product
7. Delete Product
8. Update Quantity
9. Display Low Stock Items
10. Count Products
11. Exit
Enter your choice: 4

=== Complete Inventory (Sorted by Product Name) ===
Code	Name		Price		Quantity	Received	Expiry
------------------------------------------------------------------------
104	Apple		150		100		1/11/2023	20/11/2023
105	Banana		60.25		80		2/11/2023	25/11/2023
103	Cheese		80.75		20		3/11/2023	15/12/2023
101	Milk		25.5		10		1/11/2023	1/12/2023
===================================================

