---
published: true
layout: post
title: What Every Xamarin.Forms Developer Needs to Know About UWP
date: 2017-02-15 01:23
author: tom
comments: true
tags: [UWP, Xamarin.Forms, .NET Native]
---
Over the last few weeks I've noticed that a **lot** happens in your UWP project when you flip the switch from Debug build to Release build mode.  At the heart of it, has been this little check box on the UWP project build properties: "Compile with .NET Native tool chain".  
<img src="{{site.baseurl}}/images/WhatEveryXFDevNeedsToKnowAboutUWP/CompileCheckBox.png" style="width: 500px;"/>


There are some good posts about [what it means for UWP.]( https://blogs.windows.com/buildingapps/2015/08/20/net-native-what-it-means-for-universal-windows-platform-uwp-developers/#HG2ld3KGHUOiMVQI.97)  
[StackOverflow posts here also](http://stackoverflow.com/questions/37759125/windows-store-apps-windows-8-vs-uwp)

For me, it means that our app can make the long journey into the Windows Store and none of the code-drawn graphics will be displayed on screen.  **Oops**.

We've been using the NControl library in this app for a while and it's been a great tool for drawing custom vector graphics via code.  [NControl](https://github.com/chrfalch/NControl) is a simple Xamarin.Forms wrapper control around the library that does the drawing, NGraphics.  
[NGraphics](https://github.com/praeclarum/NGraphics) is a cross platform library for rendering vector graphics on .NET.  It provides a unified API for both immediate (display to screen) and retained mode (save .png file to disk) graphics using high quality native renderers.

Here's a little sample of how to draw something and place it in an NControl:

```csharp
var width = 200;
var height = 200;

var frame = new NGraphics.Rect(-width/2, 0, width, height);
var corner = new NGraphics.Size(1, 1);
var pen = new NGraphics.Pen(NGraphics.Colors.Blue, 0);

var point1 = new NGraphics.Point(0, 0); // these don't seem to have any effect on the gradient
var point2 = new NGraphics.Point(0, 0);
var brush = new NGraphics.LinearGradientBrush(point1, point2, NGraphics.Colors.Blue, NGraphics.Colors.Green);

var control = new NControlView
{
    DrawingFunction = (canvas, rect) =>
    {
        canvas.DrawRectangle(frame, corner, pen, brush);
    },
    HorizontalOptions = LayoutOptions.Center
};

// Then put it on your Xamarin.Forms page
```

And this is the result:  
<img src="{{site.baseurl}}/images/WhatEveryXFDevNeedsToKnowAboutUWP/SampleNControl.png" style="width: 200px;"/> 

(Note that the docs on the GitHub page are a bit out of date)

Impressively, NControl currently supports native custom renderers for 6 platforms:  

- iOS
- Android
- Windows Phone (8, 8.1, and Silverlight 8.1)
- Windows Store (Windows 8.1)

But was lacking support for UWP (Windows 10).  I built this out about 6 months ago and it's been working really well in our project.  Imagine my surprise when it simply didn't work at all in Release mode :/  
When adding UWP support for these libraries, I used a common "monkey-see, monkey-do" approach, not knowing much of the graphic-y bits in the libraries.  So naturally, I assumed that the monkey messed something up along the way.

Here's a comparison of the Live Visual Tree:   
<table>
<tr>
<td>Here without .NET Native enabled</td>
<td>And with it enabled. Ta-da! no NControl!</td>
</tr>
<tr>
<td style="vertical-align: top">
<img src="{{site.baseurl}}/images/WhatEveryXFDevNeedsToKnowAboutUWP/LVT1.png" alt="NControl is in Visual Tree" style="width: 350px;"/> 
</td>

<td style="vertical-align: top">
<img src="{{site.baseurl}}/images/WhatEveryXFDevNeedsToKnowAboutUWP/LVT2.png" alt="NControl is gone" style="width: 350px;"/>
</td>
</tr>
</table>
  
  
# Debugging .NET Native

Debugging the issue has been a process of examining each component and revisiting the code I wrote, starting at the lowest layer, NGraphics.  The good news there is that it works fine when compiled with .NET Native.  Sweet!  This also gave me the chance to improve things a bit and submit a pull request back to the library (more on that below). 

On to NControl then - it contains all the necessary Windows-specific stuff, like a Windows.UI.Xaml.Controls.Canvas, and helpers to convert the NGraphics.Brush to a Windows.UI.Xaml.Media.Brush, but most importantly, a Custom Renderer that renders an NControlView as a Windows.UI.Xaml.Controls.Grid that can be displayed on UWP.  Hooking the NControl source to my repro project seemed to be the best approach.  

As you may have guessed, debugging a .NET Native compiled app is a little different.  Check out my previous [post on this](https://tomsoderling.github.io/Debugging-NET-Native) for directions, namely, you will want to debug a non-optimized .NET Native build, and this will show you how to do that.

This confirmed the true issue; the constructor of my NControlViewRenderer was never getting hit, thus all the great code I had added for UWP wasn't even getting touched :/  
I've seen this happen before when I forgot to add the assembly attribute above my renderer class.

```csharp
[assembly: ExportRenderer(typeof(NControlView), typeof(NControlViewRenderer))]
```

but that wasn't the issue here.  It's almost like Xamarin.Forms was totally oblivious of this custom renderer...  But why?  Why?  
Why...  
So I searched all the places I usually look: Google, Xamarin Forums/Bugzilla/Docs (_is that the best order?_), pdf copies of books, GitHub issues.  Zero.

It was then that I hit the point that I often hit when troubleshooting development issues

 **_This problem can't be this huge._**

If no 3rd party library's custom renderers work in Xamarin.Forms apps in the Windows Store, that would be a HUGE deal.  It can't be that hundreds of Xamarin developers around the globe are having this issue, can it?  It'd have turned up in a search somewhere, right?  Someone has got to be writing UWP apps for the Windows Store, right?  (maybe not...)


# Leave Curious

So I left work curious (I'd recommend it), and decided to Google just a bit on the bus ride home.  I think the small keyboard on my phone encouraged me to keep the search terse; just the essential keywords of my problem: **UWP ".NET Native" ViewRenderer**

And then I found it.  

One line in an unrelated library's FAQ section about license keys: 

> "For .NET Native compilation, you have to tell Xamarin.Forms which assemblies it should scan for custom controls and renderers"

AH!  That makes so much sense!

Now there have been a few UWP-specific Xamarin.Forms _intricacies_ like this that I think could be documented better. Turns out, there is a special, nearly secret, overload of the Forms.Init method just for UWP where you can pass in an IEnumerable called rendererAssemblies:

```csharp
#if WINDOWS_UWP
    public static void Init(IActivatedEventArgs launchActivatedEventArgs, IEnumerable<Assembly> rendererAssemblies = null)
#else
```

And what does it do with these assemblies?

```csharp
#if WINDOWS_UWP
    Registrar.ExtraAssemblies = rendererAssemblies?.ToArray();
#endif

Registrar.RegisterAll(new[] { typeof(ExportRendererAttribute), typeof(ExportCellAttribute), typeof(ExportImageSourceHandlerAttribute) });
```

And why would you want to register these extra assemblies?

> "Xamarin.Forms may be unable to load objects from those assemblies (such as custom renderers)."  

But the [little snippet of "documentation"](https://developer.xamarin.com/guides/xamarin-forms/platform-features/windows/installation/universal/#Troubleshooting) is a little murky here - there are directions to use this UWP-specific overload, but it's described as a fix for the "Target Invocation Exception" when using "Compile with .NET Native tool chain".  I wasn't seeing any exceptions, so the entire section under this heading wasn't on my radar at all.  The app ran great.  Blissfully unaware that my NControlView should be displayed on the screen.

In fact, I found that this is a requirement for **ALL custom renderers that live in a 3rd party library**.  For example, our app has a custom renderer for playing videos.  It has an implementation for each platform (iOS & UWP) that lives in its respective platform-specific projects and this renderer works great - so that threw us off for a bit.  By contrast, in addition to NControl, we're also using the popular 3rd party Xamarin Image Circle [plugin](https://github.com/jamesmontemagno/ImageCirclePlugin) and it also wasn't working correctly in .NET Native builds, leaving the images square.  

So what other assemblies do we have to include?  
You may notice if you look at the Xamarin Evolve [source code](https://github.com/xamarinhq/app-evolve/blob/master/src/XamarinEvolve.UWP/App.xaml.cs#L92) that James seems to add everything and the kitchen sink here. Not sure if all that is needed.  
The only answer I've heard from Xamarin was from James: "_Mostly you just need third party ones. I just did them all lol cause I wasn’t sure to be honest_"


# The Fix

Here's the bit of code that corrected it.  This goes in the App.xaml.cs file in your UWP head project:

```csharp
// For .NET Native compilation, you have to tell Xamarin.Forms which assemblies it should scan for custom controls and renderers
var rendererAssemblies = new[]
{
    typeof(NControl.UWP.NControlViewRenderer).GetTypeInfo().Assembly,
    typeof(ImageCircle.Forms.Plugin.UWP.ImageCircleRenderer).GetTypeInfo().Assembly
};
	
// Then call Init with these assembiles
Xamarin.Forms.Forms.Init(e, rendererAssemblies);
```

Even though I was calling the Init methods on these libraries:

```csharp
ImageCircleRenderer.Init();
NControl.UWP.NControlViewRenderer.Init();
```    

Without the above code, you get nothing in UWP with .NET Native builds.  No exceptions are thrown.  Nothing in the debug output window.  Nothing on screen.  Silent failure.


**I feel that this should be documented much better.**  Hence this blog post (and I've reached out to the Xamarin docs team).


If you are interested in using NControl or NGraphics on UWP, check out the pull requests [here](https://github.com/praeclarum/NGraphics/pull/63) and [here](https://github.com/chrfalch/NControl/pull/71).  

I'd love it if you would  
1.) try it out  
2.) give any feedback on how it could be improved.  
I'm really excited about this, my first PR to an open source library, so let's work together people!
