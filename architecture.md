# Home_modules architecture

## The hub

Every smart device in the home is directly or indirectly (via a room controller) connected to the _hub_. The hub is a computer (most commonly a Raspberry PI) that controls all the devices in the house. When you open the Home_modules app and control your devices, the app communicates with the hub.

The hub also hosts the web app.

## Rooms

Each home is divided into several _rooms_. Each room is a collection of devices and has a room controller.

### Room controllers

All rooms have a _controller_ which main purpose is to to simplify connections and act as a layer of abstraction.

Instead of each device having its own connection to the hub, it can connect to the room controller, which has a single connection to the hub. This way simplifies the wiring for the connections and saves IO ports on the hub.

Room controllers can be of different types.  
One example is the Arduino (Serial) room controller which is an Arduino board connected to the hub with a USB cable.

## Devices

A device can be any of the following:

- A light
- A fan or AC
- An outlet
- A door
- A thermometer
- A photocell
- A TV
- A security camera
- etc.

Devices can be of different types, each compatible with one or more room controller types.

Devices connect to the hub via a room controller.

## Plugins

Each room controller type and device type needs a piece of software inside the hub, so that the hub can communicate with it. These pieces of code are plugins.

A plugin can add support for room controller types and/or device types. For example, the `builtin` plugin brings support for room controllers based on Arduino and simple devices.
