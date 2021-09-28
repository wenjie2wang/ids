# Pandas

## History of Pandas
Developer Wes McKinney started working on pandas in 2008 while at AQR
Capital Management out of the need for a high performance, flexible tool
to perform quantitative analysis on financial data. Before leaving AQR
he was able to convince management to allow him to open source the library.


```python
## Installing and Checking version of Pandas:
# !pip3 install pandas

## Loading pandas
import pandas as pd

## Loading numpy as well
import numpy as np

## Pandas Version
pd.__version__
```

Note that an easy way to find the functions in Pandas library can
be obtained by typing in pd.<TAB> in jupyter notebook. More detailed
documentation, along with tutorials and other resources, can be
found at http://pandas.pydata.org/.

## Pandas Objects

At a basic level, Pandas objects provide an extension to numpy array
by identifying rows, columns by labels instead of simple indices 0, 1.
This can recollect memories to dataframes for those who used R.

At first, let us start with introducing 
 - `Series`
 - `DataFrame`
 - `Index`

A Pandas Series is a one-dimensional array of indexed data. It can be
created from a list or array as follows:

### Series


```python
data = pd.Series([0.25, 0.5, 0.75, 1.0])
data
```

As can be seen above, it is a numpy array but with index `0-3`


```python
data.values
```


```python
data.index
```


```python
print(f"data value at index 1 is {data[1]}")    ## f string in python.
data[0:3]  
```


```python
data = pd.Series([0.25, 0.5, 0.75, 1.0],
 index=['a', 'b', 'c', 'd'])

data['b']
```

#### Series as Dictionary

Quick to note that series may also resemble dictionary due to its index
seemingly resembling keys in dictionary. This was clarified by creating
a pandas series using a dictionary.


```python
population_dict = {'California': 38332521,
 'Texas': 26448193,
 'New York': 19651127,
 'Florida': 19552860,
 'Illinois': 12882135}
population = pd.Series(population_dict)

print(f"Dictionary way: {population_dict['Texas']}")
print(f"PD Series way: {population['Texas']}")
```

Difference however is that, the indices can still be used to get a
range of values from PD Series by doing slicing and so on, 
while it is not the same in a dictionary


```python
population['California':'Florida']
```


```python
try: 
    print(population_dict['California':'Florida'])
except:
    print(f"Failure to do Slicing")
```

- For a list or a numpy array, index defaults to integer sequence
- Can also be a dictionary in which case, key becomes index
- In all cases, it can be explicitly specified too. 
- Also, one can identify only a set of keys to be considered,
  like a set of stocks or so on.
  
These above points are exemplified below:


```python
## From a list
pd.Series([2, 4, 6])
```


```python
## Repeating numbers when index set is longer than data
pd.Series(5, index = [1, 2, 3])
```


```python
# This uses dictionary but also indexes a smaller set 
pd.Series({2:'a', 1:'b', 3:'c'}, index=[3, 2]) 
```

### DataFrames

If a Series is an analog of a one-dimensional array with flexible indices, a DataFrame
is an analog of a two-dimensional array with both flexible row indices and flexible
column names. Just as you might think of a two-dimensional array as an ordered
sequence of aligned one-dimensional columns, you can think of a DataFrame as a
sequence of aligned Series objects. Here, by “aligned” we mean that they share the
same index.


```python
area_dict = {'California': 423967, 'Texas': 695662, 'New York': 141297,
 'Florida': 170312, 'Illinois': 149995}
area = pd.Series(area_dict)

## A dataframe is a dictionary with keys as column names and values as pd.Series
states = pd.DataFrame({'population': population,
 'area': area})

states
```

Panda dataframes can be constructed in various ways: 

- Using single series. 
    ```
    pd.DataFrame(population, columns = ['population'])
    ```
- From a list of dicts
    ```
    data = [{'a':i, 'b':2*i} for i in range(3)]
    pd.DataFrame(data)
    ```
-  From a dictionary of Series objects
    This was shown above.
- From a two-dimensional numpy array.
    ```
    pd.DataFrame(np.random.rand(3,2), 
    columns = ['foo', 'bar'], index = ['a', 'b', 'c'])
    ```

## Pandas Index:

- Pandas index is an immutable array. It is like an array in many ways
  but cannot be modified unlike numpy arrays. 


