---
published: true
layout: post
title: Let Your App Use all the Extra Screen Space on iPhone X
date: 2018-01-20 01:23
author: tom
comments: true
tags: [Xamarin, Xamarin.Forms, iOS]
---
<div>
    <div style="display: inline-block; width: 80%; vertical-align: top;">The iPhone X has been out since early November (2017) and it is easily the best phone I've ever used. <br/>
    Great size, glass on glass on stainless steel design, wireless charging using Qi standard, fantastic gesture-based navigation, IP67 water resistant, convenient face ID, and the best camera I've ever owned. But topping the list is the screen; gorgeous, highly color accurate OLED that makes you think you're touching the very pixels. <br/> 
    With that nice screen comes some extra space. Space that all my old apps don't understand how to make use of. I've found myself struggling to remember each time how exactly to take advantage of that space, so here are the steps.
    </div>
    <div style="display: inline-block;" align="top">
        <img src="{{site.baseurl}}/images/UseAllTheSpaceOniPhoneX/iphone-x.png" width="150" />  
    </div>
</div>


<div>
    <div style="display: inline-block;" align="top">
        <img src="{{site.baseurl}}/images/UseAllTheSpaceOniPhoneX/unusedSpace.png" style="width: 300px;"/>  
    </div>
    <div style="display: inline-block; width: 65%; vertical-align: top;">This app is sad.  It doesn't know to use all those extra beautiful iPhone X pixels. <br/> <br/>  
    
    Turns out, if you want your app to expand into that new space above and below, you <b>must</b> create a Launch Screen Storyboard.  For those that are more familiar with Xamarin.Forms apps, this may be foreign, but don't worry. <br/> <br/> 

    Most of us are familiar with this screen, where we can specify a Launch Screen image for each device size, type (iPhone, iPad, Apple TV), and screen resolution. <br/>
    <img src="{{site.baseurl}}/images/UseAllTheSpaceOniPhoneX/launchImages.png" style="width: 300px;"/>
    </div>
</div>

As of iOS 8, we're now able to use a single Unified Storyboard to make a Launch Screen that looks correct in all cases. I've just never done it until needing to support iPhone X.

# Let's Do It

Start by right-clicking on your project in Visual Studio and select Add > New File  
In the dialog, choose > iOS (on the left panel) > Launch Screen  
<img src="{{site.baseurl}}/images/UseAllTheSpaceOniPhoneX/newLaunchScreen.png" style="width: 300px;"/>  
newLaunchScreen


[Guide from Xamarin on how to do this](https://developer.xamarin.com/guides/ios/application_fundamentals/working_with_images/launch-screens/#storyboard)