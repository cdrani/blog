---
layout: post
title: 'AWK Basic Overview'
date: 2018-04-26 18:11:41 -0600
categories: awk
---

### AWK: A Case Study

On Monday, April 23, I went to the first phase of an interview (and hopefully not the last) to be a Junior Programmer Analyst. The company seems to be the ideal opportunity for someone just starting out in the software development field, which I extrapolated from their job ad:
    
    * A university degree or a diploma in an applied field;
    * Familiarity with the following: JavaScript, HTML, CSS, Python, and SQL;
    * Familiarity with database design; and
    * Familiarity with Unix.
    * Successful candidates will have a passion for computing science. 
    * Candidates who do not have a strong desire to improve their programming skills need not apply. 
    * Junior Programmers will receive internal training – frequent code reviews by Senior Programmers, iterative quality of code feedback and direction for improvement, problem- solving meeting participation, and self-directed or classroom courses as required. 
    
These are all the markings of a great learning environment that fosters continuous improvement from senior developers and code reviews. I felt that I met a majority of what they were looking for and applied forthwith hoping against all hope just for a chance for an interview.


I was told that the initial interview phase would consist of a programming case study that last an hour. I tried to eke out any clue as to what it entailed, but to no avail, being that there isn't much preparation one could do outside of just showing up. It turned out to be quite true as whence I arrived I was presented a computer and 7 questions to answer using [**awk**](https://www.gnu.org/software/gawk/manual/gawk.html). I had tested awk once before several months ago, but at the time I was just playing around with it and didn't commit anything to memory. I believe I would have failed handsomely were it not for the fact that I could make use of the provided and any online sources. As I was pressed for time and my mind was running amok, I opted to just make use of the internet and sites like stackoverflow. I've had open book examinations before, however in all those instances I was familiar with the material and/or the provided resources (usually a book) that I was calm, cool, and collected during the entirety of the exam. 
    

# AWK Basics

I don't want to cover the entirety of the awk system, mainly because the manual would accomplish that task better, but I also want highlight some of the awk commands I used in the case study. In the case study I only got through 4 out of the 7 questions within the hour. I am not too disappointed in that fact, however the answers I provided felt hollow because I wasn't too sure of what I was inputting. Some of the commands were foreign to me, especially some of the builtin variables like **NF**, **NR**, **\$0**, **\$1**, **BEGIN**, **END**, etc, although I could take a gander at them, I would rather be certain. Therefore after I got home I set to learn the basics of AWK, focusing on the very commands I used in the case study. I can now fully see the benefits of knowing a program like awk as I could use it in place of **pandas** to explore files.


Here's some of the basics using some example files. Near the end I will use these very commands to re-answer some of the case study questions.

NOTE: **%%bash** is a magic command to load the bash shell in Jupyter. All commands can be written in a shell environment or in a textfile and run in the shell.

# Builtin Variables

AWK reads in a file and splits it into **records** and **fields**. Records default to newlines ("\n") meaning each line is a record. Fields are how lines are separated, defaulting to whitespace.

The record number (NR) and field number (NF) can be displayed in the output if desired:


```bash
%%bash
# NR at the beginning of each line, NF at the end of each line
# 8 lines --> 8 records
# For the first line --> 6 fields (the field separator is whitespace)
awk '{ print NR, $0, NF }' dukeofyork.txt
```

    1 The grand old Duke of York 6
    2 He had ten thousand men 5
    3 He marched them up to the top of the hill 10
    4 And he marched them down again 6
    5 And when they were up they were up 8
    6 And when they were down they were down 8
    7 And when they were only half-way up 7
    8 They were neither up nor down 6


We can use builtins such as **\$0**, **\$1**, etc to return specific fields from each record. As shown above $0 returns ___all___ the fields. In fact we can state the ordering of which they should be displayed. In the **names.txt** file, the order is firstnames preceded by surnames. Let's display it  as "surname, firstname" fashion. We can add the ", " as a separator by concatenation.


```bash
%%bash
# Notice the second field ($2) is first, and the firest field ($1) is second.
# The names are concatenated
awk '{ print $2 $1 }' names.txt
```

    GallowayGretchen
    SteeleIsaac
    MyersWayne
    LeeLillith
    BlackwellMolly
    ArnoldMaia
    ReeseLev
    GuthrieCarlos
    BuckSophia
    MitchellVincent
    HarrisBuffy
    MilesReuben
    FowlerBrendan
    HancockMason
    BooneNigel
    ForemanGretchen
    GoodmanSerena
    VelezShoshana
    HughesEve



