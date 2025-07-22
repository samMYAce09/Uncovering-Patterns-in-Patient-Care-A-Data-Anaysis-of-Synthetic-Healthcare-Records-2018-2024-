
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

To ensure analytical accuracy and clarity, several data preparation steps were performed:

### 1. Data Loading:

- Loaded the CSV file using pandas.read_csv() into a DataFrame named df.

```python
df = pd.read_csv(r"C:\Users\HP\Downloads\healthcare_dataset.csv")
```

### 2. Initial Inspection:

* Used .info() to check for nulls, confirm data types, and inspect structure.
* All 21 columns were non-null, including datetime fields and numerical values.

```python
df.info()
```

### 3. Standardization:

* Converted names, hospitals, doctors, and all string-based fields to title case.
* Trimmed whitespaces using str.strip().

```python
object_col = df.select_dtypes(include=['object']).columns
for col in object_col:
    df[col] = df[col].str.title()
    df[col] = df[col].str.strip()
```

### 4. Datetime Conversion:

* Converted Date of Admission and Discharge Date into datetime objects for accurate calculations.

```python
from datetime import datetime
df['Date of Admission'] = pd.to_datetime(df['Date of Admission'])
df['Discharge Date'] = pd.to_datetime(df['Discharge Date'])
```

### 5. Derived Columns:

* Length of Stay: Calculated as Discharge Date - Date of Admission.
* Length of Stay Category: Binned into Short (1–3), Moderate (4–7), Long (8–14), and Extended (15+ days).
* Age Grouped: Created grouped bins by age for demographic analysis.
* Admission Month and Day of Week: Extracted from Date of Admission.

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

* Dropped 534 duplicated rows and reset the DataFrame index.

```python
count_duplicates = df.duplicated().sum()
df = df.drop_duplicates().reset_index(drop=True)
```

## Key Insights

### 1. Abnormal Test Results Are More Prevalent in Chronic Conditions
Abnormal test results outnumber normal results across all six chronic medical conditions analyzed. For instance, Diabetes records 3,131 abnormal vs. 3,061 normal results, and Arthritis has 3,156 abnormal vs. 3,000 normal. Even conditions like Cancer, Asthma, and Obesity show a slight tilt toward abnormal outcomes. While the margins are not drastic, this pattern suggests that these chronic diseases often involve complications or unstable parameters, leading to more frequent diagnostic abnormalities.

This insight highlights the importance of precision diagnostics, frequent monitoring, and tailored follow-ups for patients with chronic illnesses. The slight dominance of abnormal results could also reflect late-stage detection, poor condition control, or limitations in current treatment protocols. Health systems should prioritize early screening, tighter disease management programs, and personalized intervention strategies, especially for patients with Diabetes and Arthritis, where the imbalance is most pronounced. Moreover, a deeper exploration into which specific tests are producing these results could lead to process improvements or the introduction of more advanced diagnostic tools.

### 2. Obesity Is the Leading Cause of Emergency Admissions
Out of all medical conditions, Obesity (3100 emergency cases) had the highest number of emergency room visits. This is medically justifiable, as obesity increases the risk of acute cardiovascular events, respiratory complications, and orthopedic emergencies, all of which may require urgent intervention.

### 3. Doctors and Abnormal Result Burden
Doctor Michael Smith handled the highest number of patients with abnormal test results (12 cases). This might indicate a higher case complexity in his caseload or specialization in critical care. Other doctors with similar high figures include James Smith (8) and David Johnson (8).

### 4. Highest Average Billing by Insurance Provider
Patients insured under Medicare had the highest average billing amount at $25,628.32 per admission. This indicates that elderly patients or those with more severe cases (often covered by Medicare) incur higher treatment costs.

### 5. Most Common Condition Among Extended Stays
Patients who stayed longer than 15 days were most frequently diagnosed with Asthma (4,925 cases). This chronic condition can lead to extended hospitalizations, especially when triggered by environmental factors or poor medication adherence.

