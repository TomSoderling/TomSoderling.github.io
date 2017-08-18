---
published: true
layout: post
title: Turn your Mac into a BLE Peripheral Device
author: tom
date: 2017-08-17 12:00
tags: [BLE]
---

This guide will show you how to turn your Mac into a Bluetooth Low Energy (BLE) peripheral device for creating your very own BLE services and characteristics.  


Steps

1. Check if you have node.js installed already
In Terminal, run this command:
```
node --version
```

2. Install node.js on your Mac  
Go to the [Node.js website](https://nodejs.org)  
Download the installer for the latest LTS version (v6.11.2 LTS at the time of writing)  
Follow the installer prompts

3. Download example files from noble github page
[Echo example](https://github.com/sandeepmistry/bleno/tree/master/examples/echo)  
It includes two files: main.js & characteristic.js  
Place these 2 files by themselves in a folder somewhere  

4. In Terminal, navigate to the folder containing your main.js and characteristic.js files 

5. Use Node Package Manager (npm) to install bleno in that directory  
Run this command in Terminal:
```
npm install bleno
```

6. In Terminal, run the main.js script to start advertising your device
```
node main.js
```

7. Open a BLE app, like LightBlue to view your device, its services and characteristics  
[LightBlue for iOS](https://itunes.apple.com/us/app/lightblue-explorer-bluetooth-low-energy/id557428110?mt=8)  
[LightBlue for MacOS](https://itunes.apple.com/us/app/lightblue/id639944780?mt=12)


<img src="{{site.baseurl}}/images/MacAsBLEPeripheral/Devices.PNG" width="250" />  
<img src="{{site.baseurl}}/images/MacAsBLEPeripheral/EchoService.PNG" width="250" />  
<img src="{{site.baseurl}}/images/MacAsBLEPeripheral/EchoCharacteristic.PNG" width="250" /> 
<img src="{{site.baseurl}}/images/MacAsBLEPeripheral/WriteValue.PNG" width="250" />  


Any value you write to the Echo Characteristic (UUID: 0xEC0E) on the device should be echoed in the Terminal window

Output in Terminal
```
State:  poweredOn
on -> advertisingStart: success
EchoCharacteristic - onUnsubscribe
EchoCharacteristic - onWriteRequest: value = ab
EchoCharacteristic - onReadRequest: value = ab

```