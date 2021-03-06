Raspberry Pi code to read DS18B20 Digital Temperature Sensors, and post to cosm

All the interesting stuff is in the readTemp python script.  The other files
are just little scripts used to start things up or keep them running.

Make sure that the one-wire drivers are loaded.  The following two lines need to be in /etc/modules:
<pre>
w1_gpio
w1_therm
</pre>


On the Raspberry Pi, I copied this entire tree into /home/pi/pi-thermometer.  

Then run (as root):  
cp init.d/thermometer /etc/init.d/  
cp cron.d/checkup /etc/cron.d/  

If you decide to move things around, you will need to fix up the paths in the script files.

The init script is based on: http://www.debian-administration.org/articles/28

init.d/thermometer:  init script to be placed into the /etc/init.d/ directory
make sure that it is executable and owned by root:  
  -rwxr-xr-x 1 root root 470 Feb  1 12:09 thermometer  

Then run (as root):  
  update-rc.d thermometer defaults  
to create the needed symlinks so that this will all start on boot  

if you decide to remove the script from the startup sequence run:  
  update-rc.d -f thermometer remove  

The checkup script pings the default gateway every 10 minutes and reboots the Pi if it doesn't get a response.  This is to recover from issues with the USB WiFi adapter.  To stop the check, remove it from /etc/cron.d


The temperature sensors are wired in parallel with a single 4.7K pullup resistor
between data and 3v3.  See http://www.cl.cam.ac.uk/freshers/raspberrypi/tutorials/temperature/ Step Two as one example.  The data pin of the sensor connects to GPIO4 on the Raspberry Pi.  That corresponds to pin 8 of the P1 connector.

The correspondence between the P1
connector and the Broadcom GPIO values can be found in a pinout diagram such
as http://www.raspberrypi-spy.co.uk/2012/06/simple-guide-to-the-rpi-gpio-header-and-pins/

Not shown in that diagram is that multiple DS18B20 can be connected in parallel.
I'm currently using two to measure the inlet and outlet temperatures on a heat pump.

Another resource is: http://blog.turningdigital.com/2012/09/raspberry-pi-ds18b20-temperature-sensor-rrdtool/


<pre>
                         DS18B20                      Phone cord     phone jack
                       +---------+                    ----------     ----------
                       |         |   Pin 1 - Gnd      Black           5
                       |         |   Pin 2 - DQ       Yellow          2
                       | 1  2  3 |   Pin 3 - Vdd      Red             4
                       +---------+          (unused)  Green
                         |  |  |
                         |  |  |
                        /   |   \
                       /    |    \
                   Gnd|   DQ|  Vdd|      3v3
                      |     |     |       |
                     Gnd    |     +-------+
                            |             |
                            |              \
                            |              /
                            |              \ 4.7k ohm
                            |              /
                            |              |
							+--------------+-------------------> P1.8 (GPIO4)

</pre>
