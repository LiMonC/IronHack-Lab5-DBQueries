# IronHack-Lab5-DBQueries: 
# Implementing and Optimizing Database Queries

## 1. SQL Query Optimization
### 1. Orders Query: Retrieve orders with many items and calculate the total price.

#### Original
```
SELECT Orders.OrderID, SUM(OrderDetails.Quantity * OrderDetails.UnitPrice) AS TotalPrice
FROM Orders
JOIN OrderDetails ON Orders.OrderID = OrderDetails.OrderID
WHERE OrderDetails.Quantity > 10
GROUP BY Orders.OrderID;
```
#### Improvement 
```
CREATE INDEX idx_orderdetails_orderid_quantity ON OrderDetails(OrderID, Quantity);
SELECT OrderID, SUM(Quantity * UnitPrice) AS TotalPrice
FROM OrdersDetails
WHERE OrderDetails.Quantity > 10
GROUP BY Orders.OrderID
LIMIT 0,10;
```

### 2. Customer Query: Find all customers from London and sort by CustomerName.

#### Original
```
SELECT CustomerName FROM Customers WHERE City = 'London' ORDER BY CustomerName;
```

#### Improvement 
```
CREATE INDEX idx_customers_city ON Customers (City);
SELECT CustomerName FROM Customers WHERE City = 'London' ORDER BY CustomerName;
```

## 2. NoSQL Query Implementation

### 1. User Posts Query: Retrieve the most popular active posts and display their title and like count
#### Original
```
db.posts
  .find({ status: "active" }, { title: 1, likes: 1 })
  .sort({ likes: -1 });
```

#### Improvement 
```
db.posts.createIndex({ "active" : 1 , "title" : 1 , likes: -1 });
db.posts
  .find({ status: "active" }, { title: 1, likes: 1 })
  .sort({ likes: -1 })
  .limit(100);
```

### 2. User Data Aggregation: Summarize the number of active users by location.
#### Original
```
db.users.aggregate([
  { $match: { status: "active" } },
  { $group: { _id: "$location", totalUsers: { $sum: 1 } } },
]);
```

#### Improvement 
```
db.users.createIndex({ status: "active" })
db.users.aggregate([
  { $match: { status: "active" } },
  { $group: { _id: "$location", totalUsers: { $sum: 1 } } },
]);
```
