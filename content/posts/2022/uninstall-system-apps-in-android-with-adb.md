---
title: "Uninstall System Apps in Android With ADB"
date: 2022-01-26T10:39:07+05:45
draft: false
description: "Remove bloatware and system apps from Android devices using ADB (Android Debug Bridge). Step-by-step guide to uninstall unwanted manufacturer apps at user level without root access."
cover:
    image: "https://i.imgur.com/MZhbQl3.png"
tags: ["Android", "ADB", "Mobile", "Bloatware", "Tutorial", "System Administration"]
---

> Computer is required 

Android is modified by manufacturers where they add some applications and games that can't be uninstalled through application manager. Some of them are straight for promotion and advertisement; I guess that makes the phone a little bit affordable. But wheb users don't have option to remove the bloatware, is simply unacceptable. Well through ADB(Android Debug Bridge), those bloatwares can be uninstalled for a user level.

First enable Developer settings in your phone. Then turn on USB debugging mode, On most newer devices a notification will popup about the risks of running the system in Debugging mode but you can just ignore it for now. 


Once that is done, download `android-tools` on your computer. 

> **for windows**, [Link](https://www.xda-developers.com/install-adb-windows-macos-linux/)  

> **for linux**,
>> Ubuntu:  `sudo apt install android-tools-adb`
>
>> Arch : `sudo pacman -Sy android-tools`


- Connect your android device to the computer 
- Head to your terminal 
- type `adb devices`, a **popup** will appear on the phone that asks whether to trust the device or not. Press allow
- if your correctly followed the instruction then the output should contain a device attached  

```
❯ adb devices
List of devices attached
20ab686f	device
```

- now type `adb shell`, a shell should be spawned through which you can interact with your phone. 

```
❯ adb shell
1906:/ $ 
``` 

- to interact with the packages/applications android provides a tool called `pm` . 
- First list all the packages with `pm list packages`

```
1906:/ $ pm list packages

package:com.f1soft.esewa
package:com.android.cts.priv.ctsshim
package:com.vivo.weather.provider
package:com.google.android.youtube
package:com.android.internal.display.cutout.emulation.corner
package:com.google.android.ext.services
package:com.atomgame.gunwar
package:com.google.android.networkstack.overlay.vivo
package:com.android.internal.display.cutout.emulation.double
package:com.android.providers.telephony
```

- To search for the package you want to remove use `pm list packages | {application_name}`. here application name can be anything, for the purpose of demonstration let's suppose it's `YouTube`. 


```
1906:/ $ pm list packages| grep youtube
package:com.google.android.youtube
```

- The portion we need starts with a colon, i.e `com.google.android.youtube`. 
- Now remove the package with this command.

```
pm uninstall -k --user 0 {PackageName}
```

```
1906:/ $ pm uninstall -k --user 0 com.google.android.youtube
Success
```

After you finish uninstalling, type `exit` and then disable android debugging. So this is how you uninstall bloatware from your device.


   

