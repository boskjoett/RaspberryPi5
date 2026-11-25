# Using the Raspberry Pi to host a public web server

The following steps describe how to set up a Raspberry Pi to be public reachable from the internet via HTTPS on a public hostname.

My home broadband setup consists of a fiber connection to a Sagemcom router. 
This router supports several dynamic DNS providers and port forwarding.

I have dynamic public IP (v4) address.

I own the hostname **skjoett.com** and I want the Pi to be reachable on a skjoett.com subdomain like *pi.skjoett.com*.

## Raspberry Pi setup

- Install Ubuntu on the Pi.
- Connect the Pi to the router's wifi.

## Router setup

- Enter the router's admin web UI at http://192.168.1.1
- Find the Pi in the list of devices connected to the wifi
- Reserve an internal IP address for the Pi based on its MAC address.
- Select port forwarding and create these mappings to the Pi's internal IP address:
  - External port 80 -> internal port 80
  - External port 443 -> internal port 443

