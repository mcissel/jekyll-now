---
layout: post
title: Ubuntu on a Virtual Box
---
I was using Ubuntu for a while and I like it, but it wasn't quite the Windows 7 that I'd gotten used to. Now I went and upgraded to Windows 8.1 and I like that too, BELIEVE IT! The tiles are pretty cool. I didn't find myself joing the whine wagon and complain about not being able to adapt.

I had to get Visual Studio for a new job, hence the need for Windows. Then, I wanted to use my favorite new web framework, Meteor, and Meteor doesn't play nice with Windows for making mobile apps, hence the need for Ubuntu. So I'm setting up Ubuntu 14.04.2 LTS as a "guest OS" virtual machine in Virtual Box 4.3.26 running on Windows 8.1, the "host OS".

1. Download [Virtual Box](https://www.virtualbox.org/wiki/Downloads)
1. Download the [Virtual Box Extension Pack](https://www.virtualbox.org/wiki/Downloads)
1. Download the correct [Ubuntu iso](http://www.ubuntu.com/download/desktop) for your system.
1. Start Virtual Box, and add a new VM, set it to Ubuntu (32 or 64, whatever you downloaded).
1. In Settings > Storage, add the iso you downloaded
  * Click the CD with the plus symbol (CD+)
  * Click "Choose Disk" and find the iso on your computer
1. Run the vm
1. Install Ubuntu as you would normally
1. You need to add "Guest Additions" on your Ubuntu guest OS to get bigger resolutions, mouse integration, etc.
  * I was installing Ubuntu 14.04.2 LTS, and I had some trouble with this. First:
  * I opened a terminal (<kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>t</kbd>) and ran:
  * <pre><code>sudo apt-get update
  sudo apt-get install virtualbox-guest-additions-iso</code></pre>
  * Then, in the Virtual Box menu bar, click Devices > Insert guest additions CD image, and install the Guest Additions. That didn't work though!!! So I got rid of that Guest Additions mess by running:
  * <pre><code>sudo apt-get purge VBox*
sudo apt-get purge vbox*</code></pre>
  * And then tried to install the correct version, from the Ubuntu repositories (versions retrieved from here are tested and maintained by the folks at Ubuntu):
  * <pre><code>sudo apt-get install build-essential linux-headers-$(uname -r)
  sudo apt-get install virtualbox-guest-x11</code></pre>
  * But the "virtualbox-guest-x11" package wouldn't install, due to unmet dependencies!!!! So what I did was:
  * <pre><code>sudo apt-get remove libcheese-gtk23
  sudo apt-get install xserver-xorg-core</code></pre>
  * (Credits to the askubuntu forums [here](http://askubuntu.com/questions/588943/experiencing-small-resolution-issue-in-ubuntu-14-04-2-with-virtualbox-getting-s/#answer-604824))
  * I then restarted the Virtual Machine and it worked! (well it gave me a Ubuntu error twice, but then... it worked!)
1. Turn off the VM and go into its settings in the Virtual Box Manager and make sure you have the following settings:
  * Settings > System > Acceleration: make sure that "Enable VT-x/AMD-V" and "Enable Nested Paging" are both checked
    * for "VT-x/AMD-V" you may have to go into your computer's BIOS and enable those virtualization options.
  * Settings > Display > Video: make sure that "Enable 3D Acceleration" is checked
  * Settings > Network, change the attached network from NAT to Bridged (allows webapps on guest be viewed on host's browsers!)
  * While you're in Settings > System, give it an appropriate amount of memory and give it access to 2 processor cores
1. Fire up the Ubuntu VM again, you're ready to rock.

From here you can run Meteor in it's native environment (your guest Linux OS) and view your apps in your faster Windows 8.1 host OS. When you run a Meteor app, it shows up in the guest OS at the URL: localhost:3000. It will show up in your host os at the URL: \<ip of guest\>:3000. You can get the guest's ip by running `ifconfig` from a terminal. Both the guest and the host will share the ip of the router (gateway). Your Ubuntu guest should be dynamically assigned, but if you're having problems, you can manually assign it an IP within the appropriate range.
