## Problem Statement
Write a program to implement deletion operations in the product inventory system using a search tree. Each product must store the following information: Unique Product Code, Product Name, Price, Quantity in Stock, Date Received, Expiration Date. Implement the following operations: Delete a product using its unique product code. Delete all expired products based on the current date.

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

class ProductInventoryDeletion_tv {
private:
    Product_tv* root;

    Product_tv* insertProductHelper_tv(Product_tv* node, int code, const char* name, float price, int quantity, Date_tv received, Date_tv expiry) {
        if (node == nullptr) {
            return new Product_tv(code, name, price, quantity, received, expiry);
        }
        

        if (code < node->productCode) {
            node->left = insertProductHelper_tv(node->left, code, name, price, quantity, received, expiry);
        } else if (code > node->productCode) {
            node->right = insertProductHelper_tv(node->right, code, name, price, quantity, received, expiry);
        }
        
        
        return node;
    }

   
    Product_tv* searchProductByCodeHelper_tv(Product_tv* node, int code) {
        if (node == nullptr || node->productCode == code) {
            return node;
        }
        
        if (code < node->productCode) {
            return searchProductByCodeHelper_tv(node->left, code);
        } else {
            return searchProductByCodeHelper_tv(node->right, code);
        }
    }

    Product_tv* searchProductByNameHelper_tv(Product_tv* node, const char* name) {
        if (node == nullptr) {
            return nullptr;
        }
        
        if (strcmp(node->productName, name) == 0) {
            return node;
        }
        
        Product_tv* found = searchProductByNameHelper_tv(node->left, name);
        if (found != nullptr) {
            return found;
        }
        
        return searchProductByNameHelper_tv(node->right, name);
    }

