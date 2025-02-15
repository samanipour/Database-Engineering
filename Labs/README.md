# **Preparing for Database Engineering Lab**

## **Objective**
This manual will guide you through:
- Understanding the six example tables used in this Lab.
- Creating the tables using either Data Import or SQL Scripts.
- Populating the tables with sample data.
- Learning the purpose and structure of each table.

These example tables are part of an **order entry system** used by an imaginary distributor of products for cartoon characters. The tables are designed to:
- Manage **vendors**
- Manage **product catalogs**
- Manage **customer lists**
- Enter **customer orders**

---

## **Step 1: Understanding the Example Tables**
1. **Overview of the Tables**
   - There are **six interconnected tables** forming a relational database.
   - These tables are designed to demonstrate data organization and relationships commonly found in real-world installations.
   - They are **simplified** for learning purposes but include:
     - `vendors`
     - `products`
     - `customers`
     - `orders`
     - `orderitems`
     - `productnotes`

2. **Purpose of Each Table**
   - `vendors`: Stores vendor information.
   - `products`: Stores product catalog details.
   - `customers`: Stores customer details.
   - `orders`: Stores customer orders.
   - `orderitems`: Stores individual order items.
   - `productnotes`: Stores notes related to products.

3. **Why This Order?**
   - Tables are listed in order of their dependencies:
     - `products` depend on `vendors`.
     - `orderitems` depend on `orders` and `products`.
   - This ensures **referential integrity** in relational design.

---

## **Step 2: Table Descriptions and Structure**

### **1. The vendors Table**
- **Purpose**: Stores vendor details for products.
- **Primary Key**: `vend_id`
- **Columns**:
  - `vend_id`: Unique numeric vendor ID (Auto Increment)
  - `vend_name`: Vendor name
  - `vend_address`: Vendor address
  - `vend_city`: Vendor city
  - `vend_state`: Vendor state
  - `vend_zip`: Vendor zip code
  - `vend_country`: Vendor country
- **Dependencies**:
  - Referenced by `products` table via `vend_id`.

**Create Table Script**:
```sql
CREATE TABLE vendors (
  vend_id INT NOT NULL AUTO_INCREMENT,
  vend_name CHAR(50) NOT NULL,
  vend_address CHAR(50) NULL,
  vend_city CHAR(50) NULL,
  vend_state CHAR(5) NULL,
  vend_zip CHAR(10) NULL,
  vend_country CHAR(50) NULL,
  PRIMARY KEY (vend_id)
) ENGINE=InnoDB;
```

---

### **2. The products Table**
- **Purpose**: Stores product catalog with one product per row.
- **Primary Key**: `prod_id`
- **Foreign Key**: `vend_id` (Links to `vendors`)
- **Columns**:
  - `prod_id`: Unique product ID
  - `vend_id`: Vendor ID (Foreign Key)
  - `prod_name`: Product name
  - `prod_price`: Product price
  - `prod_desc`: Product description

**Create Table Script**:
```sql
CREATE TABLE products (
  prod_id CHAR(10) NOT NULL,
  vend_id INT NOT NULL,
  prod_name CHAR(50) NOT NULL,
  prod_price DECIMAL(8,2) NOT NULL,
  prod_desc TEXT NULL,
  PRIMARY KEY (prod_id),
  FOREIGN KEY (vend_id) REFERENCES vendors(vend_id)
) ENGINE=InnoDB;
```

---

### **3. The customers Table**
- **Purpose**: Stores customer details.
- **Primary Key**: `cust_id`
- **Columns**:
  - `cust_id`: Unique numeric customer ID (Auto Increment)
  - `cust_name`: Customer name
  - `cust_address`: Customer address
  - `cust_city`: Customer city
  - `cust_state`: Customer state
  - `cust_zip`: Customer zip code
  - `cust_country`: Customer country
  - `cust_contact`: Customer contact name
  - `cust_email`: Customer contact email address

