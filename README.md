# SDI12-UART

Implementation of SDI-12 using a UART driver

- [What is this?](#what-is-this)
- [What is SDI-12?](#what-is-sdi-12)
- [SDI-12 Protocol](#sdi-12-protocol)
  * [Electrical Interface](#electrical-interface)
  * [Communication](#communication)
- [Implementation](#implementation)
- [References](#references)

---

## What is this?

After working on a project involving the use of SDI-12 sensors, I realised there was no SDI-12 library that was suitable for *any* microcontroller (well, any microcontroller that supports UART).
I was aware of the [Arduino-SDI-12](https://github.com/EnviroDIY/Arduino-SDI-12) library which was implemented entirely in software, but that uses the Arduino core library so would not work on *any* microcontroller.
(If you're working with Arduino, the Arduino-SDI-12 library may be the best if you're ok with the restrictions of the SoftwareSerial library).

The goal of this repository is to show a simple way to implement SDI-12 on *any* device with a UART driver.
"How?", you might ask.
Easy!
With hardware.

If you already know about SDI-12, feel free to skip to [Implementation](#implementation).

## What is SDI-12?

The SDI-12 (serial digial interface at 1200 baud) protocol was first released in 1988 designed by the US Geological Surbey's Hydrologic Instrumentation Facility and some private companies.
The SDI-12 specification is maintained by the SDI-12 Support group [1].

SDI-12 is a protocol often used for environmental monitoring due to it's low power operation.
It is intended for use in systems that are battery powered, low cost, and require a multiple sensors attached to one cable.
The SDI-12 protocol supports at least 10 sensors each with a cable length of 60 metres (200 feet).
One key reason for using SDI-12 sensors is that *all* SDI-12 sensors follow the protocol in the exact same way (barring slight differences between versions), so a variety of sensors can be used with little to no changes to the system [2].

## SDI-12 Protocol

### Electrical Interface

The SDI-12 protocol uses 2-3 wires to power and communicate with the sensor.
The figure below shows the bus connections.

![SDI-12 bus](img/sdi12-bus.png)

It is possible to not connect the 12 V line on some sensors, however, that is outside the scope of this repository.

The serial data line is a bidirectional (half-duplex) data transfer line.
It uses inverted logic as shown in the table below.

| Condition | Binary State | Voltage Range |
| --- | --- | --- |
| marking | 1 | -0.5-1.0 V |
| spacing | 0 | 3.5-5.5 V |
| transition | undefined | 1.0-3.5 V |

The 12 V line must be supplied with between 9.6 V and 16 V.

### Communication

## Implementation

## References

[1] Wikipedia Contributers, "SDI-12," Wikipedia, The Free Encyclopedia., 6 June 2017. [Online]. Available: https://en.wikipedia.org/w/index.php?title=SDI-12&oldid=784017978. [Accessed 25 May 2020].  
[2] SDI-12 Support Group, "SDI-12 Specification," 10 January 2019. [Online]. Available: http://www.sdi-12.org/specification.php. [Accessed 2020 May 25].