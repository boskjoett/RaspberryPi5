# Using Anthias Digital Signage on Raspberry Pi 5

Anthias is a free open source system for turning any TV into a digital sign (aka. Kiosk or info screen) <br />
See https://anthias.screenly.io/

GitHub repository: https://github.com/Screenly/Anthias/

## Installing an OS on Raspberry Pi

Download an image to install on an SD card from the Anthias releases page https://github.com/Screenly/Anthias/releases <br />
This will install Raspberry Pi OS Lite (no Desktop UI) and Anthias.

Write the image to an SD card (16 GB) using the Raspberry Pi imager.

When Anthias starts up you will see its logo screen. You can break out to a console by pressing Alt+F4

## Configuring wifi
In the console you can configure wifi with this command

    sudo raspi-config

or using `nmcli` (Network Manager Command Line Interface)

Command to see available wifi networks

    nmcli dev wifi list

Command to connect to a wifi network

    sudo nmcli --ask dev wifi connect <example_ssid>

See more here https://www.raspberrypi.com/documentation/computers/configuration.html#wireless-connection

### Verifying Wifi Connection
Use `ifconfig wlan0` to check for an IP address or `iwconfig` to see the connected SSID. 

## Accessing the Anthias Management UI

Open a web browser on a PC that is connected to the same wifi and enter http://<ip address>
where *IP address* is the IP address of the Raspberry Pi.

## Remote Access to Anthias Management UI

You can gain access to the Raspberry Pi from a remote PC by using Tailscale.

With Tailscale you don't need to configure NAT or port forwarding on the router. You access the Raspberry Pi via Tailscale's cloud service.

Install Tailscale on the Raspberry Pi.

### Create a funnel to the Anthias web server

    sudo tailscale --bg funnel 80

You can now acceess the Anthias web server via a URL like this

https://raspberrypi.tailccc4a6.ts.net/

