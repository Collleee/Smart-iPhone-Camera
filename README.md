# Turn Your Old iPhone into a Smart Camera for Home Assistant & More  

This repository documents how I repurposed an old iPhone as a smart security camera for **Home Assistant** and potentially **HomeKit**. By using the [**IP Camera Lite**](https://apps.apple.com/us/app/ip-camera-lite/id1013455241) or [**IP Camera Pro**](https://apps.apple.com/us/app/ip-camera-pro/id990605467) app (recommended), you can broadcast the iPhone’s camera feed over **HTTP** or **RTSP**, enabling integration with multiple smart home platforms.

## Table of Contents  
- [Features & Integration](#features--integration)  
  - [Streaming Video](#1-streaming-video)  
  - [Motion Detection Clips to FTP](#2-motion-detection-clips-to-ftp-pro-version-only-)  
  - [Toggle the motion detection and flashlight via HTTP Requests](#3-remote-control-via-http-requests)  
  - [API Endpoints](#api-endpoints)  
- [Downsides & Workarounds](#downsides--workarounds)  
- [Why This is Useful](#why-this-is-useful)  

## Features & Integration  

### 1. Streaming Video  
- Install [**IP CAMERA LITE**](https://apps.apple.com/us/app/ip-camera-lite/id1013455241) (free) or [**IP CAMERA PRO**](https://apps.apple.com/us/app/ip-camera-pro/id990605467) (recommended for more features).  
- Start the **server** to begin broadcasting via:  
  - **HTTP** (MJPEG stream)  
  - **RTSP** (recommended for Home Assistant and HomeKit)  
  - **Customize camera settings** such as username, password, resolution, FTP connections, and more within the app.

### 2. Motion Detection Clips to FTP (PRO Version Only ☹️)  
- The **Pro version** supports motion detection and can **upload clips to an FTP server**.  
- Use **Home Assistant’s FTP add-on** to receive these clips and trigger automations.  
- **Folder watcher** monitors when a new file is added to a folder in Home Assistant. I set up the **Plex Media Server add-on** with a dedicated **motion clips library**, and the folder watcher triggers an automation to **refresh the Plex library**, ensuring new recordings are instantly available for playback.  

### 3. Toggle Motion Detection & Flashlight via HTTP Requests  
The camera’s built-in **HTTP API** allows control via **GET requests**:  
- **Toggle motion detection**  
- **Turn on/off iPhone flashlight** (Only works when motion detection is off)  
- **Take snapshots**  

When requesting a **snapshot**, the response contains the **filename**. Append this to the camera’s base URL to **retrieve the image** dynamically.  

These **GET requests** can be set up as **service actions** in **Home Assistant** using **restful_command**, allowing them to be triggered within automations or via the Home Assistant UI.  

### API Endpoints  
- **Toggle iPhone Flashlight:** `http://username:password@iphone_ipaddress:8083/light`  
- **Toggle Motion Detection:** `http://username:password@iphone_ipaddress:8083/toggle_motion`  
- **Capture Snapshot:** `http://username:password@iphone_ipaddress:8083/getsnapshot` (Returns the filename of the snapshot)  
- **Retrieve Snapshot:** `http://username:password@iphone_ipaddress:8083/get/{{ snapshot_filename }}`  

## Downsides & Workarounds  
- The **app crashes occasionally**, requiring **hourly Shortcut automations** to reopen the app and restart the server.  
- **Guided Access** was tested but did not fully prevent crashes.  
- I will begin testing **email-based automations** using **iCloud push mail** to trigger a Shortcut. This allows **Home Assistant** to send an email when the camera goes offline, which can then trigger a Shortcut to restart the app automatically. The automation will be triggered by detecting the **request failure code** when the camera is down.  

## Why This is Useful  
This setup makes an **old iPhone** a powerful smart home camera, with **motion-triggered recordings**, **remote control capabilities**, and **Home Assistant integration**—**all without extra hardware**! When the **motion clips** are saved, you can also have a **Plex Media Server** host those clips, ensuring easy access and playback.  

Check out the repo for automation scripts and Home Assistant configurations.
