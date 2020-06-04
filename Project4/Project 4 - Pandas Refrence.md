# Project 4 - Pandas Reference

## General Notes
* If you're using CoLab you can run `%load_ext google.colab.data_table` to view DataFrames in a more interactive fashion.  And you can revert back the regular pandas view by running `%unload_ext google.colab.data_table` \
More info [here](https://colab.research.google.com/notebooks/data_table.ipynb#scrollTo=jcQEX_3vHOUz)

## Comparing column names
* If you want to compare precinct names with code you will probably want to standardize their format.\
  Suggestions:
	  * To standardize case:\
		 `‌">>> AbCd".upper()
		 "ABCD"`\
     `>>> ‌"AbCd".lower()
     "abcd"`
    * To strip punctuation:\
			`import string
		>>> s = "string. @With. %^Punctuation?*(*)"
		>>> s.translate(str.maketrans('', '', string.punctuation))
		"string With Punctuation"`
* Finding non-matching values:
	* Suppose we have two DataFrames `df1 = pd.DataFrame({"A": [10, 20, 23]})` and `df2 = pd.DataFrame({"B": [30, 10, 20]})`.  The non-matching values are 23 and 30.
	
		`>>> import pandas as pd
    >>> df1 = pd.DataFrame({"A": [10, 20, 23]})
    >>> df2 = pd.DataFrame({"B": [30, 10, 20]})
    >>> df1
	        A
    0  10
    1  20
    2  23
    >>> df2
	        B
    0  30
    1  10
    2  20
    >>> # Show rows in df1 where A value is not in df2.B
    >>> df1[~df1.A.isin(df2.B)]
            A
    2  23
    >>> # Show rows in df2 where B value is not in df1.A
    >>> df2[~df2.B.isin(df1.A)]
            B
    0  30`
    
    ## Merging precincts
    * Have new columns with the finalized precinct name.  Precincts to be merged are given the same name.  
	    * If the split is in the shape file, then you can use the [dissolve function in GeoPandas](https://geopandas.org/aggregation_with_dissolve.html) to create a new GeoDataFrame with those entries aggregated.
	    * If it is in the election results you can use the [pandas groupby function ](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.groupby.html)(this is similar to in behavior to the dissolve function, although it returns a [groupby object](https://pandas.pydata.org/pandas-docs/stable/reference/groupby.html). In practice what this means is a aggregation function like `.sum()` or `.first()` must be called on it to return a DataFrame)
	* Another option is to manually edit the values.
		* The [pandas at function](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.at.html) will let you do this.
		* `df.at[4, 'B']` returns the value at row 4 column B
		* `df.at[4, 'B'] = 10` set the value at row 4 column B to 10 