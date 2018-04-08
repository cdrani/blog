---
layout: post
title: 'US Gun Deaths Analysis'
date: 2018-04-08 13:45:42 -0600
categories: us csv ds
---

# Analyzing US Gun Deaths

In the last post, there was emphasis placed on how to read in csv file(s), remove the headers, work with the data, and then finally close the file. Today we will look at how to use the **csv** module for python to write and read csv textfiles. That will only be a small extent of today's focus as what we want is to actually work with data that we can make solid and accurate observations upon. For now let's learn how to make use of the **csv** module. Additionally, we will make use of a **context manager** as briefly mentioned at the end of the last post. We will not go into depth into the workings of the csv file as the [documentation](https://docs.python.org/3/library/csv.html#examples) is very easy to read and has very excellent examples to which we can refer.

## Creating A CSV FILE

```python
import csv
from pprint import pprint

nba_players_info = [
    ['player', 'team', 'pos', 'number'],
    ['K. Durant', 'GS Warriors', 'SF', 35],
    ['J. Harden', 'Houston Rockets', 'SG', 13],
    ['K.A Towns', 'Minnesota Timberwolves', 'C', 32],
    ['Kyrie Irving', 'Boston Celtics', 'PG', 11],
    ['Anthony Davis', 'NO Pelicans', 'PF', 23]
]

with open('nba_players.csv', mode='w', newline='', encoding='utf-8') as f:
    writer = csv.writer(f)
    writer.writerows(nba_players_info)
```

## Reading a CSV File

```python
with open('nba_players.csv', 'r') as f:
    reader = csv.reader(f)
    pprint(list(reader))
```

    [['player', 'team', 'pos', 'number'],
     ['K. Durant', 'GS Warriors', 'SF', '35'],
     ['J. Harden', 'Houston Rockets', 'SG', '13'],
     ['K.A Towns', 'Minnesota Timberwolves', 'C', '32'],
     ['Kyrie Irving', 'Boston Celtics', 'PG', '11'],
     ['Anthony Davis', 'NO Pelicans', 'PF', '23']]

## US Gun Deaths

Alright, now that we know how to use the csv module - at least the reader and writer objects - let's make use of it read in some data concerning gun related deaths in the US from 2012 to 2014.

```python
from csv import reader
import datetime
from pprint import pprint

f = open('guns.csv', 'r')
data = list(reader(f))
# We only need to see a small sample of the data
pprint(data[:5], width=120, depth=2, compact=True)
```

    [['', 'year', 'month', 'intent', 'police', 'sex', 'age', 'race', 'hispanic', 'place', 'education'],
     ['1', '2012', '01', 'Suicide', '0', 'M', '34', 'Asian/Pacific Islander', '100', 'Home', '4'],
     ['2', '2012', '01', 'Suicide', '0', 'F', '21', 'White', '100', 'Street', '3'],
     ['3', '2012', '01', 'Suicide', '0', 'M', '60', 'White', '100', 'Other specified', '4'],
     ['4', '2012', '02', 'Suicide', '0', 'M', '64', 'White', '100', 'Home', '4']]

```python
# Peek at the headers
from pprint import pprint
headers = data[0]
print(headers)
```

    ['', 'year', 'month', 'intent', 'police', 'sex', 'age', 'race', 'hispanic', 'place', 'education']

Let's explore the meaning behind each header:

* year -- the year in which the fatality occurred.

- month -- the month in which the fatality occurred.

* intent -- the intent of the perpetrator of the crime. This can be Suicide, Accidental, NA, Homicide, or Undetermined.

- police -- whether a police officer was involved with the shooting. Either 0 (false) or 1 (true).

* sex -- the gender of the victim. Either M or F.

- age -- the age of the victim.

* race -- the race of the victim. Either Asian/Pacific Islander, Native American/Native Alaskan, Black, Hispanic, or White.

- hispanic -- a code indicating the Hispanic origin of the victim.

* place -- where the shooting occurred. Has several categories, which you're encouraged to explore on your own.

- education -- educational status of the victim. Can be one of the following:

  1. Less than High School
  2. Graduated from High School or equivalent
  3. Some College
  4. At least graduated from College
  5. Not available

## Deaths Per Year

**Expectations**: I don't know too much about any major US conflicts that happened between 2012 - 2014, save for some [mass shootings](http://timelines.latimes.com/deadliest-shooting-rampages/) and the now frequently occurring police slaying of Black people. Those deaths will hopefully show up in the dataset, but as sad it is, it most likely will not register much in the grand sums of yearly deaths.

```python
# Remove the header from our data
gun_data = data[1:]
pprint(gun_data[:5], width=120, depth=10, compact=True)
```

    [['1', '2012', '01', 'Suicide', '0', 'M', '34', 'Asian/Pacific Islander', '100', 'Home', '4'],
     ['2', '2012', '01', 'Suicide', '0', 'F', '21', 'White', '100', 'Street', '3'],
     ['3', '2012', '01', 'Suicide', '0', 'M', '60', 'White', '100', 'Other specified', '4'],
     ['4', '2012', '02', 'Suicide', '0', 'M', '64', 'White', '100', 'Home', '4'],
     ['5', '2012', '02', 'Suicide', '0', 'M', '31', 'White', '100', 'Other specified', '2']]

```python
years = [row[1] for row in gun_data]
year_counts = {}
```

```python
for year in years:
    if year in year_counts:
        year_counts[year] += 1
    else:
        year_counts[year] = 1
```

```python
year_counts
```

    {'2012': 33563, '2013': 33636, '2014': 33599}

**Observations**: As it the data is limited year-wise, we can only infer that the amount of deaths year by year is stagnant around the ~33,590 (based on the mean). To get any significant data we will have to delve in much further.

## Deaths Per Month

**Expectations**: This one is quite harder to predict mainly because any inference I could make could be plausible. Looking at some of the intentions - Suicide, Accidental, NA, Homicide, or Undetermined - I could hypothesize this:

**INCREASE**:

* Months leading to winter holidays: Increase in deaths due to [Robbin Season](http://www.vulture.com/2018/01/atlanta-season-two-robbin-season-how-it-got-its-name.html).
* Winter holidays months:
  Increase in suicides maybe because of lack of family, SAD, etc, or the gathering of many people in enclosed spaces
* Summer months: Homicide increase due to the effect of heat on people and generally people are out and about interacting with each other more often

**DECREASE**:

* End of winter holiday months: People are awaiting Spring eagerly and have just enjoyed some much needed time off and respite from work/school. Black History month may be a factor.

```python
dates = [datetime.datetime(year=int(row[1]), month=int(row[2]), day=1) for row in gun_data]
dates[:5]
```

    [datetime.datetime(2012, 1, 1, 0, 0),
     datetime.datetime(2012, 1, 1, 0, 0),
     datetime.datetime(2012, 1, 1, 0, 0),
     datetime.datetime(2012, 2, 1, 0, 0),
     datetime.datetime(2012, 2, 1, 0, 0)]

```python
date_counts = {}

for date in dates:
    if date in date_counts:
        date_counts[date] += 1
    else:
        date_counts[date] = 1
```

```python
# display only a small sample of formatted date_counts
formatted_date_counts = [(i[1][0].strftime("%Y %b"), i[1][1]) for i in enumerate(date_counts.items())]
formatted_date_counts[:5]
```

    [('2012 Jan', 2758),
     ('2012 Feb', 2357),
     ('2012 Mar', 2743),
     ('2012 Apr', 2795),
     ('2012 May', 2999)]

```python
def max_min_deaths(input_list):
    from operator import itemgetter
    sorted_deaths = sorted(input_list, key=itemgetter(1))
    return {
        'min': sorted_deaths[0:3],
        'max': sorted_deaths[-1:-4:-1]
    }
```

```python
print(f'2012 {max_min_deaths(formatted_date_counts[:12])}')
print('------------------')
print(f'2013 {max_min_deaths(formatted_date_counts[12:24])}')
print('------------------')
print(f'2014 {max_min_deaths(formatted_date_counts[24:])}')
```

    2012 {'min': [('2012 Feb', 2357), ('2012 Nov', 2729), ('2012 Oct', 2733)], 'max': [('2012 Jul', 3026), ('2012 May', 2999), ('2012 Aug', 2954)]}
    ------------------
    2013 {'min': [('2013 Feb', 2375), ('2013 Sep', 2742), ('2013 Nov', 2758)], 'max': [('2013 Jul', 3079), ('2013 Jun', 2920), ('2013 Jan', 2864)]}
    ------------------
    2014 {'min': [('2014 Feb', 2361), ('2014 Jan', 2651), ('2014 Mar', 2684)], 'max': [('2014 Aug', 2970), ('2014 Jun', 2931), ('2014 Sep', 2914)]}

**Obsevations**:
February had the least amount of deaths in all three years with an average of 2365 deaths. The next months with the least amount of deaths tended to be Fall months (Sept, Oct, Nov). Does this decrease in deaths coincide with children going to school, regardless of whether if the deaths are attributed to adults or school-age children, or maybe the weeks leading to Thanksgiving and eventually the winter holiday festivities?

Likewise, the max deaths are generally in the summer months; with the record high of 3079 in July of 2012 and deaths hovering at or neat the 3000 mark.

```python
pprint(headers, width=120)
```

    ['', 'year', 'month', 'intent', 'police', 'sex', 'age', 'race', 'hispanic', 'place', 'education']

```python
def column_counts(input_list, column):
    col_count = {}
    for row in input_list:
        col = row[column]
        if col in col_count:
            col_count[col] += 1
        else:
            col_count[col] = 1
    return col_count
```

# Deaths Based On Binary Gender

```python
sex_counts = column_counts(gun_data, 5)
sex_counts
```

    {'F': 14449, 'M': 86349}

From the data, it seems that 6x more men are in involved in gun-related deaths than women. This huge disparity will require a further analysis into the intents of the deaths.

```python
def gender_cols_count(input_list, col):
    col_count = {'M': {}, 'F': {}}
    for row in input_list:
        sex = row[5]
        cat = row[col]
        if cat in col_count[sex]:
            col_count[sex][cat] += 1
        else:
            col_count[sex][cat] = 0
    return col_count
```

```python
gender_deaths_intents = gender_cols_count(gun_data, 3)
pprint(gender_deaths_intents)
```

    {'F': {'Accidental': 217,
           'Homicide': 5372,
           'Suicide': 8688,
           'Undetermined': 168},
     'M': {'Accidental': 1420,
           'Homicide': 29802,
           'NA': 0,
           'Suicide': 54485,
           'Undetermined': 637}}

Like most people I assumed that there would be more male deaths related to homicides, and the data supports that with 37% of males and 34% of females deaths attributed to them. Again the ratio of deaths due to suicide is 60% and 63% for females and males respectively. Although the percentages of the intents are very similar for both genders, the number of deaths for the males is astoundingly higher, especially considering the [male to female population](https://data.worldbank.org/indicator/SP.POP.TOTL.FE.ZS?end=2014&locations=US&start=2012&view=chart) of the US is close to parity. Another very concerning category is the amount of 'Accidental' and 'Undetermined'. When I see 'Accidental' deaths my thoughts are immediately filled with new stories of little children in possessions of guns and the guns going off accidentally, or even worse, stories where the police is involved and [shoot an innocent child](https://www.huffingtonpost.com/2014/09/17/aiyana-stanley-jones-joseph-weekley-trial_n_5824684.html).

## Deaths Broken Down By Race

It's bad enough that there are all theses deaths, but to address some of these deaths we need the numbers to see which communities are suffering the most. Bear in mind though that the following numbers will be need to framed with how each [race group](https://statisticalatlas.com/United-States/Race-and-Ethnicity) is represented in the US.

```python
race_counts = column_counts(gun_data, 7)
pprint(race_counts)
```

    {'Asian/Pacific Islander': 1326,
     'Black': 23296,
     'Hispanic': 9022,
     'Native American/Native Alaskan': 917,
     'White': 66237}

```python
gender_race_deaths = gender_cols_count(gun_data, 7)
pprint(gender_race_deaths)
```

    {'F': {'Asian/Pacific Islander': 243,
           'Black': 2317,
           'Hispanic': 1072,
           'Native American/Native Alaskan': 126,
           'White': 10686},
     'M': {'Asian/Pacific Islander': 1081,
           'Black': 20977,
           'Hispanic': 7948,
           'Native American/Native Alaskan': 789,
           'White': 55549}}

In all the race groups men are 5-10X more likely to die from gun-related deaths than women. This is once again not surprising considering men are more likely to be in possession of guns.

Currently all our data is focused on deaths on the general US population -- even the ones based on race groups. Let's pivot and peer into how these deaths relate to their respective race groups. We have at our disposal a **census** csv file which provides us with the population of each race in the US in 2010.

```python
from csv import reader
f = open('census.csv', 'r')
census = list(reader(f))
census_header = census[0]
pprint(census_header)
```

    ['Id',
     'Year',
     'Id',
     'Sex',
     'Id',
     'Hispanic Origin',
     'Id',
     'Id2',
     'Geography',
     'Total',
     'Race Alone - White',
     'Race Alone - Hispanic',
     'Race Alone - Black or African American',
     'Race Alone - American Indian and Alaska Native',
     'Race Alone - Asian',
     'Race Alone - Native Hawaiian and Other Pacific Islander',
     'Two or More Races']

```python
census_data = census[1:]
pprint(census_data)
```

    [['cen42010',
      'April 1, 2010 Census',
      'totsex',
      'Both Sexes',
      'tothisp',
      'Total',
      '0100000US',
      '',
      'United States',
      '308745538',
      '197318956',
      '44618105',
      '40250635',
      '3739506',
      '15159516',
      '674625',
      '6984195']]

The race groupings in the census don't fully align with the ones in the gun deaths data so we attempt to rectify this by merging the 'Asian' and 'Native Hawaiian and Other Pacific Islander' into 'Asian/Pacific Islander', while also not including the population of 'Two or More Races' group at all.

```python
mapping = {
    'Asian/Pacific Islander': 15159516 + 674625,
    'Black': 40250635,
    'Hispanic': 44618105,
    'Native American/Native Alaskan': 3739506,
    'White': 197318956
}
```

```python
def race_deaths(race_counts, race_map, per=1):
    deaths_count = {}
    for race, deaths in race_counts.items():
        deaths_count[race] = (deaths / mapping[race]) * per

    return deaths_count
```

```python
race_deaths_gen = race_deaths(race_counts, mapping)
pprint(race_deaths_gen)
```

    {'Asian/Pacific Islander': 8.374309664161763e-05,
     'Black': 0.000578773477735196,
     'Hispanic': 0.00020220491210910907,
     'Native American/Native Alaskan': 0.0002452195557381109,
     'White': 0.0003356849303419181}

The numbers above are correct but they don't really register as whole human beings at all. In these scenarios, the standard is to express crime statistics **per 100,000**.

```python
race_per_hundredk = race_deaths(race_counts, mapping, per=100000)
pprint(race_per_hundredk)
```

    {'Asian/Pacific Islander': 8.374309664161762,
     'Black': 57.8773477735196,
     'Hispanic': 20.220491210910907,
     'Native American/Native Alaskan': 24.521955573811088,
     'White': 33.56849303419181}

## Homicide Related Deaths

```python
intents = [row[3] for row in gun_data]
intents[:5]
```

    ['Suicide', 'Suicide', 'Suicide', 'Suicide', 'Suicide']

```python
races = [row[7] for row in gun_data]
races[:5]
```

    ['Asian/Pacific Islander', 'White', 'White', 'White', 'White']

```python
homicide_race_counts = {}

for i, race in enumerate(races):
    if intents[i] == 'Homicide':
        if race in homicide_race_counts:
            homicide_race_counts[race] += 1
        else:
            homicide_race_counts[race] = 1
```

```python
pprint(homicide_race_counts)
```

    {'Asian/Pacific Islander': 559,
     'Black': 19510,
     'Hispanic': 5634,
     'Native American/Native Alaskan': 326,
     'White': 9147}

Previously it was seen that overall gun-related deaths in the black community was ~23,000, but with further insight almost 20,000 of deaths were homicide related deaths! That's approximately 87%! Of the ~66,000 thousands deaths in the White community, only about 15% of them were homicide related. Let's see the level of police involvement in these deaths.

```python
police_involvement = [row[4] for row in gun_data]
police_homicides = {}

for i, race in enumerate(races):
    if intents[i] == 'Homicide' and police_involvement[i] == '1':
        if race in police_homicides:
            police_homicides[race] += 1
        else:
            police_homicides[race] = 1
```

```python
police_homicides
```

    {'Asian/Pacific Islander': 30,
     'Black': 356,
     'Hispanic': 282,
     'Native American/Native Alaskan': 25,
     'White': 709}

```python
police_homicides_hundredk = race_deaths(police_homicides, mapping, per=100000)
pprint(police_homicides_hundredk)
```

    {'Asian/Pacific Islander': 0.18946401955117112,
     'Black': 0.8844580961269306,
     'Hispanic': 0.6320304280067475,
     'Native American/Native Alaskan': 0.6685375020122979,
     'White': 0.3593167196769478}

We have to be very careful not to misread data such as the ones related to **police_homicides**. If not for the census data one could possibly build a false narrative that the white community is the one greatly affected by police-involved homicides. These narratives can be seen in many news programs that either an have an agenda, want to induce fear mongering, or for views.
Notice how the focusing the data per 100000 people shows that minority groups such as the Black, Hispanic, and Native America/Alaskan, die the most at the hands of the police. At what capacity or reason for police involvement is not shown in the data, but most likely the data encompasses deaths related to crimes, being at the wrong place at the wrong time, 'accidents', or just plain old racism. I understand data should be void of any emotions and attachments, but how could one view numbers like these without a shred of humanity?

We can still go further into the data and try to see other correlations in these deaths, such as education level and location, race and education level, homicides and the months they occur, suicides and race, etc. Any similar combinations will render insightful data on which to reflect upon, even shining a brighter light on some of our earlier predictions on the monthly data.

This has been a very eye-opening look at how gun deaths have plagued the US, even moreso due to the recent Parkland mass shootings. If I was presented with data like above and had the power to reduce these very concerning numbers I would readily do so, but alas some people will just see numbers. Numbers don't generally convince the average person; they will need further either further clarification or a simpler easily digested format. One of these formats could possibly be tables. In the next post we will use a python packages named [Pandas](https://pandas.pydata.org/) to work better with tabled data in flat files like csv's. Pandas has many awesome features that enable us to perform calculations across our data quite tersely, merge several data files, and manipulate entire rows and columns amongst other use cases.
