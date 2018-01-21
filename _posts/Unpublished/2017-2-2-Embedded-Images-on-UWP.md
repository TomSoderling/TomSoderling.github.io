---
published: false
layout: post
title: Using Embedded Images in a Xamarin.Forms App on UWP
author: tom
tags: [UWP, Xamarin, Xamarin.Forms]
---

File this one under the mysteries of UWP.
There is a special, seemingly top secret, overload for ImageSource.FromResource() that is only available on UWP.



Jason Smith, from the [Xamarin Bugzilla](https://bugzilla.xamarin.com/show_bug.cgi?id=39685#c1)

> In .NET Native you need to tell .FromResource what assembly (or type within assembly) the resource can be found in because we can't easily enumerate all assemblies.


<img src="{{site.baseurl}}/images/WiredSurfaceDebugging/thunderboldToEthernetAdapter.png" width="250" />  