    Product_tv* deleteProductByCodeHelper_tv(Product_tv* node, int code) {
        if (node == nullptr) {
            return node;
        }
        
        if (code < node->productCode) {
            node->left = deleteProductByCodeHelper_tv(node->left, code);
        } else if (code > node->productCode) {
            node->right = deleteProductByCodeHelper_tv(node->right, code);
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
                node->right = deleteProductByCodeHelper_tv(node->right, temp->productCode);
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

    void inorderHelper_tv(Product_tv* node) {
        if (node != nullptr) {
            inorderHelper_tv(node->left);
            cout << node->productCode << "\t" << node->productName << "\t\t" << node->price << "\t\t" 
                 << node->quantity << "\t\t" << node->dateReceived.day << "/" << node->dateReceived.month << "/" << node->dateReceived.year 
                 << "\t" << node->expirationDate.day << "/" << node->expirationDate.month << "/" << node->expirationDate.year << endl;
            inorderHelper_tv(node->right);
        }
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

    Product_tv* deleteExpiredProductsHelper_tv(Product_tv* node, Date_tv currentDate, int& deletedCount) {
        if (node == nullptr) {
            return nullptr;
        }
      
        node->left = deleteExpiredProductsHelper_tv(node->left, currentDate, deletedCount);
        node->right = deleteExpiredProductsHelper_tv(node->right, currentDate, deletedCount);
       
        if (isExpired_tv(node->expirationDate, currentDate)) {
           
            Product_tv* result;
            
            if (node->left == nullptr && node->right == nullptr) {
                cout << "Deleting expired product: " << node->productName << " (Code: " << node->productCode << ")" << endl;
                deletedCount++;
                delete node;
                return nullptr;
            }
            
            else if (node->left == nullptr) {
                result = node->right;
                cout << "Deleting expired product: " << node->productName << " (Code: " << node->productCode << ")" << endl;
                deletedCount++;
                delete node;
                return result;
            } else if (node->right == nullptr) {
                result = node->left;
                cout << "Deleting expired product: " << node->productName << " (Code: " << node->productCode << ")" << endl;
                deletedCount++;
                delete node;
                return result;
            }
           
            else {
                
                Product_tv* temp = findMin_tv(node->right);
                node->productCode = temp->productCode;
                strcpy(node->productName, temp->productName);
                node->price = temp->price;
                node->quantity = temp->quantity;
                node->dateReceived = temp->dateReceived;
                node->expirationDate = temp->expirationDate;
                node->right = deleteProductByCodeHelper_tv(node->right, temp->productCode);
                return node;
            }
        }
        
        return node;
    }

   
    void displayExpiredProductsHelper_tv(Product_tv* node, Date_tv currentDate) {
        if (node != nullptr) {
            displayExpiredProductsHelper_tv(node->left, currentDate);
            if (isExpired_tv(node->expirationDate, currentDate)) {
                cout << node->productCode << "\t" << node->productName << "\t\t" << node->expirationDate.day << "/" 
                     << node->expirationDate.month << "/" << node->expirationDate.year << endl;
            }
            displayExpiredProductsHelper_tv(node->right, currentDate);
        }
    }

public:
    ProductInventoryDeletion_tv() {
        root = nullptr;
    }

 
    void createInventory_tv() {
        root = nullptr;
        cout << "Product inventory deletion system created successfully!" << endl;
    }


    void insertProduct_tv(int code, const char* name, float price, int quantity, Date_tv received, Date_tv expiry) {
        root = insertProductHelper_tv(root, code, name, price, quantity, received, expiry);
        cout << "Product '" << name << "' (Code: " << code << ") inserted successfully!" << endl;
    }


    void deleteProductByCode_tv(int code) {
        Product_tv* product = searchProductByCodeHelper_tv(root, code);
        if (product != nullptr) {
            root = deleteProductByCodeHelper_tv(root, code);
            cout << "Product '" << product->productName << "' (Code: " << code << ") deleted successfully!" << endl;
        } else {
            cout << "Product with code " << code << " not found in inventory!" << endl;
        }
    }


    void deleteAllExpiredProducts_tv(Date_tv currentDate) {
        if (root == nullptr) {
            cout << "Inventory is empty!" << endl;
            return;
        }
        
        cout << "\n=== Deleting Expired Products ===" << endl;
        int deletedCount = 0;
        root = deleteExpiredProductsHelper_tv(root, currentDate, deletedCount);
        
        if (deletedCount > 0) {
            cout << "Total expired products deleted: " << deletedCount << endl;
        } else {
            cout << "No expired products found!" << endl;
        }
        cout << "=================================" << endl;
    }

    
    void displayAllProducts_tv() {
        if (root == nullptr) {
            cout << "Inventory is empty!" << endl;
            return;
        }
        
        cout << "\n=== Complete Inventory ===" << endl;
        cout << "Code\tName\t\tPrice\t\tQuantity\tReceived\tExpiry" << endl;
        cout << "------------------------------------------------------------------------" << endl;
        inorderHelper_tv(root);
        cout << "=========================" << endl;
    }

    
    void displayExpiredProducts_tv(Date_tv currentDate) {
        if (root == nullptr) {
            cout << "Inventory is empty!" << endl;
            return;
        }
        
        cout << "\n=== Expired Products ===" << endl;
        cout << "Code\tName\t\tExpiry Date" << endl;
        cout << "--------------------------------" << endl;
        displayExpiredProductsHelper_tv(root, currentDate);
        cout << "========================" << endl;
    }

    
    void searchProductByCode_tv(int code) {
        if (root == nullptr) {
            cout << "Inventory is empty!" << endl;
            return;
        }
        
        Product_tv* product = searchProductByCodeHelper_tv(root, code);
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
            cout << "Product with code " << code << " not found in inventory!" << endl;
        }
    }

  
    void searchProductByName_tv(const char* name) {
        if (root == nullptr) {
            cout << "Inventory is empty!" << endl;
            return;
        }
        
        Product_tv* product = searchProductByNameHelper_tv(root, name);
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

    int countProducts_tv() {
        int count = countProductsHelper_tv(root);
        cout << "Total products in inventory: " << count << endl;
        return count;
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

    ~ProductInventoryDeletion_tv() {
      
        root = nullptr;
    }
};

int main() {
    ProductInventoryDeletion_tv inventory_tv;
    int choice, code, quantity, numProducts;
    char name[50];
    float price;
    Date_tv received, expiry, currentDate;

    cout << "=== Product Inventory Deletion Operations ===" << endl;
    cout << "Using Search Tree Data Structure" << endl;
    cout << "Delete by Product Code and Delete Expired Products" << endl;

    while (true) {
        cout << "\n1. Create Inventory\n2. Insert Product\n3. Input Multiple Products\n4. Delete Product by Code\n5. Delete All Expired Products\n6. Display All Products\n7. Display Expired Products\n8. Search Product by Code\n9. Search Product by Name\n10. Count Products\n11. Exit\n";
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
                cout << "Enter product code to delete: ";
                cin >> code;
                inventory_tv.deleteProductByCode_tv(code);
                break;

            case 5:
                cout << "Enter current date (DD MM YYYY): ";
                cin >> currentDate.day >> currentDate.month >> currentDate.year;
                inventory_tv.deleteAllExpiredProducts_tv(currentDate);
                break;

            case 6:
                inventory_tv.displayAllProducts_tv();
                break;

            case 7:
                cout << "Enter current date (DD MM YYYY): ";
                cin >> currentDate.day >> currentDate.month >> currentDate.year;
                inventory_tv.displayExpiredProducts_tv(currentDate);
                break;

            case 8:
                cout << "Enter product code to search: ";
                cin >> code;
                inventory_tv.searchProductByCode_tv(code);
                break;

            case 9:
                cout << "Enter product name to search: ";
                cin >> name;
                inventory_tv.searchProductByName_tv(name);
                break;

            case 10:
                inventory_tv.countProducts_tv();
                break;

            case 11:
                cout << "Thank you for using the Product Inventory Deletion System!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


## Output

=== Product Inventory Deletion Operations ===
Using Search Tree Data Structure
Delete by Product Code and Delete Expired Products

1. Create Inventory
2. Insert Product
3. Input Multiple Products
4. Delete Product by Code
5. Delete All Expired Products
6. Display All Products
7. Display Expired Products
8. Search Product by Code
9. Search Product by Name
10. Count Products
11. Exit
Enter your choice: 1
Product inventory deletion system created successfully!

1. Create Inventory
2. Insert Product
3. Input Multiple Products
4. Delete Product by Code
5. Delete All Expired Products
6. Display All Products
7. Display Expired Products
8. Search Product by Code
9. Search Product by Name
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
4. Delete Product by Code
5. Delete All Expired Products
6. Display All Products
7. Display Expired Products
8. Search Product by Code
9. Search Product by Name
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
4. Delete Product by Code
5. Delete All Expired Products
6. Display All Products
7. Display Expired Products
8. Search Product by Code
9. Search Product by Name
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
4. Delete Product by Code
5. Delete All Expired Products
6. Display All Products
7. Display Expired Products
8. Search Product by Code
9. Search Product by Name
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
4. Delete Product by Code
5. Delete All Expired Products
6. Display All Products
7. Display Expired Products
8. Search Product by Code
9. Search Product by Name
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
4. Delete Product by Code
5. Delete All Expired Products
6. Display All Products
7. Display Expired Products
8. Search Product by Code
9. Search Product by Name
10. Count Products
11. Exit
Enter your choice: 6

=== Complete Inventory ===
Code	Name		Price		Quantity	Received	Expiry
------------------------------------------------------------------------
101	Milk		25.5		50		1/11/2023	1/12/2023
102	Bread		30		30		5/11/2023	10/11/2023
103	Cheese		80.75		20		3/11/2023	15/12/2023
104	Apple		150		100		1/11/2023	20/11/2023
105	Banana		60.25		80		2/11/2023	25/11/2023
=========================

1. Create Inventory
2. Insert Product
3. Input Multiple Products
4. Delete Product by Code
5. Delete All Expired Products
6. Display All Products
7. Display Expired Products
8. Search Product by Code
9. Search Product by Name
10. Count Products
11. Exit
Enter your choice: 7
Enter current date (DD MM YYYY): 15 11 2023

=== Expired Products ===
Code	Name		Expiry Date
--------------------------------
102	Bread		10/11/2023
104	Apple		20/11/2023
========================

1. Create Inventory
2. Insert Product
3. Input Multiple Products
4. Delete Product by Code
5. Delete All Expired Products
6. Display All Products
7. Display Expired Products
8. Search Product by Code
9. Search Product by Name
10. Count Products
11. Exit
Enter your choice: 4
Enter product code to delete: 103
Product 'Cheese' (Code: 103) deleted successfully!

1. Create Inventory
2. Insert Product
3. Input Multiple Products
4. Delete Product by Code
5. Delete All Expired Products
6. Display All Products
7. Display Expired Products
8. Search Product by Code
9. Search Product by Name
10. Count Products
11. Exit
Enter your choice: 6

=== Complete Inventory ===
Code	Name		Price		Quantity	Received	Expiry
------------------------------------------------------------------------
101	Milk		25.5		50		1/11/2023	1/12/2023
102	Bread		30		30		5/11/2023	10/11/2023
104	Apple		150		100		1/11/2023	20/11/2023
105	Banana		60.25		80		2/11/2023	25/11/2023
=========================

1. Create Inventory
2. Insert Product
3. Input Multiple Products
4. Delete Product by Code
5. Delete All Expired Products
6. Display All Products
7. Display Expired Products
8. Search Product by Code
9. Search Product by Name
10. Count Products
11. Exit
Enter your choice: 5
Enter current date (DD MM YYYY): 15 11 2023

=== Deleting Expired Products ===
Deleting expired product: Bread (Code: 102)
Deleting expired product: Apple (Code: 104)
Total expired products deleted: 2
=================================

1. Create Inventory
2. Insert Product
3. Input Multiple Products
4. Delete Product by Code
5. Delete All Expired Products
6. Display All Products
7. Display Expired Products
8. Search Product by Code
9. Search Product by Name
10. Count Products
11. Exit
Enter your choice: 6

=== Complete Inventory ===
Code	Name		Price		Quantity	Received	Expiry
------------------------------------------------------------------------
101	Milk		25.5		50		1/11/2023	1/12/2023
105	Banana		60.25		80		2/11/2023	25/11/2023
=========================

1. Create Inventory
2. Insert Product
3. Input Multiple Products
4. Delete Product by Code
5. Delete All Expired Products
6. Display All Products
7. Display Expired Products
8. Search Product by Code
9. Search Product by Name
10. Count Products
11. Exit
Enter your choice: 8
Enter product code to search: 101

=== Product Found ===
Product Code: 101
Product Name: Milk
Price: 25.5
Quantity: 50
Date Received: 1/11/2023
Expiration Date: 1/12/2023
====================

1. Create Inventory
2. Insert Product
3. Input Multiple Products
4. Delete Product by Code
5. Delete All Expired Products
6. Display All Products
7. Display Expired Products
8. Search Product by Code
9. Search Product by Name
10. Count Products
11. Exit
Enter your choice: 10
Total products in inventory: 2

