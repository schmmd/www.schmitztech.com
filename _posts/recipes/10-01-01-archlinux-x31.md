---
layout: default
title: Archlinux on X31
author: Michael Schmitz
category: linux
---

# Installing Arch Linux on a Thinkpad X31

First, let me recommend the [ThinkWiki](http://www.thinkwiki.org) as an
excellent source of information about installing Linux on any Thinkpad.

## Archlinux Customizations

Here are a few things that need to be customized but are not obvious. The first
is installing fonts so Firefox, Open Office, and other programs will work.

```
pacman -S ttf-ms-fonts ttf-cheapskate artwiz-fonts
pacman -S xorg-fonts-75dpi xorg-fonts-100dpi
```

You also should set the locale by editing `/etc/locale.gen` and uncommenting the
two US_ (or your own country code if it differs). `

## Custom Kernel

You don't need to compile one! Fortunately, in the AUR (Archlinux User
Repositories), there is a kernel designed specifically for thinkpads called
kernel26thinkpad. The kernel provides all of the necessary modules and
customizations for me to take full advantage of my thinkpad.

You will still want a few files, however. Run `pacman -S acpi acpid` so that
you can remap your thinkpad buttons. The ThinkWiki has
[http://www.thinkwiki.org/wiki/How_to_get_special_keys_to_work an article] for
more information. I use the following scripts for standby and sleep.
Hibernation worked out of the box after I installed suspend2, and the included
hibernate script works fine for hibernation.

standby.sh
sleep.sh

## Audio
Audio worked out of the box after `pacman -S alsa-lib alsa-utils alsa-oss`.

## Wireless
Wireless works great with the `ipw2200` driver. However, with the new version
of ArchLinux I needed to install the firmware before it would work. Also, I
still have not figured out how to configure wpa_supplicant so that I can access
encrypted networks.

## Graphics

I built a working `xorg.conf` from the sample in [Mark's
Wiki](http://stier.is-a-geek.com/~moinmoin/MarksWiki/LinuxRadeonM6LY/Conf1). It
works out of the box, but with this configuration I have hardware acceleration.


```
# Xorg configuration created by system-config-display

Section "ServerLayout"
        Identifier     "ServerLayout"
        Screen       1  "External Screen"
        Screen       0  "Internal Screen"
        InputDevice    "Mouse0" "CorePointer"
        InputDevice    "Keyboard0" "CoreKeyboard"
EndSection


Section "ServerFlags"
        Option "PM" "off"
        Option "DPMS" "off"
        Option "Xinerama" "0"
EndSection


Section "Files"

# RgbPath is the location of the RGB database.  Note, this is the name of the 
# file minus the extension (like ".txt" or ".db").  There is normally
# no need to change the default.
# Multiple FontPath entries are allowed (they are concatenated together)
# By default, Red Hat 6.0 and later now use a font server independent of
# the X server to render fonts.
   RgbPath      "/usr/share/X11/rgb"
   FontPath    "/usr/share/fonts/misc:unscaled"
   FontPath    "/usr/share/fonts/Type1"
   FontPath    "/usr/share/fonts/TTF"
   FontPath    "/usr/share/fonts/75dpi:unscaled"
   FontPath    "/usr/share/fonts/100dpi:unscaled"
EndSection

Section "Module"
        Load  "record"
        Load  "extmod"
        Load  "dbe"
        Load  "dri"
        Load  "glx"
        Load  "xtrap"
        Load  "freetype"
        Load  "type1"
        Load  "speedo"
        Load  "radeon"
        Load   "drm"
EndSection

Section "DRI"
        Mode         0666
EndSection

Section "InputDevice"
        Identifier  "Keyboard0"
        Driver      "keyboard"
        Option      "XkbModel" "pc105"
        Option      "XkbLayout" "us"
        Option      "XkbVariant" "nodeadkeys"
EndSection

Section "InputDevice"
        Identifier  "Mouse0"
        Driver      "mouse"
        Option      "Protocol" "Auto"
        Option      "Device" "/dev/input/mice"
        Option      "ZAxisMapping" "4 5"
        Option      "Emulate3Buttons" "yes"
EndSection

Section "Monitor"
        Identifier   "Monitor1"
        VendorName   "IBM"
        ModelName    "X31 TFT Screen"
        HorizSync    31.5 - 48.5
        VertRefresh  40.0 - 70.0
#        Option       "DPMS"
        DisplaySize  243.84 182.88
EndSection

Section "Monitor"
        Identifier   "Monitor0"
        VendorName   "Sharp"
        ModelName    "LL-191A-B"
        HorizSync    31.5 - 79.0
        VertRefresh  60.0 - 75.0
        DisplaySize  386.4 289.8
#        Option       "DPMS"
EndSection


Section "Device"
        Identifier      "Card0"
        Driver          "radeon"
        BusID           "PCI:1:0:0"
        Option          "AGPMode" "4"
#        Option          "AGPSize" "16"         # default: 8
        Option          "AGPFastWrite" "false"  # MUST BE FALSE!
        Option          "SWcursor" "true"           # MUST BE TRUE!
#        Option          "RingSize" "4"
#        Option          "BufferSize" "2"
        Option          "EnablePageFlip" "false"          # necessary?
        Option          "EnableDepthMoves" "false"      # MUST BE FALSE!
        Option          "RenderAccel" "false"
        Option          "AccelMethod" "XAA"     # or XAA, EXA
        Option          "DDCMode"
        Option          "SubPixelOrder" "NONE"
        Option          "ColorTiling" "false"   # MUST BE FALSE?
        Option          "DynamicClocks" "true"
        Option          "bioshotkeys"   "True"
        Option          "XAANoOffscreenPixmaps" "true"
EndSection


Section "Screen"
        Identifier "External Screen"
        Device     "Card0"
        Monitor    "Monitor0"
        DefaultDepth    24
        SubSection "Display"
                Viewport   0 0
                Depth     16
                Modes    "1280x1024" "1152x864" "1024x768" "800x600" "640x480"
        EndSubSection
        SubSection "Display"
                Viewport   0 0
                Depth     24
                Modes    "1280x1024" "1152x864" "1024x768" "800x600" "640x480"
        EndSubSection
EndSection


Section "Screen"
        Identifier "Internal Screen"
        Device     "Card0"
        Monitor    "Monitor1"
        DefaultDepth    24
        SubSection "Display"
                Viewport   0 0
                Depth     16
                Modes    "1024x768" "800x600" "640x480"
        EndSubSection
        SubSection "Display"
                Viewport   0 0
                Depth     24
                Modes    "1024x768" "800x600" "640x480"
        EndSubSection
EndSection
```
