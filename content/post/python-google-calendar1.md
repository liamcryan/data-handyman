---
title: "Python Google Calendar - part 1"
date: 2017-11-07
tags: ["google calendar", "python", "quickstart"]
draft: false
---

## Google Calendar and Python

Last time we talked about how to search Gmail for a specific email.  Then we found
specific items within the email and created and sent our own email.  And all through
Python using the smtplib and imaplib libraries!!!!!  If that doesn't sound really
interesting, next time I'll use more exclamation marks.

This time we want to create an event in Google Calendar through Python.  A really good
reference is Google's [Calendar API quickstart tutorial](https://developers.google.com/google-apps/calendar/quickstart/python).

I'm going to be referencing the quickstart tutorial, so if you want to follow
along feel free.  Here is what it looks like:

<img src="/img/google.calendar.api.quickstart.png"></img>

##  Python Quickstart Step 1

You need to turn on the Google Calendar API.  It is required no matter what language you would like to
call the API with.  Using what Google calls a
['wizard'](https://console.developers.google.com/start/api?id=calendar), you
will ultimately need to create a project in the Google Developers console
and download credentials.

## Python Quickstart Step 2

Now you should install the Google Client Library.  The purpose of downloading this
library is similar to other reasons you would download a Python library from PyPi,
but really it is needed to be able to let Python interact with the API.

This is a really easy step if you are familiar with python, in a terminal:

```bash
$ pip install --upgrade google-api-python-client
```

If you are using PyCharm as a code editor like I am, you can put it in a requirements.txt file
and PyCharm will know to install it.

## Python Quickstart Step 3

This is where you try out the sample code provided.  Take this code, and put it
in a .py file.  Make sure the credentials you downloaded are in the same folder as this .py file.

## Python Quickstart Step 4

You can run the .py file.  Stuff is going to happen.  A web browser opens and, well, just check out the video:

<center>
<video src="/img/google.calendar.api.quickstart.mp4" controls muted="true" height="auto" width="100%"></video>
</center>

Once you close your web broser, you should see something like this where you ran
the Python file:

```bash
Authentication successful.
Storing credentials to ~/.credentials/calendar-python-quickstart.json
Getting the upcoming 10 events
No upcoming events found.
```

The code in the .py found the next 10 events in your Google Calendar.  I didn't have any.

## Recap

Notice when the .py file was run, a browser was opened.  I was asked to

* ***'Choose an Account'***. -> *if I had not been logged in, I would have been asked to login*

After I chose my data.handyman.01 account,

* ***'gcal'*** -> *this is the name I assigned to my project through the wizard*

wanted to

* ***'View your calendars'***.  -> *this is the level of access that the code in the .py file is requesting*

I clicked

* ***'Allow'*** -> *this lets Google Calendar know it is OK to trust my computer*

and then was shown

* ***'The authenticaion flow has completed'***.  -> *Yay?*

## More Notes

The next time you run this file, your browser won't open.  This is because a lot
happened in the background while we were watching the browser.  OAuth 2.0.

OAuth 2.0 is the protocol used by Google's APIs for authentication, and
involves two sets of credentials.  In our case, we downloaded one set of credentials when
we turned on the Google Calender API, and another was saved to our computer when we clicked
the 'Allow' button.

## Part II coming soon

Part I of the Google Calender was meant to be a quick intro to how to get
started with the Google Calender API.  The whole process enabled you to view then 10 upcoming
events in your Google Calendar.  In Part II, we'll use email data from the
[previous post](/post/python-gmail.md) to populate my Google Calendar.

Looking forward to next time.
