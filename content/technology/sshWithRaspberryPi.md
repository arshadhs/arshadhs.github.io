+++
title = "SSH with Raspberry Pi"
date = 2014-11-08T14:08:00
draft = false
tags = ["RaspberryPi", "SSH"]
categories = ["tutorials", "raspberry-pi"]
+++

# SSH login to Raspberry Pi without password

Execute following 3 commands from PC/Laptop from which you want to login to Raspberry Pi without password.

```laptop:~$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/laptop/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/laptop/.ssh/id_rsa.
Your public key has been saved in /home/laptop/.ssh/id_rsa.pub.
The key fingerprint is:
6f:72:4e:e5:f3:81:d4:27:9d:27:06:f0:ce:f1:75:7d laptop
The key's randomart image is:
+--[ RSA 2048]----+
|          .      |
|                .|
|            +   A|
|           o = o+|
|        S   = *.+|
|         o + o +.|
|        . = + .  |
|         *   o . |
|          .   .  |
+-----------------+

NUsing ssh create a directory ~/.ssh as user pi on Raspberry -

laptop:~$ ssh pi@192.168.1.68 mkdir -p .sshpi@192.168.1.68's password:```

Append public key to pi@Raspberry:.ssh/authorized_keys and enter pi's password one last time:

```laptop:~$ cat .ssh/id_rsa.pub | ssh pi@192.168.1.68 'cat >> .ssh/authorized_keys'
pi@192.168.1.68's password:```

Now you can log into Raspberry as pi without password:

```laptop:~$ ssh pi@192.168.1.68```

Depending on your version of SSH you might also have to do the following changes:
- Put the public key in .ssh/authorized_keys2
- Change the permissions of .ssh to 700
- Change the permissions of .ssh/authorized_keys2 to 640
