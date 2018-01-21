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

Start by right-clicking on your project in Visual Studio for Mac and select **Add** > **New File**  
In the dialog, choose > **iOS** (on the left panel) > **Launch Screen**.  The default name "LaunchScreen" is just fine, but can be named anything.   

<img src="{{site.baseurl}}/images/UseAllTheSpaceOniPhoneX/newLaunchScreen.png" style="width: 600px;"/>  

From here, I'd suggest [following this guide from Xamarin](https://developer.xamarin.com/guides/ios/application_fundamentals/working_with_images/launch-screens/#storyboard), on how to design the storyboard and set the constraints for appearance on all devices. 

One additional note:  If for some reason your app _already_ has a storyboard in it, it's probably being used at the Launch Screen already.  After you create the new LaunchScreen storyboard, you may not see "LaunchScreen" appear in the Launch Screen dropdown box when editing your Info.plist file. Quit Visual Studio for Mac and relaunch it and open your solution again. It should now appear.  

<img src="{{site.baseurl}}/images/UseAllTheSpaceOniPhoneX/dropdown.png" style="width: 500px;"/>  
