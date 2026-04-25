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

The Management UI is provided by an Anthias web server listening on port 80 on the Raspberry Pi.

You can gain access to the Raspberry Pi from a remote PC by using [Tailscale](https://tailscale.com/) which is free for personal use.

With Tailscale you don't need to configure NAT or port forwarding on the router. You access the Raspberry Pi via Tailscale's cloud service.

Install Tailscale on the Raspberry Pi.

Create an account in Tailscale (using your Google account)

Login to the Tailscale Admin Console

Add your Raspberry Pi as a machine in Tailscale.

Give it a tag `webserver`.

Define a service. Call it "anthias" and connect it to port tcp:443

### Create a funnel to the Anthias web server

See https://tailscale.com/docs/reference/examples/funnel

    sudo tailscale funnel --bg 80

To see funnel status: `tailscale funnel status`

You can now acceess the Anthias web server via a URL like this

https://raspberrypi.tailccc4a6.ts.net/

