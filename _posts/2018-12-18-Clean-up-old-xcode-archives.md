---
published: true
layout: post
title: Clean Up Old Xcode Archives Automatically
date: 2018-12-18 01:23
author: tom
comments: true
tags: [iOS]
---
What's the best type of cleaning? Cleaning that happens automatically!  
About every 6 months or so, I notice that the free space on my Mac's hard drive is surprisingly low. Have I really been writing that much code?! Even though I use App Center to do all my production, App Store builds, one of the biggest space hogs for a mobile developer are old Xcode archives.



Found a good guide for how to create and edit the job here: https://ole.michelsen.dk/blog/schedule-jobs-with-crontab-on-mac-osx.html


Here's the script: 
<script src="https://gist.github.com/TomSoderling/2c473b6d65f4cede805077a5cdf3030b.js"></script> 




What's __not__ convenient and gets pretty downright annoying is being forced to click "Allow" on this pop-up __EVERY SINGLE TIME__ I do it.  
<img src="{{site.baseurl}}/images/DisableiOSSimulatorPopup/iOSSimulatorPopup.png" style="width: 600px;"/>  
*gah! yes, for the love, just allow them already and stop asking me*

Thankfully, you can use this handy little bash script to help silence this pop-up and deploy without interruption to the simulator. Sometimes, it's the little things that make a big difference. I think once you stop clicking this button on every deploy, you'll wonder how you ever got along without this little script.



## How to Use This Script

1. Copy the script above and save it to a new file with a .sh extension
2. You'll need to change the permissions on the file so you can execute the script, otherwise you'll get a "Permission denied" message if you try. You'll only need to do this once. In terminal, run the command:
```
chmod u+x [the path to your new script file.sh]
```
This grants you the user (u) execution permission (x) on this script file.
3. Then just drag the file into terminal and hit enter to run it. You'll be prompted to enter your system password for sudo access.  


For whatever reason, these two firewall exceptions will be reset whenever you restart your Mac, so put this script in a handy place because you'll need to run it again.  

<br/>
That's it! Hope this saves you a little time and clicks. Don't forget to share this script with you fellow mobile developers!  
