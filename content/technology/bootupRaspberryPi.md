+++
title = 'Bootup Script'
date = 2013-12-16T22:45:00
draft = false
tags = ["RaspberryPi", "bootup"]
categories = ["tutorials", "raspberry-pi"]
+++

# Bootup script Setup

Edit Python script, add as first line -
```#!/usr/bin/python```

Change permissions -
```chmod 755 raspiLapseCam.py```

That means you can execute it by typing `raspiLapseCam.py` rather than `python raspiLapseCam.py`

Then we can use an /etc/init.d script to get it started when the system boots.

Start by copying /etc/init.d/skeleton to /etc/init.d/timelapse -
```cd /etc/init.d && sudo cp skeleton timelapse```

edit /etc/init.d/timelapse, change lines 23 & 24 to refer to the command '/home/pi/raspiLapseCam.py' -
```DESC="Raspberry Pi TimeLapse"
DAEMON=/home/pi/raspiLapseCam.py```

Make it executable -
```sudo chmod 755 /etc/init.d/timelapse```

gets it started at system boot and killed at shutdown -
```sudo update-rc.d timelapse enable```
