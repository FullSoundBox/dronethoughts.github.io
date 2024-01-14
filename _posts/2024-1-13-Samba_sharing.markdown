---
layout: post
title:  "Setting up samba sharing with an old computer."
date:   2024-01-13 16:47:26 -0600
categories: jekyll update
---
Recently I've been making some cleaning up on my electronics drawer, and I found an old tiny PC which is barely powerful to be used as a desktop computer anymore.

So to try and give it some use, I tried installing an old version of Lubuntu (Bionic Beaver 18.04). This machine is an Aspire 1300 with a slow Intel Atom 230 @ 1.6 GHz. It barely has 2GB of RAM and I had to install an usb WiFi card.

At first the Wifi worked, but as it's common with linux distros, I had to do some research to install the proper drivers, as the download speed wasn't any good. After trying to use the vendor's provided driver without any luck, I had to use this repo from Github https://github.com/nick9859/tl-wn823N-drivers-for-ubuntu.

Then, after I had a good internet connection, I installed samba package using


{% highlight %}
sudo apt update
sudo apt install samba
{% endhighlight %}

Once installed I created a folder for sharing the files with my local network, which are primarily movies and songs.

{% highlight %}
mkdir /home/abraham/Share
{% endhighlight %}

and then I edited the configuration file so this folder is shared

{% highlight %}
sudo nano /etc/samba/smb.conf
{% endhighlight %}

{% highlight %}
[Share]
comment = File Sharing on aspire
path = /home/abraham/Share
read only = No
browsable = yes
writable = yes
{% endhighlight %}

Saved the file, and restarted the samba service for changes to take effect:

{% highlight %}
sudo service smbd restart
{% endhighlight %}

And updated the firewall rules to allow Samba traffic:

{% highlight %}
sudo ufw allow samba
{% endhighlight %}

After some time I found I had to create a password for samba so I can access the shared folder

{% highlight %}
sudo smbpasswd -a abraham
{% endhighlight %}

Then, from my laptop with PopOS! I was able to see the shared folder and look up the files, and even cut them or change their name.

Samba sharing with Windows required a few more steps but it was explained with this tutorial

https://youtu.be/-rEpQanhUoI?si=jDLumF39qTTDzEHV

So in the end I was able to store some files that take up some space aside from my laptop, and then open them from any device connected to my network.

Some notes on this setup are, that we may need to fix the IP of the mini PC, so it doesn't change every restart. I haven't had issues with this, though. The IP is always the same. Also, the power consumption has improved a lot on single board PCs, like a Raspberry Pi I have lying around, so it would be more power efficient to use it. The only downside is that I don't have a SATA-USB hard disk drive reader that is reliable enough for the Raspbery Pi. So, even when this is not power efficient, it works for now.