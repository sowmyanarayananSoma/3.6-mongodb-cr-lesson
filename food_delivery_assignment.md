# MongoDB Assignment: Food Delivery App (EatNow)
### **Lesson 7 – Data Modeling + CRUD + Relationships**

This assignment requires you to design and implement your own MongoDB data model for a **Food Delivery Application** similar to UberEats, DoorDash, or SkipTheDishes.

**Important:** *You will choose the fields, structures, embedded data, referenced data, and relationships. The data modeling is entirely up to you.*

---

# 📌 PART 1 — Create Your Database
Create a new database called:
```
eatNow
```
You will design all collections inside this database.

---

# 📌 PART 2 — Create Your Collections
You must create **at least 5 collections** that make sense for a food delivery platform. Examples of possible collections include (but are NOT required):
- restaurants
- menuItems
- users
- orders
- orderItems
- deliveryDrivers
- reviews
- categories
- addresses

**Your job is to decide:**
- What collections are needed
- How they relate to each other
- Which should embed data
- Which should reference other documents
- What the `_id` format should be for each collection

---

# 📌 PART 3 — Insert Sample Data
Insert **realistic sample data** into each collection.

You must insert at least:
- Multiple restaurants
- Multiple users
- Multiple menu items
- Multiple orders
- Enough related records to demonstrate your relationships

**You decide what fields each document will contain.**

---

# 📌 PART 4 — CRUD Operations (Required)
Write and run MongoDB commands to demonstrate the following operations:

### **A. Read (Find)**
- Query documents using filters
- Query using nested fields (dot notation)
- Query using comparison operators such as `$gt`, `$lt`, `$in`, etc.
- Query to retrieve related data (e.g., items belonging to a restaurant)

---

# 📌 PART 5 — Relationships
Demonstrate **all three relationship types** within your data:

### **1. One-to-one**
Example idea: A user and their address, a restaurant and its owner, etc.

### **2. One-to-many**
Example idea: A restaurant and its menu items, a user and their orders, etc.

### **3. Many-to-many**
Example idea: Orders that contain menu items, users who save multiple restaurants, etc.

**It is your responsibility to design how these relationships will be represented.**


Design creatively, think like a database architect, and enjoy building your own food delivery platform!

