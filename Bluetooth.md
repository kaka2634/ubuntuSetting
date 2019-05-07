Solve bluetooth cannot find device


sudo service bluetooth status

 bluetooth.service - Bluetooth service
   Loaded: loaded (/lib/systemd/system/bluetooth.service; enabled; vendor preset: enabled)
   Active: active (running) since Fri 2019-04-19 22:09:10 CST; 1min 28s ago
     Docs: man:bluetoothd(8)
 Main PID: 26803 (bluetoothd)
   Status: "Running"
   CGroup: /system.slice/bluetooth.service
           └─26803 /usr/lib/bluetooth/bluetoothd

Apr 19 22:09:37 liu-K401UQ bluetoothd[26803]: Not enough free handles to register service
Apr 19 22:09:37 liu-K401UQ bluetoothd[26803]: Not enough free handles to register service
Apr 19 22:09:37 liu-K401UQ bluetoothd[26803]: Current Time Service could not be registered
Apr 19 22:09:37 liu-K401UQ bluetoothd[26803]: gatt-time-server: Input/output error (5)
Apr 19 22:09:37 liu-K401UQ bluetoothd[26803]: Not enough free handles to register service
Apr 19 22:09:37 liu-K401UQ bluetoothd[26803]: Not enough free handles to register service
Apr 19 22:09:37 liu-K401UQ bluetoothd[26803]: Sap driver initialization failed.
Apr 19 22:09:37 liu-K401UQ bluetoothd[26803]: sap-server: Operation not permitted (1)
Apr 19 22:09:37 liu-K401UQ bluetoothd[26803]: Endpoint registered: sender=:1.66 path=/MediaEndpoint/A2DPSource
Apr 19 22:09:37 liu-K401UQ bluetoothd[26803]: Endpoint registered: sender=:1.66 path=/MediaEndpoint/A2DPSink

### solution 1 (not work)
rfkill list
rfkill unlock bluetooth


### Solution 2 (not work)

https://forums.linuxmint.com/viewtopic.php?t=240291


### information
https://askubuntu.com/questions/632336/bluetooth-broadcom-43142-isnt-working

### solution 3 (not work)
Because I found my device is RFCOMM ver 1.11 not BCM

https://ubuntuforums.org/showthread.php?t=2365022

liu@liu-K401UQ:~$ bluetoothctl
[NEW] Controller 74:C6:3B:36:55:5A liu-K401UQ [default]
[bluetooth]# power on
Changing power on succeeded
[bluetooth]# scan on
Discovery started
[CHG] Controller 74:C6:3B:36:55:5A Discovering: yes
[bluetooth]# 


### solution 4

https://askubuntu.com/questions/632336/bluetooth-broadcom-43142-isnt-working/632348#632348

