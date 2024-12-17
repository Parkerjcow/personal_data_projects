# Heart Health SQL Analysis Project

## Project Overview
This project focuses on analyzing a Heart Health Dataset to derive actionable insights into cardiovascular diseases (CVDs). By using SQL queries, we explore patterns, risk factors, and correlations that help identify high-risk groups and potential indicators of heart disease.

## Context

Cardiovascular diseases (CVDs) are the leading cause of death worldwide, accounting for 31% of all deaths (approximately 17.9 million lives each year).

## Key Facts:
- Four out of five CVD deaths are caused by heart attacks and strokes.
- One-third of these deaths occur prematurely in people under the age of 70.
- Early detection and management of risk factors like hypertension, diabetes, hyperlipidemia, and lifestyle habits are crucial to reducing CVD-related deaths.
- This dataset contains 11 features that can be used to analyze and predict heart disease. The features include demographics, vital signs, and key lifestyle indicators. Through SQL analysis, we aim to uncover trends and     provide insights to support early detection and management of cardiovascular risks.

## Table Structure

**We begin by creating the heart table to hold the dataset:**

```sql
CREATE TABLE heart(  
    Age INT,  
    Sex VARCHAR(1),  
    ChestPainType VARCHAR(3),  
    RestingBP INT,  
    Cholesterol INT,  
    FastingBS INT,  
    RestingECG VARCHAR(6),  
    MaxHR INT,  
    ExerciseAngina VARCHAR(1),  
    Oldpeak NUMERIC,  
    ST_Slope VARCHAR(4),  
    HeartDisease INT  
);```

## To verify the table and data:

```SELECT * FROM heart;```
![Table](https://github.com/Parkerjcow/personal_data_projects/blob/Heart-Failure-Predictions/Table.png?raw=true)

# Business Problems and SQL Solutions

## 1. Predictive Insights: Identifying High-Risk Groups

**Objective**

Understand which age and gender groups are at a higher risk of heart disease.

-- Starting off looking at the collumn we will be working with for this query

```SELECT age, sex, heartdisease
FROM heart;```


-- Age groups help summarize data into buckets. 
-- Using CASE statement to create age ranges then create a new column called age_group.

```SELECT 
	CASE 
		WHEN age BETWEEN 20 AND 29 THEN '20-29'
		WHEN age BETWEEN 30 AND 39 THEN '30-39'
		WHEN age BETWEEN 40 AND 49 THEN '40-49'
		WHEN age BETWEEN 50 AND 59 THEN '50-59'
		WHEN age >= 60 THEN '60+'
	END AS age_group,
	age,
	sex,
	heartdisease
FROM heart;


-- We count how many patients belong to each age group.

SELECT 
	CASE 
		WHEN age BETWEEN 20 AND 29 THEN '20-29'
		WHEN age BETWEEN 30 AND 39 THEN '30-39'
		WHEN age BETWEEN 40 AND 49 THEN '40-49'
		WHEN age BETWEEN 50 AND 59 THEN '50-59'
		WHEN age >= 60 THEN '60+'
	END AS age_group,
	sex,
	COUNT(*) AS total_patients
FROM heart
GROUP BY age_group, sex
ORDER BY age_group;


-- Now we add a count of patients with heart disease

SELECT 
	CASE 
		WHEN age BETWEEN 20 AND 29 THEN '20-29'
		WHEN age BETWEEN 30 AND 39 THEN '30-39'
		WHEN age BETWEEN 40 AND 49 THEN '40-49'
		WHEN age BETWEEN 50 AND 59 THEN '50-59'
		WHEN age >= 60 THEN '60+'
	END AS age_group,
	sex,
	COUNT(*) AS total_patients,
	SUM(CASE WHEN heartdisease = 1 THEN 1 ELSE 0 END) as patients_with_heart_disease
FROM heart
GROUP BY age_group, sex
ORDER BY age_group;


-- Finally we will calculate the percentage of heart diseases

SELECT 
	CASE 
		WHEN age BETWEEN 20 AND 29 THEN '20-29'
		WHEN age BETWEEN 30 AND 39 THEN '30-39'
		WHEN age BETWEEN 40 AND 49 THEN '40-49'
		WHEN age BETWEEN 50 AND 59 THEN '50-59'
		WHEN age >= 60 THEN '60+'
	END AS age_group,
	sex,
	COUNT(*) AS total_patients,
	SUM(CASE WHEN heartdisease = 1 THEN 1 ELSE 0 END) as patients_with_heart_disease,
	ROUND((SUM(CASE WHEN HeartDisease = 1 THEN 1 ELSE 0 END)::DECIMAL / COUNT(*)) * 100, 2) AS Heart_Disease_Percentage
FROM heart
GROUP BY age_group, sex
ORDER BY age_group;```
