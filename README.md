# Heart Health SQL Analysis Project

## Project Overview
This project focuses on analyzing a Heart Health Dataset to derive actionable insights into cardiovascular diseases (CVDs). By using SQL queries, we explore patterns, risk factors, and correlations that help identify high-risk groups and potential indicators of heart disease.

## Business Problems:

### 1. Predictive Insights: Identifying High-Risk Groups
- **Goal:** Analyze age and sex to identify groups with the highest prevalence of heart disease.
- **Purpose:** Help healthcare providers target at-risk demographics for preventative care and early intervention.
Correlation Between Lifestyle Factors and Heart Disease

### 2. Correlation Between Lifestyle Factors and Heart Disease
- **Goal:** Examine how factors such as cholesterol, resting blood pressure, and exercise-induced angina correlate with heart disease outcomes.
- **Purpose:** Provide insights into lifestyle or physiological factors contributing to heart disease risk, enabling data-driven public health strategies.

### 3. Chest Pain Type as an Indicator of Heart Disease
- **Goal:** Analyze the distribution of chest pain types and their association with heart disease.
- **Purpose:** Determine whether specific chest pain types can be used as diagnostic indicators for heart disease.

### 4. Threshold Analysis for Risk Factors
- **Goal:** Identify critical thresholds for cholesterol, blood pressure, and maximum heart rate (MaxHR) that are most commonly associated with heart disease.
- **Purpose:** Establish actionable metrics for healthcare professionals to monitor and manage heart health effectively.

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
);
```

## To verify the table and data:
```sql
SELECT * FROM heart;
```
![Table](https://github.com/Parkerjcow/personal_data_projects/blob/Heart-Failure-Predictions/Table.png?raw=true)

# Business Problems and SQL Solutions

## 1. Predictive Insights: Identifying High-Risk Groups

**Objective**

Understand which age and gender groups are at a higher risk of heart disease.
```sql
-- Starting off looking at the column we will be working with for this query

SELECT age, sex, heartdisease
FROM heart;


-- Age groups help summarize data into buckets. 
-- Using CASE statement to create age ranges then create a new column called age_group.

SELECT 
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
ORDER BY age_group;
```
![Business Solution 1 Table](https://github.com/Parkerjcow/personal_data_projects/blob/Heart-Failure-Predictions/Business%20Solution%201%20Table%20.png?raw=true)

## 2. Correlation Between Lifestyle Factors and Heart Disease

**Objective**

Analyze the correlation between cholesterol, resting blood pressure, and exercise-induced angina in patients with and without heart disease.
```sql
-- Look at the columns we will work with

SELECT  restingbp, cholesterol, exerciseangina, heartdisease
FROM heart;


-- Get the average cholesterol and restingbp of patients with and without excercise-induced angina.

SELECT 	
	ExerciseAngina,
    HeartDisease,
    ROUND(AVG(Cholesterol), 2) AS Avg_Cholesterol,
    ROUND(AVG(RestingBP), 2) AS Avg_RestingBP
FROM 
	heart
GROUP BY 
	ExerciseAngina, 
	HeartDisease
ORDER BY 
	ExerciseAngina, 
	HeartDisease;


-- Add in age range and sex and compate heart disease cases vs. normal patients.

SELECT
	CASE 
		WHEN age BETWEEN 20 AND 29 THEN '20-29'
		WHEN age BETWEEN 30 AND 39 THEN '30-39'
		WHEN age BETWEEN 40 AND 49 THEN '40-49'
		WHEN age BETWEEN 50 AND 59 THEN '50-59'
		WHEN age >= 60 THEN '60+'
	END AS age_group,
	sex,
	ROUND(AVG(cholesterol), 2) AS avg_cholesterol,
	ROUND(AVG(restingbp), 2) AS avg_restingbp,
	ExerciseAngina,
    HeartDisease
FROM 
	heart
GROUP BY 
	sex,
	age_group,
	ExerciseAngina,
    HeartDisease
ORDER BY 
	age_group;


-- To further refine the data we can look for patients with hear disease by adding the WHERE statement

SELECT
	CASE 
		WHEN age BETWEEN 20 AND 29 THEN '20-29'
		WHEN age BETWEEN 30 AND 39 THEN '30-39'
		WHEN age BETWEEN 40 AND 49 THEN '40-49'
		WHEN age BETWEEN 50 AND 59 THEN '50-59'
		WHEN age >= 60 THEN '60+'
	END AS age_group,
	sex,
	ROUND(AVG(cholesterol), 2) AS avg_cholesterol,
	ROUND(AVG(restingbp), 2) AS avg_restingbp,
	ExerciseAngina,
    HeartDisease
FROM 
	heart
WHERE 
	heartdisease = 1
GROUP BY 
	sex,
	age_group,
	ExerciseAngina,
    HeartDisease
ORDER BY 
	age_group;
 ```

![Business Solution 2 Table](https://github.com/Parkerjcow/personal_data_projects/blob/Heart-Failure-Predictions/Business%20Solution%202%20Table%20.png?raw=true)

## 3. Chest Pain Type as an Indicator of Heart Disease

**Objective**

Examine if certain chest pain types are stronger indicators of heart disease.

```sql
-- Check to see how chest pain types are distibuted.

SELECT 
	chestpaintype,
	heartdisease
FROM 
	heart
ORDER BY 
	chestpaintype


-- Counting how many total cases of each chest pain type

SELECT
	chestpaintype,
	COUNT(*) AS toal_cases
FROM 
	heart
GROUP BY 
	chestpaintype;
-- Count the number of heart diseases
SELECT
    ChestPainType,
	COUNT (*) AS total_cases
FROM 
	heart
WHERE 
	heartdisease = 1
GROUP BY 
	ChestPainType;


-- We combine the total cases with the amount of heart disease cases.

SELECT
    ChestPainType,
	COUNT (*) AS total_cases,
	SUM(CASE WHEN heartdisease = 1 THEN 1 ELSE 0 END) AS heart_disease_cases
FROM heart
GROUP BY ChestPainType;

-- Find the total proportion of heartdisease cases 

SELECT
    ChestPainType,
	COUNT (*) AS total_cases,
	SUM(CASE WHEN heartdisease = 1 THEN 1 ELSE 0 END) AS heart_disease_cases,
	ROUND((SUM(CASE WHEN heartdisease = 1 THEN 1 ELSE 0 END) * 100) / COUNT(*), 2) as heart_disease_percentage
FROM 
	heart
GROUP BY 
	ChestPainType;


-- Compare patients without heart disease

SELECT
    ChestPainType,
	COUNT (*) AS total_cases,
	SUM(CASE WHEN heartdisease = 1 THEN 1 ELSE 0 END) AS heart_disease_cases,
	ROUND((SUM(CASE WHEN heartdisease = 1 THEN 1 ELSE 0 END) * 100) / COUNT(*), 2) as heart_disease_percentage,
	ROUND((SUM(CASE WHEN HeartDisease = 0 THEN 1 ELSE 0 END) * 100) / COUNT(*), 2) AS No_Heart_Disease_Percentage
FROM 
	heart
GROUP BY 
	ChestPainType;
```
![Business Solution 3 Table](https://github.com/Parkerjcow/personal_data_projects/blob/Heart-Failure-Predictions/Business%20Solution%203%20Table.png?raw=true)
