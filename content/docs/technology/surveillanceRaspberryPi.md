+++
title = 'Surveillance'
date = 2013-12-16T22:45:00
draft = false
tags = ["RaspberryPi", "camera"]
categories = ["tutorials", "raspberry-pi"]
+++

# Raspberry Pi Surveillance

One of the hobby projects:
- Keep on capturing pictures in a loop, compare current picture with last picture, if the difference in pixel is more than threshold, capture a high resolution image, save it to RAM disk (so that memory card is safe from frequent read write operations), rename the image using date and time; e-mail it with date in subject field.
- Run a cron job to delete temporary files older than 20 minutes.
- Run the script as service and use another to monitor the service, on failure restart the service.

### Python script to detect motion

(To Do: add git link for the code here))

### Use RAM disk

I used RAM disk to store the files before they get e-mailed to me. If you want /tmp to be a ramdisk put this line into /etc/fstab -

```tmpfs           /tmp            tmpfs   defaults,noatime,nosuid   0       0```

### Delete files

And I am deleting jpg files older than 20 minutes from /tmp using a shell script -

```pi@raspberrypi ~ $ cat deleteOldFile.sh 
find /tmp/*jpg -mmin +20 -exec rm {} \;
pi@raspberrypi ~ $```

### CRON job

And run the shell script every 10 minutes -

```pi@raspberrypi ~ $ crontab -e

# m h  dom mon dow   command
*/10 * * * * /home/pi/deleteOldFile.sh

pi@raspberrypi ~ $```
