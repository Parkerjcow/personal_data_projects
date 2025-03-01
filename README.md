# 📌 Who Defaults and Why - Credit Risk Analysis

🚀 **A deep dive into loan defaults, credit risk, and financial decision-making using SQL & Data Analytics.**  

---

## 📊 Project Overview
This project explores **loan default risk factors** using SQL and PostgreSQL.  
We analyze **borrower behavior**, **credit history**, **income levels**, and **interest rates** to uncover key **patterns that lead to loan defaults**.  

**📌 Tools Used:** PostgreSQL (PGAdmin4), SQL  
**📌 Data Source:** [Credit Risk Dataset](https://www.kaggle.com/datasets/laotse/credit-risk-dataset)

---

## 📌 SQL Table Structure
We begin by creating the **credit risk analysis table** to hold the dataset:

```sql
CREATE TABLE credit_risk (
    person_age INT,
    person_income NUMERIC,
    person_home_ownership VARCHAR(20),
    person_emp_length INT,
    loan_intent VARCHAR(20),
    loan_grade VARCHAR(1),
    loan_amnt NUMERIC,
    loan_int_rate NUMERIC,
    loan_status INT,
    loan_percent_income NUMERIC,
    cb_person_default_on_file VARCHAR(1),
    cb_person_cred_hist_length INT
);
```
# **Business Questions & Analysis Approach**
The project is divided into three main sections, each addressing key business concerns.

---

## **📌 Section 1: Credit Risk & Loan Performance**
### **1️⃣ What percentage of applicants default on their loans?**
🔹 **SQL Query:** Calculate total loans vs. defaulted loans.  
🔹 **Tableau:** KPI Card + Default Rate Trend Analysis.  

```sql
-- Find total number of loans
SELECT COUNT(*) AS total_loans FROM credit_risk;

-- Find total defaulted loans
SELECT COUNT(*) AS total_default_loans FROM credit_risk WHERE loan_status = 1;

-- Compute default rate
SELECT 
    COUNT(*) AS total_loans,
    COUNT(*) FILTER (WHERE loan_status = 1) AS total_defaults,
    ROUND((COUNT(*) FILTER (WHERE loan_status =1) * 100.0 / COUNT(*)), 2) AS default_rate
FROM credit_risk;
```



