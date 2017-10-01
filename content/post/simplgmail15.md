---
title: "Simple Gmail - part 1 and 1/2"
date: 2017-09-30
tags: ["gmail", "python", "app password"]
draft: false
---

## That Darn App password

Last time, in [Simple Gmail - part 1](/post/simplgmail1/), I talked about how I
wanted Python to interact with Gmail so that data from the contact form could
be organized.  I really only went over how to provide the initial set up for
Python/Gmail interaction.  I didn't like all of the set up that was required,
and thought that I could automate this process.

For the past couple of weeks, I have been working on a Python library called
[simplgmail](https://github.com/liamcryan/simplgmail).  Here is how to create
an app password.

```python
my_app_password = simplgmail.generate_app_password(username, password, phone_number, app_name)
```

That is so much better than having to do it the manual way!  

## Simple Gmail - part 2

Coming soon!  Topics will include:

1.  How to get a Formspree email from your inbox.

2.  How to repond to the sender.

3.  How to save the name, email address, subject, message, and date that a user
submitted through my contact page.

Thank you for reading!
