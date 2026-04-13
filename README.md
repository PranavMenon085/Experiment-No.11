Experiment No. 11

Name: Pranav Menon

PRN No.: 25070123085

Batch: ENTC - B1

Aim: To create a dataset and load a dataset using the Pandas library in Python, and to
perform exploratory operations on it.


THEORY

1) Introduction to Dataset Creation and Loading

Working with data in Python begins with either creating a dataset programmatically or loading
an existing one from an external source. Pandas provides straightforward tools to accomplish
both tasks. A manually created dataset is useful for quick testing, experimentation, and
learning, while loading a real-world dataset is the standard first step in any data analysis
or machine learning project.

The Pandas library must be imported before any of its features can be used:

    import pandas as pd

NumPy is also frequently imported alongside Pandas for numerical operations and for
representing missing values:

    import numpy as np

Together, these two libraries form the core of nearly every data science workflow in Python.
Pandas is built on top of NumPy, using its ndarray as the underlying data storage mechanism
while adding labeled indexing, heterogeneous column types, and a rich API for data
manipulation.


2) Creating a Dataset Manually

A dataset can be created directly in Python using a Python dictionary, where each key
becomes a column name and the associated list becomes the column's data. This dictionary is
passed to pd.DataFrame() to construct a two-dimensional tabular structure.

    data = {
        "Roll_No":     [101, 102, 103, 104, 105],
        "Gender":      ["Female", "Male", "Female", "Male", "Female"],
        "Department":  ["Computer", "IT", "ENTC", "Mechanical", "Computer"],
        "CGPA":        [8.2, 7.5, 9.1, 6.8, 8.7]
    }
    df = pd.DataFrame(data)
    print(df)

Output:
   Roll_No  Gender  Department  CGPA
0      101  Female    Computer   8.2
1      102    Male          IT   7.5
2      103  Female        ENTC   9.1
3      104    Male  Mechanical   6.8
4      105  Female    Computer   8.7

The leftmost column (0, 1, 2, ...) is the row index, automatically assigned as integers
starting from 0. The data spans mixed types: integers (Roll_No), strings (Gender,
Department), and floating-point numbers (CGPA), all held together in one DataFrame.
Every column in a DataFrame is individually a Pandas Series, sharing the same index.


3) Saving a DataFrame to CSV — df.to_csv()

Once a dataset is created or modified, it can be saved to a CSV (Comma-Separated Values)
file using the to_csv() method. This allows the data to be persisted to disk and reloaded
in future sessions or shared with other tools.

    df.to_csv("students.csv", index=False)

The first argument is the file path or filename. The index=False parameter prevents Pandas
from writing the row integer index (0, 1, 2, ...) as a separate column in the file. If this
parameter is omitted, the index is included by default, causing an unwanted extra column
(often called "Unnamed: 0") when the file is loaded back.

CSV is the most universally compatible tabular data format, supported by Excel, Google
Sheets, databases, and virtually every data science tool. Other export formats supported by
Pandas include df.to_excel(), df.to_json(), and df.to_sql().


4) Loading an External Dataset — pd.read_csv()

Real-world datasets are most commonly stored as CSV files. Pandas provides the pd.read_csv()
function to load such files directly into a DataFrame in a single line of code.

Syntax:
    df = pd.read_csv("path/to/file.csv")

Example (loading the Cars93 dataset in Google Colab):
    import pandas as pd
    import numpy as np
    df = pd.read_csv("/content/Cars93.csv")
    df

The Cars93 dataset contains information about 93 car models with 10 columns: Manufacturer,
Model, Type, Price, MPG.city, AirBags, Horsepower, Passengers, Rear.seat.room, and
Luggage.room. Once loaded, the DataFrame behaves identically to one created manually —
all exploratory and manipulation methods work the same way regardless of how the data was
created. Simply typing df in a Jupyter or Colab cell renders the full DataFrame as an
interactive HTML table; using print(df) shows plain text output.


5) df.shape — Dimensions of the DataFrame

df.shape is an attribute (not a method, so no parentheses are needed) that returns a tuple
of the form (rows, columns) representing the dimensions of the DataFrame.

    print(df.shape)

For Cars93:
    Output: (93, 10)

This confirms the DataFrame has 93 rows and 10 columns. df.shape is the fastest way to
verify that a dataset loaded correctly with the expected number of records and fields. It
is also used to dynamically determine loop ranges, array pre-allocation sizes, and to
confirm the effect of operations like dropna() or filtering that reduce the row count.


6) df.size — Total Number of Elements

df.size is an attribute that returns a single integer equal to the total number of individual
data cells in the DataFrame, calculated as rows × columns.

    print(df.size)

For Cars93:
    Output: 930   (93 rows × 10 columns = 930 total elements)

This value counts every cell, including those that contain NaN (missing values). It differs
from df.shape in that it returns a single number rather than a tuple. For the manually
created student dataset with 5 rows and 4 columns, df.size would return 20.


7) df.info() — Comprehensive Structural Summary

df.info() prints a concise summary of the DataFrame's structure to the console. It is one
of the most information-dense single commands available in Pandas.

    print(df.info())

The output includes:
  - The class type (pandas.core.frame.DataFrame).
  - The RangeIndex, showing the total number of entries and index range.
  - A table of each column name, the count of non-null values, and the dtype.
  - The total number of columns.
  - A breakdown of dtypes present (e.g., float64(3), int64(3), object(4)).
  - The approximate memory usage of the DataFrame.

For Cars93, df.info() immediately reveals that the AirBags column has only 59 non-null
values out of 93 rows, indicating 34 missing entries. This makes df.info() the single most
useful command for understanding data completeness and type correctness in one step.

