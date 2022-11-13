# Setting up your Home_modules-powered smart home

> **Note**  
> This guide is very generic, because instructions vary between plugins. Please refer to the plugin's documentation for better guidance.

## Required items

- A computer as the hub (preferably a Raspberry PI)
- Compatible room controllers (e.g. Arduino boards)
- Compatible devices
- Appropriate connections for the room controllers and devices (e.g. long USB cables and wires)

## Setting up the hardware

This part will vary depending on the type of the room controllers and devices, but here is a general guide:

1. Install the hub in the appropriate location (e.g. in the living room)
2. Install the room controllers (e.g. Arduino boards) in the appropriate locations in each room
3. Connect the room controllers to the hub.
4. Install the devices and connect them to their room controllers.

## Setting up the software

### Install and run the hub software

[Running the hub software](running-hub.md)

### Open the web app

If the hub has a graphical operating system and a browser that you can use:

1. Open <http://localhost> (or <https://localhost> if HTTPS is enabled) on your browser.
2. Install the app if prompted to.

Otherwise:

1. Connect the hub to a local network.
2. Connect your smart phone or computer to the same network.
3. Find the local IP address of the hub and enter it in the browser's address bar on your phone or laptop.
4. Install the app if prompted to.

### Login

After opening the web app, you will see a login screen.  
Enter these default credentials:

- Username: `admin`
- Password: `admin`

After you've logged in, you will be prompted to change your password. Do so.

### Setting up the rooms and devices

For each room:

1. Navigate to Settings -> Rooms.
2. Click the '+' button at the end of the list.
3. Enter a name for the room.
4. Choose a controller type.
5. Fill in the fields.
6. Click 'save' at the bottom.

For each device:

1. Navigate to Settings -> Rooms.
2. Find the room you want to add the device to and click the plug icon near it.
3. Click the '+' button at the end of the list.
4. Choose the device type.
5. Enter a name for the device.
6. Fill in the fields.
7. Click 'save' at the bottom.
