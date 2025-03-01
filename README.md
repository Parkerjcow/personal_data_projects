# Who Defaults and Why - Credit Risk Analysis

🚀 **Uncovering the Hidden Patterns Behind Loan Defaults Using SQL & Data Analytics**  

---

## 📊 Project Overview  
Ever wondered **who is most likely to default on a loan?** What factors contribute the most to **financial risk?** In this project, we analyze **credit risk data** to find out.  

Using PostgreSQL, we break down key **borrower behaviors**, **income levels**, **credit history**, and **loan interest rates** to reveal **what drives loan defaults** and **how lenders can make smarter decisions.**  

### 🔹 **A Few Key Questions We Answer**
✅ What percentage of applicants default on their loans?  
✅ Which factors contribute most to default risk?  
✅ How do income, employment, and past defaults impact repayment?  
✅ Are high-interest loans more likely to default?  

📌 **Tools Used:** PostgreSQL (PGAdmin4), SQL  
📌 **Data Source:** [Credit Risk Dataset](https://www.kaggle.com/datasets/laotse/credit-risk-dataset)

---

# **📌 Business Questions & Analysis Approach**
We’ve divided this analysis into **three main sections**—each tackling critical questions that financial institutions and lenders would care about.  

---

## **📌 Section 1: Credit Risk & Loan Performance**
### **1️⃣ What percentage of applicants default on their loans?**
💡 **Why This Matters:** Understanding the overall **default rate** helps lenders measure **risk exposure** and improve decision-making.  

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
### **2️⃣ How do credit scores correlate with default rates?**
💡 **Why This Matters:** If lower credit scores = higher default rates, lenders can adjust loan terms accordingly 
-- Categorizing credit history length

```sql
SELECT 
    CASE 
        WHEN cb_person_cred_hist_length BETWEEN 0 AND 4 THEN 'Short Credit History (0-4 years)'
        WHEN cb_person_cred_hist_length BETWEEN 5 AND 14 THEN 'Moderate Credit History (5-14 years)'
        ELSE 'Long Credit History (15+ years)'
    END AS credit_history_group,
    COUNT(*) AS total_borrowers,
    COUNT(*) FILTER (WHERE loan_status = 1) AS total_defaults,
    ROUND((COUNT(*) FILTER (WHERE loan_status = 1) * 100.0 / COUNT(*)), 2) AS default_rate
FROM credit_risk
GROUP BY credit_history_group
ORDER BY credit_history_group;
```
### **3️⃣ What loan types have the highest and lowest default rates?**
💡 **Why This Matters:** Some loan types are riskier than others. Knowing which ones have higher default rates can help in setting better loan policies.
```sql
-- Calculate default rate per loan type
SELECT 
    loan_intent,
    COUNT(*) AS total_loans,
    COUNT(*) FILTER (WHERE loan_status = 1) AS total_defaults,
    ROUND((COUNT(*) FILTER (WHERE loan_status = 1) * 100.0 / COUNT(*)), 2) AS default_rate
FROM credit_risk
GROUP BY loan_intent
ORDER BY default_rate DESC;

```

## **📌 Section 2: Customer Demographics & Risk Assessment**
### **4️⃣ Which income groups are more likely to default?**
💡 **Why This Matters:** Do lower-income borrowers struggle more with loan repayment? Let's find out
```sql
-- Categorizing income groups & calculating default rate
SELECT 
    CASE 
        WHEN person_income < 40000 THEN 'Low Income (<40K)'
        WHEN person_income BETWEEN 40000 AND 79999 THEN 'Middle Income (40K-80K)'
        WHEN person_income BETWEEN 80000 AND 119999 THEN 'Upper Middle Income (80K-120K)'
        ELSE 'High Income (120K+)'
    END AS income_group,
    COUNT(*) AS total_borrowers,
    COUNT(*) FILTER (WHERE loan_status =1) AS total_defaults,
    ROUND((COUNT(*) FILTER (WHERE loan_status = 1) * 100.0 / COUNT(*)), 2) AS default_rate
FROM credit_risk
GROUP BY income_group
ORDER BY total_borrowers DESC;
```
### **5️⃣ How does employment status impact loan repayment?**
💡 **Why This Matters:** Does having a stable job reduce the chances of default? Let’s check.
```sql
-- Categorizing Employment Length & calculating default rates
SELECT 
    CASE 
        WHEN person_emp_length = 0 THEN 'Unemployed (0 Years)'
        WHEN person_emp_length BETWEEN 1 AND 2 THEN 'Short-Term Employment (1-2 Years)'
        WHEN person_emp_length BETWEEN 3 AND 5 THEN 'Moderate Employment (3-5 Years)'
        WHEN person_emp_length BETWEEN 6 AND 10 THEN 'Stable Employment (6-10 Years)'
        ELSE 'Long-Term Employment (10+ Years)'
    END AS employment_status,
    COUNT(*) AS total_borrowers,
    COUNT(*) FILTER (WHERE loan_status = 1) AS total_defaults,
    ROUND((COUNT(*) FILTER (WHERE loan_status = 1) * 100.0 / COUNT(*)), 2) AS default_rate
FROM credit_risk
GROUP BY employment_status
ORDER BY total_borrowers DESC;
```

## **📌 Section 3: What Are the Most Common Risk Factors for Default?**
### **6️⃣ How Does Credit History Length Impact Default Rates?**
💡 **Why This Matters:** A borrower's credit history length **could be a key indicator of financial responsibility**. But is a short credit history **always riskier**? Let’s find out.  

🔹 **SQL Query:** Categorize borrowers by **credit history length** and analyze their **default rates**.  
🔹 **Tableau:** Bar chart comparing **default rates** across different credit history groups.  

```sql
-- Categorizing Borrowers by Credit History Length & Computing Default Rate
SELECT 
    CASE 
        WHEN cb_person_cred_hist_length BETWEEN 0 AND 4 THEN 'Short Credit History (0-4 years)'
        WHEN cb_person_cred_hist_length BETWEEN 5 AND 9 THEN 'Moderate Credit History (5-9 years)'
        ELSE 'Long Credit History (10+ years)' 
    END AS credit_history_group,
    COUNT(*) AS total_borrowers,
    COUNT(*) FILTER (WHERE loan_status = 1) AS total_defaults,
    ROUND((COUNT(*) FILTER (WHERE loan_status = 1) * 100.0 / COUNT(*)), 2) AS default_rate
FROM credit_risk
GROUP BY credit_history_group
ORDER BY default_rate DESC;
```
### **7️⃣ Does Having a Past Default Increase Default Risk?**
💡 **Why This Matters:**If a borrower has defaulted before, should a lender trust them with another loan? Does past behavior predict future financial habits?
```sql
-- Analyzing past default impact on future repayment
SELECT 
    cb_person_default_on_file AS past_default,
    COUNT(*) AS total_borrowers,
    COUNT(*) FILTER (WHERE loan_status = 1) AS total_defaults,
    ROUND((COUNT(*) FILTER (WHERE loan_status = 1) * 100.0 / COUNT(*)), 2) AS default_rate
FROM credit_risk
GROUP BY past_default
ORDER BY default_rate DESC;
```
### **8️⃣ Do High Interest Rates Lead to More Defaults?**
💡 **Why This Matters:**If higher interest rates lead to higher default rates, lenders might need to rethink their pricing models. Let's analyze if there's a strong correlation.
```sql
-- Analyzing default rates across interest rate categories
SELECT 
    CASE 
        WHEN loan_int_rate < 10 THEN 'Low Interest (<10%)'
        WHEN loan_int_rate BETWEEN 10 AND 15 THEN 'Medium Interest (10-15%)'
        ELSE 'High Interest (15%+)' 
    END AS interest_rate_group,
    COUNT(*) AS total_borrowers,
    COUNT(*) FILTER (WHERE loan_status = 1) AS total_defaults,
    ROUND((COUNT(*) FILTER (WHERE loan_status = 1) * 100.0 / COUNT(*)), 2) AS default_rate
FROM credit_risk
GROUP BY interest_rate_group
ORDER BY default_rate DESC;
```

## 🎯 Final Takeaways
### 🔥 So, Who Defaults and Why?
This project has revealed critical insights that financial institutions can use to improve credit risk assessment and loan approval strategies.

✅ Key Risk Factors for Loan Defaults:
- Short credit history (0-4 years) is riskier than longer histories.
- Past defaults strongly predict future defaults.
- High-interest loans (15%+) default at alarming rates (58%).


