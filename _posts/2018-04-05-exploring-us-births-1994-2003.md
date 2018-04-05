---
layout: post
title: Exploring US Births 1994 - 2003
---

# Exploring U.S Births 1994 - 2003

There are a myriad of file extensions, some of which I probably won't ever come across, but there are some common filetypes that are used in Data Science. One of them is just a plain textfile with comma-separated vales, hence the extension **csv**. Here's an example:

```python
player,team,pos,number
K. Durant,Golden State Warriors,SF,35
L. James,Cleveland Cavaliers,SF,23
J. Harden,Houston Rockets,SG,13
```

Notice the first row is the header, and the actual data lines up with each header.
For example, K. Durant is the name of the **player**, his **team** is the Golden State Warriors, he plays at the SF (small forward) **pos**ition and his jersey **number** is 35.
This makes it not only easy to read visually -- at least if there are only a few headers. Additionally, computers can easily read and organize data that is formatted as such, regardless of how many headers and/or how many rows of values in the file.

How do read such data to a computer, though? Let's read a file with the same data as above into the computer's memory and see how.

```python
f = open('nba_example.csv', mode='r')
f
```

    <_io.TextIOWrapper name='nba_example.csv' mode='r' encoding='UTF-8'>

We just opened the file in read mode. This means we just want to read it just like a book; we have no interest in editing it all. Now that it's opened, we have to read the data inside.

```python
data1 = f.read()
data1
```

    'player,team,pos,number\nK. Durant,Golden State Warriors,SF,35\nL. James,Cleveland Cavaliers,SF,23\nJ. Harden,Houston Rockets,SG,13\nR. Westbrook,OKC Thunder,PG,0\nJ. Embiid,Philidelphia 76ers,21\n'

```python
data2 = f.readlines()
data2
```

['player,team,pos,number\n',
'K. Durant,Golden State Warriors,SF,35\n',
'L. James,Cleveland Cavaliers,SF,23\n',
'J. Harden,Houston Rockets,SG,13\n',
'R. Westbrook,OKC Thunder,PG,0\n',
'J. Embiid,Philidelphia 76ers,21\n']

We have several ways to read files into memory. Two of them are shown above, **read()** and **readlines()**. **read** returns our file in a long continuous **_string_**, while **readlines()** returns it in a **_list_**. Notice the escaped characters for newlines and carriage returns. The **read()** option seems the easier one to work with for the above file, there we need to remove the newline characters by splitting it at **\n**. This returns a list with each data row being a distinct element.

```python
nba_list = data1.split('\n')
nba_list
```

    ['player,team,pos,number',
     'K. Durant,Golden State Warriors,SF,35',
     'L. James,Cleveland Cavaliers,SF,23',
     'J. Harden,Houston Rockets,SG,13',
     'R. Westbrook,OKC Thunder,PG,0',
     'J. Embiid,Philidelphia 76ers,21',
     '']

That's about the gist of it. The above data is quite limited to work with as it's very small and there's nothing of note to further investigate given the limited headers. Alas if only it had interesting information such as fg, fga, 3P%, ppg, etc.

Once we are done reading a file, then logically we would close it afterward like so:

```python
f.close()
```

