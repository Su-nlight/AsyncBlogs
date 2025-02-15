---
title: NTFY Trigger Project
date: 2024-12-08
draft: false
tags:
  - NTFY
  - Trigger
  - ESP32
---


NTFY is a lightweight, self-hosted notification service that enables easy push notifications through simple HTTP requests. Ideal for personal projects and small-scale alerting, it offers full control over your data and security. NTFY is highly customizable, cross-platform, and perfect for seamless integration into scripts and applications.

## NTFY Server Setup

This setup is valid for any machine connected to same network as the ESP module with a static or dynamic IP address. There is a minute difference for setting up NTFY for static and dynamic addresses. For now, I will focus on setting it up with static IP address.

### Install requirements
Run the following commands in terminal to install wget, sed, Docker on the machine.
``` bash
sudo apt install wget
sudo apt install sed
sudo apt install docker.io
```
You can also simply procced to next by uncommenting some lines of `docker_setup.sh` as mentioned in the same.

### Download Setup file
Run the following command in terminal of NTFY host machine to download the setup file.
``` bash
wget -O ntfy_docker_setup.sh https://raw.githubusercontent.com/Su-nlight/mini-projects/main/ntfy-trigger/docker_setup.sh
```
**Note** : If you skipped previous step uncomment lines 9 to 11. This can be done using you choice of text editor \[i.e., vim, nano and cat (if you are feeling adventurous)\] .

you can also use actual `server.yml` file as made by NTFY's creator by uncommenting line 16 & line 21 and commenting out line 17. \[This solution is for those who don't trust me even a little bit, as I do not have a very good reputation among my colleagues.\]

As mentioned in the `docker_setup.sh` file itself you can change the external port binding (host machine's) as per your wish by changing the value `2500` (line 30 of same file) to any value (according to your machine condition) you desire.

### Final Container Creation
Run the following command mentioned and relax for it to complete if requirements are already fulfilled the script will be completed in a few instant.
``` bash
sudo chmod +x ntfy_docker_setup.sh
sudo ./ntfy_docker_setup.sh
```
`"Time for a coffee break..."` - [Network Chuck](https://www.instagram.com/networkchuck)

### Configuring NTFY on device
NTFY can be installed on both iOS devices and android from respective app stores. After install, follow following step in sequence.

    1. Click ⋮ at top right corner of the NTFY home screen and Go to settings.
    2. Click on "Default Server" option.
    3. Add the url of NTFY serving machiene with port to this.
    4. Now at the homescreen subscribe to the topic you wish to.

To test this simply from the shell of NTFY host machiene enter the following command. This publishes a simple message using a POST request.

```bash
curl -d "Test message as you wish dear" ipaddress:port/mytopic
```


## NTFY Triggers presets
NTFY triggers are automated events that initiate notifications based on specific conditions or actions. By configuring triggers, you can set up NTFY to send alerts when certain thresholds are met, such as server downtime, application errors, or system performance issues. Triggers are highly customizable, allowing for real-time monitoring and immediate response to critical events. This makes NTFY an effective tool for ensuring timely notifications and enhancing system reliability.


### NTFY API Reference

#### Example HTTP call

```http
POST /phil_alerts HTTP/1.1
Host: ntfy.sh
Title: Unauthorized access detected
Priority: urgent
Tags: warning,skull

Remote access to phils-laptop detected. Act right away.
```

#### List of all parameters

The following is a list of all parameters that can be passed when publishing a message. Parameter names are case-insensitive when used in HTTP headers, and must be lowercase when used as query parameters in the URL. They are listed in the table in their canonical form.<br><br>

| Parameter | Aliases     | Description                |
| :-------- | :------- | :------------------------- |
| `X-Message` | `Message`,`m` | Main body of the message as shown in the notification |
| `X-Title` | `Title`,`t` | Message Title |
| `X-Priority` | `Priority`,`prio`,`p`  | Message Priority |
| `X-Tags` | `Tags`,`tag`,`ta` | Tags and emoji |
| `X-Delay` | `Delay`, `X-At`,`At`, `X-In` , `In` | Timestamp or duration for delayed delivery |
| `X-Actions` | `Actions`,`Action` | JSON array or short format of user actions |
| `X-Click` | `Click` | URL to open when notification is clicked |
| `X-Attach` | `Attach` , `a` | URL to send as an attachment, as an alternative to PUT/POST-ing an attachment|
| `X-Markdown` | `Markdown` , `md` | Enable Markdown formatting in the notification body|
| `X-Icon` | `Icon` | URL to use as notification icon |
| `X-Filename` | `Filename` , `file` , `f` | Optional attachment filename, as it appears in the client|
| `X-Email` | `X-E-Mail` , `Email` , `E-Mail` , `mail` , `e` | E-mail address for e-mail notifications |
| `X-Call` | `Call` | Phone number for phone calls|
| `X-Cache` | `Cache` | Allows disabling message caching|
| `X-Firebase` | `Firebase` | Allows disabling sending to Firebase |
| `X-UnifiedPush` | `UnifiedPush` , `up` | UnifiedPush publish option, only to be used by UnifiedPush apps |
| `X-Poll-ID` | `Poll-ID` | Internal parameter, used for iOS push notifications |
| `Authorization` | - | If supported by the server, you can login to access protected topics |
| `Content-Type`| `-` | If set to `text/markdown` , Markdown formatting is enabled |


### ESP module configurations
The ESP32 is a powerful, low-cost microcontroller with built-in Wi-Fi and Bluetooth capabilities, widely used in IoT projects. It features a dual-core processor, multiple GPIO pins, and supports various communication protocols, making it versatile for a range of applications. The ESP32 is popular for its robust performance, energy efficiency, and the ability to handle complex tasks like real-time data processing and wireless communication.

#### Module connections

This project is made as a reference and ready to burn code of the presets for NTFY service using `ESP32` or `ESP8266`. The ESP development board has inbuilt wifi and bluetooth modules and has higher clock speed than arduino boards making it capable for such an application. This project uses `KY-022` infrared reciever module connected to ESP as per following pin scheme.

| ESP GPIO PIN | KY-022 PIN |
| :-----: | :-----: |
| GPIO 15 | data / `S` |
| 3.3v | +vcc |
| GND | -vsat |

#### Code upload to ESP

Nextly, open Arduino IDE and create new sketch, now add the code that is in [esp32-ntfy-presets.ino](https://github.com/Su-nlight/mini-projects/blob/main/ntfy-trigger/esp32-ntfy-presets.ino).

**Note** : I assume that appropriate board are added, if not then follow [this tutorial](https://randomnerdtutorials.com/installing-the-esp32-board-in-arduino-ide-windows-instructions). Further select ESP board and port of comuter to which it is connected (Appropriate drivers must be installed beforehand for serial communication)

Now you can configure the Switch case as per the remote youhave and its hex codes of that remote. Take assistance of the [NTFY API Reference](#NTFY-API-Reference) and make it as per your requirement, this is all upto your coding.

## Conclusion
The "ntfy-trigger" project in this GitHub repository is a tool designed to automate and manage notifications using the NTFY service. It allows users to set up triggers that monitor specific events or conditions, automatically sending alerts when these criteria are met. The project integrates seamlessly with NTFY, providing a lightweight solution for real-time monitoring and notifications. With customizable triggers and notification parameters, "ntfy-trigger" is ideal for developers and enthusiasts seeking to implement efficient, automated alert systems in their projects. This makes it especially useful for small-scale applications, personal projects, or any scenario requiring timely notifications.