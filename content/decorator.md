+++
title = "Decorator pattern in practice"
date = 2018-11-19

[taxonomies]
categories = ["programming"]
tags = ["patterns"]
+++

I had a simple task - implement a bunch of buttons.

Designer gave me paper on how buttons should look. There were a list of different designed buttons
with some similarities and some differences.
- Button with text in center and border
- Button with icon, text and border
- Button with text only
- Button with 2 text views, one is button title and other is radio state (yes/no) (kind of radio button)
When any of this buttons are focused - background is colored in a color (yellow), border (if any) should disappear, text should change its color to black.

So, having this requirements, I started to design the code in my head. After some time thinking about how should I do this, i finally thought `Decorator Patter` would be the best fit.

First I needed to create a logical separation of buttons and their properties, so I did this in this way:

I separated all this into __Button__, __Content__, __Properties__.

__Button__ is the button itself, it can be __Push__ or __Switch__ button.

__Content__ is what is considered _inside_ button, it can be __icon__ or __text__ or both.

__Properties__ is what decorates a button, it can be __border__ or __background__.

Any of this buttons can be __decorated__ with any of this properties.

I've taken my notebook and pencil and draw a diagram of how it could look.
_TODO: Add here a picture from my notebook_


