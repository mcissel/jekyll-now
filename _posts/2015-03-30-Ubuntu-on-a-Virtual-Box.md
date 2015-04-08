---
layout: post
title: Ubuntu on a Virtual Box
---
I was using Ubuntu for a while and I liked it, but it didn'd do things the Windows 7 did. I had gotten too accustomed to all the shorcuts in Windows 7, and to the tools like Ditto and Instant Eyedropper that just aren't quite the same in Linux. Now I've procured a new laptop and find myself upgraded to Windows 8.1 The tiles are pretty cool. I didn't find myself joing the whine wagon and complain about Windows 8.

I had to get Visual Studio for a new job, hence the need for Windows. But I also want to use my favorite new web framework, Meteor.js, and Meteor doesn't play nice with Windows for making mobile apps, hence the need for Ubuntu. So I'm setting up Ubuntu 14.04.2 LTS as a "guest OS" virtual machine in Virtual Box 4.3.26 running on Windows 8.1, the "host OS". I ran into some problems installing the Guest Additions, so I've documented the correct way to do it below. Make sure to make a snapshot of your virtual machine before attempting that step, in case it doesn't work for you.

1. Download [Virtual Box](https://www.virtualbox.org/wiki/Downloads)
1. Download the [Virtual Box Extension Pack](https://www.virtualbox.org/wiki/Downloads)
1. Download the correct [Ubuntu iso](http://www.ubuntu.com/download/desktop) for your system.
1. Start Virtual Box, and add a new VM, set it to Ubuntu (32 or 64, whatever you downloaded).
1. In Settings > Storage, add the iso you downloaded
  * Click the CD with the plus symbol (CD+)
  * Click "Choose Disk" and find the iso on your computer
1. Run the vm
1. Install Ubuntu as you would normally
1. Make a snapshot of your virtual machine at this point. It'll save time if the Guest Additions don't get installed correctly, or if you simply want to revert to a fresh install of Ubuntu.
1. You need to add "Guest Additions" on your Ubuntu guest OS to get bigger resolutions, mouse integration, etc.
  * I was installing Ubuntu 14.04.2 LTS on Virtual Box 4.3.26 r98988, and I had to follow these steps exactly:
  * I opened a terminal (<kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>t</kbd>) and ran:
<pre><code>sudo apt-get install build-essential
sudo apt-get install linux-headers-generic
sudo apt-get install dkms</code></pre>
  * Do not restart the guest os or the virtual machine.
  * In the Virtual Box menu bar, click Devices > Insert guest additions CD image, and install the Guest Additions.
  * Then shut down the guest OS, from within the Guest OS. (you should have the Guest Additions successfully installed at this point)
1. With the VM off, go into its settings in the Virtual Box Manager and make sure you have the following settings:
  * Settings > System > Acceleration: make sure that "Enable VT-x/AMD-V" and "Enable Nested Paging" are both checked
    * for "VT-x/AMD-V" you may have to go into your computer's BIOS and enable those virtualization options.
  * Settings > Display > Video: make sure that "Enable 3D Acceleration" is checked and slide the video memory to a good amount
  * Settings > Network, change the attached network from NAT to Bridged (allows webapps on guest be viewed on host's browsers!)
  * While you're in Settings > System, give it an appropriate amount of memory and give it access to 2 processor cores
1. Fire up the Ubuntu VM again, you're ready to rock.

From here you can run Meteor in it's native environment (your guest Linux OS) and view your apps in your faster Windows 8.1 host OS. When you run a Meteor app, it shows up in the guest OS at the URL: localhost:3000. It will show up in your host os at the URL: \<ip of guest\>:3000. You can get the guest's ip by running `ifconfig` from a terminal. Both the guest and the host will share the ip of the router (gateway). Your Ubuntu guest should be dynamically assigned, but if you're having problems, you can manually assign it an IP within the appropriate range.
