+++
title = 'Dropbox'
date = 2016-10-27T23:45:00
draft = false
tags = ["RaspberryPi", "dropbox"]
categories = ["tutorials", "raspberry-pi"]
+++

# Dropbox with Raspberry Pi
Act as root, since I wanted to run the script on startup as a service, so I must setup dropbox for 'root' user rather than 'pi' -

```pi@raspberrypi:~ $ sudo -s
root@raspberrypi:/home/pi# cd /root/```

### Get the script and link

```root@raspberrypi:~# git clone http://github.com/andreafabrizi/Dropbox-Uploader.git
Cloning into 'Dropbox-Uploader'...

remote: Counting objects: 785, done.
remote: Total 785 (delta 0), reused 0 (delta 0), pack-reused 785
Receiving objects: 100% (785/785), 237.87 KiB | 0 bytes/s, done.
Resolving deltas: 100% (409/409), done.
Checking connectivity... done.
root@raspberrypi:~# cd Dropbox-Uploader/
root@raspberrypi:~/Dropbox-Uploader# ./dropbox_uploader.sh```

 This is the first time you run this script, please follow the instructions:

 1) Open the following URL in your Browser, and log in using your account: https://www.dropbox.com/developers/apps
 2) Click on "Create App", then select "Dropbox API app"
 3) Now go on with the configuration, choosing the app permissions and access restrictions to your DropBox folder
 4) Enter the "App Name" that you prefer (e.g. MyUploader49491793716643)

 Now, click on the "Create App" button.

 When your new App is successfully created, please click on the Generate button
 under the 'Generated access token' section, then copy and paste the new access token here:

 ```# Access token: .............

 > The access token is ............. Looks ok? [y/N]: y
   The configuration has been saved.```

### Add to Python script

from subprocess import call
```Upload = "/home/pi/Dropbox-Uploader/dropbox_uploader.sh upload " + str(fileWithPath) + " " + str(fileName)
call ([Upload], shell=True)```

If you need to unlink from Dropbpx -

```root@raspberrypi:~/Dropbox-Uploader# ./dropbox_uploader.sh unlink

pi@raspberrypi ~ $ crontab -e

# m h  dom mon dow   command
*/10 * * * * /home/pi/deleteOldFile.sh

pi@raspberrypi ~ $```
