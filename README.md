# audiomoth-hid #
A Node.js library for interfacing with AudioMoth devices over USB.

### Usage ###

Obtain the time used by the onboard clock:
```
audiomoth.getTime(function (err, date) {
	console.log("Time/date on device: " + date);
});
```

---
Set the onboard clock with a `Date()` object:
```
audiomoth.setTime(new Date(), function (err, date) {
	console.log("New time/date of device: " + date);
});
```

---
Get AudioMoth's unique 16 digit ID number:
```
audiomoth.getID(function (err, id) {
	console.log("Device unique ID: " + id);
});
```

---
Get string containing the current battery state of attached AudioMoth:
```
audiomoth.getBatteryState(function (err, batteryState) {
	console.log("Attached device's current battery state: " + batteryState);
});
```

---
Send arbitrary packet of max length 62 (message limit of 64 bytes, 2 bytes required for message identification) :
```
var packet = new Uint8Array(62);
```

Insert date object into packet
```
var date = new Date();
writeLittleEndianBytes(packet, 0, 4, date.valueOf() / 1000);
```

Insert 32 bit number into packet
```
var i = 10000;
writeLittleEndianBytes(packet, 4, 4, i);
```

Insert arbitrary 8 bit values into packet:
```
packet[8] = 5;
packet[9] = 0x00;
packet[10] = 0x01;
```

```
audiomoth.setPacket(packet, function (err, packet) {
	console.log("Data returned from application specific packet: " + packet);
});
```

---
Obtain arbitrary packet set in AudioMoth firmware:
```
audiomoth.getPacket(function (err, packet) {
	console.log("Data returned from application specific packet: " + packet);
	audiomoth.convertFourBytesFromBufferToDate(packet, 1);
});
```

### Example applications using this module ###
* [AudioMoth Configuration App](https://github.com/OpenAcousticDevices/AudioMoth-Config-App)
* [AudioMoth Timesetter](https://github.com/OpenAcousticDevices/AudioMoth-TimeSetter)

### License ###

Copyright 2017 [Open Acoustic Devices](http://www.openacousticdevices.info/).

[MIT license](http://www.openacousticdevices.info/license).