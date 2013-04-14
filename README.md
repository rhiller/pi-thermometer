Raspberry Pi code to read DS18B20 Digital Temperature Sensors, and post to cosm

All the interesting stuff is in the readTemp python script.  The other files
are just little scripts used to start things up or keep them running.

On the Raspberry Pi I put all the files (except for init.d/thermometer) into
/home/pi/thermometer.  If you decide to move things around, you will need to fix
up the paths in the script files

The init script is based on: http://www.debian-administration.org/articles/28

init.d/thermometer:  init script to be placed into the /etc/init.d/ directory
make sure that it is executable and owned by root:
  -rwxr-xr-x 1 root root 470 Feb  1 12:09 thermometer

Then run (as root):
  update-rc.d thermometer defaults
to create the needed symlinks so that this will all start on boot

if you decide to remove the script from the startup sequence run:
  update-rc.d -f thermometer remove


The temperature sensors are wired in parallel with a single 4.7K pullup resistor
between data and 3v3.  See http://www.cl.cam.ac.uk/freshers/raspberrypi/tutorials/temperature/ Step Two as one example.  The data pin of the sensor connects to GPIO4 on the Raspberry Pi.

Not shown in that diagram is that multiple DS18B20 can be connected in parallel.
I'm currently using two to measure the inlet and outlet temperatures on a heat pump.

Another resource is: http://blog.turningdigital.com/2012/09/raspberry-pi-ds18b20-temperature-sensor-rrdtool/
