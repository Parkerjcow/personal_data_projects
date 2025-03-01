# Who Defaults and Why - Credit Risk Analysis

🚀 **Uncovering the Hidden Patterns Behind Loan Defaults Using SQL & Data Analytics**  

---

## 📊 Project Overview  
Ever wondered **who is most likely to default on a loan?** What factors contribute the most to **financial risk?** In this project, we analyze **credit risk data** to find out.  

Using PostgreSQL, we break down key **borrower behaviors**, **income levels**, **credit history**, and **loan interest rates** to reveal what drives loan defaults and how lenders can make smarter decisions.  

### 🔹 **Key Questions We Answer**
✅ What percentage of applicants default on their loans?  
✅ Which factors contribute most to default risk?  
✅ How do income, employment, and past defaults impact repayment?  
✅ Are high-interest loans more likely to default?  

📌 **Tools Used:** PostgreSQL (PGAdmin4), SQL  
📌 **Data Source:** [Credit Risk Dataset] *(Include link if public)*  

---

# **📌 Business Questions & Analysis Approach**
We’ve divided this analysis into **three main sections**—each tackling critical questions that financial institutions and lenders would care about.  

---

## **📌 Section 1: Credit Risk & Loan Performance**
### **1️⃣ What percentage of applicants default on their loans?**
💡 **Why This Matters:** Understanding the overall **default rate** helps lenders measure **risk exposure** and improve decision-making.  

🔹 **SQL Query:** Count total loans vs. total defaults.  
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



