---
layout: post
title:  "Headless RPi - prevent SD corruption"
date:   2018-10-09 00:00:00
categories: hardware 
---

Raspberry Pis are perfect when building interactive art projects (e.g. [20 printers printing ML generated tweets](http://www.miketyka.com/?p=usandthem)
and often it's nice to leave the Pi headless, i.e. without keyboard or monitor. And it's convenient to just power up the device to start it and pull the power when its time to shut it down. However there are two minor issues - how do you get your code to start without a login and how do you precent system corruption. Thanksfully there are solutions to that. 

## Startup: Starting your code on startup.

There a [many solutions](https://www.dexterindustries.com/howto/run-a-program-on-your-raspberry-pi-at-startup/) for this 
but personally I like to run my code as a service. To do so add a file like the one below to /etc/rc.d/ and make it executable.
This assumes your startup script lives in /home/you/mycode/run.sh and exepects to be run from that directory.

```
#! /bin/sh

### BEGIN INIT INFO
# Provides:             tweet_printer
# Required-Start:       $remote_fs $syslog
# Required-Stop:        $remote_fs $syslog
# Default-Start:        2 3 4 5
# Default-Stop:         0 1 6
# Short-Description:    Tweet stream deamon
### END INIT INFO

. /lib/lsb/init-functions

start() {
  log_action_begin_msg "Starting tweet printer daemon"
	cd /home/you/mycode/
	sh run.sh # OR python3 run.oy OR ./myexe 
  log_action_end_msg
}

stop() {
  log_action_begin_msg "Stopping tweet printer daemon"
  # <Insert command to kill your process here>
  #
  log_action_end_msg
}

case "$1" in
    start)
      start
  ;;
    stop)
      stop
  ;;
    restart)
      stop
      start
  ;;
    *)
      echo "Usage: <MYSERVICENAME> {start|stop|restart}"
      exit 1
  ;;
esac
exit 0
```
Note your code will run as root.  Note this can also be a python program directly but i prefer the shell script to keep things more seperated.


## Shutdown: Preventing SD corruption

Linux systems do not like to to be powered off suddenly and this can lead to corruption of the filesystem on the SD card. 
Last time we faced this issue we researched a bunch of UPS-like soutions that would detect power failure and provide both bridge-over 
power and a signal to the Pi to cleanly shut down using extra electronics and super caps or batteries. 
However, there's a much much simpler solution: just mount
the filesystem read only. This is not 100% straightforward and can have some drawbacks but for what we were doing it was perfect.
Doing so makes the entire system completely stateless and thus unable to corrupt itself. THe drawback is you cannot persist any state from one boot to the next but there are aalso workarounds which we'll discuss at the end. 
I'm basically following the instructions from <http://ideaheap.com/2013/07/stopping-sd-card-corruption-on-a-raspberry-pi/> here, except that
I go one more step and fully mount the system read-only. The lock/unlock scripts work around the usability issue.

0) Disable swapping

This will disable swapping: 

```
sudo dphys-swapfile swapoff
sudo dphys-swapfile uninstall
sudo update-rc.d dphys-swapfile remove

```

Check that ```free -m``` shows the swap to be 0 

1) Set up /etc/fstab to mount /var/log and /var/tmp using tmpfs.
The system needs scratch space to write into, so just mounting the root filesystem read-only doesnt work very well.
However we can setup in-RAM filesystem for this purpose.
Add these two lines to ```/etc/fstab```

```
none                  /var/log        tmpfs   size=1M,noatime   0       0
none                  /var/tmp        tmpfs   size=1M,noatime   0       0
```

Then also add ```ro``` to the line that mounts your root and boot file system. In the end the file should now look someting like this:

```
proc                  /proc           proc    defaults          0       0
PARTUUID=d6a29f93-01  /boot           vfat    ro,noatime        0       2
PARTUUID=d6a29f93-02  /               ext4    ro,noatime        0       1
none                  /var/log        tmpfs   size=1M,noatime   0       0
none                  /var/tmp        tmpfs   size=1M,noatime   0       0
```

Now the system will be stateless. However sometime its necesseray to unlock it to make mods:

2) Set up two script that will allow us to temporarily lock and unlock the filesystem if we need to edit something.

~/lock.sh
```
#!/bin/sh
mount -o remount,ro $(mount | grep " on / " | awk '{print $1}')
```

~/unlock.sh
```
#!/bin/sh
mount -o remount,rw $(mount | grep " on / " | awk '{print $1}')
```

If you're updating kernels etc oyu may also have to remount /boot but that's not as frequently necessary so I dont set up a script for that.

4) Hit ```sudo reboot```  check your service starts up

## Drawbacks of this method
In my experience this works really well in practice, but there could be drawbacks:

  * You have no persistent system log - if something went wrong it can make debugging hard.
  * You can't persist state from one boot to the next. One work around is to have a second SD card using a 
    USB stick which is mounted periodically just when needed and then unmounted. Or you can briefly remount ```/``` read-write, write your state and then remount again.
    Not perfect (if the power goes of right that second) but it makes it way less likely. Or, if you hae network, you could send the state to a farawy server.
  * The tmp dirs that are in memory fill up you have an issue.  
  * Not having a swap partition, if you run out of memory, you'll likely crash.


