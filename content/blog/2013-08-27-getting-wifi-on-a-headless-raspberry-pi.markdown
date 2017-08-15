---
title: "Getting WiFi on a Headless Raspberry Pi"
date: 2013-08-27T11:38:00-07:00
categories: [linux, raspberry pi]
---

One of the final steps in a to-be-announced project involved getting a headless [Raspberry Pi](http://www.raspberrypi.org/) up on WiFi at boot time—which turned out to be a little more challenging that first expected. Not because it was particularly difficult, but because the documentation was poor.

Below are my full configuration files that were required. The WiFi USB dongle itself was an [Adafruit USB WiFi with Antenna](https://www.adafruit.com/products/1030) which is actually a B-LINK BBL-LW05-AR5; notable becuase of its sizeable antenna, and for working with the latest Raspbian Wheezy distribution.

Prior to getting started, make sure your Raspbian is up to date by issuing:

```bash
sudo apt-get update
sudo apt-get upgrade
```

At that point, reboot and connect your WiFi module to USB. Then, edit your config. It's worth noting that I have an interface `eth0` which is set to DHCP, and my `wlan0` interface which has a static IP. This allows me to use ethernet as a failsafe if something goes wrong.

First, in the file `/etc/network/interfaces`:

```
auto lo

iface lo inet loopback
iface eth0 inet dhcp

allow-hotplug wlan0
iface wlan0 inet manual
wpa-roam /etc/wpa_supplicant/wpa_supplicant.conf

iface YOUR_WIFI_ID inet static
address 192.168.1.5
netmask 255.255.255.0
gateway 192.168.1.1

# iface default inet dhcp
```

This is setting up your ethernet to dhcp, your wifi to manual, sourcing the `wpa_supplicant.conf` file, and then assigning your named Wireless connection to a static IP. Note that *YOUR_WIFI_ID* above should be an identifier you make up yourself. For example, mine is *NORTH_WPA*.

Now, configure the network information in `/etc/wpa_supplicant/wpa_supplicant.conf`:

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
# update_config=1
network={
	ssid="Your SSID Name"
	proto=RSN
	key_mgmt=WPA-PSK
	pairwise=CCMP TKIP
	group=CCMP TKIP
	psk="Your WPA Password"
	id_str="YOUR_WIFI_ID"
}
```

Again, note that *YOUR_WIFI_ID* is present.

At that point, reboot. Assuming you've got all your information correct (and that your key management and etc. match mine) you'll be connected automatically at boot!

Much of the information in this post was gleaned from [this thread](http://pingbin.com/2012/12/setup-wifi-raspberry-pi/). The original poster's information didn't work for me, but I was able to assemble a working config from various commenters' information.

#### UPDATE 2014-01-06

When I switched from the older-style Airport Extreme base stations to the new "tower"-shaped base stations, I was having issues with the WiFi disconnected and then not reconnecting without a reboot. To resolve this, I followed one of the answers in [this stack exchange post](http://raspberrypi.stackexchange.com/questions/4120/how-to-automatically-reconnect-wifi), reproduced below for convenience. This is originally from the user [AndaluZ](http://raspberrypi.stackexchange.com/users/6365/andaluz): 

> Well, there is a very simple solution:
> 
> 1. Go to `/etc/ifplugd/action.d/` and rename the `ifupdown` file to `ifupdown.original`
> 2. Then do: `cp /etc/wpa_supplicant/ifupdown.sh ./ifupdown`
> 3. Finally: `sudo reboot`
> 
> That's all. Test this by turning off/on your AP; you should see that your Raspberry Pi properly reconnects.