NOTE: We can make use of a [**context manager**](https://jeffknupp.com/blog/2016/03/07/python-with-context-managers/) to open and close our files automatically, thus freeing up some memory.

## U.S Births 1994 - 2003

However, there's another file that's much meatier which we can use to not only pose our own questions, but in turn answer them as well. Here's some of the type of questions we will explore using the dataset:

* How many births each year
* How many births each month
* How many births each weekday
* How many births each day of the month

```python
file = open('US_births_1994-2003_CDC_NCHS.csv')
data = file.read()
data_list = data.split('\n')

# small sample of data for preview
data_list[:10]
```

    ['year,month,date_of_month,day_of_week,births',
     '1994,1,1,6,8096',
     '1994,1,2,7,7772',
     '1994,1,3,1,10142',
     '1994,1,4,2,11248',
     '1994,1,5,3,11053',
     '1994,1,6,4,11406',
     '1994,1,7,5,11251',
     '1994,1,8,6,8653',
     '1994,1,9,7,7910']

We are working with numeric integers, but all of the above data indicates that they are formatted as string values after reading the file. Furthermore the header is not really pertinent once we start working with the data values, therefore we should separate the header from the actual data. Let's create a function that will do all the above. The function will take in a filename, read the file, split the data into a list without including the header, and then finally change all the values into **integers**.

```python
def read_csv(file_name):
    file = open(file_name, 'r')
    data = file.read().split('\n')
    string_list = data[1:]
    final_list = []
    for row in string_list:
        int_fields = []
        string_fields = row.split(',')
        for val in string_fields:
            int_fields.append(int(val))

        final_list.append(int_fields)

    return final_list
```

```python
cdc_list = read_csv('US_births_1994-2003_CDC_NCHS.csv')
cdc_list[:10]
# notice no header
```

    [[1994, 1, 1, 6, 8096],
     [1994, 1, 2, 7, 7772],
     [1994, 1, 3, 1, 10142],
     [1994, 1, 4, 2, 11248],
     [1994, 1, 5, 3, 11053],
     [1994, 1, 6, 4, 11406],
     [1994, 1, 7, 5, 11251],
     [1994, 1, 8, 6, 8653],
     [1994, 1, 9, 7, 7910],
     [1994, 1, 10, 1, 10498]]

### How Many Births Per Month

The header informs us that the 0<sup>th</sup> index in each data-row will house the **year**, 1<sup>st</sup> index the **month** (jan = 1, feb = 2, etc), 2<sup>nd</sup> index the **date_of_month** (1-31), 3<sup>rd</sup> index the **day_of_week** (1-7), and 4<sup>th</sup> index the **births** on that day. To answer this question, it would be wise to define a function that takes our list, sums up how many births there are in each month from 1994 to 2003, and then returns it to us in a dictionary, with each month being a key and the value be the births for that month in that timespan.

```python
def month_births(input_list):
    births_per_month = {}
    for data in input_list:
        month = data[1]
        births = data[4]
        if month in births_per_month:
            births_per_month[month] += births
        else:
            births_per_month[month] = births

    return births_per_month
```

```python
cdc_month_births = month_births(cdc_list)
cdc_month_births
```

    {1: 3232517,
     2: 3018140,
     3: 3322069,
     4: 3185314,
     5: 3350907,
     6: 3296530,
     7: 3498783,
     8: 3525858,
     9: 3439698,
     10: 3378814,
     11: 3171647,
     12: 3301860}

From the data above, we can see that from 1994 to 2003 there was a total of 3,296,530 children born in the month of June in the U.S. Scan the dictionary to see the amount of births in other months.

### How Many Births Per Day Of Week

How about births per day of week? The function definition is very similar with only a few noticeable changes, but let's implement it anyways.

```python
def dow_births(input_list):
    births_per_weekday = {}
    for data in input_list:
        day_of_week = data[3]
        births = data[4]
        if day_of_week in births_per_weekday:
            births_per_weekday[day_of_week] += births
        else:
            births_per_weekday[day_of_week] = births

    return births_per_weekday
```

```python
cdc_day_births = dow_births(cdc_list)
cdc_day_births
```

    {1: 5789166,
     2: 6446196,
     3: 6322855,
     4: 6288429,
     5: 6233657,
     6: 4562111,
     7: 4079723}

### General Function

The dow_births and month_births functions are exactly the same, save for the index data we require, month at index 1 and day_of_week at index 3, and the variable names. In such instances we want to generalize the function so we can reuse it for these and potentially other questions we might have. We will need to include a new column parameter for our function. This column is the index of the headers from which we want to get data. For ex. if the column parameter was 0, then our function would return a dictionary with the total number of births in each year.

```python
def calc_counts(input_list, column):
    births_per_column = {}
    for data in input_list:
        col_data = data[column]
        births = data[4]
        if col_data in births_per_column:
            births_per_column[col_data] += births
        else:
            births_per_column[col_data] = births

    return births_per_column
```

```python
cdc_year_births = calc_counts(cdc_list, 0)
cdc_year_births
```

    {1994: 3952767,
     1995: 3899589,
     1996: 3891494,
     1997: 3880894,
     1998: 3941553,
     1999: 3959417,
     2000: 4058814,
     2001: 4025933,
     2002: 4021726,
     2003: 4089950}

```python
cdc_month_births = calc_counts(cdc_list, 1)
cdc_month_births
```

    {1: 3232517,
     2: 3018140,
     3: 3322069,
     4: 3185314,
     5: 3350907,
     6: 3296530,
     7: 3498783,
     8: 3525858,
     9: 3439698,
     10: 3378814,
     11: 3171647,
     12: 3301860}

```python
cdc_dom_births = calc_counts(cdc_list, 2)
cdc_dom_births
```

    {1: 1276557,
     2: 1288739,
     3: 1304499,
     4: 1288154,
     5: 1299953,
     6: 1304474,
     7: 1310459,
     8: 1312297,
     9: 1303292,
     10: 1320764,
     11: 1314361,
     12: 1318437,
     13: 1277684,
     14: 1320153,
     15: 1319171,
     16: 1315192,
     17: 1324953,
     18: 1326855,
     19: 1318727,
     20: 1324821,
     21: 1322897,
     22: 1317381,
     23: 1293290,
     24: 1288083,
     25: 1272116,
     26: 1284796,
     27: 1294395,
     28: 1307685,
     29: 1223161,
     30: 1202095,
     31: 746696}

```python
cdc_dow_births = calc_counts(cdc_list, 3)
cdc_dow_births
```

    {1: 5789166,
     2: 6446196,
     3: 6322855,
     4: 6288429,
     5: 6233657,
     6: 4562111,
     7: 4079723}

### Max And Min Births In Each Category

It would be nice to know the min. and max. births for all of the above calculations without having to search for ourselves. Since we already get a dictionary from the calc_counts() function, let's use that as the first parameter for our max_min_births() function. To make it even more general we need a second parameter so that we can specify whether we want the min or max values. The parameter will default to the min value, thus the **reverse=False** parameter for ascending order, but if what require the max value, then we need to explicitly specify that **reverse=True** for descending order.

```python
def max_min_births(input_list, reverse=False):
    import operator

    sorted_births = sorted(input_list.items(), key=operator.itemgetter(1),reverse=reverse)
    return sorted_births[0]
```

```python
cd_dow_births_min = max_min_births(calc_counts(cdc_list, 3))
cd_dow_births_min
```

    (7, 4079723)

```python
cd_dow_births_max = max_min_births(calc_counts(cdc_list, 3), reverse=True)
cd_dow_births_max
```

    (2, 6446196)

The max_min_births() returns a tuple with the first number being the value for the header, and the second as the total number of births.
As an example (7, 4079723) means that the least amount of births are on Sundays. Why is that the case? Is it because Sunday is the Sabbath? Inversely, how come a majority of births are on weekdays? Is it because people want to take a day off from work? Another interesting observation is that there are fewer births on weekends. What is the connection there? Is it because some people want to name their children after after a day of the week, but Saturday and Sunday are not truly viable child names?

## Superstitious Birth Days

You can see how hard it is to extrapolate on the data if there's no background and/or historical knowledge about U.S births -- it almost becomes laughable. An estimation window can be set for birth, but in all likelihood it's much more like double patio doors. However, because people can be a tad superstitious about what date they would rather not give birth, let's see if there's a relation to the number of births on Friday the Thirteens and on Leap Days.

```python
def superstitious_births(input_list, name, date_of_month, day_of_week=None, month=None):
    births_per_column = { name: 0, 'regular': 0}

    for data in input_list:
        if not month:
            if data[3] == day_of_week:
                if data[2] == date_of_month:
                    births_per_column[name] += data[4]
                else:
                    births_per_column['regular'] += data[4]
        else:
            if data[1] == month:
                if data[2] == date_of_month:
                    births_per_column[name] += data[4]
                else:
                    births_per_column['regular'] += data[4]

    return births_per_column
```

```python
freaky_friday_births = superstitious_births(cdc_list, name='friday_13', date_of_month=13, day_of_week=5)
freaky_friday_births
```

    {'friday_13': 182477, 'regular': 6051180}

```python
leap_day_births = superstitious_births(cdc_list, name='leap_day', date_of_month=29, month=2)
leap_day_births
```

    {'leap_day': 22337, 'regular': 2995803}

Notice the drop-offs in births on Friday 13s against regular Fridays and on Leap Days against all other days in February. This would seem to support that a **_very_** superstitious about what date they give birth, and would take action to either induce an earlier birth or maybe attempt to align their pregnancies with avoiding the end of the month in consideration. Once again, this is merely all conjecture, and even based on incorrect calculations. In our superstitious_births() function returns the sum of births on Friday 13s versus all other Friday births. The function also only returns sum of births on February 29 against all other births in February. This is not ideal in determining if there are actual drop-offs in births on those superstitious days.

We will limit ourselves to just focusing on the Leap Day data. To determine if Leap Day has an effect on births we would need to only account for the February's in Leap Years and maybe even focus our data on the days before and after Leap Day.

```python
def leap_day_births(input_list):
    # 27, 28, 29 are month_dates in Feb. 1, 2, 3 are month_dates in Mar.
    dates = { 27: 0, 28: 0, 29: 0, 1: 0, 2: 0, 3: 0 }

    for data in input_list:
        if data[0] in [1996, 2000]:
            if data[1] == 2:
                if data[2] in [27, 28, 29]:
                    dates[data[2]] += data[4]
            if data[1] == 3:
                if data[2] in [1, 2, 3]:
                    dates[data[2]] += data[4]

    return dates  
```

```python
births_around_leap_days = leap_day_births(cdc_list)
print('min_births', max_min_births(births_around_leap_days))
print('max_births', max_min_births(births_around_leap_days, reverse=True))
births_around_leap_days
```

    min_births (27, 19477)
    max_births (1, 24192)





    {1: 24192, 2: 20980, 3: 20005, 27: 19477, 28: 22643, 29: 22337}

There's an extra 2000 children born on March 1st in Leap Years. Is that enough of a correlation? Are parents conscientiously pulling a Portgas D. Rouge -- although to a sensible extent? Again, the data doesn't verify that with absolute certainty.

To run all the above calculations with additional U.S births spanning from 2004 to 2014 just combine the **US_births_1994-2003_CDC_NCHS.csv** and **US_births_2000-2014_SSA.csv**. It will be the same steps as outlined in the beginning of this post. However upon reading this additional file, measures will need to be taken to not include the header and the overlapping years (2000 - 2003) when combining these two datasets into one.

In the next post, I hope to highlight some of the basics of another highly performant package, Pandas, that not only can read in all types of data files, combine multiple datasets, but also manipulate the data therein, amongst many other uses.
