---
layout: post
title: "Building a Better Progress Bar (nd some more frosting)"
description: ""
category: admin
tags: [frosting, formula]
---
{% include JB/setup %}

Frosting is back! Little tricks for admins to make their users love them and earn their next raise.

Many of you admins may be familiar with the "progress bar formula", using the `IMAGE()` formula to display some stock images files to create a progress bar. [Eternus Solutions](http://eternussolutions.blogspot.com/2015/01/fun-with-progress-bars.html) wrote the trick up on their blog last year, and it's a great way to allow your users to visually see progress against a goal.

At it's simplest, the formula looks like this:

```
IMAGE("/img/samples/color_red.gif","red",10,((Reserved_Cabins__c/Cabins__c)*200)) &
IMAGE("/img/samples/color_green.gif","green",10,(((Cabins_Remaining__c)/Cabins__c)*200)) &
" " & TEXT((Reserved_Cabins__c/Cabins__c)*100) & "%"
```

This draws a pretty basic progress bar:
<img src="http://i.imgur.com/xKmkZMt.png"/>

## How does it work?

We're taking advantage of two facts of the formula system:

<ol>
<li>You can <code>&</code> two <code>IMAGE</code> functions together, and they'll go side by side. The red and green bars are just two images next to one another.</li>
<li>The <code>IMAGE()</code> function has two mandatory arguments (source and alt text) but also two optional arguments, height and width. We are taking a color swatch and asking for it 10 pixels high, and a width according to the % of the ship reserved.</li>
</ol>

We calculate the percent of the ship reserved and multiply that by 200 to get the width of the red bar, and do the same with the percent still available to get the width of the green bar. By multiplying the two percentages by the same number, we guarantee that the bars combined size will be that number, 200 pixels!

## But we can do better

Those sample red and green colors are pretty stark. This isn't super beautiful, and it requires us to look at relative sizes of color. The color isn't adding much here, it's a poor indicator!

But thankfully, when Salesforce released the Lightning Design System, they created a great collection of [colors to use in Salesforce](http://www.lightningdesignsystem.com/design/color). These colors look appropriate in Aloha and Lightning, so we should use those. I've created a zip of color swatches [downloadable here](http://cdcarter.github.io/assets/slds_colors.zip) which you can upload as a static resource:

<img src="http://i.imgur.com/6DjjmVx.png" width="70%"/>

Now, let's update our formula to use the new static resource images.

```
IMAGE("/resource/slds_colors/green.png","png",10,((Reserved_Cabins__c/Cabins__c)*200)) &
IMAGE("/resource/slds_colors/neutral-2.png","neutral",10,((Cabins_Remaining__c/Cabins__c)*200)) &
" " & TEXT((Reserved_Cabins__c/Cabins__c)*100) & "%"
```

Here, we've also changed from using red and green side by side to using a neutral color to fill out the rest of the bar.

<img src="http://i.imgur.com/YUd3usK.png"/>

That looks NICE! Still, we can do one better. This next formula has a few nested if's to set the bar to red, orange, or green depending on how full this ship is.

```
IF(
  (Reserved_Cabins__c/Cabins__c) > 0.65,
  IMAGE("/resource/slds_colors/green.png","MOSTLY FULL",10,((Reserved_Cabins__c/Cabins__c)*200)),
  IF(
    (Reserved_Cabins__c/Cabins__c) > 0.33,
    IMAGE("/resource/slds_colors/orange.png","PARTIALLY FULL",10,((Reserved_Cabins__c/Cabins__c)*200)),
    IMAGE("/resource/slds_colors/red.png","MOSTLY EMPTY",10,((Reserved_Cabins__c/Cabins__c)*200)))) &
IMAGE("/resource/slds_colors/neutral-2.png","",10,(((Cabins_Remaining__c)/Cabins__c)*200)) & " " &
TEXT((Reserved_Cabins__c/Cabins__c)*100) & "%"
```

<img src="http://i.imgur.com/CWpz0SO.png"/>
<img src="http://i.imgur.com/HUryMlV.png"/>

Isn't that pretty? And here, we're giving the user multiple visual indicators of the status. We see color, the bar size, and the printed percent. Plus, our alt text changes depending on the # of reservations, which makes this field still provide a quick presentation of status to users with screen readers too!

## Some other colors

I included all of the primary brand colors in the zip file of swatches

<img src="http://i.imgur.com/rmIQEoF.png"/>

So use the color best for the UI you're building!

## Two other quick tricks...

Since that wasn't cool enough, today I whipped up two other ideas:

### Emoji in Formula

I'm not kidding. It works.

<img src="http://i.imgur.com/FN0eq0D.png"/>

### DaButtonFactory Lightning Styles?

In a former post [I showed off using dabuttonfactory buttons](http://cdcarter.github.io/admin/2015/11/12/frosting) in formulas. I wanted to try and make a button that looks like LEX, but unfortunately DaButtonFactory can't quite get there. But you can get close. Here's an example:

```
IMAGE("http://dabuttonfactory.com/button.png?t=Major+Donor&f=Open+Sans-Bold&ts=14&tc=fff&hp=16&vp=8&c=5&bgt=unicolored&bgc=0070d2&bs=1&bc=569","MajorDonor")
```

<img src="http://i.imgur.com/9KNwUqX.png"/>

<small>Yea, the ship's a donor. Sure...</small>

## Last thing

Yea, the progress bar is broken in LEX. :(
