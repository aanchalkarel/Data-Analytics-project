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


# Corporate Operations & Business Analytics Ledger (Advanced Excel)

## 🎯 Project Goal
This project maps out end-to-end analytical models built to process retail operations, financial ledgers, logistics routing, and customer databases. The model demonstrates operational metrics automation using absolute cell locking, programmatic string splits, and dynamic dashboards.

## 🛠️ Advanced Excel Functions Implemented
- **Logic & Conditionals:** Nested `IF` strings for inventory categorization (Max/Min flagging filters).
- **Dynamic Array Lookups:** Cross-table references with `INDEX(MATCH)` and positional `VLOOKUP` arrays.
- **Advanced Text Manipulation:** Text extraction strings (`LEFT`, `MID`, `RIGHT`, `FIND`) to parse standardized product matrix tags.
- **Aggregation Frameworks:** Dynamic summary matrix engines via Pivot Tables.
- **Information Controls:** Column constraints via custom Data Validation fields and Conditional Formatting rules.

## 📈 Functional Analytics Summary

### 1. Financial Ledger & Operational Budgeting
- Programmed a corporate payroll model automating dual variable multi-tier tax brackets (`Deduction 1` at 6.2%, `Deduction 2` at 1.45%) mapped back to basic dynamic formulas.
- Created dynamic revenue share models allocating percentage budgets across 5 operational expense targets.

### 2. Supply Chain Data Extraction Engine
Parsed multi-character inventory strings (e.g., `100's:200-65L`) into distinct data tables:
- **Style Code Extraction:** Isolated numerical product groupings.
- **Color ID Extraction:** Tracked finish parameters from text segments.
- **Size Normalization:** Categorized physical footprints (`S`, `M`, `L`, `XL`, `XXL`).

### 3. Dynamic Matrix Routing Engine (Logistics)
- Designed an interactive distance lookup tool mapped over Indian shipping hubs (Mumbai, Delhi, Bangalore, etc.). 
- Handled coordinate calculation queries across intersection arrays using parallel `INDEX` and `VLOOKUP` setups to calculate precise shipping leg distance.

### 4. Interactive Pivot Dashboards
- Synthesized a multi-year sales transactional ledger (1,700+ rows) into dynamic Pivot tables.
- Mapped product line revenue splits, sales rep deal loops, and yearly performance trajectories (`2004` - `2006`).
- Implemented user data restriction validations enforcing calendar limits (Post `01/01/2000`) and field integrity controls.
