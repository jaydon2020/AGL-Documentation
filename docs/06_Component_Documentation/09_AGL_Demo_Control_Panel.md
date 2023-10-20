---
title: AGL Demo Control Panel
---
# AGL Demo Control Panel

## Introduction

This document describes the design and usage of the **AGL Demo Control Panel**, a **Qt5-based** tool that allows you to control and interact with various **Automotive Grade Linux (AGL)** demo applications. The tool uses **Kuksa.val** and **CAN frame messages** to communicate with the target machine that runs the AGL image(s). You can use the tool to perform tasks such as starting and stopping scripts, changing the vehicle speed and engine RPM, adjusting the HVAC settings, and providing Steering Inputs. The tool is designed to **demonstrate** the capabilities and features of AGL in a **user-friendly** and **interactive** way.
### Application Overview

To use the control panel, you need to connect the main machine that runs the control panel to the target machine that runs the AGL image(s) using a **LAN/ethernet cable**. You also need to configure the IP address of the Kuksa server and set your preferences in the tool’s settings menu.

![Layers_And_Extensions](images/AGL-Demo-Control-Panel/Application-Logic.png)
## Installation  

  

- _Note_:
	If errors occur in Debian based/Rasbian OS during installation, follow the steps mentioned below and skip to step 2:
```bash
$ nano requirements.txt
# -> Comment pyqt5 dependency using "#"
$ sudo apt install python3-pyqt5 python3-qtpy pyqt5-dev-tools python3-pyqt5.qtsvg -y
```

- Step 1
```bash
$ python3 -m venv control-panel
$ source control-panel/bin/activate
```

- Step 2
```bash
$ pip3 install -r requirements.txt
$ pyrcc5 assets/res.qrc -o res_rc.py
```
## Setup

Before using the  `AGL Demo Control Panel`, we need to make sure to run the Kuksa.val server  and also have our `can0` interface set up (Optional).

### 1. Connect the Machines

First, we need to connect the machines, i.e. the host machine (Running the control panel) and the target machine (running the AGL image) via LAN or a bridged network (QEMU or VM) 
### 2. CAN interface (WIP)

To set up the CAN interface between the Host system and the target machine(s) we use [cannelloni](https://github.com/mguentner/cannelloni),

1. Create the virtual CAN interface with the command:

	```bash
	$ sudo ip link add dev can0 type vcan
	```

2. On both machines, bring the virtual CAN interface online with the command: 

	```bash
	$ sudo ip link set up can0
	```

3. Install cannelloni from its [GitHub repository](https://github.com/mguentner/cannelloni) and run cannelloni with the following commands. 

	_Note_: `cannelloni` is available in AGL, just add `IMAGE_INSTALL:append = " cannelloni"`	to your `conf/local.conf`

	Host Machine (Running `AGL Demo Control Panel`)
	```bash
	cannelloni -I can0 -R <target-ip> -r 20000 -l 20000
	```
	 Target Machine (Running AGL image)
	```bash
	cannelloni -I can0 -R <host-ip> -r 20000 -l 20000 -p
	```

	You should now be able to send and receive CAN messages between the two machines using the vcan interface and cannelloni.

### 3. Configuration for Kuksa-val-server/ Kuksa databroker

Run the `kuksa-val-server`/`databroker` on `0.0.0.0` by either restarting the server, editing `/etc/default/kuksa-databroker` or add the `agl-demo-preload` as a feature to your build of AGL.  The server should be started using the `--address 0.0.0.0` argument.

Now, you can create a custom configuration to save your specific preferences for the settings page by creating the `*.ini*` files in the,
-  `/etc/agl-demo-control-panel.ini`
- `$HOME/.local/share/agl-demo-control-panel/config.ini`
	
```python
[default]
preferred-config=AGL-kuksa-val-server

# [cutom-config-template]
# ip=<ip address>
# port=<port number>
# protocol=<ws|grpc>       # ws/grpc -> kuksa-val-server, grpc -> databroker
# insecure=<true|false>    
# cacert=<default|/path/to/CA.pem>
# token=<default|/path/to/token>      
# tls_server_name=<name>

[kuksa-val-server]
ip=localhost
port=8090
protocol=ws
insecure=false
token=default
tls_server_name=
```

### 4. Start AGL Demo Control Panel

1. To start the control panel
	```
	$ cd /Path/to/agl-demo-control-panel
	$ source control-panel/bin/activate
	$ python -u main.py
	```

![Layers_And_Extensions](images/AGL-Demo-Control-Panel/Dashboard.png)

2. Go to settings
	- Start (load default config from drop-down menu)
	-  Configure: make changes as required
		- IP-Address
		- Port
		- Secure Mode: if toggled **"on"**, specify the `CA.pem File`
		- Protocol: websocket ← ? → gRPC
		- TLS Server Name
		- JWT/Auth Token path 
		- CA.pem certificate path
	- Start/Stop
	- Reconnect
	- Page settings: Configure the visibility of pages and switch between CAN and Kuksa messages by using the toggle for the same.

![Layers_And_Extensions](images/AGL-Demo-Control-Panel/Settings_Page.png)

1. Navigate to the desired page using the provided buttons at the bottom

|  |
|---|
| ![Layers_And_Extensions](images/AGL-Demo-Control-Panel/IC.png) |

|  |  |
|---|---|
| ![Layers_And_Extensions](images/AGL-Demo-Control-Panel/HVAC.png) | ![Layers_And_Extensions](images/AGL-Demo-Control-Panel/SC.png) |
