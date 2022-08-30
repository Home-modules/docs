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

4. Optional: To get HTTPS, copy the private key and SSL certificate (`cert.pem` and `key.pem`) to the `data` folder.
5. Optional: To host the web app from the hub, [build the web app](building-web-app.md) and copy the contents of the `build` directory to `data/webapp`.
6. Optional: To increase startup speed, compile all TypeScript files to JavaScript:

   ```sh
   tsc
   ```

   Or if you are modifying source files:

   ```sh
   tsc --watch
   ```

7. Start the hub:

   ```sh
   npm start
   ```

   Wait until you see the `Home_modules hub is now running` message.

## Command line arguments

```sh
npm start -- <arguments>
```

- `--no-log`: Disable logging to `data/log.log`
- `--debug`: Increase log verbosity level
- `--api-port <port>`: Change API server port, changing will breask the apps. (default: 703)
- `--webapp-port <port>`: Change web app hosting port. (default: 80 for HTTP or 443 for HTTPS)
