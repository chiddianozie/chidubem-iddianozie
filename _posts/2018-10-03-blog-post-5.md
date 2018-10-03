
## Data Test


```python
# import libraries
import numpy as np
import pandas as pd
# from pandas import Series,DataFrame
from scipy import stats
import matplotlib as mpl
import matplotlib.pyplot as plt
import datetime
# pd.set_option('display.max_columns', None) 
%matplotlib inline
```


```python
firstdf = pd.read_csv("dataset1.csv")
```


```python
seconddf = pd.read_csv("dataset2.csv")
```

## Premier League Selections


```python
seconddf['toverByPrice'] = ""

def productToverPrice(x, y):
    return x * y

seconddf['toverByPrice'] = seconddf.apply(lambda row: productToverPrice(row['turnover'], row['price']), axis=1)

# create two dataframes (turnover * price) and (sum of turnover)
seconddf_toverByPrice = seconddf.groupby(['date','eventId', 'selection','home','away'])[['toverByPrice']].sum().reset_index()
seconddf_toverSum = seconddf.groupby(['date','eventId', 'selection','home','away'])[['turnover']].sum().reset_index()

# merge both dataframes into one
pre_df = pd.merge(seconddf_toverByPrice, seconddf_toverSum, how='left', on=['date','eventId', 'selection','home','away'])

# calculate APL for all rows (home, away, draw)
pre_df['APL'] = ""

def calculateAPL(x, y):
    return x/y

pre_df['APL'] = pre_df.apply(lambda row: calculateAPL(row['toverByPrice'], row['turnover']), axis=1)
```


```python
# to check if each eventId has three selections
pre_df.groupby(['eventId'])[['selection']].count()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>selection</th>
    </tr>
    <tr>
      <th>eventId</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4217314</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4217315</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4217316</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4217319</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4217321</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4217322</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4217325</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4217327</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4217357</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4217420</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4413699</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4413701</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4413702</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4413705</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4413707</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4413708</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4413711</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4413713</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4413714</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4413717</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4413719</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4413720</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4413722</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4413725</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4413726</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4413728</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4413731</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4413732</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4413734</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4413737</th>
      <td>2</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>4414405</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4414406</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4414408</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4414410</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4414412</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4414414</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4414416</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4414418</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4414420</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4414422</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4414424</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4414426</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4414428</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4414430</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4414432</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4414434</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4414436</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4415138</th>
      <td>3</td>
    </tr>
    <tr>
      <th>4415139</th>
      <td>3</td>
    </tr>
    <tr>
      <th>5414376</th>
      <td>3</td>
    </tr>
    <tr>
      <th>5414377</th>
      <td>3</td>
    </tr>
    <tr>
      <th>5414378</th>
      <td>3</td>
    </tr>
    <tr>
      <th>5414381</th>
      <td>3</td>
    </tr>
    <tr>
      <th>5414382</th>
      <td>3</td>
    </tr>
    <tr>
      <th>5483887</th>
      <td>3</td>
    </tr>
    <tr>
      <th>5500119</th>
      <td>3</td>
    </tr>
    <tr>
      <th>5503425</th>
      <td>3</td>
    </tr>
    <tr>
      <th>5503426</th>
      <td>3</td>
    </tr>
    <tr>
      <th>5506717</th>
      <td>3</td>
    </tr>
    <tr>
      <th>5510651</th>
      <td>3</td>
    </tr>
  </tbody>
</table>
<p>387 rows × 1 columns</p>
</div>




```python
# need to split APLs based on Home, Away, Draw
pre_df['Home_APL'] = ""
pre_df['Away_APL'] = ""
pre_df['Draw_APL'] = ""

# first we strip trailing spaces as it messes with string comparisons
pre_df['selection'] = pre_df['selection'].map(lambda x: x.strip())
pre_df['home'] = pre_df['home'].map(lambda x: x.strip())
pre_df['away'] = pre_df['away'].map(lambda x: x.strip())

# populate the separate APLs
def populateHomeAPL(x,y,z):
    if (x == y):
        return z
pre_df['Home_APL'] = pre_df.apply(lambda row: populateHomeAPL(row['selection'],row['home'],row['APL']), axis=1)

def populateAwayAPL(x,y,z):
    if (x == y):
        return z
pre_df['Away_APL'] = pre_df.apply(lambda row: populateAwayAPL(row['selection'],row['away'],row['APL']), axis=1)

def populateDrawAPL(x, y):
    if (x == 'Draw'):
        return y
pre_df['Draw_APL'] = pre_df.apply(lambda row: populateDrawAPL(row['selection'],row['APL']), axis=1)


# because we have NAs, we fill them with same values to hold place for now - we will have duplicate rows based 
# eventId
pre_df["Home_APL"] = pre_df.groupby("eventId").transform(lambda x: x.fillna(x.mean()))["Home_APL"]
pre_df["Away_APL"] = pre_df.groupby("eventId").transform(lambda x: x.fillna(x.mean()))["Away_APL"]
pre_df["Draw_APL"] = pre_df.groupby("eventId").transform(lambda x: x.fillna(x.mean()))["Draw_APL"]

# drop unnecessary columns
final_df = pre_df.drop(['selection','toverByPrice','turnover','APL'], axis=1)

