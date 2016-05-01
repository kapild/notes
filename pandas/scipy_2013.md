### Import pic using IPython display 
```python
from IPython.core.display import HTML
HTML("<iframe src=http://pandas.pydata.org width=800 height=350></iframe>")
```

## Panda Series:
A vector like data:  

```
counts = pd.Series([632, 1638, 569, 115])
counts
```
`Values` and `Index` are its two attributes

```
counts.values
counts.index.values
``` 

One can give labels to index

```
counts = pd.Series([632, 1638, 569, 115], 
    index=['A', 'BA', 'CC', 'DA'])
```

One can then iterate on labels.    
```
bacteria[[name.endswith('A') for name in bacteria.index]]
```

We can give them name if we want.

```
counts.name = xyx
counts.index.name = columns
```

### Filter data
```python
counts[counts > 100]
```

#### Series a dict
```
bacteria_dict = {'Firmicutes': 632, 'Proteobacteria': 1638, 'Actinobacteria': 569, 'Bacteroidetes': 115}
pd.Series(bacteria_dict)

```
Also, can make this Series ordered.
```
bacteria2 = pd.Series(bacteria_dict, index=['Cyanobacteria','Firmicutes','Proteobacteria','Actinobacteria'])
bacteria2
```

#### Is Null?
```java
counts.isnull()
```

## Panda Data frame:
Tabular form of series.
```
data = pd.DataFrame({'value':[632, 1638, 569, 115, 433, 1130, 754, 555]
,'patient':[1, 1, 1, 1, 2, 2, 2, 2],
                     'phylum':['Firmicutes', 'Proteobacteria', 'Actinobacteria', 
    'Bacteroidetes', 'Firmicutes', 'Proteobacteria', 'Actinobacteria', 'Bacteroidetes']})
```

Index is still on 0,1,2

- We can change the order of the data frame 

 ```java
 data[['phylum','value','patient']]
 ```
- Index can use coulumns

	```
	data.columns
	data["coulumn_name"] ---> gives a Series.
	data.coulumn_name
	data[['value']]
	``` 
- Indexing using `ix`

 ```
 data.ix[0]['patient']
 ```	
### Data frame is created using a dict of dicts.
 - You can access the columns using 

 ```
 pd.patient[10] ----> This will return value of the 10 row for coulmn patient.
 

 ```
 - You can add a new coulumn with default value as

 ```
 pd['year'] = 2016
 ``` 
 ### Define a new series as :
 ```
 treatment = pd.Series([0]*4 + [1]*2)
 data['treatment'] = treatment   --> You can assign this to data frame.
 ```
 - We can access the underlying data using the values
 
 ```
 pd.values.shape
 ```
 
 - Index of Series is nothing but `data.index`
   
 ## Accessing files:
 - Read CSV: First row is considered as header
 
	 ```
	 pd.read_csv(path)
	 ```
 - Read table with any seperator
 
	 	```
		pd.read_table("data/microbiome.csv", sep=',')
	 	```	
 -	Hieracrchial index 	
   
	 ```
	 mb = pd.read_csv("data/microbiome.csv", index_col=['Taxon','Patient'])
	 ```
 - Other parameters
 
 	```
 	skiprows=[3,4,6]
 	nrows = 4
 	
 	```
 - Read file with index field `index_col`

 ```
 pd.read_csv("data/baseball.csv", index_col='id')
 ```		 
 
 - Tool tips comes from `SHIFT + TAB` 	

 ### Is NULL:
 ```java
 pd.isnull(data)
 ```
 - You can specify the null values using the na_values arf
 
 	```
 	pd.read_csv("data/microbiome_missing.csv", na_values=['?', -99999]).head(20)
 	```
 ### Indexing:

 - Is unique

 ```
 data.index.is_unique
 ```

  - Read file with index field `index_col`

 ```
 pd.read_csv("data/baseball.csv", index_col='id')
 ```		 
  - Create new index by combining two field
  
  ```
  id = pd.column1 + pd.column2
  data.copy().index = id
  
  ```
   - We can re-index to add new indexing to the data using `reindex`

   ```
   ii = range(data.index.min(), data.index.max())
   data.reindex(ii)
   ```
   - `Missing values` can be filled via 

   ```java
	method='ffill',
	fill_value='mr.nobody',
   ```
   - Drop rows 

   ```
   pd.drop([1,2])
   ```
   - Drop columns 

   ```
   pd.drop(['a','b', axis = 1)
   ```
   - Slice data

   ```
   baseball_newind[baseball_newind.value>500]
   ```
   
   ### STATS on the data
    - `Median`: Get median for some columns. 

   	 ```
	    stats = baseball[['h','X2b', 'X3b', 'hr']]
	    stats.apply(np.median)
	    
	    or 
	    
	    stat_range = lambda x: x.max() - x.min()
	    stats.apply(stat_range)
   	 ```
   	- You can apply stats on one row too using the `axis=1` option.

   	### Sorting the data frame.
   	- Ascending the index 
   	
   	 ```
   		 baseball.sort_index(ascending=False).head()
   	 ```
   	- Sort by value for a Series:
   	
	   	```
   		baseball.hr.order(ascending=False)
	   	```
	- Rank can be used to get instrictic ranking index

	```
	baseball.hr.rank()
	```   	

