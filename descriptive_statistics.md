# Descriptive Statistics in Python

By this point your spreadsheet should be clean and ready to work with. If it's not, please see the cleaning data in Python file.

Next, we are going to run what is known as **descriptive statistics** on our data to get a general idea of what is happening before we start plotting and running t-tests.

\*\* please note that what I am going to be demonstrating is not necessarily best practice for saving space on a cpu in a program. However, this will make it easy for you to conceptualize what is happening as we go through the steps.

## Step 1: Import pandas 

You likely already did this, but make sure pandas is imported

`import pandas as pd`

## Step 2: General overview of data

Note that we are going to be using `df` to describe a dataframe. You likely would like to reassign your dataframe to be named something more significant, but examples in pandas tend to use df.

This tells you:

- column names

- how many non-missing values each column has

- what type each column is (number? text?)

`df.info()`

## Step 3: Descriptive statistics for numerical columns

his is the classic descriptive stats table.

`df.describe()`

This usually includes:

count (non-missing values)

mean (average)

std (standard deviation)

min

25%, 50%, 75% (quartiles, where 50% is the median)

max

## Step 4: Descriptive statistics for one column

This is helpful when you want to focus on a single variable, like `test_score`.

`df["test_score"].describe()`
## Step 5: Descriptive statistics for groupings
If you have a grouping column like:

"class_period"

"group" (control vs experimental)

"gender"

"treatment"

(think, in my research, it was walking vs. sitting)

You can compare averages across groups:

`df.groupby("class_period")["test_score"].describe()`

### What is this doing? How does it work?

1) df.groupby("class_period")

This splits your dataframe into separate mini-datasets based on the values in the "class_period" column.

So if class_period has values like:

Period 1

Period 2

Period 3

…then pandas creates three groups, one for each period.

Think of it like: “Make a separate pile of rows for each class period.”

2) ["test_score"]

After the data is split into groups, this says:

“In each group, only look at the test_score column.”

So now, for each class period, pandas is focusing only on the scores.

3) .describe()

Now pandas calculates descriptive statistics for test_score inside each group.

For each class period, you’ll get stats like:

count (how many scores are not missing)

mean (average score)

std (standard deviation)

min (lowest score)

25% (first quartile)

50% (median)

75% (third quartile)

max (highest score)

So instead of one summary for the whole dataset, you get one summary per class period.
