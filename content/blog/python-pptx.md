+++
author = "Liam Cryan"
categories = ["programming", "python"]
date = "2017-12-15"
description = "Use pptx_tables to create tables more easily through python-pptx"
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Python-pptx Tables"
type = "post"

+++

## Power Point is where it's at people

#### Something like this happened recently:

> A weekly PowerPoint presentation that relies on some data processing is created.  The process
of creating this PowerPoint is a mixture of manual and automated methods.  My boss says,

> "Hey, we have been asked to put the contents of the weekly PowerPoint presentation into a mobile app."

> "How exciting," I exclaim...but I realize our current data flow is not good enough.  

#### Fast forward:

> Sweet, the data flow is automatic and feeding the mobile app nicely.  Thanks Python.
Do we really need the PowerPoint anymore?  I hear, "yes, we do."

> Do we really?  Uh, Ok. Let's do it.  Python, what do you have for me?


## Python-pptx

[Python-pptx](https://github.com/scanny/python-pptx) is a library that will help you
 to create PowerPoint Presentations programatically.


## My PowerPoint Presentations

The PowerPoint presentation I help with each week contains text, tables, and charts.
The tables and charts will need to be populated with changing data coming from a database.
I want to talk about tables this time.  Suppose the data looks something like this:

```json
{
  "average_number_of_miles": 7.1,
  "week": 1,
  "car": "Toyota"
},
{
  "average_number_of_miles": 5.6,
  "week": 2,
  "car": "Toyota"
},
{
  "average_number_of_miles": 4.4,
  "week": 3,
  "car": "Toyota"
}
```
Here is how you can create a slide and put in a table with Python-pptx:

```python
from pptx import Presentation
from pptx.util import Inches


data = [{"average_number_of_miles": 7.1, "week": 1, "car": "Toyota"},
        {"average_number_of_miles": 5.6, "week": 2, "car": "Toyota"},
        {"average_number_of_miles": 4.4, "week": 3, "car": "Toyota"}]

prs = Presentation()
slide_layout = prs.slide_layouts[1]
slide = prs.slides.add_slide(slide_layout)
shapes = slide.shapes

rows = 4
cols = 3
left = Inches(3.0)
top = Inches(3.0)
width = Inches(6.0)
height = Inches(1.5)

table = shapes.add_table(rows, cols, left, top, width, height).table

# column headers
table.cell(0, 0).text = "average_number_of_miles"
table.cell(0, 1).text = "week"
table.cell(0, 2).text = "car"

# rows of data
for i in range(0, 3):
  table.cell(i+1, 0).text = data[i]["average_number_of_miles"]
  table.cell(i+1, 1).text = str(data[i]["week"])
  table.cell(i+1, 2).text = str(data[i]["car"])

prs.save('table1.pptx')
```
And voila! A slide with your table!

<img src="/img/2017/12/table1.png"> </img>

Using Python-pptx takes quite a bit of lines of code to create a slide with a table on it.  I wanted to create a library that
made this process easier.  Hence ppt-tables.


pptx-tables
-----------

pptx-tables is a Python library that helps you get data into a PowerPoint table through
the help of Python-pptx.

The basic idea is that you feed the data and pptx-tables will put a basic
table on a PowerPoint with this data.  It is meant to make life a little bit easier.  

Here is how much easier.  This will accomplish the same as above:
```python
from pptx_tables imort PptxTable

data = [{"average_number_of_miles": 7.1, "week": 1, "car": "Toyota"},
        {"average_number_of_miles": 5.6, "week": 2, "car": "Toyota"},
        {"average_number_of_miles": 4.4, "week": 3, "car": "Toyota"}]

tbl = PptxTable(data)
tbl.set_table_location(left=Inches(3), top=Inches(3), width=Inches(6), height=Inches(1.5))
tbl.create_table(columns_sort_order=["average_number_of_miles", "week", "car"]
                  columns_headers=["average_number_of_miles", "week", "car"])

tbl.save_pptx("table2.pptx")
```
Wow, practically the same slide as before!

<img src="/img/2017/12/table2.png"> </img>


I am happy to use pptx-tables as an aid for creating the weekly PowerPoint Presentation.  There
are also quite a few things you can do to your tables.  See some examples here: [README](https://github.com/liamcryan/pptx-tables/blob/master/README.rst)
