---
title: "Python Google Calendar"
date: 2017-10-20
tags: ["google", "calendar", "python", "api"]
draft: false
---

## Google Calendar and Python

Last time we talked about how to search Gmail for a specific email.  Then we found
specific items within the email and created and sent our own email.  And all through
Python using the smtplib and imaplib libraries!!!!!  If that doesn't sound really
interesting, next time I'll use more exclamation marks.  [Feedback](/contact/) is
good too.

This time we want to create an event in Google Calendar through Python.

If you remember the first part of the [Python Gmail - Part 1](/post/python-gmail1/)
post, you may remember there was some set-up required.  There is set up required
for Python to interact with Google Calendar as well.  This time, however,
 we will be using Google APIs.

## Setting the Stage

Here is a getting started with the Google Calendar API link from Google:

* https://developers.google.com/google-apps/calendar/quickstart/python

It contains some sample code and walks you through everything you need to use
the API.

I won't even write out the steps because Google does such a good job of
explaining it.  

## Starting with quickstart.py

Assuming you have followed the link above and went through the set-up process,
the file you have created, called quickstart.py will contain the code that
is in the quickstart tutorial.  Also, you should have client_secret.json in
your working directory (in the same folder as quickstart.py).

Try running quickstart.py.  

Here is the general sequence of events:

> A browser and asks you to login to Google.  You are asked if it is OK for your
project to ***View your calendars***.  You click ***Allow***.

> Your browser shows ***The authentication flow has completed.***  

> You close the browser and then look back at your console.  Here is
what you see:

```bash
Authentication successful.
Storing credentials to ~/.credentials/calendar-python-quickstart.json
Getting the upcoming 10 events
No upcoming events found.
```
> You think, "wait, I really have upcoming events, they're just real-life events,
not ones that Google knows about."  

> You then wonder if Google knows about your
upcoming real-life events or if Google is listening to your conversations.  

> "Maybe," you think.  "Maybe."

Now, instead of geting the 10 upcoming events, I want to create 1 Calendar event.

## Scopes

My Google project asked to 'View your calendars' and I clicked 'Allow'.  
The reason it asked to 'View your calendars' is because a readonly scope was
provided in the quickstart.py code.

Here is a sample of the quickstart.py code where the scope is set:

```python
# If modifying these scopes, delete your previously saved credentials
# at ~/.credentials/calendar-python-quickstart.json
SCOPES = 'https://www.googleapis.com/auth/calendar.readonly'
CLIENT_SECRET_FILE = 'client_secret.json'
APPLICATION_NAME = 'Google Calendar API Python Quickstart'
```

The people at Google who made this tutorial left a comment for how to modify
the scope, which is in the block of code above.

Make sure to delete the file ~/.credentials/calendar-python-quickstart.json.  Here
is the updated scope to enable creating a Calendar event:

```python
# If modifying these scopes, delete your previously saved credentials
# at ~/.credentials/calendar-python-quickstart.json
SCOPES = 'https://www.googleapis.com/auth/calendar'
CLIENT_SECRET_FILE = 'client_secret.json'
APPLICATION_NAME = 'Google Calendar API Python Quickstart'
```

## Creating a Google Calendar event

We can modify the quickstart.py code so that it creates a Calendar event.

Replace the code that fetches the 10 upcoming events:

```python
>>> now = datetime.datetime.utcnow().isoformat() + 'Z' # 'Z' indicates UTC time
>>> print('Getting the upcoming 10 events')
>>> eventsResult = service.events().list(
>>>         calendarId='primary', timeMin=now, maxResults=10, singleEvents=True,
>>>         orderBy='startTime').execute()
>>> events = eventsResult.get('items', [])

>>> if not events:
>>>     print('No upcoming events found.')
>>> for event in events:
>>>     start = event['start'].get('dateTime', event['start'].get('date'))
>>>     print(start, event['summary'])
```

With the code to create a Calendar event (found in the link below):

* https://developers.google.com/google-apps/calendar/v3/reference/events/insert#examples

```python
>>> event = {
>>>   'summary': 'A Google Calendar Event ',
>>>   'location': 'My House',
>>>   'description': 'A song by Flo Rida; A place I live; A song I sing at a place I live.',
>>>   'start': {
>>>     'dateTime': '2017-11-08T09:00:00-07:00',
>>>     'timeZone': 'America/Los_Angeles',
>>>   },
>>>   'end': {
>>>     'dateTime': '2017-11-08T09:30:00-07:00',
>>>     'timeZone': 'America/Los_Angeles',
>>>   },
>>>   'recurrence': [
>>>     'RRULE:FREQ=DAILY;COUNT=2'
>>>   ],
>>>   'attendees': [
>>>     {'email': 'data.handyman.01@gmail.com'},
>>>   ],
>>>   'reminders': {
>>>     'useDefault': False,
>>>     'overrides': [
>>>       {'method': 'email', 'minutes': 24 * 60},
>>>       {'method': 'popup', 'minutes': 10},
>>>     ],
>>>   },
>>> }

event = service.events().insert(calendarId='primary', body=event).execute()
print('Event created: %s' % (event.get('htmlLink')))
```

Now run the quickstart.py file.  After the program is finshed, you will see
something like this:
```python
Event created: https://www.google.com/calendar/event?eid=c2lvczgyYjU4N29qaWF1YW9zdW41Z21jdTBfMjAxNzExMDhUMTYwMDAwWiBkYXRhLmhhbmR5bWFuLjAxQG0
```

If you follow the link, you will be taken to your newly created event!

## Closing

The original goal was to create a Calendar event with this data:

* *sender email address*
* *sender subject*
* *sender message*
* *when I need to follow up*

Throughout this post, we've looked at how to use a Google Calendar API to
create an event.  Using this information, as well as information from the
Python Gmail - [part 1](/post/python-gmail1/) and [part 2](/post/python-gmail2/),
you should be more equipped to create a Calendar event using data
retreived via Gmail.

## Until Next Time

Here is a fun video from someone at Google who talks a lot about Google APIs:

* https://www.youtube.com/watch?v=tNo9IoZMelI

Thanks for reading!