**Create Table Script**:
```sql
CREATE TABLE customers (
  cust_id INT NOT NULL AUTO_INCREMENT,
  cust_name CHAR(50) NOT NULL,
  cust_address CHAR(50) NULL,
  cust_city CHAR(50) NULL,
  cust_state CHAR(5) NULL,
  cust_zip CHAR(10) NULL,
  cust_country CHAR(50) NULL,
  cust_contact CHAR(50) NULL,
  cust_email CHAR(255) NULL,
  PRIMARY KEY (cust_id)
) ENGINE=InnoDB;
```

---

### **4. The orders Table**
- **Purpose**: Stores customer orders (without order details).
- **Primary Key**: `order_num`
- **Foreign Key**: `cust_id` (Links to `customers`)
- **Columns**:
  - `order_num`: Unique order number (Auto Increment)
  - `order_date`: Date of order
  - `cust_id`: Customer ID (Foreign Key)

**Create Table Script**:
```sql
CREATE TABLE orders (
  order_num INT NOT NULL AUTO_INCREMENT,
  order_date DATETIME NOT NULL,
  cust_id INT NOT NULL,
  PRIMARY KEY (order_num),
  FOREIGN KEY (cust_id) REFERENCES customers(cust_id)
) ENGINE=InnoDB;
```

---

### **5. The orderitems Table**
- **Purpose**: Stores items in each order, one row per item.
- **Primary Key**: `order_num`, `order_item`
- **Foreign Keys**: 
  - `order_num` (Links to `orders`)
  - `prod_id` (Links to `products`)
- **Columns**:
  - `order_num`: Order number (Foreign Key)
  - `order_item`: Sequential item number within the order
  - `prod_id`: Product ID (Foreign Key)
  - `quantity`: Quantity ordered
  - `item_price`: Price per item

**Create Table Script**:
```sql
CREATE TABLE orderitems (
  order_num INT NOT NULL,
  order_item INT NOT NULL,
  prod_id CHAR(10) NOT NULL,
  quantity INT NOT NULL,
  item_price DECIMAL(8,2) NOT NULL,
  PRIMARY KEY (order_num, order_item),
  FOREIGN KEY (order_num) REFERENCES orders(order_num),
  FOREIGN KEY (prod_id) REFERENCES products(prod_id)
) ENGINE=InnoDB;
```

---

### **6. The productnotes Table**
- **Purpose**: Stores notes related to products.
- **Primary Key**: `note_id`
- **Foreign Key**: `prod_id` (Links to `products`)
- **Columns**:
  - `note_id`: Unique note ID (Auto Increment)
  - `prod_id`: Product ID (Foreign Key)
  - `note_date`: Date the note was added
  - `note_text`: Note text (FULLTEXT indexed)

**Create Table Script**:
```sql
CREATE TABLE productnotes (
  note_id INT NOT NULL AUTO_INCREMENT,
  prod_id CHAR(10) NOT NULL,
  note_date DATETIME NOT NULL,
  note_text TEXT NOT NULL,
  PRIMARY KEY (note_id),
  FULLTEXT (note_text),
  FOREIGN KEY (prod_id) REFERENCES products(prod_id)
) ENGINE=MyISAM;
```

---

## **Step 3: Creating and Populating the Tables**

### **Option 1: Using Data Import (Recommended for MySQL Workbench)**
1. **Download Data Export File**
   - Download from [Lab Assets]()
2. **In MySQL Workbench**:
   - Go to **Administration** → **Data Import/Restore**.
   - Select **Import from Self-Contained File**.
   - Locate the downloaded file.
   - Click **Start Import**.

### **Option 2: Using SQL Scripts**
1. **Download Scripts**
   - Download `create.sql` and `populate.sql` from [Fortabook](http://www.forta.com/books/9780138223021/).
2. **Create a New Schema**
   ```sql
   CREATE SCHEMA crashcourse;
   USE crashcourse;
   ```
3. **Run Scripts**
   - Execute `create.sql` first.
   - Then execute `populate.sql`.