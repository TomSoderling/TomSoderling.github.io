---
published: true
layout: post
title: Xamarin.Forms Code You Can Delete Today! (part 1)
date: 2018-03-06 01:23
author: tom
comments: true
tags: [Xamarin.Forms, Performance]
---

<div>
    <div style="display: inline-block; width: 80%; vertical-align: top;">
    What's more fun than writing code?
    <br/><br/>
    Deleting it! 
    <br/><br/>
    Your Xamarin.Forms app definitely contains some code that you can delete. Code that is causing you a performance hit. Code you can delete today, right now, pronto, with no risk. <b>Seriously</b> 
    <br/><br/>
    Let's get philosophical for a moment. <br/>
    As much as we as developers really love to solve problems by writing code - _deleting_ code, (or writing less of it in the first place) should be a fundamental desire of mature developers who recognize that each line of code written is a trade-off: is this code worth the potential bugs and the cost of care and feed (maintenance) down the road? 
    </div>
    <div style="display: inline-block;" align="top">
        <img src="{{site.baseurl}}/images/XFCodeYouCanDelete/Anticode.png" style="width: 150px;"/> 
    </div>
</div>



## Good News for Deleters

This tip comes from the now-famous Optimizing App Performance with Xamarin.Forms talk that Jason Smith gave at the 2016 Xamarin Evolve conference. Even though the talk was given nearly 2 years ago, I/we/apps are still breaking many of the cardinal Xamarin.Forms no-no's and needlessly paying the price in performance.  

Here are the detailed steps to delete this code from your app:

1. Go to Visual Studio and search for LayoutOptions.Fill in your entire Xamarin.Forms solution  

<img src="{{site.baseurl}}/images/XFCodeYouCanDelete/FindLayoutOptions.Fill.png" style="width: 300px;"/>

2. Delete all those lines of code. (I like to use cmd+x)



That's it!  


## But Why?

1. The default value of a view's HorizontalOptions and VerticalOptions properties [is _already_ LayoutOptions.Fill](https://developer.xamarin.com/guides/xamarin-forms/user-interface/layouts/layout-options/#Overview), so it's completely unnecessary to set this.  

1. Every time you set it, your app is taking an unnecessary performance hit on the chin. Even though you’re setting it to what it already was, it comes with a cost.  



## Resources
- [Optimizing App Performance with Xamarin.Forms, by Jason Smith](https://www.youtube.com/watch?v=RZvdql3Ev0E)
- Kent Boogart put together this [really great, easily accessible summary](https://kent-boogaart.com/blog/jason-smith%27s-xamarin-forms-performance-tips) that's helpful as a reference