# csc172-data-cleaning-connor

# Data Cleaning with AI Support

## Student Information
- Name: Shir Keilah T. Connor
- Course Year: BSCS - 4
- Date: 2025-09-29

## Dataset
- Source: https://www.kaggle.com/competitions/house-prices-advanced-regression-techniques/data
- Name: House Prices - Advanced Regression Techniques

## Issues found
- Missing values: 259 missing values detected, mainly in LotFrontage, and sparse columns (PoolQC, MiscFeature, Alley, Fence) with >80% missing data.
- Duplicates: 0 duplicates rows found.
- Inconsistencies: Category values with inconsistent casing and spacing; standardized to lowercase and uniform labels.
- Outliers: Detected in numerical columns such as SalePrice, LotArea, and GrLivArea.

## Cleaning steps
1. Missing values: 
    [1] Filled 259 missing LotFrontage values using median imputation.
    [2] Dropped columns with excessive missing data: PoolQC, MiscFeature, Alley, Fence.
2. Duplicates:
    [1] Checked and confirmed no duplicate rows using df.duplicated().
3. Inconsistencies:
    [1] Standardized text data to lowercase for 11 categorical columns.
    [2] Trimmed whitespace and normalized category values.
4. Outliers:
    [1] Detected outliers using the Interquartile Range (IQR) method.
    [2] Applied clipping to limit outlier impact using the following logic:
        for col in df.select_dtypes(include=[np.number]).columns:
            Q1, Q3 = df[col].quantile([0.25, 0.75])
            IQR = Q3 - Q1
            lower, upper = Q1 - 1.5 * IQR, Q3 + 1.5 * IQR
            df[col] = np.clip(df[col], lower, upper)

## AI prompts used
- Prompt 1: "Edit this current code to detect and treat outliers using the IQR method by clipping values outside the lower and upper bounds for all numeric columns." 
- Generated code:
    for col in df.select_dtypes(include=[np.number]).columns:
        Q1, Q3 = df[col].quantile([0.25, 0.75])
        IQR = Q3 - Q1
        lower, upper = Q1 - 1.5 * IQR, Q3 + 1.5 * IQR
        df[col] = np.clip(df[col], lower, upper)

## Results
- Rows before: 1460
- Rows after: 1460
- Columns before: 81
- Columns after: 77
- Outliers treated (not dropped): 157 clipped using IQR method
- Standardized category values: 11 total
- Missing values filled: 259 for LotFrontage
- Dropped columns: PoolQC, MiscFeature, Alley, Fence
