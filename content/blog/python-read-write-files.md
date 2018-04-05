+++
author = "Liam Cryan"
categories = ["programming", "learning"]
date = "2018-04-04"
description = "How to read and write files in Python"
featured = ""
featuredalt = ""
featuredpath = "date"
linktitle = ""
title = "Reading and Writing"
type = "post"

+++

# Background

I have taken for granted the ability to read and write files in Python.  There
are libraries which make this task very easy, and it is easy to not really
understand what is going on.  This purpose of this post is to help me to
better understand reading and writing to files.  Let's start with the built
in Python function, open.

## open

open is a built in Python function which basically opens a connection to your
file and let's you interact with the data in the file.  Here are some of the
arguments to the function:

**file**: a path-like object

**mode**: optional string specifying the mode in which the file is opened

>  * 'r': read in text mode
>  * 'w': write in text mode, truncate file first
>  * 'b': binary mode
>  * 't': text mode
>  * '+': open for reading and writing

> 'r' is a synonym for 'rt' (read in text mode)

> 'w+b' opens and truncates to 0 bytes for reading/writing

>'r+b' opens but does not truncate for reading/writing

**newline**: controls how universal newlines mode works.  

>  can be None, '', '\n', '\r', '\rn'


# Experimenting

I want to write text to a file that would resemble something seen in a csv.  I
can use the write method to do this:

```python
>>> f = open("f", "wt")
>>> f.write("apples,bananas,pears\n1,2,3\n4,5,6\n")
>>> f.close()
```

My file now looks like this when I open it:

```python
apples,bananas,pears
1,2,3
4,5,6
```

Here is what that looks like reading from Python using the read method:

```python
>>> f.open("f", "rt")
>>> f.read()
"apples,bananas,pears\n1,2,3\n4,5,6\n"
```

So it looks like the read method reads the entire file.  If your file is really
big and you don't want to store all of your data in memory, you could try looping
through each line.

```python
>>> f.open("f", "rt")
>>> for i in f:
>>>   print(i)
"apples,bananas,pears"

"1,2,3"

"4,5,6"

```

You could even get fancy and read in the headers ("apples,bananas,pears") or
perform operations on the specific values.

Let's try accessing specifics:

```python
>>> f.open("f", "rt")
>>> for i in f:
>>>   print(i[:-1].split(","))
['apples', 'bananas', 'pears']
['1', '2', '3']
['4', '5', '6']
```

This is really cool!  I am not even using the csv module, but it would appear
that I am able to reading a csv!

The final thing I would like to do with my file is to perform operations on the
values.  How about square each number.  Then save the results to another file.

This took a little bit of trial and error but eventually came up with this:

```python
f = open("f", "rt")
g = open("g", "wt")
for i in f:
    for j, w in enumerate(i[:-1].split(",")):
        try:
            if j == len(i[:-1].split(",")) - 1:
                g.write(str(int(w) ** 2))
            else:
                g.write(str(int(w) ** 2) + ",")
        except ValueError:
            if j == len(i[:-1].split(",")) - 1:
                g.write(w)
            else:
                g.write(w + ",")
    g.write("\n")
f.close()
g.close()
```

Checking the file on my computer called "g", I see:

```python
apples,bananas,pears
1,4,9
16,25,36
```

# Accomplishments

Today I was able to write and read a csv file without importing any libraries,
only using built in Python functions.  I think this is really cool!

# Next steps

Take a look at the csv module and learn why it is worth using.  

Read files stored as byte strings.
