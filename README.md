# Exno:1
Data Cleaning Process

# AIM
To read the given data and perform data cleaning and save the cleaned data to a file.

# Explanation
Data cleaning is the process of preparing data for analysis by removing or modifying data that is incorrect ,incompleted , irrelevant , duplicated or improperly formatted. Data cleaning is not simply about erasing data ,but rather finding a way to maximize datasets accuracy without necessarily deleting the information.

# Algorithm
STEP 1: Read the given Data

STEP 2: Get the information about the data

STEP 3: Remove the null values from the data

STEP 4: Save the Clean data to the file

STEP 5: Remove outliers using IQR

STEP 6: Use zscore of to remove outliers

# Coding and Output
```
import pandas as pd
import numpy as np
from scipy import stats

def initial_data_info(df):
    print("Info:")
    print(df.info())
    print("\nDescribe:")
    print(df.describe())
    print("\nNull values per column:")
    print(df.isnull().sum())
    print('=' * 40)

def remove_nulls(df):
    before = df.shape[0]
    df_clean = df.dropna()
    after = df_clean.shape[0]
    print(f"Removed {before - after} rows with null values.")
    return df_clean

def remove_outliers_iqr(df, columns):
    before = df.shape[0]
    for col in columns:
        Q1 = df[col].quantile(0.25)
        Q3 = df[col].quantile(0.75)
        IQR = Q3 - Q1
        lower = Q1 - 1.5 * IQR
        upper = Q3 + 1.5 * IQR
        df = df[(df[col] >= lower) & (df[col] <= upper)]
    after = df.shape[0]
    print(f"Removed {before - after} outliers using IQR method.")
    return df

def remove_outliers_zscore(df, columns, thresh=3):
    before = df.shape[0]
    z_scores = np.abs(stats.zscore(df[columns]))
    filter_mask = (z_scores < thresh).all(axis=1)  # Keep rows where all z-scores < thresh
    df_filtered = df.loc[filter_mask]
    after = df_filtered.shape[0]
    print(f"Removed {before - after} outliers using Z-score method.")
    return df_filtered

file_names = [
    "Data_set.csv",
    "heights.csv",
    "iris.csv",
    "Loan_data.csv",
    "SAMPLEIDS.csv"
]

for file in file_names:
    print(f'Processing file: {file}')
    try:
        df = pd.read_csv(file)
    except FileNotFoundError:
        print(f"File {file} not found. Skipping.")
        print('=' * 80)
        continue
    
    print("Initial Info")
    initial_data_info(df)
    
    df_no_nulls = remove_nulls(df)
    print(f"Shape after removing nulls: {df_no_nulls.shape}")
    
    numeric_cols = df_no_nulls.select_dtypes(include=[np.number]).columns.tolist()
    
    df_iqr = remove_outliers_iqr(df_no_nulls.copy(), numeric_cols)
    print(f"Shape after removing outliers (IQR): {df_iqr.shape}")
    
    df_z = remove_outliers_zscore(df_no_nulls.copy(), numeric_cols)
    print(f"Shape after removing outliers (Z-score): {df_z.shape}")
    print('=' * 80)

```
# Result
          ![data1](https://github.com/user-attachments/assets/e3797309-9356-4a5f-b5c0-e79f6bdbc9b1)
          ![data2](https://github.com/user-attachments/assets/6c97e443-74e6-4662-8534-7e44c03329f4)
          ![data3](https://github.com/user-attachments/assets/3307d280-f33f-4c55-9ebc-9fb5e94575c7)
          ![data4](https://github.com/user-attachments/assets/0a561eae-4682-42a2-8f46-be71868ae786)
          ![data5](https://github.com/user-attachments/assets/a72ecd50-3346-4d6d-9a44-1d5e3eab1495)
          ![data6](https://github.com/user-attachments/assets/6c2d2de4-7b10-4d25-a253-b2c9deb59676)
          ![data7](https://github.com/user-attachments/assets/656c1d4f-f645-41e1-b764-4155e81de6e3)
          ![data8](https://github.com/user-attachments/assets/0e8e3267-6c82-469f-b27d-e312332e7826)
          ![data9](https://github.com/user-attachments/assets/3ef40cf4-c746-410a-9e12-22dac4190234)