```bash
%%bash
# To add a space between the first and surnames we have two options
# awk '{print $2, $1}' names.txt
# or 
# awk '{print $2 " " $1}' names.txt
#
# However we want to separate the names by way of a comma
awk '{ print $2 ", " $1 }' names.txt
```

    Galloway, Gretchen
    Steele, Isaac
    Myers, Wayne
    Lee, Lillith
    Blackwell, Molly
    Arnold, Maia
    Reese, Lev
    Guthrie, Carlos
    Buck, Sophia
    Mitchell, Vincent
    Harris, Buffy
    Miles, Reuben
    Fowler, Brendan
    Hancock, Mason
    Boone, Nigel
    Foreman, Gretchen
    Goodman, Serena
    Velez, Shoshana
    Hughes, Eve


We are not limited to just separating our output using spaces and commas, we have the options for other characters such as "\t", "\n", etc. Just as we can be specific with which fields we want to retrieve, likewise, we can specify the records we want.


```bash
%%bash
# NR==8 focuses on only the 8th line
awk 'NR==8 { print $0 }' dukeofyork.txt

```

    They were neither up nor down



```bash
%%bash
awk 'NR==8 || NF>=8 { print NR, NF, $0 }' dukeofyork.txt
```

    3 10 He marched them up to the top of the hill
    5 8 And when they were up they were up
    6 8 And when they were down they were down
    8 6 They were neither up nor down


In the above output we use comparison operators to output the lines of text where the record number was either 8 or the number of fields were 8 or greater. Since awk is both a program and language it comes instilled with very similar operators and conditionals present in C and a majority of other languages.

# Control Structures

awk has control structures such as if/else statements for branching and for loops for ... looping.


```bash
%%bash
awk '
    {
        if (NF <= 6) {
            print "Short line: ", $0, NF
        } else {
            print "Long line: ", $0, NF
        }
    }
' dukeofyork.txt
```

    Short line:  The grand old Duke of York 6
    Short line:  He had ten thousand men 5
    Long line:  He marched them up to the top of the hill 10
    Short line:  And he marched them down again 6
    Long line:  And when they were up they were up 8
    Long line:  And when they were down they were down 8
    Long line:  And when they were only half-way up 7
    Short line:  They were neither up nor down 6


Notice the nice formatting, indentation, spacing, et al. The job ad mentioned adherence to **clean code**, of which formatting is a tenet. In a majority of my code I wrote in the case study I didn't follow that tenet closely opting for completion and foolishly thinking I could come back and format it later. It's justifiable to think that I might have lost some points there. Also of consideration is that as your code starts to span more than one line as above, it would be wise to move it to a file, with **awk** as the commonly used extension. 


```bash
%%bash
cat > for_ex.awk
{
    for ( i = 1; i <= 2; i++ ) {
        print "Line " NR ", field " i ": " $i;
    }
}
```


```bash
%%bash
# -f flag denotes that there is an awk program file to be read from
# prints the first two fields in each line. 
awk -f for_ex.awk dukeofyork.txt
```

    Line 1, field 1: The
    Line 1, field 2: grand
    Line 2, field 1: He
    Line 2, field 2: had
    Line 3, field 1: He
    Line 3, field 2: marched
    Line 4, field 1: And
    Line 4, field 2: he
    Line 5, field 1: And
    Line 5, field 2: when
    Line 6, field 1: And
    Line 6, field 2: when
    Line 7, field 1: And
    Line 7, field 2: when
    Line 8, field 1: They
    Line 8, field 2: were


# Regex Pattern Matching

The AWK program requires a command/action and/or a pattern. The above examples have focused on the commands thus far, especially the common used **print** command. We can also use a regex pattern to either filter our data to exact a command upon or to search for specific patterns. 


```bash
%%bash
awk '/M/ { print $0 }' names.txt
```

    Wayne Myers
    Molly Blackwell
    Maia Arnold
    Vincent Mitchell
    Reuben Miles
    Mason Hancock


