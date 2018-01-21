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
    For those that are more familiar with Xamarin.Forms, you may be surprised to know that you must now create a storyboard for your iOS app in order expand into that space above and below.  Have no fear.  
    </div>
</div>


Point out that you MUST use a storyboard now