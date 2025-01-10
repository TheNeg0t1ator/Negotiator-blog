---
title : "Digital Arrow counter"
date : 2024-06-18
description : "Arrowcounter"

summary : "Arrowcounter"
---
{{< katex >}}
## Why?

How did i get this idea?
Well, as you may or may not know, i do Recurve archery.
And in this sport, for many it is usefull and sometimes compulsary that you count the amount of arrows you shot.
This is a good measurement of how much effort you are putting in.
For this many people use a mechanical counter.
<!-- <image src="https://dosgroup.co.za/wp-content/uploads/2019/10/tally-counter-1-768x768.png" width="300" desc="a mechanical counter"></image> -->
As I do electronics, and found this way of counting a bit archaïc, 
I thought maybe I could try and digitize the way of counting the arrows, storing the amount, date, and distance shot.

### Set count
I'll now go over some of the advantages of the digital systems.
One of the most obvious upgrades I found for the system is counting up a set.
This way if an archer wants, he/she can setup the setcount (amount of arrows in a set).
If enabled (setcount > 1) each time the button is pressed, the counter goes up by the setcount.
example.
If the setcount is set to 1, each time you press the shot button, it counts up by one. (this disables set based features)
If the setcount is set to 6, each time you press the shot button, it counts up by 6.

### keeping Time

One of the advantages of having this system, is that it can automaticcally find shooting sessions,
and calculate the average arrowtime.
Another perk is using a start-stop set counter (*Push to start set, push to end set*). this way you can see total set time, and average shot time in a set. 

### Saving
Another great thing is saving. By this I mean the sessions will be automatically saved to onboard flash. this data can then be used on a pc, to get your training info.

## Part selection
The dificult part, decision making.
### Required specifications.
One of the requirements is to have it be usable for 2 years without a recharge or a battery change.
this meant, going low-power where possible.

### Display
First things first, The display is probably the most important part of this device.
The choice of this is thus quite important.

It should be usable in:
- Dim lit environments (winter evenings)
- Brightly lit environments (full sun, Summertime).

This limits the amounts of displaytypes that are usable.


#### TN LCD
The LCD, one of the most used display in the world so far, found on your phone, to the device your reading this on right now.
As the unlit TN LCD is the one used on most digital watches, (*except smartwatches for some reason?*)
it was the obvious first choice.
on to research then.

#### How does one drive an lcd then?
Well, as it turns out, that was the achiles heel of this solution.
As I found out, a tn lcd needs to be driven by AV (alternating voltage).
this means **I need** to swap the polarity of **each** segment at \\(40 H_z\\) .
This is a problem. this means i give up 10 GPIO pins and a few timers, just to run the display, and also has another consequence.
You cannot, go in low power while displaying.
This was obviously not usable.
> if you want more information on how to drive tn lcd's, EEVBlog has an excelent video series on how to drive them.


#### E-Ink
Well, what type of display is made to be in the sun? **E-INK!**



### Button

### MCU

## Language selection

### C/C++

### Rust?



To be continued