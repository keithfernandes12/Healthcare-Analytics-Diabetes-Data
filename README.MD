# Diabetes Prediction Using Demographic Data and Body Measurements

A machine learning project that predicts diabetes status using data from the **[National Health and Nutrition Examination Survey (NHANES) 2013–2014](https://www.cdc.gov/nchs/nhanes/about_nhanes.htm)**, a program run by the CDC's National Center for Health Statistics that combines interviews, physical examinations, and laboratory tests on a nationally representative sample of U.S. adults and children.

## Dataset

The analysis merges five NHANES components into a single dataset of **9,813 participants**:

| Component | File | Key Variables Used |
|-----------|------|--------------------|
| **Laboratory** | `labs.csv` | LBXGH - Glycohemoglobin (%) |
| **Examination** | `examination.csv` | BMXARMC - Arm Circumference (cm), BMDAVSAD - Avg. Sagittal Abdominal Diameter (cm), MGDCGSZ - Combined Grip Strength (kg) |
| **Demographic** | `demographic.csv` | RIAGENDR - Gender, DMDYRSUS - Years in the US, INDFMPIR - Family Income-to-Poverty Ratio |
| **Diet** | `diet.csv` | DRABF - Breast-fed indicator |
| **Questionnaire** | `questionnaire.csv` | Supporting survey data |

All datasets are joined on `SEQN` (Respondent Sequence Number).

## Target Variable

Diabetes status is classified into three categories using Glycohemoglobin (HbA1c) thresholds:

| Class | Label | HbA1c Range |
|-------|-------|-------------|
| 0 | Normal | < 6.0% |
| 1 | High-Risk / Prediabetes | 6.0% – 6.4% |
| 2 | Diabetes | ≥ 6.5% |

## Project Structure

```
├── Healthcare_Analytics_Diabetes_Data.ipynb   # Main analysis notebook
├── demographic.csv                            # NHANES demographic data (10,175 rows)
├── examination.csv                            # Physical examination data (9,813 rows)
├── labs.csv                                   # Laboratory test results (9,813 rows)
├── diet.csv                                   # Dietary recall data (9,813 rows)
├── questionnaire.csv                          # Survey questionnaire data (10,175 rows)
└── README.md
```

## Analysis Pipeline

### 1. Loading and Preparing the Data
- Merge all five NHANES datasets on respondent ID
- Rename columns for readability
- Handle missing values (forward-fill for income, median imputation for body measurements, NaN replacement for years in US)
- Select the 8 relevant features for modeling

### 2. Creating the Models

Six models are trained and evaluated:

| Model | Description |
|-------|-------------|
| **Linear Regression** | Baseline continuous regression model |
| **AdaBoost** | Adaptive boosting with Decision Tree base estimators (100 estimators, learning rate 0.5) |
| **Bagging - Decision Tree** | Bootstrap-aggregated Decision Trees (50% samples, 50% features) |
| **Bagging - KNeighbors** | Bootstrap-aggregated KNN classifiers (50% samples, 50% features) |
| **Gradient Boosting** | Sklearn's GradientBoostingClassifier with default parameters |
| **MLP Neural Network** | Multi-layer perceptron with hidden layers (1000, 300, 300), Adam optimizer |

### 3. Summarizing Performance

Model accuracies are compared in a bar chart. Best performing models achieve ~88% accuracy on the test set.

## Results

| Model | Test Accuracy |
|-------|--------------|
| Bagging - KNeighbors | 88.1% |
| MLP Neural Network | 88.1% |
| Gradient Boosting | 87.4% |
| Bagging - Decision Tree | 87.0% |
| AdaBoost | 86.5% |
| Linear Regression | 72.9% |

## Requirements

- Python 3.x
- NumPy
- Pandas
- Scikit-learn
- Seaborn
- Matplotlib

Install dependencies:

```bash
pip install numpy pandas scikit-learn seaborn matplotlib
```

## Usage

1. Clone the repository:
   ```bash
   git clone https://github.com/keithfernandes12/Healthcare-Analytics-Diabetes-Data.git
   cd Healthcare-Analytics-Diabetes-Data
   ```

2. Ensure all five CSV files are in the same directory as the notebook.

3. Open and run the notebook:
   ```bash
   jupyter notebook Healthcare_Analytics_Diabetes_Data.ipynb
   ```

## Data Source

All data is publicly available from the [NHANES 2013–2014 survey cycle](https://wwwn.cdc.gov/nchs/nhanes/continuousnhanes/default.aspx?BeginYear=2013):

- [Demographic Variables](https://wwwn.cdc.gov/Nchs/Nhanes/Search/variablelist.aspx?Component=Demographics&CycleBeginYear=2013)
- [Dietary Data](https://wwwn.cdc.gov/Nchs/Nhanes/Search/variablelist.aspx?Component=Dietary&CycleBeginYear=2013)
- [Examination Data](https://wwwn.cdc.gov/Nchs/Nhanes/Search/variablelist.aspx?Component=Examination&CycleBeginYear=2013)
- [Laboratory Data](https://wwwn.cdc.gov/Nchs/Nhanes/Search/variablelist.aspx?Component=Laboratory&CycleBeginYear=2013)
- [Questionnaire Data](https://wwwn.cdc.gov/Nchs/Nhanes/Search/variablelist.aspx?Component=Questionnaire&CycleBeginYear=2013)

Thousands of research findings have been [published using NHANES data](https://www.ncbi.nlm.nih.gov/pubmed?orig_db=PubMed&term=NHANES&cmd=search).
