
+++
author = "Liam Cryan"
categories = ["programming"]
date = "2017-09-10"
description = "Getting data from a static site"
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Formspree?"
type = "post"

+++

## Why Formspree?

Creating a form itself isn't too complicated or interesting to me.  What
interests me about creating forms is the 'action' taken once you press the submit
button.

I want to be able to respond to the person who is contacting
me.  So, the 'action' I am looking for the form to take is to
email.  In a later section, I want to go over organizing this data, but
the first step is receiving it.

Formspree is the service I will be using to email the form's data if you haven't
guessed already, and it will enable a form to send data to my email address.  

### Diving into Formspree

Why Formspree?  Hmm.  No real reason...it was high on my google search maybe?
I looked at Formio and thought Formspree would be quicker to get started?  I guess
you've got to start somewhere.

I went to this part of Formspree's site to do a quick test:

* [http://testformspree.com/](http://testformspree.com/)

Follow the directions on the site start with their HTML to get going.
The code below was copied from their site to get you started.  But if you want
to try it out, you need to be using a web server.

```html
<form method="POST" action="http://formspree.io/YOUREMAILHERE">
  <input type="email" name="email" placeholder="Your email">
  <textarea name="message" placeholder="Your message"></textarea>
  <button type="submit">Send</button>
</form>
```

### How is your free lunch?

Ok, so as I mentioned, this service is free.  But free comes with a price!
If you fill out the form on the contact page, you are redirected to Formspree's
website and required to check a box and you might even spend a minute or two
clicking on images!  See the video below :)

<center>
<video src="/img/2017/09/data-handyman-formspree-1.mp4" autoplay="true" loop="true" muted="true" height="auto" width="100%"></video>
</center>

Maybe Formspree isn't exactly what I am looking for, I'll admit.  I can pay them
to remove the reCAPTCHA and redirection, but that would defeat the purpose of
these posts.  I'm a data handyman, and I want you to see possibilities.  I am going
to keep the Formspree form for now, but it will be changed, and I'll let you know what
I come up with as an alternative (to Formspree's $9.99 a month gold plan).

In the future, I'll go through how to organize the form data that was sent to my
email address.

