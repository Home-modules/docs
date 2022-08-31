# Creating a plugin

To create a plugin, you have to:

- Locate the hub software folder
- Create a folder in `/data/plugins` with a machine-friendly name of the plugin (e.g. `/data/plugins/xyz`)
- Create a `.js` or `.ts` file with the same name (e.g. `/data/plugins/xyz/xyz.js`)
  
  This file is the plugin's main source file.

There is currently no way to share / install plugins other than copying its folder and editing `plugins.json`.
