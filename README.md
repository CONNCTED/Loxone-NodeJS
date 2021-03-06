# Loxone-NodeJS
Loxone nodejs project to read &amp; control inputs and outputs.
Based on http://www.loxone.com/enen/service/documentation/api/webservices.html

## Installation
```bash
npm install loxone-nodejs --save
```

## Usage
### Create a new file 'my-loxone.js'
```javascript
var LoxoneAPI = require('loxone-nodejs');

var loxone = new LoxoneAPI({
    ip: "192.168.1.200",
    debug: true,
    username: "admin",
    password: "password"
});

module.exports = loxone;
```

This example connects to a Harmony hub available on the IP `192.168.1.200`. 
Also provide a username and password.

### In another nodejs file
```javascript
var loxone = require('./my-loxone');

loxone.get("AI_SEN2-T", function(output){
    if (output.LL.Code == 200) {
        console.log('Device does exists!');
        console.log('Value = ' + output.LL.value);
    } else {
        console.error('Device does NOT exists!');
    }
});
```

The following functions are available: `get(device, callback)`, `getValue(device, callback)` and `set(device, action, callback)`. 

## Extending
### Extending 'my-loxone.js'
Add the following code to the my-loxone.js file to expose named functions to read an output.
```javascript
...
loxone.getOutsideTemperature = function(callback) {
    this.getValue("AI_SEN2-T", callback);
};
...
```

`AI_SEN2-T` is the name of an output.

### In another nodejs file
```javascript
var loxone = require('./my-loxone');

loxone.getOutsideTemperature(function(value) {
    console.log("The outside temperature is: " + value + "°C");
});
```

## Examples
### Simple Example
This example contains a very example that connects to the Loxone to get the value of a certain output.
Configure the my-loxone.js file to point to your Loxone.

```bash
cd example-simple
npm install
npm start
```

### Homekit Example
This example is using another node JS project build on top of Homekit Accessory Protocol (HAP):
https://github.com/KhaosT/HAP-NodeJS

Configure the my-loxone.js file to point to your Loxone.
Also rename the virtual output name in loxone_temperatures.js.

```bash
cd example-hap
npm install
npm start
```

Install a free Homekit app from the App Store to find the homekit accessory.