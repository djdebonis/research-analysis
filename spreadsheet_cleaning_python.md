# Lesson: Cleaning Data in Python (pandas + matplotlib)

**Goal:** Learn a practical, repeatable workflow for cleaning a `.csv` dataset in a Jupyter Notebook so it’s easier to analyze and visualize.

**Tools:** `pandas`, `matplotlib`

---

## Learning Targets

By the end of this lesson, you will be able to:

1. Import a `.csv` file into a Jupyter Notebook using `pandas`.
2. Inspect a dataset to understand its structure and common issues.
3. Rename **column names** so they are consistent and easy to type.
4. Clean up **string values** inside columns (variables) to remove messy spacing and make categories consistent.
5. Make a quick plot using `matplotlib` to confirm your cleaning worked.

---

## Why Data Cleaning Matters

Real-world datasets are messy. Common problems include:

- Column names with spaces: `"Test Score"` instead of `"test_score"`
- Inconsistent labels: `"Freshman"`, `"freshman"`, `" Freshman "`
- Extra spaces: `"New York "` vs `"New York"`
- Missing values: blank cells, `"N/A"`, `"unknown"`

Cleaning makes your code:
- easier to write,
- less error-prone,
- more readable,
- and more consistent for analysis.

---

## Part 1 — Setup and Import Your CSV

In a Jupyter Notebook cell:

```python
import pandas as pd

# Import your dataset (replace filename with yours)
# I also reccomend using a more representative name than just `df`
# Something that tells you what is in the sheet
df = pd.read_csv("your_file.csv")
df.head()
```

## Part 3 — Rename Column Names (Make Them Easy to Call)

### The Problem
Column names often include spaces or inconsistent capitalization, like:

- "Student Name"  
- "Test Score"  
- "Class Period"  

These are annoying to type and easy to mess up.

### The Goal
Use a consistent style:

- lowercase  
- underscores instead of spaces  
- no weird punctuation  

### Example Style
- "Student Name" → "student_name"  
- "Test Score" → "test_score"

# Two Options

You can either (1) Rename the columns manually, or (2) leverage an algorithm to change the existing strings.

### Option 1

```python
df = df.rename(columns={
    "Student Name": "student_name",
    "Test Score": "test_score"
})
```

### Option 2

```python
df.columns = (
    df.columns
      .str.strip()                 # remove leading/trailing spaces
      .str.lower()                 # lowercase everything
      .str.replace(" ", "_")       # replace spaces with underscores
)
```
