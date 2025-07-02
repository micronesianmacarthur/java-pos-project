## Schema Design
### Table: users
- id: INT, Primary Key, Auto Increment
- name: VARCHAR, Not Null
- username: VARCHAR, Not Null
- password: VARCHAR, Not Null, encrypted
- role_id: INT, Foreign key &rarr; roles(id)
- accountStatus: INT (0 = deactivated, 1 = active)
- createDate: DATE, Not Null
- deactivateDate: DATE

### Table: products
- id: INT, Primary Key, Auto Increment
- name: VARCHAR, Not Null
- category_id: INT, Foreign Key &rarr; categories(id)
- cost: Decimal (If category = service then NULL, else Not Null)
- price: Decimal, Not Null
- stock_level: INT, Not Null
- reorderThreshold: INT, Not Null

### Table: categories
- id: INT, Primary Key, Auto Increment
- name: VARCHAR

### Table: customers
- id: INT, Primary Key, Auto Increment
- name: VARCHAR, Not Null
- phone: VARCHAR
- email: VARCHAR
- accountStatus: INT (0 = deactivated, 1 = active)
- createDate: DATETIME, Not Null
- deactivateDate: DATE

### Table: orderDetails
- id: INT, Primary Key, Auto Increment
- 