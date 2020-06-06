---
published: true
layout: post
title: Code Snippet for Xamarin Month
date: 2020-06-06 01:23
author: tom
comments: true
tags: [C#]
---

Thanks to Louis Matos' for organizing this Xamarin Month on the topic of Code Snippets! Check out the [twitter hashtag to find more posts](https://twitter.com/hashtag/xamarinmonth) or see the [monthly calendar of all posts here](https://luismts.com/code-snippetss-xamarin-month) from Louis.

This Xamarin Month was especially interesting to me because I love code snippets! They save lots of time by stubbing in repetitive code with just a few keystrokes. The snippet I'm posting is one I use almost every day and helps provide a consistent name and outline when writing unit tests.  

## Naming
We write our unit tests using NUnit and follow the [Microsoft naming convention](https://docs.microsoft.com/en-us/dotnet/core/testing/unit-testing-best-practices#best-practices) for all tests so anyone on the team can quickly understand the intent of the test and get all the necessary info just by reading its name.  That convention breaks the name into three parts: 
- The name of the method or unit under test
- The scenario or state under which it's being tested
- The expected behavior or result when the scenario is invoked

These parts are sometimes hard for me to remember, so this snippet gives me some clues to jog my memory.  

## Outline
We also structure every unit test we write using the same basic outline, so anyone on the team can more easily get up to speed on an unfamiliar test and understand what it's doing. That outline is known as the Triple-A (AAA) Pattern: Arrange, Act, Assert.

I'll provide the snippet in two formats.  

The first one you can copy/paste into Visual Studio for Mac by going to Preferences > Text Editor > Code Snippets > C# group, and creating a new code snippet.  
<img src="{{site.baseurl}}/images/XamarinMonthCodeSnippets/VSForMacSnippetScreen.png" style="width: 800px;"/>  

The 2nd format is the entire snippet file as VS for Mac saves it. Just drop the file here: ~/Library/VisualStudio/7.0/Snippets and you're good to go. (may have to restart VS)

<script src="https://gist.github.com/TomSoderling/06cbd9dd3800c1e4beae988c3847d2f9.js"></script> 




That's it! Hope this saves you a little time and typing!
