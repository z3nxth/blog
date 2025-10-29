---
title: "Rooting a S20FE with Magisk"
date: 2025-10-29
categories: Software
tags: [rooting, android]
---
Rooting is the process of gaining elevated privileges on a comparatively restricted device, such as a phone. In this walkthrough, we will run through the process of rooting a phone (S20FE 5G, `r8q`) running LineageOS 23 (Android 16).
We will be installing Magisk by sideloading it here.

> Please ensure you have a locked bootloader before continuing!
{: .prompt-warning }
> I claim no responsibility for any data loss or bricking that occurs from following this walkthrough. This guide is for the S20FE running LineageOS, although it may work it is not tested and you continue at your own risk.
{: .prompt-warning }

### Rebooting into sideload using `adb`

Firstly, ensure that USB Debugging is enabled on your device. I am using an S20FE running LineageOS 23.
Then, connect your device to your computer with a data USB cable. Unlock your phone, and you should get a prompt asking you to allow USB Debugging:
![](/assets/images/rooting/screenshot.png)

After allowing this, we can then navigate to our terminal and reboot our device into recovery mode:
```bash
adb reboot sideload
```
![](/assets/images/rooting/sideload.jpeg)

After this, we will now be in Lineage's recovery mode where we can install the Magisk `.zip`. Navigate to the [official Magisk Repo](https://github.com/topjohnwu/Magisk/releases/) and download an APK file for flashing
![](/assets/images/rooting/magisk.png)
_(I used `v30.0`)_

Now, we can begin flashing. Before flashing, double-check that the device is in sideload mode:
```bash
adb devices
```
![](/assets/images/rooting/adbd.png)

Now, we can patch the `.apk` to our device:
```bash
adb -d sideload Magisk-v30.0.apk
```

We may get a notice with something such as `Signature Verification Failed`. This is expected, and we can just continue.
![](/assets/images/rooting/sig.png)

![](/assets/images/rooting/mag.png)


After a successful installation, we can boot into our phone as normal and we should successfully be rooted 🎉
![](/assets/images/rooting/reboot.png)

Check the newly-installed "Magisk" app to ensure root was successfully installed.
