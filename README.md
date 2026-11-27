# Online Shopping System (Group 51)

A menu-driven C++ console application that manages products, customers, orders, and administrators using **object-oriented programming**, **custom linked lists**,**Data Strucure** and **file handling**.

**Group focus:** Order status and delivery tracking.

> Replace every `[bracketed placeholder]` with your own details before submitting.

---

## Table of Contents

1. [Features](#features)
2. [Group-Specific Features](#group-specific-features)
3. [Project Structure](#project-structure)
4. [Design Overview](#design-overview)
5. [Data Files](#data-files)
6. [Build and Run](#build-and-run)
7. [Usage Guide](#usage-guide)
8. [Initial Test Data](#initial-test-data)
9. [Test Cases](#test-cases)
10. [Demonstration Checklist](#demonstration-checklist)
11. [Team and Contributions](#team-and-contributions)

---

## Features

### Customer
| # | Operation | Description |
|---|-----------|-------------|
| 1 | Sign in | Existing customers log in with credentials |
| 2 | Register | New customers create an account; a customer ID is assigned automatically |
| 3 | Display products | Shows only products with stock greater than zero |
| 4 | Buy a product | Enter product ID and quantity; stock decreases accordingly |
| 5 | Return a product | Return a previously purchased item; stock increases accordingly |
| 6 | Display receipt | Shows receipts for the customer's own orders |
| 7 | Log out | Returns to the main menu |

### Admin
| # | Operation | Description |
|---|-----------|-------------|
| 8 | Sign in | Authorized administrators only |
| 9 | Display products | Shows all products, including zero-stock items |
| 10 | Adjust product | Modify product information by product ID |
| 11 | Delete product | Remove a product by product ID |
| 12 | Add product | Add a product; a product ID is assigned automatically |
| 13 | Low-stock report | Lists products with stock below an admin-specified quantity |
| 14 | Customer order report | Shows the number of orders made by a given customer ID |
| 15 | Log out | Returns to the main menu |

### Validation
- Product IDs, quantities, and login credentials are validated.
- Customers cannot buy more than the available stock.
- Returns must correspond to a previous purchase and cannot exceed the quantity originally purchased.
- Clear success and error messages are shown for every operation.

---

## Group-Specific Features

1. **Order status.** Every order is assigned a status: `placed` → `processing` → `shipped` → `delivered`.
2. **Tracking reference.** Each order stores a simple tracking reference (e.g. `TRK-0001`), and the customer can view the order status and tracking reference.

Additional attributes on the `Orders` class:

| Attribute | Type | Purpose |
|-----------|------|---------|
| `status` | `string` / enum | Current order status (`placed`, `processing`, `shipped`, `delivered`) |
| `trackingRef` | `string` | Simple tracking reference generated when the order is placed |

---

## Project Structure

```
.
├── README.md
├── src/
│   ├── main.cpp
│   ├── Product.h / Product.cpp
│   ├── Customer.h / Customer.cpp
│   ├── Orders.h / Orders.cpp
│   ├── Admin.h / Admin.cpp
│   └── [LinkedList classes / FileManager / etc.]
├── data/
│   ├── products.txt
│   ├── customers.txt
│   ├── orders.txt
│   └── admins.txt
├── docs/
│   ├── class_diagram.[png|pdf]
│   └── test_evidence/
└── [Makefile / CMakeLists.txt]
```

> Adjust the tree to match your actual files.

---

## Design Overview

### Classes
- **Product**: ID, name, price, stock quantity.
- **Customer**: ID, name, password, plus a link to their orders.
- **Orders**: order ID, customer ID, product ID, quantity, total, `status`, `trackingRef`, returned quantity.
- **Admin**: admin ID, username, password.
- **Linked-list managers**: one per entity (`ProductList`, `CustomerList`, `OrderList`, `AdminList`) supporting add, display, search, modify, and delete.

### Linked lists
Every node holds a data object and a `next` pointer. No STL containers (`vector`, `list`, etc.) are used in place of the linked lists. Typical operations:

- **Insert**: create a new node and append it to the tail (or insert at head).
- **Search**: traverse from `head` until the ID matches or `nullptr` is reached.
- **Delete**: relink the previous node's `next` to skip the target node, then free it.

### File handling
All data is loaded into the linked lists at startup and written back to the files on exit or after each change, so records persist between runs. Storage uses **no more than four data files**.

### Class relationships
`Customer` 1 — * `Orders` * — 1 `Product`. `Admin` manages `Product` records. See `docs/class_diagram` for the full diagram.

---

## Data Files

| File | Contents |
|------|----------|
| `products.txt` | Product ID, name, price, stock |
| `customers.txt` | Customer ID, name, password |
| `orders.txt` | Order ID, customer ID, product ID, quantity, status, tracking reference, returned quantity |
| `admins.txt` | Admin ID, username, password |

> Update the formats above to match your implementation.

---

## Build and Run

**Requirements:** a C++11 (or later) compiler such as `g++` or `clang++`.

```bash
# Compile
g++ -std=c++17 -Wall -o shop src/*.cpp

# Run (from the repository root so the data/ folder is found)
./shop
```

On Windows (MinGW):

```bash
g++ -std=c++17 -Wall -o shop.exe src/*.cpp
shop.exe
```

> Add your own build instructions if you use a Makefile, CMake, or an IDE project.

---

## Usage Guide

1. Start the program and choose **Customer** or **Admin** from the main menu.
2. **Customers:** register or sign in, browse products, buy, return, and view receipts and order status/tracking.
3. **Admins:** sign in with the default credentials below to manage products and run reports.
4. Choose **Log out** to return to the main menu, and **Exit** to save and close.

**Default admin (test only):** `[username]` / `[password]`

All names and passwords in this project are fictional test data.

---

## Initial Test Data

### Products

| Product ID | Name | Price (EGP) | Stock |
|-----------|------|-------------|-------|
| P5101 | Laptop bag | 13 | 12 |
| P5102 | Water bottle | 16 | 19 |
| P5103 | Desk lamp | 19 | 26 |
| P5104 | Wireless mouse | 22 | 9 |
| P5105 | Notebook | 25 | 16 |
| P5106 | Headphones | 28 | 23 |
| P5107 | Backpack | 31 | 6 |
| P5108 | Phone stand | 34 | 13 |
| P5109 | USB hub | 37 | 20 |
| P5110 | Travel mug | 40 | 27 |
| P5111 | Desk organizer | 43 | 10 |
| P5112 | Keyboard | 46 | 17 |

### Customers

| Customer ID | Name | Password |
|------------|------|----------|
| C5101 | Customer One | `[test password]` |
| C5102 | Customer Two | `[test password]` |
| C5103 | Customer Three | `[test password]` |

### Test orders (minimum five, across at least two customers)

| Order | Customer | Product | Qty | Scenario | Expected result |
|-------|----------|---------|-----|----------|-----------------|
| 1 | C5101 | P5101 | `[ ]` | Valid purchase | Success, stock decreases |
| 2 | C5101 | P5104 | `[ ]` | Valid purchase | Success, stock decreases |
| 3 | C5102 | P5107 | `[ > 6 ]` | Exceeds stock | Rejected with validation message |
| 4 | C5102 | P5105 | `[ ]` | Valid purchase | Success, stock decreases |
| 5 | C5101 | P5101 | `[ ]` | Valid return | Success, stock increases |

> Fill in quantities and add more rows as needed.

---

## Test Cases

### Feature 1: Order status

| ID | Type | Input / Action | Expected result |
|----|------|----------------|-----------------|
| F1-T1 | Normal | Place a new order | Status is `placed` |
| F1-T2 | Normal | Admin advances an order `placed` → `processing` → `shipped` → `delivered` | Status updates at each step |
| F1-T3 | Invalid | Try to set an invalid status (e.g. `"cancelled"` or `"abc"`) | Rejected with an error message; status unchanged |
| F1-T4 | Boundary | Try to advance an order that is already `delivered` | Rejected; status stays `delivered` |

### Feature 2: Tracking reference

| ID | Type | Input / Action | Expected result |
|----|------|----------------|-----------------|
| F2-T1 | Normal | Customer places an order, then views its status | Tracking reference and status are displayed |
| F2-T2 | Normal | Close and restart the program, then view the order | Tracking reference and status are unchanged (persistence) |
| F2-T3 | Invalid | Look up a tracking reference or order ID that does not exist | "Order not found" message |
| F2-T4 | Boundary | Customer tries to view another customer's order | Access denied or not found |

> Edit these to match what your program actually does.

---

## Demonstration Checklist

- [ ] Customer registration and sign in
- [ ] Display available products and purchase a valid quantity
- [ ] Attempt to buy more than available stock (validation message)
- [ ] Display a receipt linked to the customer's actual order
- [ ] Return a purchased item and show the updated stock
- [ ] Admin sign in; add, adjust, and delete a product
- [ ] Low-stock report and customer order count
- [ ] Group feature 1: order status
- [ ] Group feature 2: tracking reference and status display
- [ ] Close and restart the program to demonstrate file persistence
- [ ] At least one invalid-input test

---

## Team and Contributions

| Member |
|--------|
| `[Belal Hossam Afify]` |
| `[Omar Farouk Ibrahim]` | 
| `[Adam Akram Abdelaziz]` | 
| `[Ali Sadek]` | 
| `[Khaled Sourour]` |  

---

## License

For academic coursework use only. `[Add a license if you wish to share publicly.]`
