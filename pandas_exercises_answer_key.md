# Pandas DataFrame Exercises - ANSWER KEY

**Dataset**: `class_pipeline_prices_v1_aapl_ge_psct.parquet`

---

## Setup

```python
import pandas as pd
import numpy as np

# Load the data
df = pd.read_parquet('class_pipeline_prices_v1_aapl_ge_psct.parquet')
```

---

## Section 1: Creating & Viewing Data

### Exercise 1.1: First Look

```python
# 1. Display first 5 rows
df.head()

# 2. Display last 10 rows
df.tail(10)

# 3. Print shape
print(df.shape)
# Or
print(f"Rows: {df.shape[0]}, Columns: {df.shape[1]}")

# 4. List all column names
print(df.columns.tolist())
# Or
print(list(df.columns))
```

---

### Exercise 1.2: Data Overview

```python
# 1. Use info()
df.info()

# 2. Use describe()
df.describe()

# 3. Display data types
print(df.dtypes)
# Or
df.dtypes
```

---

## Section 2: Selecting Data

### Exercise 2.1: Column Selection

```python
# 1. Select only ticker column
df['ticker']
# Or
df.ticker

# 2. Select ticker and close
df[['ticker', 'close']]

# 3. Select multiple columns
df[['date', 'ticker', 'open', 'close']]
```

---

### Exercise 2.2: Filtering with Conditions

```python
# 1. Filter for AAPL only
df[df['ticker'] == 'AAPL']

# 2. Filter where close > 150
df[df['close'] > 150]

# 3. GE stock where volume > 50,000,000
df[(df['ticker'] == 'GE') & (df['volume'] > 50000000)]

# 4. Rows in 2023
df[df['date'].dt.year == 2023]
# Or if date is string
df[df['date'].str.contains('2023')]
```

---

### Exercise 2.3: Advanced Selection

```python
# 1. First 100 rows using iloc
df.iloc[:100]
# Or
df.iloc[0:100, :]

# 2. AAPL rows with only date and close using loc
df.loc[df['ticker'] == 'AAPL', ['date', 'close']]

# 3. Rows 50 to 60
df.iloc[50:61]
# Note: 61 because iloc is exclusive of end
```

---

## Section 3: Adding, Deleting, Modifying

### Exercise 3.1: Drop Columns

```python
# 1. Drop volume column
df_new = df.drop('volume', axis=1)
# Or
df_new = df.drop(columns=['volume'])

# 2. Drop multiple columns
df_new = df.drop(['open', 'high', 'low'], axis=1)
# Or
df_new = df.drop(columns=['open', 'high', 'low'])
```

---

### Exercise 3.2: Add New Columns

```python
# 1. Create price_range column
df['price_range'] = df['high'] - df['low']

# 2. Create percent_change column
df['percent_change'] = ((df['close'] - df['open']) / df['open']) * 100

# 3. Create year column
df['year'] = df['date'].dt.year
# Or if date is not datetime
df['year'] = pd.to_datetime(df['date']).dt.year
```

---

### Exercise 3.3: Rename Columns

```python
# 1. Rename close to closing_price
df_renamed = df.rename(columns={'close': 'closing_price'})

# 2. Rename multiple columns
df_renamed = df.rename(columns={
    'open': 'opening_price',
    'high': 'highest_price'
})

# If you want to modify in place:
df.rename(columns={'close': 'closing_price'}, inplace=True)
```

---

## Section 4: Sorting

### Exercise 4.1: Basic Sorting

```python
# 1. Sort by date ascending
df.sort_values('date')
# Or explicitly
df.sort_values('date', ascending=True)

# 2. Sort by close descending
df.sort_values('close', ascending=False)

# 3. Sort by ticker then date
df.sort_values(['ticker', 'date'])
```

---

### Exercise 4.2: Advanced Sorting

```python
# 1. AAPL top 10 trading days by volume
df[df['ticker'] == 'AAPL'].sort_values('volume', ascending=False).head(10)

# 2. Multiple column sorting with mixed order
df.sort_values(['ticker', 'date', 'close'], 
               ascending=[True, False, True])
```

---

## Section 5: Missing Values

### Exercise 5.1: Detect Missing Values

```python
# 1. Check for any missing values
df.isnull().any().any()
# Or
df.isna().any().any()

# 2. Count missing values per column
df.isnull().sum()
# Or
df.isna().sum()

# 3. Show rows with any missing values
df[df.isnull().any(axis=1)]
# Or
df[df.isna().any(axis=1)]
```

---

### Exercise 5.2: Handle Missing Values

