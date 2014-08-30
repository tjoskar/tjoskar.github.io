---
layout: post
title: Backup raspberry pi image
date: 2014-08-30 10:00:10
published: True
categories: ['raspberry pi']
tags: []
---

Creating a disk image will preserve not only files but also the filesystem structure and when you decide to flash your new SD card, you will be able to just plug it in and it will work.

Mac
Plug in your SD card and find out the disk name by enter <code>diskutil list</code> and then:
{% highlight bash %}
$ sudo dd if=/dev/rdiskx of=/path/to/new_backup_image.img bs=1m
{% endhighlight %}
Where /dev/rdiskx is your SD card.

To restore the image, just type:
{% highlight bash %}
$ diskutil unmount /dev/disk1sx
$ sudo dd if=/path/to/new_backup_image.img of=/dev/rdiskx bs=1m
{% endhighlight %}

[That's all folks](https://www.youtube.com/watch?v=gBzJGckMYO4)
