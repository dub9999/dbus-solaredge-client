# dbus-solaredge-client

## Tested hardware setup
- Multiplus-II GX 48/3000/35-32
- 3 x US2000C Pylontech battery connected via CAN-bus
- 1 x HD Wave SE3000H Solaredge inverter with Solaredge modbus meter.

## Purpose
Connect a solaredge inverter and the associated grid meter to a venus device to make both devices registered on the dbus and available.

## Details
This code uses an old version of dbus-modbus-client repository. This code would require some update to align with actual version of dbus-modbus-client

Solaredge provides data from the inverter and grid meter via modbus-tcp using the sunspec standard.
Inverter and meter are available on the same modbus unit but at different register addresses.  
dbus-modbus-client does not allow to address multiple devices in one unit.

The file sunspec.py was developed to allow creation of a sunspec hub modbus client which can be connected to the venus via modbus-tcp and provides connections of multiple devices at the same address. Both devices are then treated as if they would independently connect to the venus with different IP address although they have the same address.

After initialization is completed, the 2 devices are fully recognized by the venus system and are available in VRM as well.

Some methods available in the original dbus-modbus-client.py file have been commented as they do not serve here (for example the scan method).

For some reason, it randomly happens that solaredge modbus client messages are incomplete or the client even does not respond to calls. In order to handle this situation, if the connection to sunpec devices returns errors more than the authorized MAX_ERRORS number, all devices get fully re-initialized automatically. As these errors only happen during night, this has no impact on the management of the venus device. 

NOTA: The required files from dbus-modbus-client have been copied from the original repository available at time this code was developed so this code does not need to be updated each time something changes on the victron side.

A full update would be required to take the last revisions made by Victron into consideration

## Installation

This repository must be installed on /data to survive to firmware updates.

- Create a repository '/data/projects/dbus-solaredge-client' in the venus device and copy all files of this repository.
- Check and if needed adjust the file 'sunspec.py' to include all the Solaredge device properties and the register addresses

To lauch manually from console, type the command './run.sh' while in the /data/dbus-solaredge-client folder.

Nota: do not forget to make 'run.sh' file executable after transferring the module in the Multiplus.

To lauch automatically at system start up, insert a 'rc.local' file in '/data' with the following instructions (or add the instruction to the 'rc.local' file if it exists): python3 /data/projects/dbus-solaredge-client/solaredgeclient.py

Before lauching the code, login to the victron console, go to menu Settings, then Modbus TCP devices, saved devices, add and insert the IP address, the communication port (502 or 1502) and the unit (normally 1)) of the solaredge inverter. When code is launched, the inverter and the meter will be automatically recognized and will show up is the device list.

To stop the program nicely, create an empty file named 'kill' in the '/data/projects/dbus-solcast-forecast' folder. The file named 'kill' will be deleted automatically.

## Sources used to develop this code and thanks

This project has been possible thanks to the information and codes provided by Victron on their web site and their GitHub space.

A great thanks to Victron for sharing all these stuff.

The following repositories have been a very valuable source of information:

https://github.com/victronenergy/dbus-modbus-client

https://github.com/victronenergy/velib_python


