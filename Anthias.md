# Using Anthias Digital Signage on Raspberry Pi 5

Anthias is a free open source system for turning any TV into a digital sign (aka. Kiosk or info screen) <br />
See https://anthias.screenly.io/

GitHub repository: https://github.com/Screenly/Anthias/

Anthias only supports Debian-based Armbian images (Bookworm / Trixie). The installer wires up the Docker apt repository under download.docker.com/linux/debian, so Ubuntu-based Armbian downloads (Jammy / Noble) will fail at the apt update step.

## Installing an OS on Raspberry Pi

Use Raspberry Pi Imager to install Raspberry Pi OS Lite 64-bit on an SD card (32 GB).

During the installation process you will be asked to 
- enter a root username and password.
- enter Wifi name and password
- whether to enable SSH - choose yes
- whether to enable Raspberry Pi Connect - choose yes

Boot up on the new OS and install Anthias by using its installation script.

    bash <(curl -sL https://install-anthias.srly.io)

When Anthias starts up you will see its logo screen. \
You can break out to a console by pressing Ctrl + Alt + F1 (or Alt + F4)

With this installation method you have SSH access to the Raspberry Pi for further customization. 
If you choose to install Anthias directly from the Raspberry Pi Imager you will get BalenaOS and no SSH access.

## Configuring wifi
In the console you can configure wifi with this command

    sudo raspi-config

or using `nmcli` (Network Manager Command Line Interface)

Command to see available wifi networks

    nmcli dev wifi list

Command to connect to a wifi network

    sudo nmcli --ask dev wifi connect <example_ssid>

See more here https://www.raspberrypi.com/documentation/computers/configuration.html#wireless-connection

Sometimes connection fails because the Wifi access point does not allow more connections.
Try connecting to 5G instead.

### Verifying Wifi Connection
Use `ifconfig wlan0` to check for an IP address or `iwconfig` to see the connected SSID. 

## Accessing the Anthias Management UI

Open a web browser on a PC that is connected to the same LAN or wifi and enter http://<ip address>
where *IP address* is the IP address of the Raspberry Pi.

When Anthias boots up it also shows a splash screen with its IP address for a few seconds.

## Remote Access to Anthias Management UI

The Management UI is provided by an Anthias web server listening on port 80 on the Raspberry Pi.

You can gain access to the Raspberry Pi from a remote PC by using the free [Raspberry Pi Connect](https://www.raspberrypi.com/software/connect/) service.

Or you can use [Tailscale](https://tailscale.com/) which is free for personal use.

With Tailscale and Raspberry Pi Connect you don't need to configure NAT or port forwarding on the router. 
You access the Raspberry Pi via Tailscale's or Raspberry Pi Connect's cloud service.

When you make the Raspberry Pi Image using the Raspberry Pi Imager program on Windows you are asked if you
want to install Raspberry Pi Connect - say yes.

Create a account in Raspberry Pi Connect (sign in with Google using your Google account).

Install Tailscale on the Raspberry Pi.

Create an account in Tailscale (sign in with Google using your Google account).

Login to the Tailscale Admin Console

Add your Raspberry Pi as a machine in Tailscale.

Give it a tag `webserver`.

Define a service. Call it e.g. "anthias-raspberry4" and connect it to port tcp:443

Define the same service on the Raspberry device with a command like this

    sudo tailscale serve --service=svc:anthias-raspberry4 --https=443 127.0.0.1:80

Approve the service in the Tailscale admin console.

### Create a funnel to the Anthias web server

See https://tailscale.com/docs/reference/examples/funnel

    sudo tailscale funnel --bg 80

To see funnel status: `tailscale funnel status`

```
# tailscale funnel status

# Funnel on:
#     - https://raspberrypi-4-anthias.tailccc4a6.ts.net

https://raspberrypi-4-anthias.tailccc4a6.ts.net (Funnel on)
|-- / proxy http://127.0.0.1:80
```

You can now acceess the Anthias web server via a URL like this

https://raspberrypi.tailccc4a6.ts.net/

