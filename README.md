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

We begin by creating the heart table to hold the dataset:

sql
Copy code
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

## To verify the table and data:

sql
Copy code
SELECT * FROM heart;
