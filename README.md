# 💰 Bank Loan Modelling — SQL Analytics Project

**Author:** Rohan Kumar Jha  
**Tools Used:** MySQL Command Line Client, MySQL Workbench  
**Dataset:** [Bank Loan Modelling Dataset (UniversalBank)(Kaggle)](https://www.kaggle.com/datasets/itsmesunil/bank-loan-modelling)

---

## 🧭 1. Project Overview

This project analyzes customer demographic and financial data from a retail bank to understand **which customers accepted a personal loan offer** and what factors influence loan acceptance.

**Goals:**
- Explore customer behavior
- Identify key drivers of loan acceptance
- Build SQL-based risk and marketing insights
- Perform segmentation and feature engineering
- Create a clean, professional analytics report

**Skills Demonstrated:**
- SQL data import
- Data validation
- Exploratory analysis
- Customer segmentation
- Risk and marketing analytics
- Clean, structured reporting

---

## 📦 2. Dataset Description

The dataset contains **5000+ customer records** with demographic, financial, and behavioral attributes.

| Column Name | Description |
|---|---|
| `ID` | Unique customer ID |
| `Age` | Customer age (years) |
| `Experience` | Years of professional experience |
| `Income` | Annual income ($000) |
| `ZIPCode` | Customer ZIP code |
| `Family` | Family size |
| `CCAvg` | Avg. monthly credit card spending ($000) |
| `Education` | 1: Undergrad, 2: Graduate, 3: Professional |
| `Mortgage` | Mortgage value ($000) |
| `Personal_Loan` | 1 = accepted loan, 0 = rejected |
| `Securities_Account` | 1 = has securities account |
| `CD_Account` | 1 = has certificate of deposit |
| `Online` | 1 = uses internet banking |
| `CreditCard` | 1 = has bank-issued credit card |

**Why this dataset is valuable:**
- Realistic banking domain
- Perfect for credit risk & marketing analytics
- Clean, structured numerical data
- Ideal for SQL + data science workflows
- Allows segmentation, scoring, and pattern discovery

---

## 🧱 3. Database & Table Creation

```sql
-- Create database
CREATE DATABASE bank_loans;
USE bank_loans;

-- Create table
CREATE TABLE loan_data (
  ID                  INT,
  Age                 INT,
  Experience          INT,
  Income              INT,
  ZIPCode             INT,
  Family              INT,
  CCAvg               FLOAT,
  Education           INT,
  Mortgage            INT,
  Personal_Loan       INT,
  Securities_Account  INT,
  CD_Account          INT,
  Online              INT,
  CreditCard          INT
);
```

---

## 🖥️ 4. Importing the Dataset Using CMD

Move your CSV to:
```
C:\Users\itsro\OneDrive\Desktop\loan_data.csv
```

```sql
USE bank_loans;

LOAD DATA LOCAL INFILE 'C:\\Users\\itsro\\OneDrive\\Desktop\\loan_data.csv'
INTO TABLE loan_data
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;
```

---

## 🔍 5. Data Validation in MySQL Workbench

```sql
-- Display first 5 rows
SELECT * FROM loan_data LIMIT 5;

-- Count total rows
SELECT COUNT(*) FROM loan_data;
```

---

## 📘 6. Basic SQL Analysis (10 Tasks)

#### 1. Loan acceptance count
```sql
SELECT Personal_Loan, COUNT(*) AS total
FROM loan_data
GROUP BY Personal_Loan;
```

#### 2. Average income of customers
```sql
SELECT AVG(Income) AS avg_income
FROM loan_data;
```

#### 3. Count customers by education level
```sql
-- 1: Undergrad, 2: Graduate, 3: Professional
SELECT Education, COUNT(*) AS total
FROM loan_data
GROUP BY Education;
```

#### 4. Average credit card spending
```sql
SELECT AVG(CCAvg) AS avg_spending
FROM loan_data;
```

#### 5. Customers with a mortgage
```sql
SELECT COUNT(*) AS mortgage_customers
FROM loan_data
WHERE Mortgage > 0;
```

#### 6. Loan acceptance by family size
```sql
SELECT Family, Personal_Loan, COUNT(*) AS total
FROM loan_data
GROUP BY Family, Personal_Loan;
```

#### 7. Customers using online banking
```sql
SELECT Online, COUNT(*) AS total
FROM loan_data
GROUP BY Online;
```

#### 8. Customers with bank credit cards
```sql
SELECT CreditCard, COUNT(*) AS total
FROM loan_data
GROUP BY CreditCard;
```

#### 9. Highest income customers
```sql
SELECT ID, Income
FROM loan_data
ORDER BY Income DESC
LIMIT 10;
```

#### 10. Customers with high credit card spending
```sql
SELECT ID, CCAvg
FROM loan_data
WHERE CCAvg > 3;
```

---

## 🔥 7. Advanced SQL Analysis (15 Tasks)

#### 1. Income-to-spending ratio
```sql
SELECT ID, Income, CCAvg,
  Income / NULLIF(CCAvg, 0) AS income_spending_ratio
FROM loan_data;
```

#### 2. Loan acceptance by income group
```sql
SELECT
  CASE
    WHEN Income < 50 THEN 'Low Income'
    WHEN Income BETWEEN 50 AND 100 THEN 'Middle Income'
    ELSE 'High Income'
  END AS income_group,
  Personal_Loan,
  COUNT(*) AS total
FROM loan_data
GROUP BY income_group, Personal_Loan;
```

#### 3. Loan acceptance by education
```sql
SELECT Education, Personal_Loan, COUNT(*) AS total
FROM loan_data
GROUP BY Education, Personal_Loan;
```

#### 4. Customers with high income but no loan
```sql
SELECT *
FROM loan_data
WHERE Income > 100 AND Personal_Loan = 0;
```

#### 5. Customers with low income but accepted loan
```sql
SELECT *
FROM loan_data
WHERE Income < 40 AND Personal_Loan = 1;
```

#### 6. Loan acceptance by credit card usage
```sql
SELECT CreditCard, Personal_Loan, COUNT(*) AS total
FROM loan_data
GROUP BY CreditCard, Personal_Loan;
```

#### 7. Loan acceptance by online banking usage
```sql
SELECT Online, Personal_Loan, COUNT(*) AS total
FROM loan_data
GROUP BY Online, Personal_Loan;
```

#### 8. Loan acceptance by securities account
```sql
SELECT Securities_Account, Personal_Loan, COUNT(*) AS total
FROM loan_data
GROUP BY Securities_Account, Personal_Loan;
```

#### 9. Loan acceptance by CD account
```sql
SELECT CD_Account, Personal_Loan, COUNT(*) AS total
FROM loan_data
GROUP BY CD_Account, Personal_Loan;
```

#### 10. Mortgage vs loan acceptance
```sql
SELECT
  CASE WHEN Mortgage > 0 THEN 'Has Mortgage' ELSE 'No Mortgage' END AS mortgage_status,
  Personal_Loan,
  COUNT(*) AS total
FROM loan_data
GROUP BY mortgage_status, Personal_Loan;
```

#### 11. High-risk customers (low income + high spending)
```sql
SELECT *
FROM loan_data
WHERE Income < 40 AND CCAvg > 3;
```

#### 12. Build a "Marketing Score"
```sql
SELECT ID,
  ROUND(
    Income * 0.4 +
    CCAvg * 0.3 +
    Online * 10 +
    CreditCard * 10,
  2) AS marketing_score
FROM loan_data
ORDER BY marketing_score DESC;
```

#### 13. Build a "Risk Score"
```sql
SELECT ID,
  ROUND(
    Income * 0.3 +
    Mortgage * 0.2 -
    CCAvg * 0.5 -
    CD_Account * 20,
  2) AS risk_score
FROM loan_data
ORDER BY risk_score ASC;
```

#### 14. Customers likely to accept future loans (SQL heuristic)
```sql
SELECT *
FROM loan_data
WHERE Income > 60
  AND CCAvg > 2
  AND Online = 1
  AND CreditCard = 1;
```

#### 15. Top ZIP codes with the highest loan acceptance rate
```sql
SELECT
  ZIPCode,
  SUM(Personal_Loan) AS loans_accepted,
  COUNT(*) AS total_customers,
  ROUND(SUM(Personal_Loan) / COUNT(*) * 100, 2) AS acceptance_rate
FROM loan_data
GROUP BY ZIPCode
HAVING COUNT(*) > 20
ORDER BY acceptance_rate DESC
LIMIT 10;
```

---

## 🏁 8. Conclusion

This Bank Loan Modelling SQL project provides a complete analysis of customer behavior and loan acceptance patterns.

**Through this project, we explored:**
- Customer demographics
- Income and spending behavior
- Loan acceptance trends
- Risk and marketing insights
- Segmentation and scoring
- Feature engineering for predictive modelling

**SQL capabilities demonstrated:**
- Data import & validation
- Aggregations and grouping
- Conditional logic (CASE statements)
- Scoring models (Marketing & Risk)
- Analytical storytelling
