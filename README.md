# Who Defaults and Why - Credit Risk Analysis

🚀 **Uncovering the Hidden Patterns Behind Loan Defaults Using SQL & Data Analytics**  

Ever wondered **who is most likely to default on a loan?** What factors contribute the most to **financial risk?** In this project, I analyze **credit risk data** to find out.  

Using PostgreSQL, I break down key **borrower behaviors**, **income levels**, **credit history**, and **loan interest rates** to reveal **what drives loan defaults** and **how lenders can make smarter decisions.**  

### 🔹 **A Few Key Questions I Answered**
✅ What percentage of applicants default on their loans?  
✅ Which factors contribute most to default risk?  
✅ How do income, employment, and past defaults impact repayment?  
✅ Are high-interest loans more likely to default?  

**Tools Used:** PostgreSQL (PGAdmin4), SQL  
**Data Source:** [Credit Risk Dataset](https://www.kaggle.com/datasets/laotse/credit-risk-dataset)

# **Business Questions & Analysis Approach**

We’ve divided this analysis into **three main sections**—each tackling critical questions that financial institutions and lenders would care about.  


## **📌 Section 1: Credit Risk & Loan Performance**
### **1️⃣ What percentage of applicants default on their loans?**
**Why This Matters:** Understanding the overall **default rate** helps lenders measure **risk exposure** and improve decision-making.  

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
![What percentage of applicants default on their loans](https://github.com/Parkerjcow/personal_data_projects/blob/Credit-Risk-Analysis/tables/1%20-%20What%20percentage%20of%20applicants%20default%20on%20their%20loans.png?raw=true)

## Insights
- 21.81% of borrowers defaulted on their loans.
- While this isn't an extreme default rate, 1 in 5 loans is still a significant risk for lenders.
- The default percentage gives lenders a baseline to assess how risky their loan portfolio is.
  
### Business Actions

- Financial institutions should ensure they have strong risk management strategies in place.
- Consider increasing interest rates or collateral requirements for higher-risk borrowers.
- Further investigate which specific borrower characteristics are driving defaults.
---
### **2️⃣ How do credit scores correlate with default rates?**
**Why This Matters:** If lower credit scores = higher default rates, lenders can adjust loan terms accordingly 
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
![How do credit scores correlate with default rates](https://github.com/Parkerjcow/personal_data_projects/blob/Credit-Risk-Analysis/tables/2-%20How%20do%20credit%20scores%20correlate%20with%20default%20rates.png?raw=true)
## Insights
- Borrowers with short credit history (0-4 years) default the most (22.72%).
- Surprisingly, those with long credit history (15+ years) still default at 21.72% – almost the same as moderate history borrowers.
  
### Business Actions
- Simply having a long credit history doesn’t guarantee financial stability – lenders should look at other factors (income, past defaults).
- Consider combining credit history with past repayment behavior for better risk assessment.
- New borrowers (short credit history) should be monitored closely before receiving high loan amounts.
---
### **3️⃣ What loan types have the highest and lowest default rates?**
**Why This Matters:** Some loan types are riskier than others. Knowing which ones have higher default rates can help in setting better loan policies.
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
![How do credit scores correlate with default rates](https://github.com/Parkerjcow/personal_data_projects/blob/Credit-Risk-Analysis/tables/2-%20How%20do%20credit%20scores%20correlate%20with%20default%20rates.png?raw=true)
```
![What loan types have the highest and lowest default rates](https://github.com/Parkerjcow/personal_data_projects/blob/Credit-Risk-Analysis/tables/3%20-%20What%20loan%20types%20have%20the%20highest%20and%20lowest%20default%20rates.png?raw=true)

## Insights
- Debt Consolidation loans have the highest default rate (28.59%) – borrowers taking out loans to pay off existing debt may already be financially struggling.
- Medical loans (26.70%) have the second-highest default rate, likely due to unexpected medical expenses impacting income.
- Venture (14.81%) and Education (17.22%) loans have the lowest default rates, possibly because they increase future income potential.
  
### Business Actions
- Debt consolidation loans should require stricter credit evaluations before approval.
- Consider offering longer repayment periods or flexible plans for medical loans to reduce risk.
- Education and venture loans should have competitive interest rates, as they are lower-risk.
  
## **📌 Section 2: Customer Demographics & Risk Assessment**
### **4️⃣ Which income groups are more likely to default?**
**Why This Matters:** Do lower-income borrowers struggle more with loan repayment? Let's find out
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
![Which income groups are more likely to default](https://github.com/Parkerjcow/personal_data_projects/blob/Credit-Risk-Analysis/tables/4%20-%20Which%20income%20groups%20are%20more%20likely%20to%20default.png?raw=true)
## Insights
- Low-income borrowers (<$40K) have the highest default rate (39.71%).
- Default rates drop significantly for higher income groups:
 - Middle Income (40K-80K): 18.60%
 - Upper Middle Income (80K-120K): 9.43%
 - High Income (120K+): 8.36%
  
### Business Actions
- Low-income borrowers pose the highest risk – lenders should either offer lower loan amounts or require stricter credit checks.
- Higher-income borrowers could be offered premium financial products with lower interest rates due to their lower risk.
- Consider alternative repayment structures (graduated repayment) for lower-income groups.
---

### **5️⃣ How does employment status impact loan repayment?**
**Why This Matters:** Does having a stable job reduce the chances of default? Let’s check.
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
![How does employment length impact loan repayment](https://github.com/Parkerjcow/personal_data_projects/blob/Credit-Risk-Analysis/tables/5%20-%20How%20does%20employment%20length%20impact%20loan%20repayment.png?raw=true)
## Insights
- Unemployed borrowers have the highest default rate (28.58%).
- Borrowers with short-term employment (1-2 years) default at 26.51%, meaning job stability plays a key role in repayment.
- Default rates decrease steadily for borrowers with longer employment histories:
 - Moderate Employment (3-5 years): 19.99%
 - Stable Employment (6-10 years): 18.07%
 - Long-Term Employment (10+ years): 16.23%
  
### Business Actions
- Lenders should avoid giving high loan amounts to unemployed or short-term employed individuals without additional guarantees.
- Consider offering better interest rates for borrowers with longer, more stable employment.
- Financial institutions should target stable borrowers (6+ years employment) for premium loan offers.
  
## **📌 Section 3: What Are the Most Common Risk Factors for Default?**
### **6️⃣ How Does Credit History Length Impact Default Rates?**
**Why This Matters:** A borrower's credit history length **could be a key indicator of financial responsibility**. But is a short credit history **always riskier**? Let’s find out.  
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
![How Does Credit History Length Impact Default Rates](https://github.com/Parkerjcow/personal_data_projects/blob/Credit-Risk-Analysis/tables/6%20-%20How%20Does%20Credit%20History%20Length%20Impact%20Default%20Rates.png?raw=true)

## Insights
- Borrowers with short credit history (0-4 years) default the most (22.72%).
- Moderate (5-9 years) and long credit history (10+ years) have similar default rates (~20.71%).
  
### Business Actions
- Short credit history borrowers need additional screening – consider requiring co-signers or collateral for large loans.
- Rather than just looking at credit history length, analyze past default behavior for a more complete risk profile.
- Credit-building programs could be introduced to help short-history borrowers build financial trust.
  
---
### **7️⃣ Does Having a Past Default Increase Default Risk?**
**Why This Matters:** If a borrower has defaulted before, should a lender trust them with another loan? Does past behavior predict future financial habits?
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
![Does Having a Past Default Increase Default Risk](https://github.com/Parkerjcow/personal_data_projects/blob/Credit-Risk-Analysis/tables/7%20-%20Does%20Having%20a%20Past%20Default%20Increase%20Default%20Risk.png?raw=true)

## Insights
- Past defaulters have a 37.81% default rate, double the risk of non-defaulters (18.39%).
- Even though past defaulters only make up 17.6% of borrowers, they account for nearly 30% of all defaults.
  
### Business Actions
- Past defaulters should undergo stricter approval processes – consider requiring higher credit scores or collateral.
- Lenders should closely monitor past defaulters and implement risk-mitigation strategies (e.g., lower loan amounts).
- Consider offering financial literacy or counseling programs to past defaulters to reduce future risk.


---
### **8️⃣ Do High Interest Rates Lead to More Defaults?**
**Why This Matters:** If higher interest rates lead to higher default rates, lenders might need to rethink their pricing models. Let's analyze if there's a strong correlation.
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
![Do High Interest Rates Lead to More Defaults](https://github.com/Parkerjcow/personal_data_projects/blob/Credit-Risk-Analysis/tables/8%20-%20Do%20High%20Interest%20Rates%20Lead%20to%20More%20Defaults.png?raw=true)
## Insights
- Borrowers with high-interest loans (15%+) default at an alarming rate of 58.01%.
- Medium Interest (10-15%) loans have a 22.20% default rate, which is much lower.
- Low-interest loans (<10%) default the least (12.91%), suggesting that affordability plays a major role in loan repayment.
  
### Business Actions
- High-interest borrowers are at extreme risk of default – lenders should reassess risk-based pricing strategies.
- If possible, offering slightly lower interest rates to high-risk borrowers may reduce defaults and improve long-term repayment.
- Consider refinancing options or offering lower interest rates to financially stable borrowers who initially took high-interest loans.

---

## 🎯 Final Takeaways - So, Who Defaults and Why?
This project has revealed critical insights that financial institutions can use to improve credit risk assessment and loan approval strategies.

- Key Risk Factors for Loan Defaults:
Short credit history borrowers default the most (22.72%) – lenders need better screening.
- Past defaults = 2x higher chance of defaulting again (37.81%) – stricter approval needed.
- High-interest loans (15%+) default at 58.01% – lenders should reconsider pricing models.
- Low-income borrowers default the most (39.71%), showing financial strain.
- Stable employment (6+ years) significantly reduces default risk.

## Business Recommendations:
- Implement stricter loan approval processes for high-risk borrowers (low income, past defaults, short credit history).
- Offer better loan terms to stable borrowers (low-risk customers).
- Consider alternative repayment models to reduce defaults, such as graduated payment plans.


