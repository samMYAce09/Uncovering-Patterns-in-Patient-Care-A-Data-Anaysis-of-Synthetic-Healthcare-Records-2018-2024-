
# Uncovering Patterns in Patient Care: A Data Analysis of Synthetic Healthcare Records (2018–2024)


---

## Introduction

The healthcare industry generates vast volumes of data daily, yet the challenge lies in uncovering actionable insights to enhance patient care, optimize hospital operations, and streamline financial decisions. In this project, I explored a rich synthetic healthcare dataset titled “Healthcare Regonet”, which spans admissions from 2018 through 2024. This dataset mimics real-world hospital records and was generated using Python’s Faker library for educational and research purposes.

Analyzing such a dataset offers a valuable opportunity to explore trends in hospital efficiency, financial management, doctor performance, insurance dynamics, and health outcomes. With real healthcare data often being inaccessible due to privacy laws, this synthetic alternative helps us simulate real-world analytical challenges and derive useful, policy-shaping insights for health institutions.

## About the Dataset

This dataset contains 54,966 patient records, offering a comprehensive overview of hospital admissions across various variables. It includes information such as patient demographics, admission details, treatments, billing data, and outcomes. Below is a summary of its structure:

- **Scope**: 54,966 patient records across multiple hospitals, conditions, and time periods.
- **Timeframe**: January 2018 to June 2024.
- **Content**: Admission type, test results, billing, hospital info, etc.
- **File Type**: CSV format loaded and processed with Python (Pandas).
- **Structure**: 21 columns covering demographics, medical info, and admin data.
- **Dataset**: Synthetic but mirrors real-world hospital systems.
- **Records**: 54,966 individual patient entries.
- **Fields**: 21 unique variables including name, age, hospital, doctor, admission and discharge date, medication, medical condition, and billing amount.

## Data Cleaning and Transformation Summary

### 1. Data Loading:
```python
df = pd.read_csv(r"C:\Users\HP\Downloads\healthcare_dataset.csv")
```

### 2. Initial Inspection:
```python
df.info()
```

### 3. Standardization:
```python
object_col = df.select_dtypes(include=['object']).columns
for col in object_col:
    df[col] = df[col].str.title()
    df[col] = df[col].str.strip()
```

### 4. Datetime Conversion:
```python
from datetime import datetime
df['Date of Admission'] = pd.to_datetime(df['Date of Admission'])
df['Discharge Date'] = pd.to_datetime(df['Discharge Date'])
```

### 5. Derived Columns:
```python
# Length of Stay
df['Length of Stay'] = (df['Discharge Date'] - df['Date of Admission']).dt.days

# Admission Month and Day of Week
df['Admitted Month Name'] = df['Date of Admission'].dt.month_name()
df['Admitted Month'] = df['Date of Admission'].dt.month
df['Days of week'] = df['Date of Admission'].dt.day_name()

# Length of Stay Category
bins = [0, 3, 7, 14, df['Length of Stay'].max()]
labels = ['Short Stay (1-3)', 'Moderate Stay (4-7)', 'Long Stay (8-14)', 'Extended Stay (15+)']
df['Length of Stay Category'] = pd.cut(df['Length of Stay'], bins=bins, labels=labels, right=True)

# Age Grouped
bins = [0, 24, 34, 49, 64, 79, df['Age'].max()]
labels = ['13-24yrs', '25-34yrs', '35-49yrs', '50-64yrs', '65-79yrs', '80+yrs']
df['Age Grouped'] = pd.cut(df['Age'], bins=bins, labels=labels, right=True)
```

### 6. Duplicates Handling:
```python
count_duplicates = df.duplicated().sum()
df = df.drop_duplicates().reset_index(drop=True)
```

## Key Insights

### 1. Abnormal Test Results Are More Prevalent in Chronic Conditions
Abnormal test results outnumber normal results across all six chronic medical conditions analyzed. For instance, Diabetes records 3,131 abnormal vs. 3,061 normal results, and Arthritis has 3,156 abnormal vs. 3,000 normal...

### 2. Obesity Is the Leading Cause of Emergency Admissions
Out of all medical conditions, Obesity (3,100 emergency cases) had the highest number of emergency room visits...

### 3. Doctors and Abnormal Result Burden
Doctor Michael Smith handled the highest number of patients with abnormal test results (12 cases)...

### 4. Highest Average Billing by Insurance Provider
Patients insured under Medicare had the highest average billing amount at $25,628.32 per admission...

### 5. Most Common Condition Among Extended Stays
Patients who stayed longer than 15 days were most frequently diagnosed with Asthma (4,925 cases)...

### 6. Monthly Trends in Emergency Admissions
July (1,573 cases) had the highest emergency admissions, followed by January (1,547)...

## Other Insights

- Emergency Room Efficiency
- Elderly Patient Distribution
- Emergency Admission Patterns Vary Across the Week
- Seasonal Peaks in Healthcare Spending Reveal High-Cost Months
- Middle-aged and Elderly Patients Bear the Highest Burden of Chronic Conditions
- Short Stays & Medications
- Cost by Condition & Medication

## Recommendations

1. Implement Specialized Emergency Units for Obesity Management  
2. Review High-Billing Insurance Plans like Medicare  
3. Allocate More Staff During Emergency Peak Months  
4. Monitor Doctors with High Abnormal Result Loads  
5. Target Asthma for Extended Stay Reduction Programs  

## Conclusion

This analysis of the Healthcare Regonet dataset not only demonstrated how synthetic data can mimic real-world hospital operations but also uncovered key patterns that are applicable to real healthcare settings...
