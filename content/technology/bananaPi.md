+++
title = 'Banana Pi HomeCloud Setup'
date = 2020-03-30T12:29:34+01:00
draft = false
tags: [BananaPi]
categories: [tutorials, banana-pi]
+++

### Refresh MiniDLNA
MiniDLNA couldn't find any new files in my media_dir even though I was adding new files. I think the problem was that my media_dir was set to an external hard drive. 

(media_dir=v,/media/ExtHD/x/x). I got it working, here are the steps I used to fix.

> sudo service minidlna stop
> sudo rm -rf /var/cache/minidlna/*
> sudo nano /etc/minidlna.conf

- Set the media_dir back to default location - media_dir=V,/var/lib/minidlna/video
- Create a link in /var/lib/minidlna to the directory you want monitored.
> sudo ln -s /media/ExtHD/video /var/lib/minidlna/videos
> sudo service minidlna force-reload

Your files.db will be recreated, this can take a while though.
