
+++
author = "Liam Cryan"
categories = ["programming", "python"]
date = "2017-11-17"
description = "The part where we actually do something"
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Google Calendar - part 2"
type = "post"

+++

## Google Calendar and Python - Part 2

Let's get to the good stuff.  

[Last time](/post/python-google-calendar1) we set up everything we needed so that
we could interact with the Google Calendar API.  I wanted to use data from a
Formspree email to create an event in my Google Calendar.

To get the data, I ran the code from last time which I saved [here](https://github.com/liamcryan/googleapp/blob/master/googleapp/gmail_python_part_2.py) on github:

```python
if __name__ == "__main__":
    email_info = get_formspree_email_info()
    print(email)
```

The result was:

```python
('Liam Cryan', 'cryan.liam@gmail.com', 'This is a test')
```

There it is!  The data!  

## My Google Calendar Event

I want this to happen:

1.  The Google Calendar app on my phone alerts me 3 days from the time the event was created informing me
of the contactor's name, email address, and subject.

That's it.

## Oh yea, one more thing - Scopes

Google APIs use what they call scopes to identify exactly what a service is
able to access.  When we went through the quickstart tutorial [last time](/post/python-google-calendar1), we were
using a read-only scope.  This time, we want to use a read/write scope.  This
amounts to changing:

* SCOPES = 'https://www.googleapis.com/auth/calendar.readonly'

to:

* SCOPES = "https://www.googleapis.com/auth/calendar"

and deleting this file:

* ~/.credentials/calendar-python-quickstart.json

Ok, we're ready.


## Creating the Event

It is always good to start with an example:

* https://developers.google.com/google-apps/calendar/v3/reference/events/insert#examples

But first, let's get all the data we need to populate the event:

```python
name, email, subject = get_formspree_email_info()
now_date = datetime.datetime.utcnow()
reminder_date = (now_date + datetime.timedelta(days=3)).isoformat() + 'Z'
```

Now let's use the example code populated with our data:

```python
credentials = get_credentials()
http = credentials.authorize(httplib2.Http())
service = discovery.build('calendar', 'v3', http=http)

event = {
  'summary': 'data handyman contact reminder',
  'description': 'Liam!  {} contacted you on your website!\n'
                  'Isn\'t that amazing?\n'
                  'Here is the info:\n'
                  '{}\n{}'.format(name, email, subject),
  'start': {
    'dateTime': '{}'.format(reminder_date),
    'timeZone': 'America/Los_Angeles',
  },
  'end': {
    'dateTime': '{}'.format(reminder_date),
    'timeZone': 'America/Los_Angeles',
  },
}

event = service.events().insert(calendarId='primary', body=event).execute()
print('Event created: %s' % (event.get('htmlLink')))
```

## Did it work?

A natural question to ask is 'did it work?'  If you don't remember going through Google's [quickstart tutorial](https://developers.google.com/google-apps/calendar/quickstart/python), I'll
just tell you that the end result provided you with next 10 events in your calendar.

I'm going to try it now using a little function called 'next_x_events' that essentially wraps
the code from Google's quickstart tutorial.

```python
credentials = get_credentials()
http = credentials.authorize(httplib2.Http())
service = discovery.build('calendar', 'v3', http=http)

next_x_events(service, x=10)
```

Here is the output:

```python
Getting the upcoming x event(s)
2017-11-20T19:49:51-08:00 data handyman contact reminder

Process finished with exit code 0
```

Oh my gosh it worked!