```python
# 1. Drop rows with any missing values
df_clean = df.dropna()

# 2. Fill missing volume with 0
df['volume'] = df['volume'].fillna(0)
# Or
df_filled = df.fillna({'volume': 0})

# 3. Fill missing close with mean
df['close'] = df['close'].fillna(df['close'].mean())
# Or
df_filled = df.fillna({'close': df['close'].mean()})
```

---

## Section 6: Grouping & Aggregation

### Exercise 6.1: Basic Grouping

```python
# 1. Average close price by ticker
df.groupby('ticker')['close'].mean()

# 2. Maximum high price by ticker
df.groupby('ticker')['high'].max()

# 3. Count trading days per ticker
df.groupby('ticker').size()
# Or
df.groupby('ticker')['date'].count()
```

---

### Exercise 6.2: Multiple Aggregations

```python
# 1. Mean, min, max for close by ticker
df.groupby('ticker')['close'].agg(['mean', 'min', 'max'])

# 2. Multiple metrics
df.groupby('ticker').agg({
    'close': 'mean',
    'volume': 'sum',
    'close': 'std'  # This will overwrite the mean!
})

# Better approach:
df.groupby('ticker').agg({
    'close': ['mean', 'std'],
    'volume': 'sum'
})
```

---

### Exercise 6.3: Advanced Grouping

```python
# 1. Average annual closing price
df['year'] = df['date'].dt.year
df.groupby(['ticker', 'year'])['close'].mean()

# 2. Total volume per year per ticker
df.groupby(['ticker', 'year'])['volume'].sum()
```

---

## Section 7: Data Transformation

### Exercise 7.1: Apply Functions

```python
# 1. Price categorization
def categorize_price(price):
    if price < 100:
        return 'Low'
    elif price <= 200:
        return 'Medium'
    else:
        return 'High'

df['price_category'] = df['close'].apply(categorize_price)

# Alternative with lambda
df['price_category'] = df['close'].apply(
    lambda x: 'Low' if x < 100 else ('Medium' if x <= 200 else 'High')
)

# 2. Stock direction
df['direction'] = df.apply(
    lambda row: 'Up' if row['close'] > row['open'] else 'Down', 
    axis=1
)

# 3. Map company names
company_names = {
    'AAPL': 'Apple Inc.',
    'GE': 'General Electric',
    'PSCT': 'Prescient'
}
df['company_name'] = df['ticker'].map(company_names)
```

---

### Exercise 7.2: Type Conversion

```python
# 1. Convert volume to int
df['volume'] = df['volume'].astype(int)

# 2. Convert date to datetime
df['date'] = pd.to_datetime(df['date'])

# 3. Convert ticker to category
df['ticker'] = df['ticker'].astype('category')
```

---

## Section 8: Combining Data

### Exercise 8.1: Concatenation

```python
# 1. Split and concatenate
aapl_df = df[df['ticker'] == 'AAPL']
ge_df = df[df['ticker'] == 'GE']
combined = pd.concat([aapl_df, ge_df])

# Or reset index
combined = pd.concat([aapl_df, ge_df], ignore_index=True)

# 2. First and last 100 rows
first_100 = df.head(100)
last_100 = df.tail(100)
combined = pd.concat([first_100, last_100])
```

---

### Exercise 8.2: Merging

```python
# 1. Merge with company names
company_df = pd.DataFrame({
    'ticker': ['AAPL', 'GE', 'PSCT'],
    'company_name': ['Apple Inc.', 'General Electric', 'Prescient']
})
merged = df.merge(company_df, on='ticker', how='left')

# 2. Merge with industry
industry_df = pd.DataFrame({
    'ticker': ['AAPL', 'GE', 'PSCT'],
    'industry': ['Technology', 'Industrial', 'Healthcare']
})
merged = df.merge(industry_df, on='ticker', how='left')
```

---

## Section 9: Reshaping

### Exercise 9.1: Pivot Tables

```python
# 1. Average closing price: tickers × years
df['year'] = df['date'].dt.year
pivot1 = df.pivot_table(
    values='close', 
    index='ticker', 
    columns='year', 
    aggfunc='mean'
)

# 2. Total volume by ticker and month
df['month'] = df['date'].dt.month
pivot2 = df.pivot_table(
    values='volume',
    index='ticker',
    columns='month',
    aggfunc='sum'
)
```

---

### Exercise 9.2: Wide to Long Format

```python
# 1-2. Melt the data
subset = df[['date', 'ticker', 'open', 'close', 'high', 'low']]
melted = subset.melt(
    id_vars=['date', 'ticker'],
    value_vars=['open', 'close', 'high', 'low'],
    var_name='price_type',
    value_name='value'
)
```

