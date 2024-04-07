# Config 2

This directory contains schematics for a microcontroller with the following properties:

- Inverted: No
- Can send break: Not by default
  - Reinitialisable: Yes
- GPIO Voltage: 5V

Refer to [this code](https://github.com/arduino-libraries/ArduinoRS485/blob/1993679d2b0588d650bf064b1d62c2ca67b88a0e/src/RS485.cpp#L173) for how to send a break. `_serial->` is the equivalent of `Serial.`

