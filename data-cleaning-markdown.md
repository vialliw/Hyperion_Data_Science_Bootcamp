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
    df_