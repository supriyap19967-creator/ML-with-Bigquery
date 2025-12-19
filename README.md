# ML-with-Bigquery
# Employee Churn Prediction – Course 1 Mini Project

## Problem
Predict whether an employee will leave the company based on years at the company.
Goal: help HR plan retention strategies.

## Why BigQuery ML & Logistic Regression
- Data is structured → BigQuery ML fits
- Binary classification → logistic regression is suitable
- Simple, fast, low-cost → ideal for a small demo

## Prediction Type
Batch prediction — decisions are made daily, not real-time.

## SQL Snippets

### Create Model
```sql
-- 1️⃣ Create Model
CREATE OR REPLACE MODEL `my_dataset.churn_model`
OPTIONS(
  model_type='logistic_reg',
  input_label_cols=['left_company']
) AS
WITH employee_data AS (
  SELECT 1 AS employee_id, 5 AS years_at_company, 0 AS left_company UNION ALL
  SELECT 2, 2, 1 UNION ALL
  SELECT 3, 10, 0 UNION ALL
  SELECT 4, 1, 1
)
SELECT *
FROM employee_data;

-- 2️⃣ Evaluate Model
SELECT *
FROM ML.EVALUATE(
  MODEL `my_dataset.churn_model`,
  (
    SELECT *
    FROM (
      SELECT 1 AS employee_id, 5 AS years_at_company, 0 AS left_company UNION ALL
      SELECT 2, 2, 1 UNION ALL
      SELECT 3, 10, 0 UNION ALL
      SELECT 4, 1, 1
    )
  )
);

-- 3️⃣ Predict
SELECT employee_id,
       predicted_left_company AS predicted_label,
       predicted_left_company_probs AS predicted_probability
FROM ML.PREDICT(
  MODEL `my_dataset.churn_model`,
  (
    SELECT 5 AS employee_id, 3 AS years_at_company
  )
);

```

## Key Learnings

- Learned how to use **BigQuery ML** to train machine learning models directly using SQL without writing Python code.
- Understood **logistic regression** for binary classification problems such as employee churn prediction.
- Practiced the complete ML workflow in BigQuery using:
  - `CREATE MODEL` to train a model
  - `ML.EVALUATE` to assess model performance
  - `ML.PREDICT` to generate batch predictions
- Gained hands-on experience with **batch prediction** for business decision-making use cases.


## Results

Below are the evaluation metrics obtained after training and evaluating the model using BigQuery ML.

![Screenshot_20-12-2025_2147_github com](https://github.com/user-attachments/assets/5f4795ef-09cf-4b2e-bc63-e47b83031771)