To limit our search to specific fields we use the **~** to match or **!~** to not match.


```bash
%%bash
awk '$1 ~ /M/ { print $0 }' names.txt
```

    Molly Blackwell
    Maia Arnold
    Mason Hancock



```bash
%%bash
# print names of people with surnames 8 letters or longer
awk '$2 ~ /.{8,}/ { print $0 }' names.txt
```

    Gretchen Galloway
    Molly Blackwell
    Vincent Mitchell



```bash
%%bash
awk '/en/ { print $0 }' names.txt dukeofyork.txt
```

    Gretchen Galloway
    Vincent Mitchell
    Reuben Miles
    Brendan Fowler
    Gretchen Foreman
    Serena Goodman
    He had ten thousand men
    And when they were up they were up
    And when they were down they were down
    And when they were only half-way up


It is of great import that that I mention that multiple files can be used for awk's input.

# Overwriting Defaults

As mentioned earlier, awk has some built in defaults which we can also modify. All the files we have worked with are broken up by whitespace (FS) and newlines (RS), but this won't be the case with all the files we will ever work with or on. 


```python
cat nba_players.csv
```

    
    
    
    
    
    


Our nba_players.csv file's "fields" are separated by commas - at least that's what we want, or else awk might resort to it's default and render this:


```bash
%%bash
awk '{ print NF, $0 }' nba_players.csv
```

    1 player,team,pos,number
    3 K. Durant,GS Warriors,SF,35
    3 J. Harden,Houston Rockets,SG,13
    3 K.A Towns,Minnesota Timberwolves,C,32
    3 Kyrie Irving,Boston Celtics,PG,11
    3 Anthony Davis,NO Pelicans,PF,23


Notice "player,team,pos,number" is considered ___one___ field, not the four desired, with the rest of fields in the other records having misleading fields numbers as well.


```bash
%%bash
# -F allows us to input our own field separator, in this case it's ","
awk -F , '{ print NF, $0 }' nba_players.csv
```

    4 player,team,pos,number
    4 K. Durant,GS Warriors,SF,35
    4 J. Harden,Houston Rockets,SG,13
    4 K.A Towns,Minnesota Timberwolves,C,32
    4 Kyrie Irving,Boston Celtics,PG,11
    4 Anthony Davis,NO Pelicans,PF,23



```bash
%%bash
# We can use any character or even a pattern for the FS
# The FS has been replace with '|' to highlight where the fields separate
awk -F a '{ print NF, $1 "|" $2 "|" $3 }' names.txt
```

    3 Gretchen G|llow|y
    3 Is||c Steele
    2 W|yne Myers|
    1 Lillith Lee||
    2 Molly Bl|ckwell|
    3 M|i| Arnold
    1 Lev Reese||
    2 C|rlos Guthrie|
    2 Sophi| Buck|
    1 Vincent Mitchell||
    2 Buffy H|rris|
    1 Reuben Miles||
    2 Brend|n Fowler|
    3 M|son H|ncock
    1 Nigel Boone||
    2 Gretchen Forem|n|
    3 Seren| Goodm|n
    3 Shosh|n| Velez
    1 Eve Hughes||


# BEGIN & END

BEGIN allows us to setup variables, modify defaults, etc before any action is taken upon any input file(s). 
END can be used to reformat how our data is outputted.


```bash
%%bash
# Here we set our FS to be ",". No need for -F flag
awk 'BEGIN{FS=","} { print $ 0}' nba_players.csv
```

    player,team,pos,number
    K. Durant,GS Warriors,SF,35
    J. Harden,Houston Rockets,SG,13
    K.A Towns,Minnesota Timberwolves,C,32
    Kyrie Irving,Boston Celtics,PG,11
    Anthony Davis,NO Pelicans,PF,23



```bash
%%bash 
awk 'BEGIN{FS=","} { print NF, $1, $2, $3, $4 }' nba_players.csv
```

    4 player team pos number
    4 K. Durant GS Warriors SF 35
    4 J. Harden Houston Rockets SG 13
    4 K.A Towns Minnesota Timberwolves C 32
    4 Kyrie Irving Boston Celtics PG 11
    4 Anthony Davis NO Pelicans PF 23


Everything now is tab separated, but if we want it be be separated nicely we have to make use of the **printf** command.


