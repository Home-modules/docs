# Running the hub software

1. Make sure you have Node.JS (v14 or higher) and NPM (v7 or higher) installed.
2. Clone this repository

   ```sh
   git clone https://github.com/Home-modules/hub --recursive
   cd webapp
   ```

3. Install dependencies

   ```sh
   npm install
   ```

4. Optional: To get HTTPS for the API and web app servers, copy the private key and SSL certificate (`cert.pem` and `key.pem`) to the `data` folder.

   > **Warning**  
   > Enabling HTTPS will disable HTTP.

   > **Note**  
   > To be able to install the web app, HTTPS must be enabled, unless the web app is accessed from the hub itself.

   > **Warning**  
   > Do not use a self-signed certificate, because:
   >
   > - You will get a big warning when the web app is opened
   > - Username and password autofill will be disabled.
   > - You can't install the web app.

5. Optional: To host the web app from the hub, [build the web app](building-web-app.md) and copy the contents of the `build` directory to `data/webapp`.

6. Optional: To increase startup speed, compile all TypeScript files to JavaScript:

   ```sh
   npx tsc
   ```

   > **Note**  
   > Don't forget to do this again after updating the hub. (this isn't required if you skipped this step or deleted the JS files.)

   Or if you are modifying source files:

   ```sh
   npx tsc --watch
   ```

7. Start the hub:

   ```sh
   npm start
   ```

   Wait until you see the `Home_modules hub is now running` message.

   If you've followed the instructions correctly and the above command didn't work, try this:

   ```sh
   npm run start-no-pm2
   ```

   > Warning  
   > If you use any of the fallbacks, pm2 will not be used. As a side effect, the hub doesn't restart correctly, it will simply quit.

   If that didn't work either, try this:

   ```sh
   cd src
   npx ts-node index.js
   ```

   If that didn't work either, compile the TypeScript files if you haven't done so, then try this:

   ```sh
   cd src
   node index.js
   ```

   If that still doesn't work, double check that you've followed the instructions correctly. If you've followed the instructions correctly, tried all fallbacks and the hub still refuses to start, [create an issue on our GitHub](https://github.com/Home-modules/hub/issues/new/choose).

## Command line arguments

```sh
npm start -- <arguments>
```

- `--no-log`: Disable logging to `data/log.log`
- `--debug`: Increase log verbosity level
- `--api-port <port>`: Change the API server port, will break the apps. (default: 703)
- `--webapp-port <port>`: Change the web app hosting port. (default: 80 for HTTP or 443 for HTTPS)

If the command line arguments were ignored (an NPM bug), add another `--`:

```sh
npm start -- -- <arguments>
```
