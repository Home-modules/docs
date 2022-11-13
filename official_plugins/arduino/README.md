# The Arduino plugin

This plugin allows you to use Arduino boards as room controllers. It also provides support for some DIY devices.

> **Warning**  
> This plugin is not affiliated with the company Arduino.

## Room controllers

### Arduino (Serial)

Communicate with the Arduino board via a serial port. This serial port can be a USB connection, bluetooth or any other means of connection that works with Arduino IDE.

ID: `arduino:serial`

- [Setup](arduino_serial_setup.md.md)

## Devices

### Light

#### Standard

A simple light that is controlled by a relay and an optional physical switch.

ID: `light:standard`

- [Setup](light_standard_setup.md)
