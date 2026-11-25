# Using the Raspberry Pi to host a public web server

The following steps describe how to set up a Raspberry Pi to be public reachable from the internet via HTTPS on a public hostname.

My home broadband setup consists of a fiber connection to a Sagemcom router. \
This router supports several dynamic DNS providers and port forwarding. \
I have a dynamic public IP (v4) address.

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

Port 80 is used by Let's Encrypt to verify that you control the domain.

Because my IP address is dynamic I need to setup dynamic DNS in my router so it informs a DNS service about IP address changes. \
I will use Dynu, a free Dynamic DNS provider. \
Dynu has specific setup instructions for a Sgamecom router at 
https://www.dynu.com/DynamicDNS/IPUpdateClient/Sagemcom?utm_source=chatgpt.com

### Create a Dynu account
Go to Dynu.com and create a free account.

In the control panel:
1. Select **DDNS Services**.
2. Click **Add**.
3. Choose **Use Your Domain Name**.
4. Enter **pi.skjoett.com**.
5. Save

### Configure your Sagemcom router
Use these settings:

| Router field | Value              |
| ------------ | ------------------ |
| Provider.    | Custom             |
| Username	   | Your Dynu username |
| Password	   | Your Dynu password |
| Hostname	   | pi.skjoett.com     |
| Basic Authentication | ON         |
| Server       | api.dynu.com       |
| Port.        | 8245               |
| Request      | /nic/update?       |

### Connect the subdomain to Dynu

Since your domain is hosted at an ISP, we need to delegate just the pi.skjoett.com subdomain to Dynu. \
In the ISP's DNS settings, create NS records for hostname pi, pointing to Dynu's nameservers:

| Type | Hostname | Target       |
| -----| -------- | ------------ |
| NS   | pi       | ns1.dynu.com |
| NS   | pi       | ns2.dynu.com |
| NS	 | pi       | ns3.dynu.com |
| NS	 | pi	      | ns4.dynu.com |
| NS	 | pi	      | ns5.dynu.com |

This delegates only pi.skjoett.com; your main domain and email can remain at the ISP.

## Get a certificate

We use Let's Encrypt to obtain a free, trusted HTTPS certificate, and Caddy to manage it automatically on the Raspberry Pi.

The nice thing about Caddy is that it:
- Obtains the certificate automatically.
- Renews it before it expires.
- Configures HTTPS for you.
- Redirects HTTP traffic to HTTPS.
- Requires very little maintenance.

You won't need to buy a certificate or manually renew one.

