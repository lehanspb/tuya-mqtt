## Example items for OpenHAB 3.x Bindings

### Tuya Smart Thermostat Radiator Valve behind Tuya Gateway

### Things channels (configured via web):

#### Thermostat mode:
Channel identifier:
```
mode
```

State:
```
tuya/zgw1/1a24fkfffe6b4e24/dsp/4/state
```
Example output: {"4":"auto"}
Possible values: auto/temp_auto/holiday/manual/comfort/eco/BOOST


Command:
```
tuya/zgw1/dps/command
```

Outgoing Value Format:
```
{"dps": 4, "set": "%s", "cid": "1a24fkfffe6b4e24"}
```

#### Temperature Setpoint
Channel identifier:
```
setpoint
```

State:
```
tuya/zgw1/1a24fkfffe6b4e24/dsp/2/state
```
Example output: {"2": 220}

Command:
```
tuya/zgw1/dps/command
```

Incoming Value Transformations:
```
JS:tuya-in.js
```

Outgoing Value Transformation:
```
JS:tuya-out.js
```

Outgoing Value Format:
```
{"dps": 2, "set": "%s", "cid": "1a24fkfffe6b4e24"}
```

#### Current Temperature
Channel identifier:
```
temperature
```

State:
```
tuya/zgw1/1a24fkfffe6b4e24/dsp/3/state
```

Incoming Value Transformations:
```
JS:tuya-in.js
```

#### Valve percent
Channel identifier:
```
valve_percent
```

State:
```
tuya/zgw1/1a24fkfffe6b4e24/dsp/109/state
```

Command:
```
tuya/zgw1/dps/command
```

Outgoing Value Format:
```
{"dps": 109, "set": %s, "cid": "1a24fkfffe6b4e24"}
```

### Transformations
tuya-in.js:
```
(function(i) {
      return (i / 10)
})(input)
```

tuya-out.js:
```
(function(i) {
      return (i * 10)
})(input)

```

### items/thermostat.items

```
String  Radiator_Mode           "Mode"                              <radiator>      { channel="mqtt:topic:home:zgw1dev1:mode" }
Number  Radiator_Setpoint       "Temperature setpoint [%.1f °C]"    <radiator>      { channel="mqtt:topic:home:zgw1dev1:setpoint" }
Number  Radiator_Temperature    "Current temperature [%.1f °C]"     <temperature>   { channel="mqtt:topic:home:zgw1dev1:temperature" }
Number  Radiator_Valve_Percent  "Valve percent [%d %%]"                             { channel="mqtt:topic:home:zgw1dev1:valve_percent" }
```

### sitemaps/home.sitemap

```
Frame label="Heating" {
    Setpoint	item=Radiator_Setpoint minValue=15 maxValue=30 step=0.5
    Selection	item=Radiator_Mode mappings=[auto='Auto', temp_auto='Auto temp', manual='Manual', comfort='Comfort']
    Text	item=Radiator_Setpoint
    Text	item=Radiator_Temperature
    Text	item=Radiator_Valve_Percent
}
```




### Simple on/off switch with power measurement capability

### Things channels (configured via web):

#### Power switch
Channel identifier:
```
power
```

State:
```
tuya/tuya_device_1/state
```
Example output: {"4":"auto"}
Possible values: auto/temp_auto/holiday/manual/comfort/eco/BOOST

Command:
```
tuya/tuya_device_1/command
```

Custom On/Open Value:
```
ON
```

Custom Off/Closed Value:
```
OFF
```

#### Power consumption watts
Channel identifier:
```
w
```

State:
```
tuya/tuya_device_1/dps/19/state
```

Incoming Value Transformations:
JS:tuya-energy.js

#### Power consumption volts
Channel identifier:
```
v
```

State:
```
tuya/tuya_device_1/dps/20/state
```

Incoming Value Transformations:
JS:tuya-energy.js

### transform/tuya-energy.js
```
(function(i) {
      return Math.ceil(i / 10)
})(input)
```

### items/socket.items

Switch  Socket_Power     "Socket"            { channel="mqtt:topic:socket:power" }
Number  Socket_W         "Power (W)"         { channel="mqtt:topic:socket:w" }
Number  Socket_Vt        "Power (V) [%s]"    { channel="mqtt:topic:socket:v" }

### sitemaps/home.sitempa

Switch	item=Socket_Power
Text	item=Socket_W
Text	item=Socket_Vt