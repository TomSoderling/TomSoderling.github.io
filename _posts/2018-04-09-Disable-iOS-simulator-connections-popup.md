---
published: true
layout: post
title: Turn Off the iOS Simulator 'Accept Incoming Network Connections' Pop-up
date: 2018-04-09 01:23
author: tom
comments: true
tags: [Xamarin.Forms, iOS, Simulator]
---
Deploying to the iOS simulators is great - so fast and convenient.  
What's __not__ convenient and gets pretty downright annoying is being forced to click "Allow" on this pop-up EVERY SINGLE TIME I do it.  
<img src="{{site.baseurl}}/images/DisableiOSSimulatorPopup/iOSSimulatorPopup.png" style="width: 500px;"/>  
*yes, for the love, just allow them already and stop asking me*

Thankfully, you can use this handy little bash script to help silence this pop-up and deploy without interruption to the simulator. Sometimes, it's the little things that make a big difference. I think once you stop clicking this button on every deploy, you'll wonder how you ever got along without this little script.

## So What Does This Script Do, Exactly?

Yes, before downloading and running a bash script on your personal machine, you should probably know what it's doing. Fear not, everything is on the up and up.  

Your Mac has a firewall. A firewall that doesn't remember that you've clicked "Allow" a thousand times.  
What it doesn't realize is that incoming connections to the Simulator are probably not malicious. In fact, you probably __always__ want to allow those connections because you're writing an app that is designed to make connections with the outside world.  

Thankfully this firewall allows you to make exceptions for certain apps, but even though the pop-up says you can change this setting on the Firewall pane, I wasn't able to do it; hence this script.  

We need to make an exception for two apps: the Simulator and Xcode. In order to do that, we need to turn the firewall off for a moment to add these exceptions, and the last statement in the file turns it back on and gives a confirmation. As you can imagine, turning the firewall off/on requires super user access (that's what the sudo command is for) so terminal will prompt you for you system password.

Here's the script: 
<script src="https://gist.github.com/TomSoderling/9b3d582c4c895dde4ed1eac3f987b764.js"></script>


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
That's it! Hope this saves you a little time and clicks. Don't forget to share this script!  