# we drop those duplicates, keep just one row based on eventId -  our final dataframe (final_df) 
final_df = final_df.drop_duplicates(subset='eventId', keep="first")

# final_df.to_dense().to_csv("finaldf.csv", index = False, sep=',', encoding='utf-8')
final_df.dropna(inplace=True)
```

## Premier League SPs
This is where we clean the second dataset


```python
firstdf = firstdf.drop(["home.goals","away.goals","competition"],axis=1)

firstdf['Home_TruPrice'] = ""
firstdf['Away_TruPrice'] = ""
firstdf['Draw_TruPrice'] = ""
```

## We populate the columns here


```python
# compute true price for all selections as a mean of back and lay prices
def calculateHomeTP(x, y):
    return (x+y)/2
firstdf['Home_TruPrice'] = firstdf.apply(lambda row: calculateHomeTP(row['home.back'], row['home.lay']), axis=1)

def calculateAwayTP(x, y):
    return (x+y)/2
firstdf['Away_TruPrice'] = firstdf.apply(lambda row: calculateAwayTP(row['away.back'], row['away.lay']), axis=1)

def calculateDrawTP(x, y):
    return (x+y)/2
firstdf['Draw_TruPrice'] = firstdf.apply(lambda row: calculateDrawTP(row['draw.back'], row['draw.lay']), axis=1)

# strip trailing spaces as a precaution
firstdf['home'] = firstdf['home'].map(lambda x: x.strip())
firstdf['away'] = firstdf['away'].map(lambda x: x.strip())
```

## non-uniform spellings
we will follow the convention in the PremierLeagueSelections file
- PremierLeagueSelections , PremierLeagueSPs
- Hull, Hull City
- West Ham, West Ham United
- Bournemouth, AFC Bournemouth
- Manchester Utd, Manchester United



```python
def correctSpellings(x):
    if(x=='Hull City'):
        return 'Hull'
    if(x=='West Ham United'):
        return 'West Ham'
    if(x=='AFC Bournemouth'):
        return 'Bournemouth'
    if(x=='Manchester United'):
        return 'Manchester Utd'
    else:
        return x
    
firstdf['home'] = firstdf.apply(lambda row: correctSpellings(row['home']), axis=1)
firstdf['away'] = firstdf.apply(lambda row: correctSpellings(row['away']), axis=1)
```

## Merge the two dataframes


```python
trialdf = pd.merge(final_df, firstdf, how='left', on=['date','home','away'])
trialdf = trialdf.drop(["home.back","home.lay","draw.back","draw.lay","away.back","away.lay"],axis=1)
```


```python
# trialdf
# trialdf.loc[trialdf['home'] == 'Arsenal']
# firstdf.loc[firstdf['home'] == 'Arsenal']
```


```python
trialdf.info()
# len(trialdf)
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 382 entries, 0 to 381
    Data columns (total 10 columns):
    date             382 non-null object
    eventId          382 non-null int64
    home             382 non-null object
    away             382 non-null object
    Home_APL         382 non-null float64
    Away_APL         382 non-null float64
    Draw_APL         382 non-null float64
    Home_TruPrice    380 non-null float64
    Away_TruPrice    380 non-null float64
    Draw_TruPrice    380 non-null float64
    dtypes: float64(6), int64(1), object(3)
    memory usage: 32.8+ KB



```python
trialdf.dropna(inplace=True)
```


```python
trialdf.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 380 entries, 0 to 381
    Data columns (total 10 columns):
    date             380 non-null object
    eventId          380 non-null int64
    home             380 non-null object
    away             380 non-null object
    Home_APL         380 non-null float64
    Away_APL         380 non-null float64
    Draw_APL         380 non-null float64
    Home_TruPrice    380 non-null float64
    Away_TruPrice    380 non-null float64
    Draw_TruPrice    380 non-null float64
    dtypes: float64(6), int64(1), object(3)
    memory usage: 32.7+ KB


## Getting the Margin


```python
trialdf['CompareHomeTruAPL'] = ""
trialdf['CompareAwayTruAPL'] = ""
trialdf['CompareDrawTruAPL'] = ""
```


```python
# workingdf

def compareAPLTruePrice(x,y):
    a = ((1/x)-(1/y))
    b = a/(1/x)
    return b

trialdf['CompareHomeTruAPL'] = trialdf.apply(lambda row: compareAPLTruePrice(row['Home_APL'], row['Home_TruPrice']), axis=1)
trialdf['CompareAwayTruAPL'] = trialdf.apply(lambda row: compareAPLTruePrice(row['Away_APL'], row['Away_TruPrice']), axis=1)
trialdf['CompareDrawTruAPL'] = trialdf.apply(lambda row: compareAPLTruePrice(row['Draw_APL'], row['Draw_TruPrice']), axis=1)



```

## Results


```python
print("The percentage return on Home matches is " + str(round(trialdf["CompareHomeTruAPL"].mean() *100,2)) + "%")
```

    The percentage return on Home matches is 3.0%



```python
print("The percentage return on Away Matches is " + str(round(trialdf["CompareAwayTruAPL"].mean()*100,2)) + "%")
```

    The percentage return on Away Matches is 4.46%



```python
print("The percentage return on Draw Matches is " + str(round(trialdf["CompareDrawTruAPL"].mean()*100,2)) + "%")
```

    The percentage return on Draw Matches is 4.81%

