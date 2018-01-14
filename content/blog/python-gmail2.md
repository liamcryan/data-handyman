
+++
author = "Liam Cryan"
categories = ["programming", "python"]
date = "2017-10-09"
description = "Finding and sending emails with Gmail through Python"
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Python Gmail - part 2"
type = "post"

+++

## Gmail and Python (part 2)

Last time we went over the required steps for getting Python to interact with
Gmail.  They are:

1)  [Enable 2 step authentication](/post/python-gmail1/#2-step-authentication)

2)  [Create an app password](/post/python-gmail1/#app-password)


### Finally! Let's get to the data!

I want to go over how to get a Formspree email and then send a response to the
sender.  To search for an email we will be using the imaplib library, and to send,
we'll use the smtplib library.


### Login using imaplib

```python
>>> import imaplib
>>> m = imaplib.IMAP4_SSL("imap.gmail.com")
>>> m.login("data.handyman.01@gmail.com", "my_app_password")
('OK', [b'data.handyman.01@gmail.com authenticated (Success)'])
```

OK, looks like I was able to login!

### Select the Formspree label (the Formspree folder)

Now, I'll select the label I created (within Gmail itself),
the 'Formspree' label (which is basically like a folder).

```python
>>> m.select("Formspree")
('OK', [b'1'])
```

As I selected the Formspree label, the response was OK and [b'1'].  The
[b'1'] part of the reponse is telling me that I have 1 email in this label and
it is identified by '1'.

### Grab the Formspree email

The email I am looking for is identified by '1', so I want to use this information
to find the Formspree email.

```python
message = m.fetch(b'1', '(RFC822)')
```

The response is very long so I won't put it here.  Some of the things I need from
the 'message' variable are:

1.  The sender's name

2.  The sender's email address

3.  The sender's subject

Here is an image of the opened Formspree email.

<img src="/img/2017/10/gmail2-1.png"> </img>

* The sender's email address in this case is **'cryn.lim@gmail.com'**

* The subject I am looking for is **'This is a test'**  

Not to be confused with the actual sender ***submissions@formspree.io***, and the
actual subject ***New submission from data-handyman.com/contact/***.


### Finding the sender's data

A fairly simple way of obtaining sender's name, email and subject involves converting
the message to a string, then using the find method for the fields we want.  Here is
what that looks like:

```python
>>> msg = str(message[1][0][1])
>>> a = msg.find("name:")
>>> msg[a:a+100]
'name:\\r\\nLiam Cryan\\r\\n\\r\\n\\r\\nemail:\\r\\ncryn.lim@gmail.com\\r\\n\\r\\n\\r\\nsubject:\\r\\nThis is a test\\'
```

It looks like there is a pattern:

* **'name:'**

* **'\\\r\\\n'**

* **'Liam Cryan'**

* **'\\\r\\\n\\\r\\\n\\\r\\\n'**

*  **'email:'**

*  **...etc...**


Let's see if this pattern holds true for all of the fields I want.

```python
>>> a = msg.find("name:\\r\\n")
>>> b = msg.find("email:\\r\\n")
>>> c = msg.find("subject:\\r\\n")
>>> d = msg.find("message:\\r\\n")
>>> msg[a+len('name:\\r\\n'):b-len('\\r\\n\\r\\n\\r\\n')]
'Liam Cryan'
```

Yes!  It looks like we got the sender's name.  Let's try the rest:

```python
>>> msg[b+len('email:\\r\\n'):c-len('\\r\\n\\r\\n\\r\\n')]
'cryn.lim@gmail.com'
>>> msg[c+len('subject:\\r\\n'):d-len('\\r\\n\\r\\n\\r\\n')]
'This is a test'
```

Awesome!  The pattern worked and we are able to get all of the fields we wanted!

### Sending a response email

Now we will be switching gears to the smtplib.  Using the data that we have collected
with the imaplib, we will send an email.

I want to send an email that looks something like:

* subject:  **"This is a test"**

* body:  'Hi **"Liam Cryan"**, thanks for your email.  I'll be getting back to you shortly,
and look forward to talking with you.'


### Logging in with smtplib

```python
>>> import smtplib
>>> s = smtplib.SMTP('smtp.gmail.com:587')
>>> s.starttls()
>>> s.login("data.handyman.01@gmail.com", "my_app_password")
(235, b'2.7.0 Accepted')
```

Wow, this is going really well.  Let's try sending the email.

### Actually sending the email

Earlier we figured out who to send the email to and also the subject.

```python
>>> send_to = msg[b+len('email:\\r\\n'):c-len('\\r\\n\\r\\n\\r\\n')]
>>> send_subject = msg[c+len('subject:\\r\\n'):d-len('\\r\\n\\r\\n\\r\\n')]
>>> send_name = msg[a+len('name:\\r\\n'):b-len('\\r\\n\\r\\n\\r\\n')]
>>> print((send_to, send_subject, send_name))
('cryn.lim@gmail.com' 'This is a test', 'Liam Cryan')
```

smtplib has a method, sendmail, which is used to send emails.  It does not
take a subject as a parameter, but instead can be placed within the message.  Here
is what I am talking about:

```python
message = "Subject:{}\n\nHi {}, thanks for your email.  I'll be getting back to you shortly and look forward to talking with you".format(send_subject, send_name)
>>> s.sendmail(from_addr="data.handyman.01@gmail.com", to_addrs=send_to, msg=message)
{}
```

### Checking that it worked

Let's go back to the imaplib and select the 'Sent Mail' label to check that the message sent.

Logging in again:

```python
>>> import imaplib
>>> m = imaplib.IMAP4_SSL("imap.gmail.com")
>>> m.login("data.handyman.01@gmail.com", "my_app_password")
('OK', [b'data.handyman.01@gmail.com authenticated (Success)'])
```

Selecting the 'Sent Mail' label:

```python
>>> m.select('"[Gmail]/Sent Mail"')
('OK', [b'10'])
```

I selected [Gmail]/Sent Mail...weird right?  Well you can list out all of the labels using:

```python
>>> m.list()[1]
[b'(\\HasNoChildren) "/" "Formspree"', b'(\\HasNoChildren) "/" "INBOX"', b'(\\HasChildren \\Noselect) "/" "[Gmail]"', b'(\\All \\HasNoChildren) "/" "[Gmail]/All Mail"', b'(\\Drafts \\HasNoChildren) "/" "[Gmail]/Drafts"', b'(\\HasNoChildren \\Important) "/" "[Gmail]/Important"', b'(\\HasNoChildren \\Sent) "/" "[Gmail]/Sent Mail"', b'(\\HasNoChildren \\Junk) "/" "[Gmail]/Spam"', b'(\\Flagged \\HasNoChildren) "/" "[Gmail]/Starred"', b'(\\HasNoChildren \\Trash) "/" "[Gmail]/Trash"']
```

Look!  the label is called [Gmail]/Sent Mail.  Also weird is that I needed to have single quotes
around the double quotes '"[Gmail]/Sent Mail"'.  I kept getting errors until I put the single
quotes around the double quotes.

Grab the most recently sent email (the 10th):

```python
>>> sent_message = m.fetch(b'10', '(RFC822)')
>>> str(sent_message[1][0][1]).find("cryan.liam@gmail.com")
7
>>> str(sent_message[1][0][1]).find("This is a test")
609
>>> str(sent_message[1][0][1]).find("Hi Liam Cryan,")
631
>>> str(sent_message[1][0][1]).find("I'll be getting back to you shortly and look forward to talking with you")
670
```

All right!  We have just verified that the email was sent.  We found the sender email,
the sender subject, the sender name, and the message we wrote to the sender in
the sent email!

### Thoughts on Gmail and Python

Being able to automatically send someone an email after they contact you is really
cool.  A lot of apps and services use some sort of emailing system to contact you
automatically.  I want to go one step further and make sure I actually get back to
the sender.  Imagine I send an automatic email when you contact me saying I will contact you soon, but I never do!
That's not cool.  I think I have a fun solution that will help me respond to
a sender.  

###  Google Calendar

Yes, Google Calendar, why not?  I originally thought about organizing
the sender's data in Google Sheets, but Google Calendar just sounds more
fun and interactive.  I have never used the Google Calendar API, but I know
there will be more set up.  I also want the reminder to have some information such as:

* *sender email address*
* *sender subject*
* *sender message*
* *when I need to follow up*

I think the next post will need to be called Python Google Calendar.  Looking forward
to next time!
