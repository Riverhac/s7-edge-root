
# S7 Edge Rooting Guide

If you have the single or dual variant of the S7 Edge G935F
(Global) this guide will help you to root and flash the device.

The global version of the device seems to be the one sold
on Amazon, its package has also the `64Bit Octacore` label
on it.

Chipset: Exynos 8890 Octa
CPU: Quad-core 2.3 GHz Mongoose + Quad-core 1.6 GHz Cortex-A53



# Key Combos for Reboot

Download Mode: [Vol Down] + [Home] + [Power]
Recovery Mode: [Vol Up] + [Home] + [Power]



## 0. Install Heimdall

You have to install [Heimdall](http://glassechidna.com.au/heimdall)
in order to flash the device's recovery partition. Heimdall as a
tool is essentially necessary and somehow inavoidable.

On OSX El Capitan and upwards, you have to use a newer version
than the binary builds offered on Heimdall's website.

I uploaded a copy of the dmg file into the releases section of
this repository.



## 1. Make a copy of the PIT partition file

Have the device already booted up Android. Activate Developer
Mode by going into Settings / About Device / Kernel and tapping
a couple times on the `Build Version`.

Go to Settings / Developer Mode and turn off `OEM Lock`.

Now you have to make a copy of the PIT file.
The device will automatically reboot after each heimdall command
(which is okay and _wanted_).

Make sure you have everything backed up.
Bootup the device in Download Mode.

On your computer, use heimdall to create the partition table file:

```bash
heimdall download-pit --output my-s7edge.pit;
```

The device will reboot. When the screen turns black, quickly
get into Download Mode again.



## 2. Flash the TWRP Recovery Image

On your computer, use heimdall to flash the TWRP recovery image
that ships with this repository:

```bash
heimdall flash --RECOVERY recovery.img --pit my-s7edge.pit --verbose;
```

The device will reboot. When the screen turns black, quickly
get into Recovery Mode (NOT Download Mode).



## 2. Format data partition

In Recovery Mode, follow these steps:

- Swipe to allow Modifications
- Select Wipe
- Select Format Data / EXT4 (explicitely important and necessary)
- Reboot device (into Recovery Mode)



## 3. Install SuperSU

In Recovery Mode, follow these steps:

- Swipe to allow Modifications
- Select Advanced
- Select ADB Sideload
- Check Wipe (both) Caches
- Swipe to activate ADB Sideload

Go to your computer and use adb:

```bash
adb sideload SuperSU-v2.74-2.zip
```



## 4. Reboot into Android OS

Now you got a rooted device and can use adb shell and su
in order to modify the device.



## 5. Further Hints

The /system partition is mounted read-only by default.
If you want to get rid of the shitty stock apps (you
probably don't need them as nearly every person on the
planet) you can use adb shell in order to modify it now.

First, you have to activate Developer Mode and USB Debugging again.
Install the ADB tools on your system (probably android-platform-tools package).

```bash
adb shell;

# ... connects to Android device ...

su;
mount -o rw,remount,rw /system;

cd /system/priv-app;

ls -la; # will show all shitty apps
rm -rf someshittyapp;
```

In case you are unsure which packages were installed by
default on the system this repository also contains the
[packages.txt](packages.txt) containing a list of those.

