
+++
author = "Liam Cryan"
categories = ["programming"]
date = "2017-09-15"
description = "Required set up for python - gmail interaction"
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Python Gmail - part 1"
type = "post"

+++

## Gmail and Python

In the [last post](/post/formspree/) I mentioned that I wanted to organize the data
received from the contact form.  If you don't remember, the important thing to know
is that the form on my Contact page is sent to my Gmail account via a service
called Formspree.

To do this automatically, I will use Python, my favorite programming language.  Getting
Python to interact with Gmail was not too difficut.  There are a lot of helpful resources
online.

### Gmail Security Hurdles

The biggest problem I was facing when I fist tried this out was Gmail not letting
me access its email server via Python.  This is due to Gmail's security.  Here is
a typical way to connect to Gmail via Python:

```python
import imaplib
m = imaplib.IMAP4_SSL("imap.gmail.com")
m.login("data.handyman.01@gmail.com", "mypassword1234")
```

Here is the error I received:
```
imaplib.IMAP4.error:
b'[ALERT] Please log in via your web browser:
https://support.google.com/mail/accounts/answer/78754 (Failure)'
```

### Use an App Password or Allow Less Secure Apps

The link provided in this error message suggest a couple of options that you have to
get around this problem.  See the highlighted text in the image below for a
description.

<img src="/img/2017/09/gmail1-1.png"> </img>

### ...And an App Password it is

I decided to choose an to generate an App Password because I didn't like the
idea of making something less secure.  Generating an App Password involves first
enabling 2-step authentication, and then generating your App Password.

### Enable 2-Step Authentication<a name="2-step-authentication"></a>

To enable 2-step authentication, I started at this link:

* [https://myaccount.google.com/intro/security?pli=1](https://myaccount.google.com/intro/security?pli=1)

With 2-step authentication, each time you log on to Gmail, your phone receives
a special code which you will have to then enter.  It is more of a hassle
than one password, but it is worth it to me for the sake of getting Gmail to
interact with Python.

### Generate an App Password<a name="app-password"></a>

To generate an App Password, I looked at the following link:  

* [https://support.google.com/accounts/answer/185833?hl=en](https://support.google.com/accounts/answer/185833?hl=en)

 There is a section on the webpage that talks about how to generate an App
 Password as well as some pretty good instructions.  Once you have you 16
 character App Password, run this code to test that it works:

 ```python
 import imaplib
 m = imaplib.IMAP4_SSL("imap.gmail.com")
 m.login("data.handyman.01@gmail.com", "my_app_password")
 ```

 This time you will see this:
 ```
 ('OK', [b'data.handyman.01@gmail.com authenticated (Success)'])
 ```

### Simple Gmail - part 2 - precursor

It is great that we are now set up to interact with Gmail via Python!  Sometimes
you need to get things set up, and while it is important, it isn't always fun.

Topics in part 2 include:

In part 2 we will be going over how to search for the data that Formspree
sent when someone presses the Submit on the Contact form.  We'll also talk about
some things you can do with that data once Python has control of it.

You're awesome.  Thanks for reading.
