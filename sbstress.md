'sbstress' - SBrick Stress Test Tool for Linux

This is a python GUI toll to control the SBrick. Soon it will be renamed to something more appropriate.
This is a short video demo of v0.50:

[![SBrick Video Demo](http://img.youtube.com/vi/4ee8OOaZ0eI/0.jpg)](http://www.youtube.com/watch?v=4ee8OOaZ0eI)

Usage:

sbstress.py -a 'adapter>' -d 'device' -p 'period'
  
- 'adapter' is the hci ID of your bluetooth 4.0+ adapter (like hci0)
-  'device'  is the SBrick Bluetooh address (like AA:BB:CC:DD:EE:FF)
-  'period'  is the time in ms that occurs between each command sent to the SBrick (like 100)
  
This  script works with the original firmware (4.0) as also with 4.2 (well, at least 4.2b2). It reads the firmware version from the SBrick and expects just these versions so if you receive a different version from Vengit you need to change the script.

Firmware 4.0 doesn't expose internal temperature and voltage values so this tools shows 0.0ºC and 0V instead.
Firmware 4.2 has a lot of new functions but also some Bluetooth Low Energy handles were changed so gatttool commands are slightly different. There are other new features including a watchdog: by default, SBrick now drops the BLE session after 500 ms so we need to keep talking to it (firmware 4.0 doesn't have this watchdog but the gatttool drops the BLE session after 2.2~2.5 seconds). I still need to address the other features.

The «period» value of 100 may be reduced a bit to achieve slightly better latencies but when testing with my laptop and PERIOD=75 noticed some commands being ignored. It can also be increased but close to 500 ms you will see the SBrick resetting all ports.

This tool also implements:
- a checkbox for each of the 4 ports to toggle the direction of the DRIVE commands
- a quick 'STOP ALL' button
- 'Identify' LED button

Please note that the temperature values are read from a sensor inside the SBrick BLE chipset - other parts of the SBrick, especially the power drivers and the electrical connection WILL BE hotter. I achieved to melt a bit of plastic around the power pins of my Stress Test SBrick with temperature readings ~70ºC (overall current ~7A)

The SBrick can work with low voltages, I've used it with 3.7V LiPo batteries without issues. But with high loads internal voltage will drop, even if you use excellent high current batteries. If internal voltage drops bellow 3.3~3.5V the SBrick turns off.
