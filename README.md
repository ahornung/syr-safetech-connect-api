# SYR SafeTech Connect HTTP/JSON API Documentation
syr-safetech-connect-api

This is an attempt to publicy document the fields and features of the SYR SafeTech Connect JSON API as it lacks documentation from the vendorside. The API is identical for the "Wasserwächter" from Polygonvatro / AXA, as it's mostly a labeled SYR device. Please feel free to file pull requests in case you have any additions or corrections. Most of the field descriptions are the result of trial and error or available information from other projects.

Inspired by https://github.com/thannaske/oekofen-json-documentation and partly based on the following information:
* https://loxwiki.atlassian.net/wiki/spaces/LOX/pages/1605271685/Syr+Safe+Tech
* https://www.loxforum.com/forum/hardware-zubeh%C3%B6r-sensorik/291732-wasserleckerkennung-mit-syr-safe-tech
* https://www.msxfaq.de/sonst/iot/syr_safe-t_connect.htm

## Accessing the API
The API can simply be accessed via HTTP and returns fields as JSON.
Reading out most of the values requires regularly setting the "admin" or expert mode, otherwise most of the interesting fields will be returned as `"ERROR: ADM"`. 
```
http://{$ip}:5333/safe-tec/set/ADM/(2)f
```

After that, you can query all current values with
```
http://{$ip}:5333/safe-tec/get/all
```

## Returned Data

### Fields
| Field | Type   |Description |
|-------|--------|----------|
|getSRN | int | Serial Number
|getVer | string | ID and Version number, e.g. "Safe-Tech V3.53"
|getMAC | | mac address
|getAVO | int w. "mL" suffix | Accumulated Volume of current water tap? resets to 0
|getVOL | int w. "Vol[L]" prefix| Total accumulated volume in Liter - main interesting measurement data
|getFLO | int | Current Flow in own unit - can be manually scaled to L/min
|getLTV | int | Last total tapped volume in Liter - persists until next water tapping
| getBAR| ??? | Water pressure (unconfirmed) - returns "ERR 65535" on a Polygonvatro device identifying as "Safe-Tech V3.53"
| getCEL| ??? | Water temperature (unconfirmed) - returns 0 on a Polygonvatro device identifying as "Safe-Tech V3.53"
| getAB | 1/0 | State of the motor valve - 0 means closed
| getCND| | Water conductivity or hardness - returns 0 on a Polygonvatro device identifying as "Safe-Tech V3.53"
| getALA| string | Alarm information
|getWFC | string | WiFi SSID
|getWIP | string | WiFi IP Address
|getWGW | string | WiFi Gateway IP
|getWFR | | WiFi RSSI
|getFSL | | Connected Floor sensor(s) Serial Numbers
|getBAT | float "," separator! | battery voltage 
|getNET | float "," separator! |  ??? 
| getPRF| int | Active profile for leak detection
|getPN1 | string | Profile 1, name
|getPV1 | | Profile 1, max volume for leak
|getPT1 | | Profile 1, max minutes for leak
|getPF1 | | Profile 1, max flow l/h for leak
|getPM1 |1/0|  micro leak detection on / off
|getPR1 | | ???
|getPB1 | | ???
|getPA1 | | ???
|getPW1 | | ???

... to be completed

## Setting Values
Close the water valve (to be confirmed / tested)

```
http://{$ip}:5333/safe-tec/set/ab/2
```
