+++
author = "Liam Cryan"
categories = ["programming", "reporting", "tutorial"]
date = "2018-03-05"
description = "understanding how to create python-pptx workaround functions"
featured = ""
featuredalt = ""
featuredpath = "date"
linktitle = ""
title = "Python-pptx workaround functions tutorial"
type = "post"
+++

# The wonderful world of data visualization using PowerPoint

If you have ever tried to create PowerPoints using Python, you have probably
run into [Python-pptx](https://github.com/scanny/python-pptx) library.  It is a
great tool for helping you create PowerPoint presentations.  You can theoritically
replicate anything in any PowerPoint using this library (statement not fact chcecked).  In practice,
the library has limitations and you may find yourself googling how to do something
that the library is not capable of doing.

# Workaround Functions

The creator of the library, [Steve Canny](https://github.com/scanny),  has written how you can bypass 
the libraries limitations through workaround functions.  I found very few resources that provided the 
amount of help I felt I needed for creating a workaround function and decided to help anyone out that 
was in the same boat I was.

# My need for a workaround

I wanted to turn the red line:

<img src="/img/2018/02/blue-red-lines.png"> </img>

Into a black line:

<img src="/img/2018/02/blue-black-lines.png"> </img>

I looked through the documentation and online to see how to change the color of a particular line.  It 
turns out that the library doesn't have a way to do this for this chart type.  Let's walk through how 
to work around this limitation.

## Approach

The idea to create a workaround function is to find out what is different between the two PowerPoint's (the original and new ppts) xml 
structure.  Once you have identified the differences, then you need to figure out how use the library 
to modify your PowerPoint's xml structure accordingly.

## Create 2 PowerPoints

Create two PowerPoints.  The first one is your original PowerPoint, call it original.pptx.  Remove all of the unnecessary slides so 
your comparison doesn't pick up extra slide information.  The second one is your new PowerPoint, new.pptx.  In my case, I needed to 
manually change the color of the red line to black and save as new.pptx.  

## use the library opc-diag to find the differences

To determine the difference of the xml structure between the two PowerPoints, use the command line tool, 
[opc-diag](https://github.com/python-openxml/opc-diag).  The tool is recommended by the Python-pptx creator for 
doing this type of thing.

In a terminal, navigate to the location of the PowerPoints and type:
```bash
opc diff original.pptx new.pptx
```
You will see a string containing the differences:
```bash
b'--- original/docProps/core.xml\n\n+++ new/docProps/core.xml\n\n@@ -12,5 +12,5 @@\n\n   <cp:revision>119</cp:revision>\n   <cp:lastPrinted>2015-07-16T19:15:58Z</cp:lastPrinted>\n   <dcterms:created xsi:type="dcterms:W3CDTF">2015-06-09T01:07:55Z</dcterms:created>\n-  <dcterms:modified xsi:type="dcterms:W3CDTF">2018-02-28T14:54:05Z</dcterms:modified>\n+  <dcterms:modified xsi:type="dcterms:W3CDTF">2018-03-02T18:27:15Z</dcterms:modified>\n </cp:coreProperties>\n\n--- original/ppt/charts/chart1.xml\n\n+++ new/ppt/charts/chart1.xml\n\n@@ -84,6 +84,13 @@\n\n               </c:strCache>\n             </c:strRef>\n           </c:tx>\n+          <c:spPr>\n+            <a:ln>\n+              <a:solidFill>\n+                <a:srgbClr val="081B53"/>\n+              </a:solidFill>\n+            </a:ln>\n+          </c:spPr>\n           <c:marker>\n             <c:symbol val="none"/>\n           </c:marker>\n@@ -128,11 +135,11 @@\n\n         </c:dLbls>\n         <c:marker val="1"/>\n         <c:smooth val="0"/>\n-        <c:axId val="32150656"/>\n-        <c:axId val="37369728"/>\n+        <c:axId val="11178752"/>\n+        <c:axId val="11180288"/>\n       </c:lineChart>\n       <c:catAx>\n-        <c:axId val="32150656"/>\n+        <c:axId val="11178752"/>\n         <c:scaling>\n           <c:orientation val="minMax"/>\n         </c:scaling>\n@@ -141,7 +148,7 @@\n\n         <c:majorTickMark val="out"/>\n         <c:minorTickMark val="none"/>\n         <c:tickLblPos val="nextTo"/>\n-        <c:crossAx val="37369728"/>\n+        <c:crossAx val="11180288"/>\n         <c:crosses val="autoZero"/>\n         <c:auto val="1"/>\n         <c:lblAlgn val="ctr"/>\n@@ -149,7 +156,7 @@\n\n         <c:noMultiLvlLbl val="0"/>\n       </c:catAx>\n       <c:valAx>\n-        <c:axId val="37369728"/>\n+        <c:axId val="11180288"/>\n         <c:scaling>\n           <c:orientation val="minMax"/>\n         </c:scaling>\n@@ -160,7 +167,7 @@\n\n         <c:majorTickMark val="out"/>\n         <c:minorTickMark val="none"/>\n         <c:tickLblPos val="nextTo"/>\n-        <c:crossAx val="32150656"/>\n+        <c:crossAx val="11178752"/>\n         <c:crosses val="autoZero"/>\n         <c:crossBetween val="between"/>\n       </c:valAx>\n\n--- original/ppt/notesMasters/notesMaster1.xml\n\n+++ new/ppt/notesMasters/notesMaster1.xml\n\n@@ -84,7 +84,7 @@\n\n           <a:p>\n             <a:fld id="{84582515-438F-4227-889B-FA174C31C90B}" type="datetimeFigureOut">\n               <a:rPr lang="en-US" smtClean="0"/>\n-              <a:t>2/28/2018</a:t>\n+              <a:t>3/2/2018</a:t>\n             </a:fld>\n             <a:endParaRPr lang="en-US"/>\n           </a:p>\n\n--- original/ppt/presProps.xml\n\n+++ new/ppt/presProps.xml\n\n@@ -22,6 +22,7 @@\n\n     </p:extLst>\n   </p:showPr>\n   <p:clrMru>\n+    <a:srgbClr val="081B53"/>\n     <a:srgbClr val="333366"/>\n     <a:srgbClr val="5151A2"/>\n     <a:srgbClr val="5DBFD4"/>\n\n--- original/ppt/slides/slide1.xml\n\n+++ new/ppt/slides/slide1.xml\n\n@@ -45,7 +45,7 @@\n\n           <p:nvPr>\n             <p:extLst>\n               <p:ext uri="{D42A27DB-BD31-4B8C-83A1-F6EECF244321}">\n-                <p14:modId xmlns:p14="http://schemas.microsoft.com/office/powerpoint/2010/main" val="235761300"/>\n+                <p14:modId xmlns:p14="http://schemas.microsoft.com/office/powerpoint/2010/main" val="329723006"/>\n               </p:ext>\n             </p:extLst>\n           </p:nvPr>\n\n--- original/ppt/viewProps.xml\n\n+++ new/ppt/viewProps.xml\n\n@@ -10,12 +10,12 @@\n\n   </p:normalViewPr>\n   <p:slideViewPr>\n     <p:cSldViewPr snapToGrid="0" snapToObjects="1" showGuides="1">\n-      <p:cViewPr>\n+      <p:cViewPr varScale="1">\n         <p:scale>\n-          <a:sx n="93" d="100"/>\n-          <a:sy n="93" d="100"/>\n+          <a:sx n="117" d="100"/>\n+          <a:sy n="117" d="100"/>\n         </p:scale>\n-        <p:origin x="-396" y="-42"/>\n+        <p:origin x="-1560" y="-102"/>\n       </p:cViewPr>\n       <p:guideLst>\n         <p:guide orient="horz" pos="2160"/>\n'
``` 

## Tips on reading output

It is difficult to see the difference like this, so try to reformat the string so the ```\n``` become line breaks.  There is also 
some noise which you can ignore, some of which is related to dates and times associated with saving a new PowerPoint.

I had some difficulty viewing the ```\n``` as line breaks in the terminal.  I 
ended up copying the string after the ```b``` (the first character of the output), then using the Python console and typing:
```python
print("the string without the 'b'")
```

Here is what that looks like (without the noise):

```python
--- original/ppt/charts/chart1.xml
+++ new/ppt/charts/chart1.xml
@@ -84,6 +84,13 @@
               </c:strCache>
             </c:strRef>
           </c:tx>
+          <c:spPr>
+            <a:ln>
+              <a:solidFill>
+                <a:srgbClr val="000000"/>
+              </a:solidFill>
+            </a:ln>
+          </c:spPr>
           <c:marker>
             <c:symbol val="none"/>
           </c:marker>
```

Since I am looking at the differnce between charts, a clue that I am on the right track is the 'chart1.xml'.  The ```-``` 
associates with anything that was removed between the two PowerPoints and the ```+``` with anything that was added.

There are a few lines that have been added to the PowerPoint to create the black line.  They appear after the
```<c/tx>``` tag and before the ```<c:marker>``` tag.  The vaule ```000000``` is the hexidecimal 
value for the color black, so we are really on track now.  

## Modify the xml in the original PowerPoint with Python-pptx

Now that we know basically what needs to be changed, we need to figure out how to actually change it.  Since this 
is a tutorial involving Python-pptx, I will assume that the original Powerpoint, original.pptx, was created using
Python-pptx. 

I am going to run the file that creates original.pptx in interactive debugger mode and pause execution when the 
chart gets created.  Without getting too much into creating the chart, here is an idea of what the code looks like:

```python
chart = slide.shapes.add_chart(XL_CHART_TYPE.LINE, x, y, cx, cy, chart_data).chart
print('debugging here to analyze the 'chart' object')
```

## The hard part

We need to find the correct element containing the xml you need to change.  Look in 
the chart object for underlying xml until you have found the ```<c/tx>``` and ```<c:marker>``` tags.  

You can start simple by looking at ```chart._element.xml```.  This will be a string of all of the xml elements 
within the chart.


## Tips on finding the right chart element

I started at the ```chart._element``` level and viewed the xml using the xml using ```chart._element.xml ```.  There 
is a lot of xml not related to the xml element that needs to be
modified.  Looking deeper within the chart object, I realized that I needed to be looking within 
the ```chart.series._element.sers[1]``` level.

In my case, there are two lines.  I found the ```chart.series._element.sers``` object was a list containing two elements.  I 
was able to determine that the second element was the red line I wanted to change, and verified the xml 
contained ```<c/tx>``` and ```<c:marker>``` tags.


## Another hard part

You can't just find the xml element and try:
```python
chart.series._element.sers[1] = 'the xml i want to insert'
```
It doesn't work that way.  You need to add the xml using the existing library methods.  Try to get an idea of all of the 
methods of the ```chart.series._element.sers[1]``` using:

```python
for i in dir(chart.series._element.sers[1]):
    print(i)
```

You should do to do this for each of the xml elements you need to add to determine the method that corresponds to 
adding an element or setting an attribute.  

Since I want to add this xml:
```python
<c:spPr>
    <a:ln>
        <a:solidFill>
            <a:srgbClr val="000000"/>
        </a:solidFill>
    </a:ln>
</c:spPr>
```

Programatically, I really need to add the ```spPr``` element:
```python
<c:spPr></c:spPr>
```
then within the ```spPr``` element add:
```python
<a:ln></a:ln>
```
then within the ```ln``` element add:
```python
<a:solidFill></a:solidFill>
```
then within the ```solidFill``` element add:
```python
<a:srgbClr/>
```
then in the ```srgbClr``` element set the attribute:
```python
val="1000"
```

## My workaround function

Putting it all together, this amounts to:

```python
sp_pr = chart.series._element.sers[requirement_index].get_or_add_spPr()  # add the spPr element
ln = sp_pr._add_ln()  # within the spPr element add the ln element
solid_fill = ln._add_solidFill()  # within the ln element add the solidFill element
srgb_clr = solid_fill._add_srgbClr()  # within the solidFill element add the srgbClr element
srgb_clr.set("val", "000000")  # in the srgbClr element set the attribute
```

## Final Comments

This task seemed pretty daunting at first, and I wasn't sure if I would be able to figure it out.

Once I started this process, I had a black line within about 6 hours.  It was time well spent I would 
say, because I have much more confidence to make custom changes outside the ability of the Python-pptx api.

I hope you don't need to spend 6 hours on your workaround function, and you find this tutorial helpful.