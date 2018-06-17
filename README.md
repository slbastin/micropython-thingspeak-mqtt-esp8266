## MQTT protocol with Thingspeak using Micropython on ESP8266 and ESP32
Example code showing how to use the MQTT protocol with the Thingspeak cloud service [https://thingspeak.com]

* Publish data
* Subscribe to data
* Micropython
* ESP8266

## Thingspeak configuration
1. Create a Thingspeak account https://thingspeak.com
1. Create a new Thingspeak channel (select New Channel on the My Channels page)
1. Find the following Thingspeak channel information
    * Channel ID (found at the top of every channel page)
    * User ID (found in Account-> My Profile)
    * Read API Key (found in the API Keys TAB)
    * Write API Key (found in the API Keys TAB)
    * MQTT API Key (found in Account-> My Profile)

## Example code configuration
* The code is commented at various places with **"EDIT"** indicating places where Thingspeak configuration
parameters are needed 

## Limitations
* no exception handling in example code
* examples support only one field in the channel

## Tested with Hardware
* Adafruit Feather HUZZAH ESP8266
* Adafruit Feather HUZZAH ESP32 (need to install umqtt package from micropython-lib... see below)
* WeMos D1 Mini

## Tested with Micropython Versions
* Micropython v1.9.3
* Micropython v1.9.4

## Recommended Tools
* Adafruit Ampy
    * install version 1.0.3 which has the -d option (use **-d1** to avoid USB connection issues in Windows)
* Putty

## Making it work
1. Configure example code with Thingspeak parameters
1. Use Ampy to put Publish code to main.py on ESP8266
1. Reset ESP8266
1. Observe that data is being Published to the Thingspeak channel at Thingspeak.com
1. Use Ampy to put Subscribe code to main.py on 2nd ESP8266
1. Reset the 2nd ESP8266
1. Connect to 2nd ESP8266 with Putty
1. Observe in Putty window that Subscribed data is being received by ESP8266

## Installing UMQTT Package
The example code requires the MicroPython MQTT (UMQTT) Package.  Some firmware releases have this package built-in, others don't.

| Firmware Release        | umqtt built-in?           | GitHub Library | 
| ------------- |:-------------:| :-----:|
| MicroPython 1.9.3 for ESP8266 | Yes | n/a |
| MicroPython 1.9.4 for ESP8266 | Yes | n/a | 
| MicroPython 1.9.4 for ESP32   | **No** | [Micropython lib](https://github.com/micropython/micropython-lib) |

##### How to install the UMQTT package (if "No" is indicated in the table above)
1. Navigate to the GitHub library indicated in the table 
1. Select the "Clone or download" button
1. Select "Download ZIP"
1. Extract the ZIP.  Should see folder called "micropython-lib-master"
1. Two files need to be copied to the MicroPython filesystem
    * micropython-lib-master\umqtt.robust\umqtt\simple.py
    * micropython-lib-master\umqtt.robust\umqtt\robust.py
  
Copy these two files to the MicroPython filesystem with the directory structure shown below.  

```
boot.py
lib
  |
  umqtt
     simple.py
     robust.py
```

Example with Ampy:    
```
>ampy -pCOM27 -d1 ls
boot.py
>ampy -pCOM27 -d1 mkdir lib
>ampy -pCOM27 -d1 mkdir lib/umqtt
>ampy -pCOM27 -d1 put simple.py lib/umqtt/simple.py
>ampy -pCOM27 -d1 put robust.py lib/umqtt/robust.py
>ampy -pCOM27 -d1 ls
boot.py
lib
>ampy -pCOM27 -d1 ls lib
umqtt
>ampy -pCOM27 -d1 ls lib/umqtt
simple.py
robust.py
```
##### Validating the UMQTT package install
From the REPL (using Putty, etc) execute the following commands and observe similar output
```
>>> from umqtt.robust import MQTTClient

>>> dir(MQTTClient)
['reconnect', 'log', 'publish', '__module__', 'wait_msg', 'delay', '__qualname__', 'DELAY', 'DEBUG']
```

If you see this result you have successfully installed the umqtt package. :tada: :relieved:






