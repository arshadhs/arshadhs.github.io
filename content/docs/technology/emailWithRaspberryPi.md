+++
title = 'e-mail'
date = 2013-12-16T22:45:00
draft = false
tags = ["RaspberryPi", "e-mail"]
categories = ["tutorials", "raspberry-pi"]
+++

# E-Mail with Raspberry Pi

A quick guide to setting up the E-Mail on Raspberry Pi.

---
## Using SSMTP

```
pi@raspberrypi ~ $ sudo apt-get install ssmtp mailutils mpack
```
Now edit the file /etc/ssmtp/ssmtp.conf as root and add the next lines. Please note that some of the lines already exist and may need to be changed. Others don't exist yet and need to be added to the end of the file.

```
mailhub=smtp.gmail.com:587
hostname=ENTER YOUR RPI'S HOST NAME HERE
AuthUser=YOU@gmail.com
AuthPass=PASSWORD
useSTARTTLS=YES
```

Add each account that you want to be able to send mail from by editing, ‘/etc/ssmtp/revaliases‘:

```
pi:YOU@gmail.com:smtp.gmail.com:587
```

## Test

```
pi@raspberrypi ~ $ echo 'hi' | mail -s "Test" YOU@gmail.com
```

## Logs

```
pi@raspberrypi ~ $ grep -riI ssmtp /var/log
```

---
{{< home-link "Home" >}} | {{< section-index >}}