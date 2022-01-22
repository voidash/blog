---
title: "How to switch between Nepali and English keyboard layouts in Linux"
date: 2022-01-22T10:52:37+05:45
draft: false
---

Romanized Nepali has become de-facto standard for typing Nepali. This is going to be a short guide on how to enable romanized nepali on Linux. Keep in mind that there is nothing to download as linux comes with the keyboard layout by default. However some of the linux distribution like Ubuntu make this process even simpler by giving a GUI interface. So This post will link external posts for those distribution and will only contain barebones method to change layout, which will work for any desktop environment running on X11 Server. So this tutorial will not work for wayland. 

> if you are just looking for answer and in hurry. go to your terminal and paste 

` setxkbmap -model pc104 -layout us,np -option grp:alt_shift_toggle `

Now press `alt+shift`. To make it persistent. Go to your `.profile` which is usually on `~/.profile` and paste the command.


### Gnome  
> Distribution covered: Ubuntu, Fedora, OpenSUSE, Manjaro GNOME Edition etc

[Chnage keyboard layout in GNOME](https://help.gnome.org/users/gnome-help/stable/keyboard-layouts.html.en)

### XFCE
> Distribution covered: Linux Mint XFCE, Endeavour OS etc

[change keyboard layout in XFCE](https://forums.linuxmint.com/viewtopic.php?t=55191)

### KDE
> Distribution covered: KDE Neon, OpenSUSE KDE, Fedora KDE, Manjaro KDE etc

[Change keyboard layout in KDE](https://divvun.no/keyboards/userdocs/linux/EnableKeyboardsInKDE.html)

## Using `setxkbmap`

`setxkbmap` sets the keyboard to use layout determined by various options on the command line. If one wants to map their keyboard `CAPS_LOCK` to `ESC`this is the command to go, or if one wants to change the variant of the keyboard from QWERTY to DVORAK, this is the command to look for. If you want to know more type [`man setxkbmap`](https://linux.die.net/man/1/setxkbmap) on your terminal or click the link. 

> [Here is the list of all the possible configuration option for `setxkbmap`.](https://gist.github.com/jatcwang/ae3b7019f219b8cdc6798329108c9aee)


```
setxkbmap -model pc104 -layout np,us -option grp:alt_shift_toggle
```

-  `-model`: *pc104* is a generic keyboard with 104 characters, find your model on the list of possible configuration option. 

- `-layout`: here `us,np` means we are using US layout and Nepali layout. Again for your Country layout code you can look on the link above.

- `-option`: `grp` means switching to another layout and `alt_shift_toggle` is one of the combination to toggle between nepali and english. 