## Hierarchical indexing
- Combine 3 index: It doesn't repeat multiple values. 

	```
	baseball_h = baseball.set_index(['year', 'team', 'player'])
	
	```	
- Print head

	```
	baseline.index
	```	
- is Unique?

	```
	baseball_h.index.is_unique
	```	
- Acess can be done 
	
	```
	baseball_h.ix[(2007, 'ATL', 'francju01')]
	```	
- Can give names to index and coulumns

	```
	frame.index.names = ['key1', 'key2']
	frame.columns.names = ['state', 'color']
	```	
- Acess can be done 
	
	```
	baseball_h.ix['a']['Ohio']
	or 
	frame.ix['b', 2]['Colorado']
	```	
- You can swap and sort the sub-index too.
	

## Missing Data	
- NaN, null,
- `isnull()`
- Drop missing values using 

	```
	pd.dropna()
	```	
- Drop using is null()
	
	```
	bacteria2[bacteria2.notnull()]
	```	

- Drop na drops all rows even with one NA. You can specify how()

	```
	data.dropna(how='all')
	```	
- Or, you can specify treshold of how many values to be present

	```
	data.dropna(thresh=4)
	```	
- fill na: It fills the na values with `0` or `dict`

	```
	bacteria2.fillna(0)
	data.fillna({'year': 2013, 'treatment':2})
	```
- Fill NA with `mean()`	

	```
	bacteria2.fillna(bacteria2.mean())
	```

## Data Summarization
- `sum`
- `mean()`
- Summing over rows:

	```
	extra_bases = baseball[['X2b','X3b','hr']].sum(axis=1)
	```
- `describe()`: All stats in one	for all coulumns
	
	```
	baseball.describe()
	```
- Describe one column:

	```
	baseball.player.describe()
	```	
- Correlation and Co-variance across two coulumn:

	```
	baseball.hr.cov(baseball.X2b)
	baseball.hr.corr(baseball.X2b)
	```		
- On the entire Data frame: Correlation

	```
	baseball.corr()
	```		
- Sum over hireachical index.	

	```
	mb.sum(level='Taxon')
	```		

### Write data to file 
- CSV

	```
	data.to_csv(file_name)
	```		
- Pickle:

	```
	data.to_pickle(file_name)
	data.read_pickle(file_name)
	```	
	
## Notebook 2

### 1. Date time
- Import date time 

	```
	from datetime import datetime
	now = datetime.now()
	now.day
	now.weekday()
	```
- Just use date or time 

	```
	from datetime import date, time	
	time(3, 24)
	my_age = now - datetime(1970, 9, 3)
	my_age.days/365.
	```
- parsing date from text to date

	```
	datetime.strptime(date_str, '%m/%d/%y %H:%M')	```

- Plot histogram using `series.hist()`

	```
	segments.seg_length.hist(bins=500)
	```
	
