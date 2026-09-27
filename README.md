# Data Discrepancy & Combined Analysis using MySQL

## 🎯 Project Goal
The objective of this project was to analyze and identify data mismatches, missing tracking records, and volume distributions between two retail tracking transaction tables (`data1` and `data2`). This analysis helps inventory or order management systems ensure data integrity across parallel databases.

## 🛠️ Tech Stack & SQL Concepts Used
- **Database Engine:** MySQL Workbench
- **Key Concepts:** Left Joins, Filtering Null Values, Subqueries, Aggregate Functions (`COUNT`, `SUM`), Set Operators (`UNION`).

## 📊 Business Questions Solved

### Q1: Mismatched Records (Present in Data1 but Missing in Data2)
Identified the specific `Order ID` and `Product ID` combinations that failed to sync over to table 2.
- **Total Missing Records:** 507 rows
- **Query Code:**
  ```sql
  SELECT d1.`order id`, d1.`product id`
  FROM data1 d1
  LEFT JOIN data2 d2 
    ON d1.`order id` = d2.`order id`
    AND d1.`product id` = d2.`product id`
  WHERE d2.`order id` IS NULL;
  ```

### Q2: Mismatched Records (Missing in Data1 but Present in Data2)
Discovered entries generated in table 2 that do not have baseline data recorded in table 1.
- **Total Missing Records:** 508 rows
- **Query Code:**
  ```sql
  SELECT d2.`order id`, d2.`product id`
  FROM data2 d2
  LEFT JOIN data1 d1 
    ON d2.`order id` = d1.`order id`
    AND d2.`product id` = d1.`product id`
  WHERE d1.`order id` IS NULL;
  ```

### Q3: Volume Impact of Mismatched Rows
Calculated the sum of the total quantity (`qty`) fields for items that existed exclusively in `data2` to estimate untracked product volume.
- **Total Mismatched Quantity:** 1,956 units
- **Query Code:**
  ```sql
  SELECT SUM(d2.qty) AS mq1
  FROM data2 d2
  LEFT JOIN data1 d1 
    ON d2.`order id` = d1.`order id`
    AND d2.`product id` = d1.`product id`
  WHERE d1.`order id` IS NULL;
  ```

### Q4: Combined Unique Footprint
Calculated the absolute distinct universe of product-order relationships across the entire dataset.
- **Total Unique Unified Records:** 9,986 entries
- **Query Code:**
  ```sql
  SELECT COUNT(*) AS total_unique_records
  FROM (
      SELECT `order id`, `product id` FROM data1
      UNION
      SELECT `order id`, `product id` FROM data2
  ) t;
  ```

---
*Analysis completed by Aanchal Karel.*