---

## Section 10: String Operations

### Exercise 10.1: String Manipulation

```python
# 1. Convert to lowercase
df['ticker_lower'] = df['ticker'].str.lower()

# 2. Check if contains 'A'
df['has_A'] = df['ticker'].str.contains('A')

# 3. Ticker length
df['ticker_length'] = df['ticker'].str.len()
```

---

## Section 11: Time Series Operations

### Exercise 11.1: Date Extraction

```python
# 1. Extract year, month, day of week
df['year'] = df['date'].dt.year
df['month'] = df['date'].dt.month
df['day_of_week'] = df['date'].dt.dayofweek
# Or for day name
df['day_name'] = df['date'].dt.day_name()

# 2. Quarter
df['quarter'] = df['date'].dt.quarter
# Or custom formatting
df['quarter'] = 'Q' + df['date'].dt.quarter.astype(str)

# 3. Filter for Mondays (0 = Monday)
mondays = df[df['date'].dt.dayofweek == 0]
```

---

### Exercise 11.2: Time-based Analysis

```python
# 1. Set date as index
df_indexed = df.set_index('date')

# 2. Monthly average for AAPL
aapl = df[df['ticker'] == 'AAPL'].set_index('date')
monthly_avg = aapl['close'].resample('M').mean()

# 3. Rolling 7-day average
df_sorted = df.sort_values(['ticker', 'date'])
df_sorted['rolling_7day'] = df_sorted.groupby('ticker')['close'].transform(
    lambda x: x.rolling(window=7, min_periods=1).mean()
)
```

---

## Section 12: Other Common Operations

### Exercise 12.1: Duplicates

```python
# 1. Check for duplicate rows
has_duplicates = df.duplicated().any()
print(has_duplicates)

# 2. Check duplicate date-ticker combinations
duplicate_combos = df.duplicated(subset=['date', 'ticker']).any()
print(duplicate_combos)

# 3. Remove duplicates
df_no_dupes = df.drop_duplicates()
# Or for specific columns
df_no_dupes = df.drop_duplicates(subset=['date', 'ticker'])
```

---

### Exercise 12.2: Unique Values and Counts

```python
# 1. Unique tickers
unique_tickers = df['ticker'].unique()
print(unique_tickers)

# 2. Count records per ticker
ticker_counts = df['ticker'].value_counts()
print(ticker_counts)

# 3. Number of unique dates
n_unique_dates = df['date'].nunique()
print(n_unique_dates)
```

---

### Exercise 12.3: Index Operations

```python
# 1. Set date as index
df_indexed = df.set_index('date')

# 2. Multi-index with date and ticker
df_multi = df.set_index(['date', 'ticker'])

# 3. Reset index
df_reset = df_multi.reset_index()
# Or if you want to drop the index
df_reset = df_multi.reset_index(drop=True)
```

---

## Section 13: Export Data

### Exercise 13.1: Save Results

```python
# 1. Export to CSV
df.to_csv('stock_data.csv', index=False)

# 2. Export AAPL to Excel
aapl_df = df[df['ticker'] == 'AAPL']
aapl_df.to_excel('aapl_only.xlsx', index=False)

# 3. Save to parquet
df.to_parquet('stock_data_cleaned.parquet', index=False)
```

---

### Common Mistakes:

1. **Forgetting `axis=1` when dropping columns**
   - `df.drop('column')` vs `df.drop('column', axis=1)`

2. **Not using double brackets for multiple columns**
   - `df['col1', 'col2']` ❌ vs `df[['col1', 'col2']]` ✓

3. **Confusing `loc` and `iloc`**
   - `loc` uses labels, `iloc` uses integer positions

4. **Not using `&` and `|` for multiple conditions**
   - `df[df['a'] > 5 and df['b'] < 10]` ❌
   - `df[(df['a'] > 5) & (df['b'] < 10)]` ✓

5. **Forgetting parentheses in boolean indexing**
   - `df[df['a'] > 5 & df['b'] < 10]` ❌
   - `df[(df['a'] > 5) & (df['b'] < 10)]` ✓

6. **Using `=` instead of `==` in conditions**
   - `df[df['ticker'] = 'AAPL']` ❌
   - `df[df['ticker'] == 'AAPL']` ✓

7. **Not understanding inplace parameter**
   - Many students forget that without `inplace=True`, the original DataFrame isn't modified

8. **Chaining without copy warnings**
   - `df[df['a'] > 5]['b'] = 10` can cause SettingWithCopyWarning

