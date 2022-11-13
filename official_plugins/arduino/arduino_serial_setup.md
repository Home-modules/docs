# Arduino (Serial) room controller setup

## Hardware

1. Pick an Arduino board (Uno or Nano, or Mega if there are many devices) and install a breakout shield. You can also design your own board.
2. Upload the [`controller-arduino-serial`][firmware] firmware to the board.

   You can do this step before setting up the hub software, but make sure the current program in the Arduino board doesn't interfere with the hardware.
3. Connect a 5V power source to the board.
4. Connect the board to the hub via a long enough USB cable.
5. Install the board somewhere in the room, preferably on the wall.

> **Warning**
>
> Make sure all connections are solid. Preferably use more permanent connectors like this:
>
> <image src="../../img/simple-connector.jpg" height="150" alt="Connector"/>

## Software

Install the [Arduino IDE](https://arduino.cc/en/software) and upload the [`controller-arduino-serial`][firmware] firmware to the Arduino board if you haven't done so already.

When creating the room, choose 'Arduino (Serial)' as the room controller.

When specifying the serial port, use the dropdown to select the desired serial port. You can disconnect the board and refresh the list to see which port the board corresponds to.

[firmware]: https://github.com/Home-modules/controller-arduino-serial
