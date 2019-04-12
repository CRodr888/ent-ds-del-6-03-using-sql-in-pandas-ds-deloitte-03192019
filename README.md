
# Using SQL with Pandas

## Introduction

It turns out that you can write SQL to retrieve and mutate information within a DataFrame. We wanted to make sure to provide you with at least a brief introduction to the topic on your last day of the course!

## Objectives
You will be able to:
- Use the basic syntax for querying pandas DataFrames with SQL statements
- Explain how Pandas can be used to work directly with .sql files
- Reference the pandasql package documentation as needed

## Pandas and SQL

Consider the structure of a **_Pandas DataFrame_**.  

<img src="images/df_example2.png" width="850">


Now, let's consider the structure of a table from a **_SQL database_**.


<img src="images/sql_example2.png">

You've probably noticed by now that they're essentially the same--a table of values, with each row having a unique index and each column having a unique name.  Even better, the Pandas team has implemented the ability to retrieve information from a DatFrame using SQL, allowing you to leverage SQL for working with data in a DataFrame.

## Using `.query()`

Pandas DataFrames come with a built-in query method, which allows you to get information from DataFrames quickly without using the cumbersome slicing syntax.  

See the following examples:

```python
# Getting Data using slicing syntax
foo_df = bar_df[bar_df[bar_df['Col_1'] > bar_df['Col_2']]]

# Using The query method
foo_df = bar_df.query("Col_1 > Col_2")

# These two lines are equivalent!
```

Note that if you want to use `AND` and `OR` statements with the `.query()` method, you'll need to use `"&"` and `"|"` instead.

```python
foo_df = bar_df.query("Col_1 > Col_2 & Col_2 <= Col_3")

```

## Using SQL syntax with `pandasql`

Since SQL is such a powerful tool for Data Scientists, some people had the bright idea of creating a library that lets users query DataFrames using SQL-style syntax.  This library is called [pandasql](https://pypi.org/project/pandasql/).

We can install `pandasql` using the bash command `pip install pandasql`.

### Importing pandasql

In order to use `pandasql`, we need to start by importing a `sqldf` object from `pandasql`

```python
from pandasql import sqldf
```

Next, it's helpful to write a lambda function that will make it quicker and easier to write queries.  Normally, you would have to pass in the global variables every time we use an object.  In order to avoid doing this every time, here's how to write a lambda that does this for you:

```python
pysqldf = lambda q: sqldf(q, globals())
```

Now, when you pass a query into `pysqldf`, the lambda will also pass along the globals, saving you that repetitive task. 

### Writing Queries

To write a query, you just format it as a multi-line string!

```python
q = """SELECT
        m.date, m.beef, b.births
     FROM
        meats m
     INNER JOIN
        births b
           ON m.date = b.date;"""
```

In order to query DataFrames, you can just pass in the query string you've created to our `sqldf` object that you stored in `pysqldf`.  This will return a DataFrame.  

```python
results = pysqldf(q)
```

## Summary

These advanced methods for querying DataFrames can make your life a lot easier by simplifying the syntax and allowing us to make use of SQL. Use them to save yourself time and give keep your SQL skills strong!
