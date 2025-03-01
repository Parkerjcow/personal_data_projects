# 📌 Who Defaults and Why - Credit Risk Analysis

🚀 **A deep dive into loan defaults, credit risk, and financial decision-making using SQL & Data Analytics.**  

---

## 📊 Project Overview
This project explores **loan default risk factors** using SQL and PostgreSQL.  
We analyze **borrower behavior**, **credit history**, **income levels**, and **interest rates** to uncover key **patterns that lead to loan defaults**.  

### 🔹 **Key Business Questions Answered**
✅ What percentage of applicants default on their loans?  
✅ Which factors contribute most to default risk?  
✅ How do income, employment, and past defaults impact repayment?  
✅ Are high-interest loans more likely to default?  

**📌 Tools Used:** PostgreSQL (PGAdmin4), SQL  
**📌 Data Source:** [Credit Risk Dataset] *(https://www.kaggle.com/datasets/laotse/credit-risk-dataset)*

---

## 📌 Key Business Insights

### **📊 1. How Does Credit History Length Impact Default Rates?**
#### 🔍 Findings:
- **Borrowers with short credit history (0-4 years) have the highest default rate (22.72%).**  
- **Moderate (5-9 years) and Long-Term (10+ years) credit borrowers default at similar rates (~20.7%).**  
- **Business Takeaway:** Credit history alone **isn’t a perfect risk indicator**—lenders should assess other factors like past defaults.  

📜 **SQL Query:** [`SQL-Queries/01_credit_history_analysis.sql`](SQL-Queries/01_credit_history_analysis.sql)  

---

### **📊 2. Does Having a Past Default Increase Default Risk?**
#### 🔍 Findings:
- **Borrowers who have defaulted before are twice as likely to default again (37.81% vs. 18.39%).**  
- **Past defaulters make up only 17% of borrowers but contribute nearly 30% of all defaults.**  
- **Business Takeaway:** Past defaults **are a major risk factor**—lenders should enforce stricter loan conditions for repeat defaulters.  

📜 **SQL Query:** [`SQL-Queries/02_past_defaults_analysis.sql`](SQL-Queries/02_past_defaults_analysis.sql)  

---

### **📊 3. Do High Interest Rates Lead to More Defaults?**
#### 🔍 Findings:
- **High-interest loans (15%+) default at a shocking rate of 58.01%!**  
- **Default rates decrease as interest rates lower—22.20% (10-15%) and 12.91% (<10%).**  
- **Business Takeaway:** Lenders may need to **reassess risk policies for high-interest loans** and explore **alternative credit models**.  

📜 **SQL Query:** [`SQL-Queries/03_interest_rate_analysis.sql`](SQL-Queries/03_interest_rate_analysis.sql)  

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
