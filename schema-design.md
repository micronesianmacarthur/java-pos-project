## MySQL Database Design
### Table: users
- id: INT, Primary Key, Auto Increment
- name: VARCHAR, Not Null
- username: VARCHAR, Not Null
- password: VARCHAR, Not Null, encrypted
- role_id: INT, Foreign key &rarr; roles(id)
- enabled: BOOLEAN, Not Null, DEFAULT True
- created: DATE, Not Null, DEFAULT current timestamp
- updated: DATE, onUpdate

### Table: roles
- id: INT, Primary Key, Auto Increment
- name: VARCHAR, Not Null

### Table: products
- id: INT, Primary Key, Auto Increment
- name: VARCHAR, Not Null
- sku: VARCHAR, Not Null
- category_id: INT, Foreign Key &rarr; categories(id)
- cost: Decimal (If category = service then NULL, else Not Null)
- price: Decimal, Not Null
- stock_level: INT, Not Null
- reorderThreshold: INT, Not Null
- supplierId: INT, Foreign Key &rarr; suppliers(id)
- version: INT, Not Null, Default 0 &rarr; optimistic locking

### Table: suppliers
- id: INT, Primary Key, Auto Increment
- name: VARCHAR, Not Null
- phone: VARCHAR
- email: VARCHAR
- address: VARCHAR

### Table: categories
- id: INT, Primary Key, Auto Increment
- name: VARCHAR

### Table: customers
- id: INT, Primary Key, Auto Increment
- firstName: VARCHAR, Not Null
- lastName: VARCHAR, Not Null
- phone: VARCHAR
- email: VARCHAR
- address: VARCHAR
- accountBalance: DECIMAL, Not Null, Default 0.00
- created: DATETIME, Not Null, Default current timestamp

### Table: sales
- id: INT, Primary Key, Auto Increment
- sale_time: DATETIME, current timestamp
- clerkId: INT, Foreign Key &rarr; users(id)
- customerId: INT, Foreign Key &rarr; customers(id)
- totalAmount: DECIMAL, Not Null
- fullPayment: BOOLEAN, Default True
- notes: VARCHAR
- version: INT, Not Null, Default 0 &rarr; optimistic locking

### Table: sale_item
- id: INT, Primary key, Auto Increment
- saleId: INT, Foreign key &rarr; sales(id)
- productId: INT, Foreign key &rarr; products(id)
- quantity: INT, Not Null
- unitPrice: DECIMAL, Not Null

### Table: payments
- id: INT, Primary key, auto increment
- saleId: INT, foreign key &rarr; sales(id)
- type: VARCHAR -- 'CASH', 'CARD', 'ACCOUNT'
- amount: DECIMAL, Not null

### Table: register_sessions
- id: INT, primary key, auto increment
- clerkId: INT, foreign key &rarr; users(id)
- open_time: DATETIME
- close_time: DATETIME
- start_cash: DECIMAL, Not null
- end_cash: DECIMAL
- total_sales: DECIMAL &rarr; COALESCE((SUM(amount) from payments p join sales s ON s.id = p.sale_id
- &rarr;&rarr;&rarr;&rarr;&rarr;&rarr;&rarr;&rarr;&rarr;&rarr;&rarr; WHERE s.sale_time BETWEEN open_time AND close_time),0)
- discrepancy: DECIMAL &rarr; (end_cash - start_cash - total_sales)
- manager_override: BOOLEAN, Default False

### Table: purchase_orders
- id: INT, primary key, auto increment
- supplierId: INT, foreign key &rarr; suppliers(id)
- created: DATETIME
- expected: DATE
- status: VARCHAR, Default 'Pending': 'Pending', 'Received', 'Cancelled'

### Table: purchase_order_items
- id: INT, primary key, auto increment
- purchase_orderId: INT, foreign key &rarr; purchase_orders(id)
- productId: INT, foreign key &rarr; products(id)
- quantity: INT, Not null
- received_qty: INT, Not null, Default 0

## MongoDB Collection Design
### Collection: audit_logs
```java
@Document(collection = "audit_logs")
public class AuditLog {
    @Id
    private String id;
    private Instant timestamp;
    private String username;
    private String action;
    private Document details;
    // getters and setters ...
}
```