# Inventory-Management-System

Description:
This project involves designing a simple inventory management system to track stock levels, product details, and operations like adding, updating, or removing items. The system provides functionality for managing inventory data efficiently and is particularly useful for small businesses or educational purposes. It is implemented using Python and can integrate databases like SQLite for storing product information

Key Features:

Product Management: Add, edit, or delete product details, including ID, name, and quantity.
Stock Updates: Record inventory changes in real time.
Search Functionality: Retrieve product information quickly using unique identifiers.
Data Persistence: Store and manage data using an SQLite database for reliability.

Key Features in Code
1.CRUD Operations:

Create: Add new products.
Read: View all inventory data.
Update: Modify product quantities.
Delete: Remove products from inventory.

2.Database Persistence:
Uses SQLite for reliable data storage.

3.User-Friendly Interface:
Command-line-based menu system.

import sqlite3

# Connect to SQLite database (or create it if it doesn't exist)
conn = sqlite3.connect("inventory.db")
cursor = conn.cursor()

# Create the products table if it doesn't exist
cursor.execute("""
CREATE TABLE IF NOT EXISTS products (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    quantity INTEGER NOT NULL CHECK (quantity >= 0)
)
""")
conn.commit()

# Function to add a new product
def add_product(name, quantity):
    cursor.execute("INSERT INTO products (name, quantity) VALUES (?, ?)", (name, quantity))
    conn.commit()
    print(f"Product '{name}' added with quantity {quantity}.")

# Function to update the quantity of a product
def update_product_quantity(product_id, new_quantity):
    cursor.execute("UPDATE products SET quantity = ? WHERE id = ?", (new_quantity, product_id))
    conn.commit()
    print(f"Product ID {product_id} quantity updated to {new_quantity}.")

# Function to delete a product
def delete_product(product_id):
    cursor.execute("DELETE FROM products WHERE id = ?", (product_id,))
    conn.commit()
    print(f"Product ID {product_id} deleted from inventory.")

# Function to view all products
def view_inventory():
    cursor.execute("SELECT * FROM products")
    products = cursor.fetchall()
    print("\nInventory:")
    if not products:
        print("No products in inventory.")
    else:
        for product in products:
            print(f"ID: {product[0]}, Name: {product[1]}, Quantity: {product[2]}")
    print()

# Function to validate integer input
def get_valid_integer(prompt):
    while True:
        try:
            value = int(input(prompt))
            if value < 0:
                raise ValueError("Value must be non-negative.")
            return value
        except ValueError as e:
            print(f"Invalid input: {e}. Please try again.")

# Main menu for the inventory system
def main_menu():
    while True:
        print("\nInventory Management System")
        print("1. Add Product")
        print("2. Update Product Quantity")
        print("3. Delete Product")
        print("4. View Inventory")
        print("5. Exit")
        choice = input("Enter your choice: ")

        if choice == '1':
            name = input("Enter product name: ")
            quantity = get_valid_integer("Enter product quantity: ")
            add_product(name, quantity)
        elif choice == '2':
            product_id = get_valid_integer("Enter product ID to update: ")
            new_quantity = get_valid_integer("Enter new quantity: ")
            update_product_quantity(product_id, new_quantity)
        elif choice == '3':
            product_id = get_valid_integer("Enter product ID to delete: ")
            delete_product(product_id)
        elif choice == '4':
            view_inventory()
        elif choice == '5':
            print("Exiting Inventory Management System.")
            break
        else:
            print("Invalid choice. Please try again.")

# Run the inventory system
if __name__ == "__main__":
    try:
        main_menu()
    except KeyboardInterrupt:
        print("\nProgram interrupted by user. Exiting gracefully...")
    finally:
        # Close the database connection when done
        conn.close()

![image](https://github.com/user-attachments/assets/090dfbc2-78be-4335-bad5-f66d51a47259)
![image](https://github.com/user-attachments/assets/8fcc230c-fd81-4a85-a215-87705c2c6f17)

