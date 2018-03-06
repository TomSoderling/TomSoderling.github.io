---
published: false
layout: post
title: Rebranding and Re-Signing iOS Apps
date: 2017-02-13 05:23
author: tom
comments: true
tags: [iOS, Xamarin.Forms, Xamarin.iOS]
---

In the world of enterprise mobile apps, there's often a need to re-label, re-brand, and re-package apps.  Our company produces a white-labeled app that can be rebranded per client if desired, so one single app becomes many apps, all with a custom look and title. On iOS this can be a tricky thing to do, but this post will show you how, and how to automate it with a bash shell script.

Before we dive in, a little perspective. I found this line at the bottom of some Apple documentation and I wish it had been at the top instead. As you build and perfect your bash script, keep this in mind.

> Writing a shell script is like riding a bike. You fall off and scrape your knees a lot at first. With a bit more experience, you become comfortable riding them around town, but also quickly discover why most people drive cars for longer trips.

That being said, bash scripting is becoming one of my favorite ways to automate simple things on my mac and in a Continuous Integration (CI) build for apps.

## What's Bash?

In case this is all brand new to you, as it was for me: "bash" is short for **B**orne-**A**gain **She**ll and it runs on macintosh computers. 

[Put better description here] 

The particular version of bash that your system is running can be **very** important as you look a documentation and script samples from others, so to find out what that is, open Terminal and type in this command

```
bash --version
```
I will sometimes put this line at the beginning of my script, so no matter what machine it runs on (local, CI build agent, coworkers), I can check the bash version to make sure all the commands in my script are going to be compatible and peform as I had planned.


## What needs to happen to an .IPA to re-brand and re-sign?

Here's a list of what we do to take a base app and spit out a rebranded one.  We'll dive into these deeper below, but I want to give a high level perspective of what needs to be done.

- Unzip. An .ipa file is just a zip file that makes up the App Bundle, so we'll need to unzip it to get at its folders and files
- Export the entitlements.plist file.  This will be used later on to resign the bunlde
- Update the Bundle ID a.k.a App ID. Each iOS must have a unique bundle ID, like com.YourCompany.FantasticApp2000.  Since each resigned app is a completely **different** app, it must have a different bundle ID from the base white-labeled app
- 


sds
<img src="{{site.baseurl}}/images/2016-12-16/CompileCheckBox.png" style="width: 500px;"/>


```csharp
var width = 200;
var height = 200;
```



# Shoot. Doesn't this mean _someone else_ can do this to _my app_ too?

Yes. Sorry. And it certainly is being done. A lot of apps that hit the iOS App Store are immediately ripped off by people that resign and resubmit them as their own. So expect that it will happen and plan for it.  

 - These days, most business or enterprise apps are pretty useless without access to the backend server, so being able to control that access is key.
 - Obfuscate code you don't want de-compiled or messed with.  I've heard good things about [Babel](http://babelfor.net). It's a 3rd of the price of the popular Dotfuscator and can obfuscate public methods - something others can't do.
 - [WHAT ELSE CAN BE DONE? Link?]

# Resources

- https://developer.apple.com/library/content/documentation/OpenSource/Conceptual/ShellScripting/shell_scripts/shell_scripts.html