Unlike df.describe(), which only covers numeric columns, df.info() covers every column
including strings and mixed types.


8) df.describe() — Statistical Summary of Numeric Columns

df.describe() generates a comprehensive statistical summary of all numerical columns in the
DataFrame. The output is itself a DataFrame where columns map to the original numeric columns
and rows are statistical metrics.

    print(df.describe())

Metrics produced for each numeric column:
  - count:  Number of non-null (non-NaN) values in the column.
  - mean:   Arithmetic average of all non-null values.
  - std:    Standard deviation — measures how spread out values are from the mean.
  - min:    The smallest value present.
  - 25%:    First quartile (Q1) — 25% of values fall below this point.
  - 50%:    Median (Q2) — the middle value when sorted.
  - 75%:    Third quartile (Q3) — 75% of values fall below this point.
  - max:    The largest value present.

For Cars93, this reveals that car prices range from approximately 7.4 to 61.9 (thousands),
and that horsepower ranges from 63 to 300 with a mean around 143. The IQR (75% - 25%) is
useful for detecting outliers. Non-numeric columns like Manufacturer and AirBags are
automatically excluded from df.describe() unless dtype='all' is passed as an argument.


9) df.head() and df.tail() — Row Previews

df.head(n) returns the first n rows of the DataFrame, and df.tail(n) returns the last n
rows. The default value of n is 5 for both methods.

    print(df.head())    # first 5 rows
    print(df.tail())    # last 5 rows

These are the most commonly used methods for quickly inspecting a dataset after loading it.
For Cars93, head() shows the first five car models (Acura Integra through BMW 535i) and
tail() shows the last five (Volkswagen Eurovan through Volvo 850). Custom row counts can be
specified: df.head(10) returns the first 10 rows.

These methods are non-destructive — they return a copy of the specified rows and do not
modify the original DataFrame.


10) df.columns — Column Names

df.columns is an attribute that returns a Pandas Index object containing all column names.

    print(df.columns)


11) df.dtypes — Data Type of Each Column

df.dtypes is an attribute that returns a Pandas Series where the index holds column names
and the values are the corresponding Pandas data types.

    print(df.dtypes)

The object dtype indicates string or mixed-type data. int64 is used for whole-number
integers and float64 for decimal numbers. Verifying dtypes before performing calculations
is essential — attempting arithmetic on an object column raises a TypeError or silently
produces incorrect results.


12) df.sample(n) — Random Row Selection

df.sample(n) returns n randomly selected rows from the DataFrame without replacement by
default. Unlike head() and tail(), which always return the same rows, sample() provides
a different random subset on each call.

    print(df.sample(5))

For Cars93, this might return rows for models like Lexus ES300, Dodge Shadow, Audi 100,
Infiniti Q45, and Mazda MPV — a representative mix from different parts of the dataset.
The random_state parameter can be passed to make results reproducible:
    df.sample(5, random_state=42)

This is especially useful for large datasets where it is impractical to inspect all rows.


13) df.isnull().sum() — Missing Value Count per Column

df.isnull() returns a DataFrame of the same shape filled with True where values are missing
(NaN) and False where values are present. Chaining .sum() aggregates this Boolean DataFrame
column-wise, producing the total count of missing values per column.

    print(df.isnull().sum())


This shows that 34 cars have no airbag data, 11 are missing luggage room measurements, and
2 are missing rear seat room measurements. This information directly informs subsequent
preprocessing decisions — whether to drop, fill, or leave these missing values.


14) df.duplicated().sum() — Duplicate Row Detection

df.duplicated() returns a Boolean Series where True marks each row that is an exact
duplicate of any preceding row. Calling .sum() on this gives the total count of duplicates.

    print(df.duplicated().sum())

A result of 0 confirms that all 93 rows are unique records. If duplicates existed, they
could be removed with df.drop_duplicates(inplace=True). Duplicate rows can skew frequency
counts, statistical averages, and model training if left unaddressed.


15) df.nunique — Unique Value Count per Column

df.nunique (accessed as a bound method reference in the experiment) returns the number of
distinct non-null values in each column. The standard callable form is df.nunique().

    print(df.nunique)

This reveals the cardinality of each column. For Cars93, columns like Type (Small, Midsize,
Compact, Large, Sporty, Van) have low cardinality (6 unique values), while Model has high
cardinality (93 unique values, one per row). Low-cardinality columns are typically
categorical; high-cardinality columns are often identifiers or continuous measurements.
Cardinality information is important when deciding which columns to encode, group, or use
as features in a machine learning model.


16) Importance of Exploratory Data Analysis (EDA)

The commands covered in this experiment — pd.read_csv(), df.to_csv(), df.shape, df.size,
df.info(), df.describe(), df.head(), df.tail(), df.columns, df.dtypes, df.sample(),
df.isnull().sum(), df.duplicated().sum(), and df.nunique — collectively form the standard
EDA (Exploratory Data Analysis) workflow. EDA is performed immediately after loading any
new dataset to understand its size, structure, completeness, and statistical properties
before any cleaning, transformation, or modelling takes place.

Skipping EDA and jumping directly to analysis or model building on raw data is a common
source of errors in data science projects. For example, performing a mean imputation on a
column that df.info() would have revealed to be stored as object (string) dtype will silently
fail, producing incorrect results. EDA prevents such mistakes by ensuring that the analyst
fully understands the data before working with it.


CONCLUSION

The creation and loading of datasets using the Pandas library in Python was successfully
completed. 