### 2. Merging/Join data frames:
- get counts of each type  `value_counts()`

	```
	vessels.column.value_counts()
	```
- Merge  `pd.merge`: Merges on the common coloumn of both the data frame.

	```
	pd.merge(df1, df2)
	pd.merge(df1, df2, how='outer')
	```
- Merge when left has it as a index and right as an coloumn

	```
	pd.merge(vessels, segments, left_index=True, right_on='mmsi')
	or 
	vessels.merge(segments, left_index=True, right_on='mmsi').head()
	```
	
### 3. Concatenate
- Appending across rows and columns 

	```
	np.concatenate([np.random.random(5), np.random.random(5)])
	np.r_[np.random.random(5), np.random.random(5)]
	np.c_[np.random.random(5), np.random.random(5)]	
	```
- Set hierarchical index using two columns. 

	```
	pd.set_index(['patient','obs'])
	
	```

- Unstack one coulumn in multiple rows in same coulumn.

	```
	cdystonia.set_index(['patient','site','id','treat','age','sex','week'])['twstrs'].unstack('week').head()
	```	 

### 3. Pivot
- Pivot helps convert multiple coulumn, obvservation into one row style
	
	```
	cdystonia.pivot(index='patient', columns='obs', values='twstrs').head()
	
	obs       1   2   3   4   5   6
	patient                        
	1        32  30  24  37  39  36
	2        60  26  27  41  65  67
	3        44  20  23  26  35  35
	4        53  61  64  62 NaN NaN
	5        53  35  48  49  41  51
	```
- User pivot table for more 

	```
	cdystonia.pivot_table(rows=['site', 'treat'], cols='week', values='twstrs', aggfunc=max).head(20)
	```
- Group frequencies using cross_tab

## Data transformation
- Drop duplicates 
	
	```
	vessels.drop_duplicates(['names'])
	```
- Update each value of coulumn using `map`
	
	```
	cdystonia.treat.value_counts()
	treatment_map = {'Placebo': 0, '5000U': 1, '10000U': 2}
	cdystonia['treatment'] = cdystonia.treat.map(treatment_map)
	
	- this replaces the all the values of treatment with one specified in the map.
	```
- Same thing using replace. 
	
	```
	vals = vals.replace(0, 1e-6)
	cdystonia2.treat.replace({'Placebo': 0, '5000U': 1, '10000U': 2})
	```
	
- One-hot encoding 	

	```
	pd.get_dummies(vessels5.type).head(10)
	```
 
### Discretization
- Cut data into bins

	```
	cdystonia.age.describe()
	pd.cut(cdystonia.age, [20,30,40,50,60,70,80,90])[:30]
	pd.cut(cdystonia.age, [20,30,40,50,60,70,80,90], right=False)[:30]
	```
- You can also give them names

	```
	pd.cut(cdystonia.age, [20,40,60,80,90], labels=['young','middle-aged','old','ancient'])[:30]	```

### Quantile cut
- You can cut in equal sections of quantile

	```
	pd.qcut(cdystonia.age, 4)[:30]
	```
- Or, you can specify the quantile cuts yourself. 
	
	```
	quantiles = pd.qcut(segments.seg_length, [0, 0.01, 0.05, 0.95, 0.99, 1])
	```
- You can combine discretiztion (cut) with one-hot encoding 
	
	```
	pd.get_dummies(quantiles).head(10)	
	```

### Permutation and Sampling
- `With Replacement`: Generate a sequence of indexed numbers for a given length.
	
	```
	new_order = np.random.permutation(len(segments))
	```
- `Take` to reorder the sequence. 
	
	```
	segments.take(new_order).head()
	```
	
## GroupBy?
- `Group by `: Generate a sequence of indexed numbers for a given length.
	
	```
	cdystonia_grouped = cdystonia.groupby(cdystonia.patient)
	for patient, group in cdystonia_grouped:
	    print patient
	    print group
	    print
	```
- ` split-apply-combine`: Apply aggregate function to groups.
- `Group by `: Generate a sequence of indexed numbers for a given length.
	
	```
	cdystonia_grouped.agg(mean).head()
	```
