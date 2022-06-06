 ---
author: "Koushik"
date: 2020-04-03
#linktitle: MacOS on Linux VirtualBox
title: MacOS on Linux VirtualBox
weight: 10
comments: true
tags: ["macOS", "VirtualBox", "File Sharing", "Samba"]
categories: ["Hackintosh"]
---


Installation of VirtualBox was straight-forward on KDE Neon as I had to download the Ubuntu version from [this website](https://www.virtualbox.org/wiki/Linux_Downloads) and run the following code from the downloaded directory.
```
sudo dpkg -i package-name.deb
```
Following the instructions by Helmreich Joinery on [Youtube](https://www.youtube.com/watch?v=FTnn4BrpJro) made the installation process faster and easier. Although few complications were there after the installation as it failed to boot into the OS itself. The boot sequence got stuck in the line stating a **`kernel panic`** followed by **`system uptime in nanoseconds: some_number`**. After reading through the comments section, it was confirmed that the error was due to a wrongly set cdpuidset parameter which was part of the shell script. The following command would modify the cpuidset parameter if entered in the terminal.
```
VBoxManage modifyvm Sierra --cpuidset 00000001 000306a9 04100800 7fbae3ff bfebfbff
```

Shown below is the picture of first successful boot of macOS Sierra from VirtualBox.

![Successful installation of macOS Sierra on a virtual machine](/images/pic1.jpg)
 

#### Transferring files from host to guest machine

Now came the tough part, transferring files from the _host machine_ (i.e. Linux in my case) to _guest machine_ (macOS Sierra). Initially on the guest machine, I tried using the standard command for adding a new user on the guest machine which is,  **`sudo adduser USERNAME vboxsf`** after creating a shared folder on the host machine. It did not work, as expected, since it was not a _Unix_ command.

After a lot of trial and error, I came across this post (https://askubuntu.com/questions/22973/share-folder-with-mac-guest-on-a-linux-ubuntu-host)  where one of the comments read that _"Guest Additions for Mac doesn't exist"_ and the only way out of it would be setting up Samba on the host machine and connecting it from the guest machine by typing **`smb://ip_of_host`**. It was later that I realised, setting up Samba would not only benefit the host machine but all the devices connected to that network.
 
#### Logic Pro X on VirtualBox MacOS
 
 Installed Logic Pro X on macOS Sierra very well knowing that VirtualBox would not be able to handle such heavy softwares.

 ![Logic Pro X running on macOS on a virtual machine](/images/pic2.png)
 
 As I tried to record a normal midi track, I started seeing glitches. The seek was not moving at all after the recording began but the midi data was getting saved as usual. Working with such a software on VBox was such a pain. Unable to record any further, I decided to uninstall the software but keep the OS for sometime just to get a hang of it.
