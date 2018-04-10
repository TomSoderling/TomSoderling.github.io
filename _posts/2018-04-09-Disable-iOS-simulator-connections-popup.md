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
*yes, for the love, just connect already and stop asking me*

Thankfully, you can use this handy little bash script to help silence this popup and deploy without interruption to the simulator. Sometimes, it's the little things that make a big difference. I think once you stop clicking this button on every deploy, you'll wonder how you ever got along without this little script.

## So What Does This Script Do, Exactly?

Yes, before downloading and running a bash script on your personal machine, you should probably know what it's doing. Especially since this one needs your password to execute the commands needed. Fear not, everything is on the up and up.

Your Mac has a firewall. A firewall this is a little too protective and doesn't remember that you've clicked "Allow" a thousand times. What it doesn't realize is that incoming connections to the Simulator are probably not malicious. In fact, you probably __always__ want to allow those connections because you're writing an app that is designed to make connections with the outside world.  

Thankfully this firewall allows you to make exceptions for certain apps, but I wasn't able to do it using the UI; hence this script. We need to make an exception for 2 apps: the Simulator and Xcode.


<script https://gist.github.com/TomSoderling/9b3d582c4c895dde4ed1eac3f987b764></script>


## How to Use This Script

1. Save this script to a new file with a .sh extension
1. You'll need to change the execute permissions on the file so you can actually run the script. In terminal, run:
```
chmod u+x [the path to your new script file.sh]
```
