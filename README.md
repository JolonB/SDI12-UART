# SDI12-UART
Implementation of SDI-12 using UART driver

- [What is this?](#what-is-this-)
- [What is SDI-12?](#what-is-sdi-12-)
- [SDI-12 Protocol](#sdi-12-protocol)

---

## What is this?

After working on a project involving the use of SDI-12 sensors, I realised there was no SDI-12 library that was suitable for *any* microcontroller (well, any microcontroller that supports UART).
I was aware of the [Arduino-SDI-12](https://github.com/EnviroDIY/Arduino-SDI-12) library which was implemented entirely in software, but that uses the Arduino core library so would not work on *any* microcontroller. 
(If you're working with Arduino, the Arduino-SDI-12 library may be the best if you're ok with the restrictions of the SoftwareSerial library).

The goal of this repository is to show a simple way to implement SDI-12 on *any* device with a UART driver.

## What is SDI-12?


## SDI-12 Protocol