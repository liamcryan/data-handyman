+++
author = "Liam Cryan"
categories = ["programming", "learning"]
date = "2018-04-05"
description = "I can read and write files.  Why do I need the csv module?"
featured = ""
featuredalt = ""
featuredpath = "date"
linktitle = ""
title = "Reading and Writing (2)"
type = "post"

+++


# The csv module

I want to take a look at understanding the importance of 
the csv module, as well as when and why it should be used.


# Creating a csv file (the old fashioned way)

I am going to use the code below to create a csv file, f.  You can
copy and paste this code below to follow along.  If you remember
the last post, this should look very familiar.  It is the same code.


```python
f = open("f", "wt")
f.write("apples,bananas,pears\n1,2,3\n4,5,6\n")
f.close()
```

# reading

I want to view the contents of each row of my csv file both
with using the csv module and without using the csv module.  Then
it should be easy to compare differences.

With the csv module:

```python
>>> import csv
>>> f = open("f", "rt")
>>> reader = csv.reader(f)
>>> for row in reader:
>>>     print(row)
>>> f.close()
['apples', 'bananas', 'pears']
['1', '2', '3']
['4', '5', '6']
```

Without the csv module:

```python
>>> f.open("f", "rt")
>>> for row in f:
>>>   print(row[:-1].split(","))
>>> f.close()
['apples', 'bananas', 'pears']
['1', '2', '3']
['4', '5', '6']
```

The code looks very similar in both situations.  The main difference is without 
using the csv module, you are required to split the row at the separator values to 
produce the list.  I am sure there would be more differences if I wasn't using 
a well formed csv file.

# writing

Using a similar approach to the previous post, I want to read the contents of 
my file, square each integer, and then save to a new file.  There should be 
some differences here as well, and having both approaches handy will highlight them.

With the csv module:

```python
f = open("f", "rt")
g = open("g", "wt")

reader = csv.reader(f)
writer = csv.writer(g)

for row in reader:
    for i, value in enumerate(row):
        try:
            row[i] = int(value) ** 2
        except ValueError:
            pass
            
    writer.writerow(row)

g.close()
f.close()
```

Without the csv module:
```python
f = open("f", "rt")
g = open("g", "wt")

for row in f:
    split_row = row[:-1].split(",")
    for i, value in enumerate(split_row):
        try:
            split_row[i] = str(int(value) ** 2)
        except ValueError:
            pass
            
    for i in range(len(split_row)-1, 0, -1):
        split_row.insert(i, ",")
    split_row.extend("\n")
    g.writelines(split_row)
    
f.close()
g.close()
```

In my example, there are more differences when wirting to a csv than when reading.  If you
don't use the csv module, then you need to split the row at a separator value.  Then you need to 
format the resulting row so that it can be written.  This means you need to convert
non-strings to strings and place commas (",") and line breaks ("\n) appropriately.


# Conclusion

It is very intersting to see why someone would want to write a csv module.  There is a 
lot of routine work to be done to correctly format your file as a csv.  I will definitely 
be using the csv module when reading and writing csv files in the future.

# Next steps

I am curious how to read a file that is not on your computer.  The case I am thinking of is 
obtaining a file from an email attachment, and this file is returned as a sequence of bytes.

How can I read a sequence of bytes that I know is a csv file and begin to perform operations 
such as squaring each number?  I'll let you know.