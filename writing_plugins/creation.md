# Creating a plugin

Home_modules plugins are in the form of NPM packages.  
To create a plugin, you have to:

- Create a folder in `<hub>/node_modules` for the plugin. The name of the folder should start with `hmp-`. (e.g. `hmp-arduino`)
- Run `npm init` to generate a package.json file.
- Manually add the following fields to package.json:
  - `"type"`: `"module"`
  - `"title"`: A human readable name of the plugin
  - `"compatibleWithHub"`: A [semver range](https://docs.npmjs.com/cli/v6/using-npm/semver#ranges) that represents the versions of the hub that you want to support.
- Create a `.js` or `.ts` file with the name you specified as the entry point when creating package.json. This is the main file of your plugin.
  
  If you want to use TypeScript:
  - Install TypeScript as a dev dependency of the plugin.
  - Create a file named `tsconfig.json` with the following content:

    ```json
    {
      "ts-node": {
        "esm": true
      },
      "compilerOptions": {
        "target": "es2016",
        "module": "ESNext",
        "moduleResolution": "node",
        "esModuleInterop": true,
        "forceConsistentCasingInFileNames": true,
        "strict": true,
      }
    }
    ```

  - Use `tsc` or `tsc --watch` to compile the TypeScript files. The hub cannot run TypeScript plugins, so it is required to ship compiled JavaScript files too.
  - Make sure the `main` property in `package.json` has the `.js` extension.
- Open the `package.json` of the **Hub** (not your plugin) and add your plugin to the list of the dependencies:
  
  ```json
  {
    ...
    "dependencies": {
      ...
      "hmp-myplugin": "1.0.0"
    }
  }
  ```

  (Make sure to change the plugin name and version to match the one for your plugin.)

> **Warning**  
> Use EcmaScript modules instead of CommonJS, and use the `.js` extension when importing files.

<!--This comment is here to separate the block-quotes-->

> **Warning**  
> Do not import from any hub source file other than `plugins.js`.  
> You are only allowed to import the following items from `plugins.js`:
>
> - `HMApi`
> - `Log`
> - `RoomControllerInstance`
> - `DeviceInstance`
> - `registerRoomController`
> - `registerDeviceType`

After doing these steps, open a Home_modules app and go to setting > plugins. The plugin should appear in the list and you should be able to activate it. If not, check the logs of the hub (`<hub>/data/log.log`) to see the problem.

---
Currently, the only way to install a plugin is to open a terminal on the hub and run NPM commands.

```sh
npm install hmp-arduino
npm uninstall hmp-arduino
```
