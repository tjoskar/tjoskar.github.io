---
layout: post
title: Setup Tellstick on Raspberry Pi
published: True
categories: []
date: 2014-09-02 11:46:10
tags: []
---

Install Raspbian by downloading the image from [http://www.raspberrypi.org/downloads/](http://www.raspberrypi.org/downloads/) and run:
{% highlight bash %}
# Replace diskn with your disk, you can find it by: diskutil list
$ diskutil unmountDisk /dev/diskn
$ sudo dd bs=1m if=path_of_your_image.img of=/dev/diskn
{% endhighlight %}

SSH and update
{% highlight bash %}
$ ssh pi@<pi-ip>
$ sudo apt-get update
$ sudo apt-get upgrade
{% endhighlight %}

Install some basic dependencies
{% highlight bash %}
$ sudo apt-get install build-essential cmake libconfuse-dev libftdi-dev help2man
{% endhighlight %}

Install telldus-core
{% highlight bash %}
$ mkdir -p telldus-tmp && cd telldus-tmp
$ sudo nano /etc/apt/sources.list.d/telldus.list
$ deb-src http://download.telldus.com/debian/ stable main
$ wget http://download.telldus.se/debian/telldus-public.key
$ sudo apt-key add telldus-public.key
$ sudo apt-get update
$ sudo apt-get build-dep telldus-core
$ sudo apt-get --compile source telldus-core
$ sudo dpkg --install *.deb
$ cd .. && rm telldus-tmp
{% endhighlight %}

Configure tellstick
{% highlight bash %}
$ sudo nano /etc/tellstick.conf
{% endhighlight %}
You can find some information here: [http://developer.telldus.com/wiki/TellStick_conf](http://developer.telldus.com/wiki/TellStick_conf)

My config looks like this:
{% highlight bash %}
user = "nobody"
group = "plugdev"
deviceNode = "/dev/tellstick"
ignoreControllerConfirmation = "false"

device {
  id = 1
  name = "Kitchen Window"
  protocol = "everflourish"
  model = "everflourish"
  parameters {
    house = "1"
    unit = "1"
  }
}

device {
  id = 2
  name = "Bedroom window"
  protocol = "everflourish"
  model = "everflourish"
  parameters {
    house = "1"
    unit = "2"
  }
}

device {
  id = 3
  name = "Living room window"
  protocol = "everflourish"
  model = "everflourish"
  parameters {
    house = "1"
    unit = "3"
  }
}

device {
  id = 4
  name = "Living room lamp"
  protocol = "arctech"
  model = "selflearning-switch"
  parameters {
    house = "2622"
    unit = "1"
  }
}

device {
  id = 5
  name = "Hallway table"
  protocol = "arctech"
  model = "selflearning-switch"
  parameters {
    house = "1406"
    unit = "1"
  }
}

device {
  id = 6
  name = "Hallway Door"
  protocol = "arctech"
  model = "selflearning-switch"
  parameters {
    house = "1282"
    unit = "1"
  }
}

controller {
  id = 1
  type = 1
  serial = "A900I90U"
}
{% endhighlight %}

Restart tellstick
{% highlight bash %}
$ sudo /etc/init.d/telldusd restart
{% endhighlight %}

Commands:
{% highlight bash %}
$ tdtool -e 1 # Learn unit 1
$ tdtool --on 1 # Turn on unit 1
$ tdtool --off 1 # Turn off unit 1
$ tdtool -l # list all units
{% endhighlight %}

[That's all folks](https://www.youtube.com/watch?v=gBzJGckMYO4)
