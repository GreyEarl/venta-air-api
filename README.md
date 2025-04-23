| Warning! Venta has decided to move to Tuya for their newer devices. Devices with Tuya are not supported!

# Venta Air Original Connect - local REST API

This is a short description for anyone who owns or wants to buy an Venta Air Original Connect humifidier and seeks for integration within the local network.
The manufactures does not communicate on the use of API's. Officially Venta Air is only offering an app which you can use to control their humidifier labeled as Original Connect. Also contacting Venta Air on API support came out empty. They simply denied the existence of such thing.

In the end, I figured out the app is communicating by simply using an local API, which you can use within your home domotica controller, like Home Assistant.
By sniffing network traffic between the app and my humidifer (type AH510) I have reverse enginereed the API. 

Please be aware, this might not be complete or 100% correct as there is no official description.  No authentication/bearer token is required.

## Tested with the Original Connect AH510

### Connecting to your network
Connect the device by initially using the Venta Air app. Alternatively connect to the initial default AP. The device is hosting a webserver, so you can go to the assigned IP address in your browser for connecting to your network, see basic information/state and the option to update the firmware.

![image](https://user-images.githubusercontent.com/33351068/235794581-53f143c6-54ec-4ea9-8ea0-84b9d620fca2.png)

### Get state

`POST <device-ip>/api/telemetry`

You will get something like this as response:

`{"Header":{"DeviceType":500,"MacAdress":"<MAC>","ProtocolV":"3.0"},"Action":{"Power":1,"FanSpeed":2,"TargetHum":50,"SleepMode":0,"Automatic":0,"BaLi":10},"Info":{"OperationT":46571,"ServiceT":367,"ServiceMax":2016,"Warnings":1,"SWMain":"1.0.0.6","HwIndexMB":1},"Measure":{"Temperature":25.66224,"Humidity":43.41344}}`

The meaning of most items in the above response makes sense by default. Some more details on a few.

- TargetHum: Which value in humidity percentage should the device work to in automatic mode.
- Automatic/SleepMode: binary values, off(0) or on(1)
- OperationT: Operation Time
- Warnings: Binary value. When 1 there is an error. In most cases it indicates low water level.
- ServiceMax: When ServiceT hits this value, it is time to clean the device.
- ServiceT: When the value hits ServiceMax (by defauly 2016), the service indicator will light up on the device, meaning you have to clean the device.

### Set state

`POST <device-ip>/api/telemetry?request=set`

Some examples below used as payload.

Set fanspeed to level 1:

`{
    "Power": 1,
    "Automatic": 0,
    "SleepMode": 0,
    "FanSpeed": 1,
    "Action": "control"
}`

Set auto mode on:

`{
    "Power": 1,
    "Automatic": 1,
    "Action": "control"
}`

Turn off device:

`{
    "Power": 0,
    "Automatic": 0,
    "SleepMode": 0,
    "FanSpeed": 1,
    "Action": "control"
}`












