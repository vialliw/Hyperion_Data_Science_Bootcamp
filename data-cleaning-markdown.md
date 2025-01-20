# Data Cleaning Guide: Working with Free Text Data

This guide demonstrates how to clean free text data that should match a finite set of values using Python libraries like pandas, fuzzywuzzy, and chardet. Common scenarios include cleaning product categories, state names, or any other field where free text input should map to a standardized set of values.

## Prerequisites

```bash
pip install pandas fuzzywuzzy python-Levenshtein chardet
```

## Core Cleaning Function

Here's our main cleaning function that handles text normalization and fuzzy matching:

```python
import pandas as pd
import numpy as np
from fuzzywuzzy import fuzz, process
import chardet
from collections import Counter

def clean_text_column(df, column_name, expected_values, threshold=85):
    """
    Clean a text column containing free text that should match a finite set of values.
    
    Parameters:
    -----------
    df : pandas.DataFrame
        Input dataframe
    column_name : str
        Name of the column to clean
    expected_values : list
        List of expected/valid values
    threshold : int
        Fuzzy matching threshold (0-100)
        
    Returns:
    --------
    pandas.DataFrame
        DataFrame with cleaned column and diagnostic information
    """
    # Create a copy to avoid modifying original
    df_clean = df.copy()
    
    # Step 1: Basic cleaning
    df_clean[column_name] = df_clean[column_name].astype(str)
    df_clean[column_name] = df_clean[column_name].str.strip()
    df_clean[column_name] = df_clean[column_name].str.lower()
    
    # Step 2: Remove special characters and extra spaces
    df_clean[column_name] = df_clean[column_name].str.replace(r'[^\w\s]', '', regex=True)
    df_clean[column_name] = df_clean[column_name].str.replace(r'\s+', ' ', regex=True)
    
    # Step 3: Create diagnostic columns
    df_clean[f'{column_name}_original'] = df[column_name]
    df_clean[f'{column_name}_was_cleaned'] = (
        df_clean[column_name] != df_clean[f'{column_name}_original'].str.lower().str.strip()
    )
    
    # Step 4: Fuzzy matching
    def find_best_match(value):
        if value in expected_values:
            return value, 100
        
        best_match = process.extractOne(
            value,
            expected_values,
            scorer=fuzz.ratio
        )
        return best_match if best_match[1] >= threshold else (value, 0)
    
    # Apply fuzzy matching and capture results
    matches = df_clean[column_name].apply(find_best_match)
    df_clean[f'{column_name}_cleaned'] = matches.apply(lambda x: x[0])
    df_clean[f'{column_name}_match_score'] = matches.apply(lambda x: x[1])
    
    # Step 5: Flag potential issues
    df_clean[f'{column_name}_needs_review'] = df_clean[f'{column_name}_match_score'] < 100
    
    return df_clean
```

## Analysis Function

To understand the impact of our cleaning process:

```python
def analyze_cleaning_results(df, column_name):
    """
    Analyze the results of the cleaning process.
    
    Parameters:
    -----------
    df : pandas.DataFrame
        Cleaned dataframe from clean_text_column()
    column_name : str
        Name of the cleaned column
        
    Returns:
    --------
    dict
        Dictionary containing analysis results
    """
    results = {
        'total_rows': len(df),
        'rows_cleaned': df[f'{column_name}_was_cleaned'].sum(),
        'rows_needing_review': df[f'{column_name}_needs_review'].sum(),
        'unique_values_before': df[f'{column_name}_original'].nunique(),
        'unique_values_after': df[f'{column_name}_cleaned'].nunique(),
        'low_confidence_matches': df[df[f'{column_name}_match_score'] < 100][
            [f'{column_name}_original', f'{column_name}_cleaned', f'{column_name}_match_score']
        ].sort_values(f'{column_name}_match_score')
    }
    return results
```

## Example Usage

Here's how to use these functions with sample data:

```python
# Sample data with messy product categories
data = {
    'product_category': [
        'electronics ', 
        'ELECTRONICS',
        'electronic',
        'home & garden',
        'Home and Garden',
        'Home_Garden',
        'sporting goods',
        'sports_goods',
        'Sport Goods',
        'unknown category'
    ]
}

# Expected/valid categories
expected_categories = [
    'electronics',
    'home and garden',
    'sporting goods'
]

# Create DataFrame
df = pd.DataFrame(data)

# Clean the data
cleaned_df = clean_text_column(
    df,
    'product_category',
    expected_categories,
    threshold=85
)

# Analyze results
analysis = analyze_cleaning_results(cleaned_df, 'product_category')
```

## Features and Benefits

1. **Comprehensive Cleaning**
   - Strips whitespace
   - Normalizes case
   - Removes special characters
   - Handles fuzzy matching

2. **Quality Control**
   - Original values preserved
   - Cleaning actions tracked
   - Match confidence scores
   - Low-confidence matches flagged

3. **Analysis Tools**
   - Before/after comparisons
   - Detailed statistics
   - Review recommendations

## Best Practices

1. **Setting the Threshold**
   - Start with a threshold of 85
   - Adjust based on your data quality
   - Higher threshold = stricter matching
   - Lower threshold = more matches but potential errors

2. **Handling Low Confidence Matches**
   - Review matches below 100% confidence
   - Consider manual mapping for frequent mismatches
   - Document decisions for consistency

3. **Encoding Issues**
   If you encounter encoding issues, add this preprocessing step:

```python
def detect_and_fix_encoding(file_path):
    # Read raw bytes
    with open(file_path, 'rb') as file:
        raw_data = file.read()
    
    # Detect encoding
    result = chardet.detect(raw_data)
    encoding = result['encoding']
    
    # Read with detected encoding
    df = pd.read_csv(file_path, encoding=encoding)
    return df
```

## Extending the Solution

Consider these enhancements for specific use cases:

1. Custom Preprocessing:
```python
# Add to clean_text_column before fuzzy matching
df_clean[column_name] = df_clean[column_name].str.replace(
    specific_pattern, 
    replacement, 
    regex=True
)
```

2. Custom Scoring:
```python
# Modify the find_best_match function
best_match = process.extractOne(
    value,
    expected_values,
    scorer=fuzz.token_sort_ratio  # or other scorers
)
```

## Troubleshooting

Common issues and solutions:

1. **Slow Performance**
   - Install python-Levenshtein for faster matching
   - Process in batches for large datasets
   - Consider pre-filtering obvious matches

2. **Memory Issues**
   - Process data in chunks
   - Use dtype optimization
   - Clear unnecessary columns

3. **Poor Matching**
   - Adjust threshold
   - Try different fuzzywuzzy scorers
   - Add custom preprocessing steps

## Next Steps

1. Implement error logging
2. Add validation rules
3. Create custom scorers for specific use cases
4. Build a review workflow for low-confidence matches

Remember to always back up your data before cleaning and validate the results thoroughly.
