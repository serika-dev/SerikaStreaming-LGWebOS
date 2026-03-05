# Serika Streaming – LG TV (WebOS)

LG WebOS web app wrapper for [Serika Streaming](https://streaming.serika.dev).

## Overview

This is a lightweight WebOS web application that loads the Serika Streaming website inside the LG OS. It:

- Navigates to the login page (or auto-redirects to browse if already logged in)
- Identifies itself via `?platform=webos` so the website hides Chromecast UI and sends `X-Serika-Platform: samsung-tv` headers
- Registers LG remote control keys for media playback
- Keeps the screen awake during use

## Project Structure

```
├── appinfo.json    # WebOS app manifest (privileges, access, metadata)
├── index.html      # Entry point with loading screen
├── js/
│   └── main.js     # Remote key registration, navigation logic
├── css/
│   └── style.css   # Loading screen styles
├── icon.png        # App icon (114×114)
└── largeicon.png
```

## Requirements

- [The LG WebOS CLI](https://webostv.developer.lge.com/develop/tools/cli-installation)
- LG WebOS developer mode enabled (for sideloading)

## Building & Deploying

### Using the WebOS CLI

1. Open the terminal in the project root
2. type ares-generate -l
3. This produces a `.ipk` file in the project root

### Sideloading to a LG TV (This only lasts for a certain time period, or when offline after 10 reboots)

1. Create a LG Developer account
2. Install the [Developer Mode app](https://gb.lgappstv.com/main/tvapp/detail?appId=232503&catCode1=&moreYn=N&cateYn=N&orderType=0&headerName=&appRankCode=&sellrUsrNo=)
3. Boot into the Developer Mode app and login with your Developer account.
4. Click on the "Dev Mode Status" button to reboot the TV into Developer Mode
5. Execute the ares-setup-device command and select add.
6. Enter the details about your TV (NOTE: You do not need to enter any password. It is not required)
7. When your tv is added to the list, enable the Key Server button.
8. Get the Key File with the following command
```bash
ares-novacom --device (INPUT THE NAME YOU GAVE THE DEVICE IN STEP 6 HERE) --getkey
```
9. Enter the passphrase into the input as displayed on your tv. This is case sensitive.
10. Check the connection with the following command:
```bash
ares-device --system-info --device (INPUT THE NAME YOU GAVE THE DEVICE IN STEP 6 HERE)
```
11. Install the app by running the next command:
```bash
ares-install --device (INPUT THE NAME YOU GAVE THE DEVICE IN STEP 6 HERE) ./(INPUT THE NAME OF THE IPK FILE HERE)
```
12. Use ares-launch to run the app:
```bash
ares-launch --device (INPUT THE NAME YOU GAVE THE DEVICE IN STEP 6 HERE) (INPUT THE NAME OF THE APP IN THE FOLLOWING FORMAT: com.domain.app)
```
NOTE: If the commands don't work, type npx in front of the command.

## Configuration

Edit `js/main.js` to change the target URL:

```js
var WEBSITE_URL = 'https://streaming.serika.dev';
```

## License

See [LICENSE](LICENSE).