```python
ind = pd.Index([2, 3, 5, 7, 11])
ind
```


```python
## Accessing index
print(ind[1])
print(ind[::2])  ## notice the difference between a single : and double ::
print(ind[:2])
```


```python
try:
    ind[1] = 0
except:
    "Error found"
```


```python
indA =  pd.Index([1,3, 5, 7, 9])
indB = pd.Index([2,3, 5, 7, 11])

indA & indB  #intersection
```


```python
## deprecated warnings
indA | indB
```

## Data Indexing and Selection:


```python
data = pd.Series([0.25, 0.5, 0.75, 1.0],
index=['a', 'b', 'c', 'd'])
data
```


```python
data['b']     ## accessing the b index location
```


```python
data.keys()     ## also gives the index object
```


```python
list(data.items())   ## gives both keys and values
```

The following mutability of the objects is a convenient feature: under
the hood, Pandas is making decisions about memory layout and data
copying that might need to take place; the user generally does not
need to worry about these issues.


```python
data['e'] = 1.25
data
```


```python
print("slicing by explicit index")
print(data['a':'c'])

print("slicing by implicit integer index")
print(data[0:2])

print("masking (paranthesis have a lot of importance)")
print(data[(data > 0.3) & (data < 0.8)])

print("fancy indexing")
print(data[['a','e']])

## remember when you are accessing like a list ":" won't work
```

## Indexers, loc, iloc, ix

**Note** These slicing and indexing conventions can be a source of
confusion. For example, if your Series has an explicit integer index,
an indexing operation such as `data[1]` will use the explicit indices,
while a slicing operation like `data[1:3]` will use the implicit
Python-style index.


```python
data = pd.Series(['a', 'b', 'c'], index=[1, 3, 5])
data[3]
```


```python
data[1:3]
```


```python
## thus 'loc' always slices using explicit index
print(data.loc[1])
print(data.loc[1:3])

# does show the value in index 3
```


```python
# and 'iloc' uses implicit python index
print(data.iloc[1])
print(data.iloc[1:3])

## doesn't show the valuee in location 3
```

## Data Selection in Dataframe


```python
area = pd.Series({'California': 423967, 'Texas': 695662,
'New York': 141297, 'Florida': 170312,
'Illinois': 149995})
pop = pd.Series({'California': 38332521, 'Texas': 26448193,
'New York': 19651127, 'Florida': 19552860,
'Illinois': 12882135})
data = pd.DataFrame({'area':area, 'pop':pop})
data
```


```python
print(data['area'])
print(data.area)

print(data.area is data['area'])
## wont work if area is also a special command for dataframes
```


```python
print(data.pop is data['pop'])
print(data.pop)
```


```python
data['density'] =  data['pop']/data['area']
data
```

## DataFrame as two-dimensional array

As mentioned previously, we can also view the DataFrame as an enhanced
twodimensional array. We can examine the raw underlying data array
using the values attribute:


```python
data.values
```


```python
data.T    ## for Transpose
```


```python
data.values[0]  ## accesses a row (like a dictionary)
```


```python
data.iloc[:3, :2]
```


```python
data.loc[:'Florida', :'pop']
```


```python
data.ix[:3, :'pop']   ## hybrid of iloc and loc

## maybe removed since.
```


```python
data.loc[data.density > 100, ['pop', 'density']]
```

## Operating on Data in Pandas

Pandas inherits much of this functionality from NumPy, and the ufuncs
that we introduced in “Computation on NumPy Arrays: Universal
Functions”on page 50 are key to this.

## Ufuncs: Index Preservation


```python
rng = np.random.RandomState(42)
ser = pd.Series(rng.randint(0, 10, 4))
print(ser)

df = pd.DataFrame(rng.randint(0, 10, (3, 4)),
columns=['A', 'B', 'C', 'D'])
print(df)
```

If we apply a NumPy ufunc on either of these objects, the result will
be another Pandas object with the indices preserved:


```python
np.exp(ser)
```


```python
np.sin(df*np.pi/4)
```

## UFuncs: Index Alignment




```python
area = pd.Series({'Alaska': 1723337, 'Texas': 695662,
'California': 423967}, name='area')
population = pd.Series({'California': 38332521, 'Texas': 26448193,
'New York': 19651127}, name='population')

print(population / area)

print(population.divide(area, fill_value=0))
```