### 6. Monthly Trends in Emergency Admissions
Patients who stayed longer than 15 days were most frequently diagnosed with Asthma (4,925 cases). This chronic condition can lead to extended hospitalizations, especially when triggered by environmental factors or poor medication adherence.

## Other Insights

- Emergency Room Efficiency
Hospitals like Group Richards, Smith-Mack, and Johnson-Bell all had an average emergency stay length of just 1 day, suggesting highly efficient emergency discharge protocols.

- Elderly Patient Distribution
Hospitals such as Ltd Smith and Llc Smith had the highest elderly caseload, both recording 15 patients aged 65 or older. This point to a geriatric-focused specialization.

- Emergency Admission Patterns Vary Across the Week
Emergency admissions fluctuate across the week, with Tuesdays recording the highest volume (≈2.63k). This midweek surge may stem from delayed care-seeking over the weekend or symptom escalation after Monday. In contrast, Saturday and Monday see the fewest emergency visits. This pattern suggests hospitals should optimize staffing and triage systems on Tuesdays to manage demand effectively and prevent patient overload.

- Seasonal Peaks in Healthcare Spending Reveal High-Cost Months
The average billing amount per patient spikes in October (≈$26.02k) and June, pointing to seasonal peaks in healthcare spending. These months likely coincide with chronic disease flare-ups or insurance-driven behavior. Conversely, September and May see the lowest costs. These patterns can help hospitals and insurers prepare budgets, manage resources, and anticipate high-expense periods.

- Middle-aged and Elderly Patients Bear the Highest Burden of Chronic Conditions
Across conditions like Diabetes, Hypertension, and Arthritis, age groups 50–64 and 65–79 consistently show the highest patient counts — each with over 4,000 cases combined per condition. Younger age groups have notably lower rates. This reinforces the need for early interventions in adults aged 35+, and targeted public health programs before patients enter high-risk decades. Hospitals should gear resources toward long-term care for older adults.

- Short Stays & Medications
Medications like Lipitor (1,119 short stays) and Ibuprofen (1,112) were most associated with short hospital stays, potentially indicating milder conditions & highly effective treatment.

- Cost by Condition & Medication
The costliest treatment combo was Diabetes + Paracetamol ($26,398.78), followed by Asthma + Ibuprofen ($26,240.45). These could highlight where chronic diseases combined with specific drugs drive high resource use.


## Recommendations

1. Implement Specialized Emergency Units for Obesity Management
**why?** With 3,100 emergency admissions from obesity alone, there’s a critical need to address obesity-related complications promptly. Having dedicated units can reduce repeat visits and improve care efficiency.

2. Review High-Billing Insurance Plans like Medicare 
**Why?** Medicare patients averaged $25,628.32 in bills — highest among all insurers. A billing audit can uncover whether longer stays, advanced treatments, or chronic management are inflating costs.
3. Allocate More Staff During Emergency Peak Months 
**Why?** Emergency spikes in July (1,573) and January (1,547) stress hospital systems. Proactive staffing and preparedness can reduce wait times and improve patient outcomes.
4. Monitor Doctors with High Abnormal Result Loads  
**Why?** Doctors like Michael Smith (12 abnormal cases) may be dealing with more severe patient profiles. Providing them support or additional diagnostics resources can enhance decision-making and reduce burnout.
5. Target Asthma for Extended Stay Reduction Programs  
**Why?** Asthma accounted for 4,925 extended stays. Introducing asthma education, medication adherence monitoring, and air quality alerts may significantly reduce hospitalization time.

## Conclusion

This analysis of the Healthcare Regonet dataset not only demonstrated how synthetic data can mimic real-world hospital operations but also uncovered key patterns that are applicable to real healthcare settings. From identifying conditions that lead to emergency spikes, to understanding cost structures and length of stay drivers, the findings offer rich insights for hospitals, insurers, and policy makers. With effective recommendations such as targeted care programs and resource reallocation, institutions can leverage data to make informed decisions that enhance patient outcomes and operational efficiency.
