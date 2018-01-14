+++
author = "Liam Cryan"
categories = ["reporting"]
date = "2018-01-13"
description = "Central Location for your pdf reports"
featured = "pdf-repo-viewer.png"
featuredalt = ""
featuredpath = "date"
linktitle = ""
title = "Pdf Repository Viewer"
type = "post"

+++

## From PPTX to PDF

In the [last post](/post/python-pptx/), I talked about how to programmatically create Power Point Presentations.  It 
was great fun and today's topic is somewhat of a logical continuation.


The problem with pptx files is that they appear slightly differnetly depending upon the program that opens the file.  I 
found this out as I was neatly crafting the sizes of my charts and tables, only to find that when I opened it with Microsoft 
PowerPoint they looked very wrong.  My solution was to convert to pdf first.  Apparently pdf files appear the same despite the 
program opening it.  Awesome!


## Motivation for a central location

Suppose you are someone who creates a lot of reports.  These reports need to go out to a lot of people.  How are you going 
to get it to them?

* #### Option 1:  Give them the reports

* #### Option 2:  Tell them where to find the reports
    

Before continuing, I want to talk about the some immediate consequences.

#### Option 1:  Give them the reports

    1.  The receiver has the report
    2.  The receiver archives the report wherever they please
    3.  The receiver comes to you when a new report is needed

#### Option 2:  Tell them where to find the reports

    1.  The receiver has the report
    2.  The receiver archives the report wherever they please
    3.  The receiver looks for the report when a new report is needed
    
I don't want to try to push this argument one way or another (haha, yes I do), but I would say that there is only one real difference.


## What's the difference

Is it better for a receiver of a report to come to you when they need a report, or is it better for the receiver to look for the report when the need it?

Well, that depends.  Does the receiver know where to look?

Most of the time the answer is no.  So I would say it is better for you to keep giving them reports.  Sound appealing?

Honestly, it doesn't seem like a big deal to email reports.  But trust me, it can be!  So, what if the receiver knows where to look?  

Ok, now we are getting somewhere. 


## Finding a suitable location for your pdf reports

It may not be an easy task to find somewhere to place your pdf reports.  When I need to get a report to someone at work, I always send the report.  This 
is because people don't really know where to find my reports.  They are all stored on an internal network.  This is for security, but it also 
compounds the issue because some people in other regions of the company are unable to access the files (sounds weird, right?).


## Deciding on a central location

I decided that it would be cool if people could view my reports through the internet.  This would solve the problem of not knowing where to find a report.  People
would also know that any report I put there is not still being worked on.  An added benefit of this is that pdf files can be opened, viewed, and downloaded in a browser on a computer or a phone, so actually viewing it is nice.


## Aaa-chhooo, PDF Repository Viewer

Yes, I just sneezed out a pdf repository viewer.  A demo is here:  https://pdf-repo-viewer.herokuapp.com/

Basically, this demo lets you navigate through a filesystem and open up pdf files.  It sounds super exciting, I know.  

I had a lot of fun making it, and plan on improving it in the future.  One problem with it right now is that literally anyone can view 
these files.  For it to be useable at the workplace, there needs to be access control.  Luckily in Python via Flask, this can is accomplished
quite often and is documented all over the internet.


And we are off to greener pastures.  Thanks for reading.  Now go out and do some push ups.