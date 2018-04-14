---
layout: post
title: 'Pandas Basic Overview'
date: 2018-04-14 17:20:19 -0600
categories: pandas ds
---

# Pandas Basics

Much like the [Numpy Basics Overview](https://cdrainxv.github.io/blog/numpy/python/2018/03/28/numpy-basics.html), this will be a quite terse overview of the package. The focus will be on simple and common use cases such as importing the package, creating and reading data files, shaping it, transforming it, etc. Pandas has a very simply written documentation that's easy to read and access (especially on a Jupyter notebook).

## Import Pandas And Read Data


```python
import pandas as pd

nba_players = pd.read_csv('nba_players.csv')
nba_players.head()
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
      <th>player</th>
      <th>team</th>
      <th>pos</th>
      <th>number</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>K. Durant</td>
      <td>GS Warriors</td>
      <td>SF</td>
      <td>35</td>
    </tr>
    <tr>
      <th>1</th>
      <td>J. Harden</td>
      <td>Houston Rockets</td>
      <td>SG</td>
      <td>13</td>
    </tr>
    <tr>
      <th>2</th>
      <td>K.A Towns</td>
      <td>Minnesota Timberwolves</td>
      <td>C</td>
      <td>32</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Kyrie Irving</td>
      <td>Boston Celtics</td>
      <td>PG</td>
      <td>11</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Anthony Davis</td>
      <td>NO Pelicans</td>
      <td>PF</td>
      <td>23</td>
    </tr>
  </tbody>
</table>
</div>



Voilá! Look how it pretty it looks (on this notebook, but hopefully my css table stylings do it justice). Everything is arranged in a nicely formatted table with rows, columns, and headings.

### Attributes & Methods For Simple Exploring

In most scenarios when we first load data we take explore it. This is to get a sense of what analyses we can perform on it as well as to see if it's tidy or requires further cleaning on our part. In this **very** simple dataset there's not much to explore and nothing to tidy, but regardless of that, let's see some methods.


```python
first = nba_players.head(1)
sample = nba_players.sample(1)
print(first)
print(sample)
```

          player         team pos  number
    0  K. Durant  GS Warriors  SF      35
          player         team pos  number
    0  K. Durant  GS Warriors  SF      35



```python
nba_players.columns
```




    Index(['player', 'team', 'pos', 'number'], dtype='object')




```python
nba_players.describe
```




    <bound method NDFrame.describe of           player                    team pos  number
    0      K. Durant             GS Warriors  SF      35
    1      J. Harden         Houston Rockets  SG      13
    2      K.A Towns  Minnesota Timberwolves   C      32
    3   Kyrie Irving          Boston Celtics  PG      11
    4  Anthony Davis             NO Pelicans  PF      23>




```python
# General info about column names, type of data, length of data, and memory usage
nba_players.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 5 entries, 0 to 4
    Data columns (total 4 columns):
    player    5 non-null object
    team      5 non-null object
    pos       5 non-null object
    number    5 non-null int64
    dtypes: int64(1), object(3)
    memory usage: 240.0+ bytes


# Add New Data

### Merge DataFrames


```python
# New DataFrame with same heading and type of data as initial nba_players 
nba_2 = [
    {'player': 'Kawhi Leonard', 'team': 'SA Spurs', 'pos': 'SF', 'number': 2},
    {'player': 'Damian Lilard', 'team': 'Portland Trailblazers', 'pos': 'PG', 'number': 0},
    {'player': 'Klay Thompson', 'team': 'GS Warriors', 'pos': 'SG', 'number': 11},
    {'player': 'Rudy Gobert', 'team': 'Utah Jazz', 'pos': 'C', 'number': 27},
    {'player': 'Kevin Love', 'team': 'Cleveland Cavaliers', 'pos': 'PF', 'number': 0},
]

nba_players_2 = pd.DataFrame(nba_2)
nba_players_2
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
      <th>number</th>
      <th>player</th>
      <th>pos</th>
      <th>team</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>Kawhi Leonard</td>
      <td>SF</td>
      <td>SA Spurs</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>Damian Lilard</td>
      <td>PG</td>
      <td>Portland Trailblazers</td>
    </tr>
    <tr>
      <th>2</th>
      <td>11</td>
      <td>Klay Thompson</td>
      <td>SG</td>
      <td>GS Warriors</td>
    </tr>
    <tr>
      <th>3</th>
      <td>27</td>
      <td>Rudy Gobert</td>
      <td>C</td>
      <td>Utah Jazz</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>Kevin Love</td>
      <td>PF</td>
      <td>Cleveland Cavaliers</td>
    </tr>
  </tbody>
</table>
</div>



The merging process is similar to SQL JOINs.


```python
# right returns only the new dataframe
nba_players.merge(nba_players_2, how='right')
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
      <th>player</th>
      <th>team</th>
      <th>pos</th>
      <th>number</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Kawhi Leonard</td>
      <td>SA Spurs</td>
      <td>SF</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Damian Lilard</td>
      <td>Portland Trailblazers</td>
      <td>PG</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Klay Thompson</td>
      <td>GS Warriors</td>
      <td>SG</td>
      <td>11</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Rudy Gobert</td>
      <td>Utah Jazz</td>
      <td>C</td>
      <td>27</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Kevin Love</td>
      <td>Cleveland Cavaliers</td>
      <td>PF</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# left returns only the initial dataframe
nba_players.merge(nba_players_2, how='left')
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
      <th>player</th>
      <th>team</th>
      <th>pos</th>
      <th>number</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>K. Durant</td>
      <td>GS Warriors</td>
      <td>SF</td>
      <td>35</td>
    </tr>
    <tr>
      <th>1</th>
      <td>J. Harden</td>
      <td>Houston Rockets</td>
      <td>SG</td>
      <td>13</td>
    </tr>
    <tr>
      <th>2</th>
      <td>K.A Towns</td>
      <td>Minnesota Timberwolves</td>
      <td>C</td>
      <td>32</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Kyrie Irving</td>
      <td>Boston Celtics</td>
      <td>PG</td>
      <td>11</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Anthony Davis</td>
      <td>NO Pelicans</td>
      <td>PF</td>
      <td>23</td>
    </tr>
  </tbody>
</table>
</div>




```python
# outer returns new dataframe of combined initial and new dataframe
nba_players = nba_players.merge(nba_players_2, how='outer')
nba_players
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
      <th>player</th>
      <th>team</th>
      <th>pos</th>
      <th>number</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>K. Durant</td>
      <td>GS Warriors</td>
      <td>SF</td>
      <td>35</td>
    </tr>
    <tr>
      <th>1</th>
      <td>J. Harden</td>
      <td>Houston Rockets</td>
      <td>SG</td>
      <td>13</td>
    </tr>
    <tr>
      <th>2</th>
      <td>K.A Towns</td>
      <td>Minnesota Timberwolves</td>
      <td>C</td>
      <td>32</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Kyrie Irving</td>
      <td>Boston Celtics</td>
      <td>PG</td>
      <td>11</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Anthony Davis</td>
      <td>NO Pelicans</td>
      <td>PF</td>
      <td>23</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Kawhi Leonard</td>
      <td>SA Spurs</td>
      <td>SF</td>
      <td>2</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Damian Lilard</td>
      <td>Portland Trailblazers</td>
      <td>PG</td>
      <td>0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Klay Thompson</td>
      <td>GS Warriors</td>
      <td>SG</td>
      <td>11</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Rudy Gobert</td>
      <td>Utah Jazz</td>
      <td>C</td>
      <td>27</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Kevin Love</td>
      <td>Cleveland Cavaliers</td>
      <td>PF</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



### Concat DataFrames


```python
nba_misc_details = [
    {'coach': 'Steve Kerr', 'arena': 'Oracle Arena', 'championships': 5},
    {'coach': 'Mike D\'Antoni', 'arena': 'Toyota Center', 'championships': 2},
    {'coach': 'Tom Thibodeau', 'arena': 'Target Center', 'championships': 0},
    {'coach': 'Brad Stevens', 'arena': 'TD Center', 'championships': 17},
    {'coach': 'Alvin Gentry', 'arena': 'Smoothie King Center', 'championships': 0},
    {'coach': 'Gregg Popovich', 'arena':'AT&T Center', 'championships': 5},
    {'coach': 'Terry Stotts', 'arena': 'Moda Center', 'championships': 1},
    {'coach': 'Steve Kerr', 'arena': 'Oracle Arena', 'championships': 5},
    {'coach': 'Quin Snyder', 'arena': 'Vivint Smart Home Arena', 'championships': 0},
    {'coach': 'Tyronn Lue', 'arena': 'Quicken Loans Arena', 'championships': 1},
]

nba_misc = pd.DataFrame(nba_misc_details)

# concat behaves like an append, attaching the new dataframe either as row data or column data
nba_players_details = pd.concat([nba_players, nba_misc], axis=1)
nba_players_details
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
      <th>player</th>
      <th>team</th>
      <th>pos</th>
      <th>number</th>
      <th>arena</th>
      <th>championships</th>
      <th>coach</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>K. Durant</td>
      <td>GS Warriors</td>
      <td>SF</td>
      <td>35</td>
      <td>Oracle Arena</td>
      <td>5</td>
      <td>Steve Kerr</td>
    </tr>
    <tr>
      <th>1</th>
      <td>J. Harden</td>
      <td>Houston Rockets</td>
      <td>SG</td>
      <td>13</td>
      <td>Toyota Center</td>
      <td>2</td>
      <td>Mike D'Antoni</td>
    </tr>
    <tr>
      <th>2</th>
      <td>K.A Towns</td>
      <td>Minnesota Timberwolves</td>
      <td>C</td>
      <td>32</td>
      <td>Target Center</td>
      <td>0</td>
      <td>Tom Thibodeau</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Kyrie Irving</td>
      <td>Boston Celtics</td>
      <td>PG</td>
      <td>11</td>
      <td>TD Center</td>
      <td>17</td>
      <td>Brad Stevens</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Anthony Davis</td>
      <td>NO Pelicans</td>
      <td>PF</td>
      <td>23</td>
      <td>Smoothie King Center</td>
      <td>0</td>
      <td>Alvin Gentry</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Kawhi Leonard</td>
      <td>SA Spurs</td>
      <td>SF</td>
      <td>2</td>
      <td>AT&amp;T Center</td>
      <td>5</td>
      <td>Gregg Popovich</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Damian Lilard</td>
      <td>Portland Trailblazers</td>
      <td>PG</td>
      <td>0</td>
      <td>Moda Center</td>
      <td>1</td>
      <td>Terry Stotts</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Klay Thompson</td>
      <td>GS Warriors</td>
      <td>SG</td>
      <td>11</td>
      <td>Oracle Arena</td>
      <td>5</td>
      <td>Steve Kerr</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Rudy Gobert</td>
      <td>Utah Jazz</td>
      <td>C</td>
      <td>27</td>
      <td>Vivint Smart Home Arena</td>
      <td>0</td>
      <td>Quin Snyder</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Kevin Love</td>
      <td>Cleveland Cavaliers</td>
      <td>PF</td>
      <td>0</td>
      <td>Quicken Loans Arena</td>
      <td>1</td>
      <td>Tyronn Lue</td>
    </tr>
  </tbody>
</table>
</div>



# Edit Entire Column

It seems that the we have attributed to the players the wrong championships information. Those values are actually the total championships for the teams. We would actually like that column to be for individual championships.


```python
nba_players_details['championships'] = [1, 0, 0, 1, 0, 1, 0, 2, 0, 1]
nba_players_details
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
      <th>player</th>
      <th>team</th>
      <th>pos</th>
      <th>number</th>
      <th>arena</th>
      <th>championships</th>
      <th>coach</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>K. Durant</td>
      <td>GS Warriors</td>
      <td>SF</td>
      <td>35</td>
      <td>Oracle Arena</td>
      <td>1</td>
      <td>Steve Kerr</td>
    </tr>
    <tr>
      <th>1</th>
      <td>J. Harden</td>
      <td>Houston Rockets</td>
      <td>SG</td>
      <td>13</td>
      <td>Toyota Center</td>
      <td>0</td>
      <td>Mike D'Antoni</td>
    </tr>
    <tr>
      <th>2</th>
      <td>K.A Towns</td>
      <td>Minnesota Timberwolves</td>
      <td>C</td>
      <td>32</td>
      <td>Target Center</td>
      <td>0</td>
      <td>Tom Thibodeau</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Kyrie Irving</td>
      <td>Boston Celtics</td>
      <td>PG</td>
      <td>11</td>
      <td>TD Center</td>
      <td>1</td>
      <td>Brad Stevens</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Anthony Davis</td>
      <td>NO Pelicans</td>
      <td>PF</td>
      <td>23</td>
      <td>Smoothie King Center</td>
      <td>0</td>
      <td>Alvin Gentry</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Kawhi Leonard</td>
      <td>SA Spurs</td>
      <td>SF</td>
      <td>2</td>
      <td>AT&amp;T Center</td>
      <td>1</td>
      <td>Gregg Popovich</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Damian Lilard</td>
      <td>Portland Trailblazers</td>
      <td>PG</td>
      <td>0</td>
      <td>Moda Center</td>
      <td>0</td>
      <td>Terry Stotts</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Klay Thompson</td>
      <td>GS Warriors</td>
      <td>SG</td>
      <td>11</td>
      <td>Oracle Arena</td>
      <td>2</td>
      <td>Steve Kerr</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Rudy Gobert</td>
      <td>Utah Jazz</td>
      <td>C</td>
      <td>27</td>
      <td>Vivint Smart Home Arena</td>
      <td>0</td>
      <td>Quin Snyder</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Kevin Love</td>
      <td>Cleveland Cavaliers</td>
      <td>PF</td>
      <td>0</td>
      <td>Quicken Loans Arena</td>
      <td>1</td>
      <td>Tyronn Lue</td>
    </tr>
  </tbody>
</table>
</div>



# Rearrange Columns


```python
nba_players_details.columns
```




    Index(['player', 'team', 'pos', 'number', 'arena', 'championships', 'coach'], dtype='object')




```python
nba_players_details = nba_players_details[['player', 'number', 'team', 'pos', 'championships', 'coach', 'arena']]
nba_players_details
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
      <th>player</th>
      <th>number</th>
      <th>team</th>
      <th>pos</th>
      <th>championships</th>
      <th>coach</th>
      <th>arena</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>K. Durant</td>
      <td>35</td>
      <td>GS Warriors</td>
      <td>SF</td>
      <td>1</td>
      <td>Steve Kerr</td>
      <td>Oracle Arena</td>
    </tr>
    <tr>
      <th>1</th>
      <td>J. Harden</td>
      <td>13</td>
      <td>Houston Rockets</td>
      <td>SG</td>
      <td>0</td>
      <td>Mike D'Antoni</td>
      <td>Toyota Center</td>
    </tr>
    <tr>
      <th>2</th>
      <td>K.A Towns</td>
      <td>32</td>
      <td>Minnesota Timberwolves</td>
      <td>C</td>
      <td>0</td>
      <td>Tom Thibodeau</td>
      <td>Target Center</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Kyrie Irving</td>
      <td>11</td>
      <td>Boston Celtics</td>
      <td>PG</td>
      <td>1</td>
      <td>Brad Stevens</td>
      <td>TD Center</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Anthony Davis</td>
      <td>23</td>
      <td>NO Pelicans</td>
      <td>PF</td>
      <td>0</td>
      <td>Alvin Gentry</td>
      <td>Smoothie King Center</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Kawhi Leonard</td>
      <td>2</td>
      <td>SA Spurs</td>
      <td>SF</td>
      <td>1</td>
      <td>Gregg Popovich</td>
      <td>AT&amp;T Center</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Damian Lilard</td>
      <td>0</td>
      <td>Portland Trailblazers</td>
      <td>PG</td>
      <td>0</td>
      <td>Terry Stotts</td>
      <td>Moda Center</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Klay Thompson</td>
      <td>11</td>
      <td>GS Warriors</td>
      <td>SG</td>
      <td>2</td>
      <td>Steve Kerr</td>
      <td>Oracle Arena</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Rudy Gobert</td>
      <td>27</td>
      <td>Utah Jazz</td>
      <td>C</td>
      <td>0</td>
      <td>Quin Snyder</td>
      <td>Vivint Smart Home Arena</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Kevin Love</td>
      <td>0</td>
      <td>Cleveland Cavaliers</td>
      <td>PF</td>
      <td>1</td>
      <td>Tyronn Lue</td>
      <td>Quicken Loans Arena</td>
    </tr>
  </tbody>
</table>
</div>



# Rename Columns


```python
nba_players_details.columns = ['Player', 'Number', 'Team', 'Position', 'Rings', 'Coach', 'Arena']
nba_players_details
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
      <th>Player</th>
      <th>Number</th>
      <th>Team</th>
      <th>Position</th>
      <th>Rings</th>
      <th>Coach</th>
      <th>Arena</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>K. Durant</td>
      <td>35</td>
      <td>GS Warriors</td>
      <td>SF</td>
      <td>1</td>
      <td>Steve Kerr</td>
      <td>Oracle Arena</td>
    </tr>
    <tr>
      <th>1</th>
      <td>J. Harden</td>
      <td>13</td>
      <td>Houston Rockets</td>
      <td>SG</td>
      <td>0</td>
      <td>Mike D'Antoni</td>
      <td>Toyota Center</td>
    </tr>
    <tr>
      <th>2</th>
      <td>K.A Towns</td>
      <td>32</td>
      <td>Minnesota Timberwolves</td>
      <td>C</td>
      <td>0</td>
      <td>Tom Thibodeau</td>
      <td>Target Center</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Kyrie Irving</td>
      <td>11</td>
      <td>Boston Celtics</td>
      <td>PG</td>
      <td>1</td>
      <td>Brad Stevens</td>
      <td>TD Center</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Anthony Davis</td>
      <td>23</td>
      <td>NO Pelicans</td>
      <td>PF</td>
      <td>0</td>
      <td>Alvin Gentry</td>
      <td>Smoothie King Center</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Kawhi Leonard</td>
      <td>2</td>
      <td>SA Spurs</td>
      <td>SF</td>
      <td>1</td>
      <td>Gregg Popovich</td>
      <td>AT&amp;T Center</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Damian Lilard</td>
      <td>0</td>
      <td>Portland Trailblazers</td>
      <td>PG</td>
      <td>0</td>
      <td>Terry Stotts</td>
      <td>Moda Center</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Klay Thompson</td>
      <td>11</td>
      <td>GS Warriors</td>
      <td>SG</td>
      <td>2</td>
      <td>Steve Kerr</td>
      <td>Oracle Arena</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Rudy Gobert</td>
      <td>27</td>
      <td>Utah Jazz</td>
      <td>C</td>
      <td>0</td>
      <td>Quin Snyder</td>
      <td>Vivint Smart Home Arena</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Kevin Love</td>
      <td>0</td>
      <td>Cleveland Cavaliers</td>
      <td>PF</td>
      <td>1</td>
      <td>Tyronn Lue</td>
      <td>Quicken Loans Arena</td>
    </tr>
  </tbody>
</table>
</div>



# Edit Single Column Data


```python
nba_players_details.iloc[3]
```




    Player        Kyrie Irving
    Number                  11
    Team        Boston Celtics
    Position                PG
    Rings                    1
    Coach         Brad Stevens
    Arena            TD Center
    Name: 3, dtype: object




```python
nba_players_details.loc[3, 'Arena']
```




    'TD Center'




```python
nba_players_details.loc[3, 'Arena'] = 'Boston Garden'
nba_players_details
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
      <th>Player</th>
      <th>Number</th>
      <th>Team</th>
      <th>Position</th>
      <th>Rings</th>
      <th>Coach</th>
      <th>Arena</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>K. Durant</td>
      <td>35</td>
      <td>GS Warriors</td>
      <td>SF</td>
      <td>1</td>
      <td>Steve Kerr</td>
      <td>Oracle Arena</td>
    </tr>
    <tr>
      <th>1</th>
      <td>J. Harden</td>
      <td>13</td>
      <td>Houston Rockets</td>
      <td>SG</td>
      <td>0</td>
      <td>Mike D'Antoni</td>
      <td>Toyota Center</td>
    </tr>
    <tr>
      <th>2</th>
      <td>K.A Towns</td>
      <td>32</td>
      <td>Minnesota Timberwolves</td>
      <td>C</td>
      <td>0</td>
      <td>Tom Thibodeau</td>
      <td>Target Center</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Kyrie Irving</td>
      <td>11</td>
      <td>Boston Celtics</td>
      <td>PG</td>
      <td>1</td>
      <td>Brad Stevens</td>
      <td>Boston Garden</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Anthony Davis</td>
      <td>23</td>
      <td>NO Pelicans</td>
      <td>PF</td>
      <td>0</td>
      <td>Alvin Gentry</td>
      <td>Smoothie King Center</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Kawhi Leonard</td>
      <td>2</td>
      <td>SA Spurs</td>
      <td>SF</td>
      <td>1</td>
      <td>Gregg Popovich</td>
      <td>AT&amp;T Center</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Damian Lilard</td>
      <td>0</td>
      <td>Portland Trailblazers</td>
      <td>PG</td>
      <td>0</td>
      <td>Terry Stotts</td>
      <td>Moda Center</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Klay Thompson</td>
      <td>11</td>
      <td>GS Warriors</td>
      <td>SG</td>
      <td>2</td>
      <td>Steve Kerr</td>
      <td>Oracle Arena</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Rudy Gobert</td>
      <td>27</td>
      <td>Utah Jazz</td>
      <td>C</td>
      <td>0</td>
      <td>Quin Snyder</td>
      <td>Vivint Smart Home Arena</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Kevin Love</td>
      <td>0</td>
      <td>Cleveland Cavaliers</td>
      <td>PF</td>
      <td>1</td>
      <td>Tyronn Lue</td>
      <td>Quicken Loans Arena</td>
    </tr>
  </tbody>
</table>
</div>



# Total Number of Championships


```python
total_rings = nba_players_details['Rings'].sum()
total_rings
```




    6




```python
nba_players_details.groupby('Team', axis=0)['Player'].count()
```




    Team
    Boston Celtics            1
    Cleveland Cavaliers       1
    GS Warriors               2
    Houston Rockets           1
    Minnesota Timberwolves    1
    NO Pelicans               1
    Portland Trailblazers     1
    SA Spurs                  1
    Utah Jazz                 1
    Name: Player, dtype: int64



Of the teams represented in the table only the GS Warriors have two players, whereas all the other teams have one player apiece.


```python
nba_players_details.groupby('Rings', axis=0)['Player'].count()
```




    Rings
    0    5
    1    4
    2    1
    Name: Player, dtype: int64



Five players have no rings, 4 have one, and only one player has two rings.

That's where this post will be wrapped up. We have looked at some common ways to work with data using the Pandas package. There are a multitude of even more specific uses that we didn't delve into, but in the upcoming posts we'll reiterate some of the above operations as well as some new ones.
