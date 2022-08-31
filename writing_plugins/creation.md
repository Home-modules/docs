# Creating a plugin

To create a plugin, you have to:

- Locate the hub software folder
- Create a folder in `/data/plugins` with a machine-friendly name of the plugin (e.g. `/data/plugins/xyz`)
- Create a `.js` or `.ts` file with the same name (e.g. `/data/plugins/xyz/xyz.js`)
  
  This file is the plugin's main source file.

> **Warning**  
> Use EcmaScript modules instead of CommonJS, and use the `.js` extension when importing files.

<!--This comment is here to separate the block-quotes-->

> **Warning**  
> Do not import from any hub source file other than `plugins.js`.  
> Do not import `initPlugins` from `plugins.js`.

There is currently no way to share / install plugins other than copying its folder and editing `plugins.json`.
