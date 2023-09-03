# ESPArduinoRenogy

This is a fork of Wrybread's project at: https://github.com/wrybread/ESP32ArduinoRenogy
The majority of the code interacting with the Renogy controller is from that project. Some of the web interface code is bodged from the HelloServer ESP8266WebServer example.

My intention with this fork was to use one of my many unused ESP8266 modules (Specifically, the Wemos D1 Mini) to read the status of my Renogy solar setup over WiFi and integrate it into Home Assistant. 

Most of the modifications to the original code were due to the ESP8266 only having one hardware UART, so I ripped out most of the serial debugging commands and am using the UART for talking with the Renogy controller. 

## Wiring / Setup 
In my setup, I'm using an LM2696 DC-DC converter module to take power from the RJ12 connector to have a tidier setup. It also works fine when powered using the MicroUSB connector on the microcontroller.

![Wiring Diagram](https://i.imgur.com/4VDK7ai.png)

![Completed wiring](https://i.imgur.com/33c3uLC.jpg)

## Web Interface
When accessing the microcontroller's IP in a browser, you can see a web interface that shows all the stats and the ability to toggle the load

![Web Interface](https://i.imgur.com/3phXtOU.png)

There's also a JSON output of this information at http://YOURDEVICEIPHERE/rest

An example output of this would be:
```
{"battery_soc":100,"battery_voltage":13.199999809265137,"battery_charging_amps":0.15000000596046448,"battery_charging_watts":1.9800000190734863,"controller_temperature":27,"battery_temperature":0,"controller_temperatureF":80.5999984741211,"battery_temperatureF":32,"load_voltage":0,"load_amps":0,"load_watts":0,"load_status":false,"solar_panel_voltage":13.800000190734863,"solar_panel_amps":0.070000000298023224,"solar_panel_watts":1,"min_battery_voltage_today":0,"max_battery_voltage_today":13.199999809265137,"max_charging_amps_today":6.3000001907348633,"max_discharging_amps_today":3.7999999523162842,"max_charge_watts_today":79,"max_discharge_watts_today":2,"charge_amphours_today":14,"discharge_amphours_today":0,"charge_watthours_today":182,"discharge_watthours_today":13,"controller_uptime_days":0,"total_battery_overcharges":0,"total_battery_fullcharges":0,"last_update_time":3291357,"voltage_rating":0,"amp_rating":20,"discharge_amp_rating":0,"type":0,"controller_name":0,"software_version":"1768","hardware_version":"01280","serial_number":"049153"}
```
I have used that JSON output to monitor the stats with Home Assistant using its REST sensor. I have included my renogywanderer.yaml file, you will have to modify the IP address in the file to point to your device. I would recommend setting a static DHCP lease in your router for your ESP so its IP doesn't change
![Home Assistant](https://i.imgur.com/QjTWIST.png)

There's also the /modbustest endpoint that displays the raw data from the Renogy controller for testing. 
