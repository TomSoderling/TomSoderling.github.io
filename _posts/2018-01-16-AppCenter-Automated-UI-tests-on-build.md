---
published: true
layout: post
title: How to Run Automated UI Tests from a VS App Center Build
date: 2018-01-17 01:23
author: tom
comments: true
tags: [Visual Studio App Center, Xamarin, Xamarin.UITest, UI Test]
---
Visual Studio App Center continues to amaze and impress me. Over Christmast break, I set up CI (Continuous Integration) and Release/App Store builds for all my little app projects - which was truely a delightful experience - and they've been humming along smoothly. 
It's surprising how quickly I've already taken them for granted. I push code, they build, run the automated launch test on a real device, I get a nice email to install the build on test devices, and can push it to the app stores if it makes the cut. So nice. It's hard to imagine I used to do it any other way.  
<img src="{{site.baseurl}}/images/AppCenter-AutomatedUITests/CIBuilds.png" style="width: 700px;"/> 

Apart from the builds, I think the next most important feature of a good DevOps pipeline is automated testing. We all write unit tests (right???!), which I think is really valuable, but even on my small apps, there are functional areas that I rarely test anymore - now that development of that feature is done.  
For example, one of my apps, Pickster, features a ListView of names where you can swipe on any name and be presented with a context action to delete the name. I realized the other day that I haven't personally tested that functionality for probably 6 months or so. It's just not part of my regular use. I add names a lot as I'm developing and testing, but hardly ever delete one or use those context actions. This app doesn't even really do a lot, but has areas that rarely get manual testing love. Think of big apps. Humans just aren't good at testing everything, or testing it the same every time.  

Enter automated UI tests.  

I really like to use the [Xamarin UI Test Recorder](https://developer.xamarin.com/guides/testcloud/testrecorder/) to get the inital scaffolding of a UI test built and then tweak it from there. Using the recorder, you simply tap/type/swipe around the app and use it like you normally would, and the test recorder translates that into Xamarin.UITest commands that make up your automated UI test.  
<img src="{{site.baseurl}}/images/AppCenter-AutomatedUITests/testrecorder.png" style="width: 1000px;"/> 

Okay, so you've got some UI tests written and running locally on simulator or device - great. Now we want to run them in Xamarin Test Cloud on that huge collection of real mobile devices they've got. 3017 last I heard.  

Start by going to the Test beacon in App Center and click on the grey Test Series button. With CI builds, you have the option to launch the app on a test device at the end of a build. That's a test series named launch-tests.  You'll want to create and name a new one. Say "smoke-tests" or something.  
Next, create a new test run by clicking on the New Test Run button. It guides you through picking your devices, the framework the test are written in (Xamarin.UITest for me), and generates the command to run the tests in App Center, filling in some of the info for you.  
<img src="{{site.baseurl}}/images/AppCenter-AutomatedUITests/generatedCommand.png" style="width: 500px;"/> 
This really only gets you part of the way there though. It took me exactly **34** builds after I generated that command until I had the automated UI tests working as part of my CI build. Thirty. Four. And that was with the help of App Center support towards the end. (Which is really great. They have a nice on-page chat window, and responses were usually really quick). Hence this blog post.  

For whatever reason, the directions from in App Center state to run the command on your LOCAL machine to run the tests in App Center. Well that is incredibly L-A-M-E. We don't want to do stuff. We want the machines to do the stuff for us. Ya know, automation?  

Good news, that command can be run as part of your build pipeline by writing a really small post-build script. It really only needs to contain that one line, and with the help of a guy that failed 34 times at it, you should do just fine.  

Here is the App Center CLI (Command Line Interface) command that you need to put in the script:  
```bash

appcenter test run uitest --app $appName --devices $deviceSetName --app-path $APPCENTER_OUTPUT_DIRECTORY/Pickster.ipa --test-series $testSeriesName --locale "en_US" --build-dir $APPCENTER_SOURCE_DIRECTORY/Pickster.UITests/bin/Debug --uitest-tools-dir $APPCENTER_SOURCE_DIRECTORY/packages/Xamarin.UITest.*/tools --token $appCenterLoginApiToken 

```

Note that this command has 8 parameters, instead of the wimpy 6 in the command that App Center generated for you.  The extras are crucial, and are --uitest-tools-dir and --token

Let's break down what each of these 8 parameters are.  I'll try to explain what they are and what values you'll probably want to use for them.

```bash
--app "tomso/Pickster"  
--devices "tomso/top-devices"  
--app-path pathToFile.ipa  
--test-series "launch-tests"  
--locale "en_US"  
--build-dir [pathToUITestBuildDir] (Yes, this is really the UI test project build directory. When built with the solution, it's in the APPCENTER_SOURCE_DIRECTORY. Use globbing (*), so updating the package version won't break this script.)  
--uitest-tools-dir [where to find test-cloud.exe] (see /packages/Xamarin.UITest.2.2.1/tools in APPCENTER_SOURCE_DIRECTORY)  
--token [The name of the App Center build environment variable that contains your App Center API token generated in step 1.1 above]  
```