```bash
%%bash
awk 'BEGIN{FS=","} { printf("%s\t%s\t%s\t%s\n", $1, $2, $3, $4) }' nba_players.csv
```

    player	team	pos	number
    K. Durant	GS Warriors	SF	35
    J. Harden	Houston Rockets	SG	13
    K.A Towns	Minnesota Timberwolves	C	32
    Kyrie Irving	Boston Celtics	PG	11
    Anthony Davis	NO Pelicans	PF	23


Now every column is lined up, but not under the correct heading as the fields have a lot of characters that bleed over to the following columns. We need to add some padding.


```bash
%%bash
awk 'BEGIN{FS=","} { printf("%-15s %-24s %-4s %s\n", $1, $2, $3, $4) }' nba_players.csv
```

    player          team                     pos  number
    K. Durant       GS Warriors              SF   35
    J. Harden       Houston Rockets          SG   13
    K.A Towns       Minnesota Timberwolves   C    32
    Kyrie Irving    Boston Celtics           PG   11
    Anthony Davis   NO Pelicans              PF   23


We removed the "\t" character and opted for adding a width character that pads any unused space with whitespace. For ex. "%-15s" **left-aligns** the column and gives it a width of 15 characters, padding any leftover width with whitespace.

I want to save this nicely formatted, columnar data as a **tsv** (tab separated values) file.


```bash
%%bash
awk 'BEGIN{FS=","} { printf("%-15s %-24s %-4s %s\n", $1, $2, $3, $4) }' nba_players.csv > nba_players.tsv
```


```python
cat nba_players.tsv
```

    
    
    
    
    
    


# Case Study Questions

I don't have the exact test files used, but I can create similar ones. The first type of file just consisted of low integers, mainly 1, 0, -1. Something akin to this:



The second type had integers interspersed with string characters, quite similar to this:



```bash
%%bash

cat > test1.txt
1  -1 1 0
1  1  0 0
-1 -1 1 1
-1 0  0 1
```


```bash
%%bash
cat test1.txt
```

    1  -1 1 0
    1  1  0 0
    -1 -1 1 1
    -1 0  0 1


## 1) Return Coordinates Of Invalid Values (Negatives).


```bash
%%bash 
awk '
    {
        for ( i = 1; i <= 4; i++ ) {
            if ( $i < 0 ) {
                print "Line:", NR, "Column:", i, "Value:", $i
            }
        }
    }
' test1.txt
```

    Line: 1 Column: 2 Value: -1
    Line: 3 Column: 1 Value: -1
    Line: 3 Column: 2 Value: -1
    Line: 4 Column: 1 Value: -1



```bash
%%bash 
cat test1.txt
```

    1  -1 1 0
    1  1  0 0
    -1 -1 1 1
    -1 0  0 1


## 2) Change All Invalid Numbers To 0


```bash
%%bash
awk '
    {
        for ( i = 1; i <= NF; i++ ) {
            if ( $i < 0 ) {
                $i = sub(/$i/, 0)
            }
        }
        print $0
    }
' test1.txt
```

    1 0 1 0
    1  1  0 0
    0 0 1 1
    0 0 0 1


## 3) Sum All Fields


```bash
%%bash
awk '
    {
            for ( i = 1; i <= NF; i++ ) {
                sum[i] += $i
            }
    }
    
    END {
            for ( i = 1; i <= NR; i++ ) {
                print "column:", i, "sum:", sum[i]
            }
    }
' test1.txt
```

    column: 1 sum: 0
    column: 2 sum: -1
    column: 3 sum: 2
    column: 4 sum: 2



```python
cat test1.txt
```

    1  -1 1 0
    1  1  0 0
    -1 -1 1 1
    -1 0  0 1


## 4) Count Unique Values


```bash
%%bash
awk '
    { 
            for ( i = 1; i <= NF; i++ ) {
                arr[$i]++
            }
    } 
    END { 
            for (n in arr)  {
                print n, arr[n] 
            }
    }
' test1.txt
```

    -1 4
    0 5
    1 7


That's where we'll leave this overview for now. A lot of information has been brushed over and left to be investigated on own's one, including user-defined functions, additional builtins, commandline flags, etc. In the following posts and even in the wild, I'll attempt to use awk more often.
