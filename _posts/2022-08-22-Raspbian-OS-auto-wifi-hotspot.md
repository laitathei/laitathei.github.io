---
title: 'Raspbian OS auto wifi hotspot'
date: 2022-08-22
author: Lai Tat Hei
permalink: /posts/2022/08/Raspbian-OS-auto-wifi-hotspot/
excerpt: 'This guide is followed https://www.raspberrypi.com/documentation/computers/configuration.html#setting-up-a-routed-wireless-access-point'
tags:
  - Raspberry Pi
---

Step 1<br/>
```sudo apt install hostapd```<br/>
<br/><img src='/images/Raspbian_OS_auto_wifi_hotspot_guide/step1.PNG'><br/>

Step 2<br/>
```sudo systemctl unmask hostapd```<br/>
<br/><img src='/images/Raspbian_OS_auto_wifi_hotspot_guide/step2.PNG'><br/>

Step 3<br/>
```sudo systemctl enable hostapd```<br/>
<br/><img src='/images/Raspbian_OS_auto_wifi_hotspot_guide/step3.PNG'><br/>

Step 4<br/>
```sudo apt install dnsmasq```<br/>
<br/><img src='/images/Raspbian_OS_auto_wifi_hotspot_guide/step4.PNG'><br/>

Step 5<br/>
```sudo DEBIAN_FRONTEND=noninteractive apt install -y netfilter-persistent iptables-persistent```<br/>
<br/><img src='/images/Raspbian_OS_auto_wifi_hotspot_guide/step5.PNG'><br/>

Step 6<br/>
```sudo nano /etc/dhcpcd.conf```<br/>
<br/><img src='/images/Raspbian_OS_auto_wifi_hotspot_guide/step6.PNG'><br/>

Step 7<br/>
Add to the end of the file
```
interface wlan0
    static ip_address=192.168.4.1/24
    nohook wpa_supplicant
```
<br/>
<br/><img src='/images/Raspbian_OS_auto_wifi_hotspot_guide/step7.PNG'><br/>

Step 8<br/>
```sudo nano /etc/sysctl.d/routed-ap.conf```<br/>
<br/><img src='/images/Raspbian_OS_auto_wifi_hotspot_guide/step8.PNG'><br/>

Step 9<br/>
```
# Enable IPv4 routing
net.ipv4.ip_forward=1
```
<br/>
<br/><img src='/images/Raspbian_OS_auto_wifi_hotspot_guide/step9.PNG'><br/>

Step 10<br/>
```sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE```<br/>
<br/><img src='/images/Raspbian_OS_auto_wifi_hotspot_guide/step10.PNG'><br/>

Step 11<br/>
```sudo netfilter-persistent save```<br/>
<br/><img src='/images/Raspbian_OS_auto_wifi_hotspot_guide/step11.PNG'><br/>

Step 12<br/>
```sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.orig```<br/>
<br/><img src='/images/Raspbian_OS_auto_wifi_hotspot_guide/step12.PNG'><br/>

Step 13<br/>
```sudo nano /etc/dnsmasq.conf```<br/>
<br/><img src='/images/Raspbian_OS_auto_wifi_hotspot_guide/step13.PNG'><br/>

Step 14<br/>
Change the address as you want such as `192.168.5.1`, after change the address, you need to change dhcp-range as well
```
interface=wlan0 # Listening interface
dhcp-range=192.168.4.2,192.168.4.20,255.255.255.0,24h
                # Pool of IP addresses served via DHCP
domain=wlan     # Local wireless DNS domain
address=/gw.wlan/192.168.4.1
                # Alias for this router
```
<br/>
<br/><img src='/images/Raspbian_OS_auto_wifi_hotspot_guide/step14.PNG'><br/>

Step 15<br/>
```sudo rfkill unblock wlan```<br/>
<br/><img src='/images/Raspbian_OS_auto_wifi_hotspot_guide/step15.PNG'><br/>

Step 16<br/>
```sudo nano /etc/hostapd/hostapd.conf```<br/>
<br/><img src='/images/Raspbian_OS_auto_wifi_hotspot_guide/step16.PNG'><br/>

Step 17<br/>
ssid refers to the wifi name, you can change to your favourite
wpa_passphrase refers to wifi password, you can change to your favourite
```
country_code=GB
interface=wlan0
ssid=NameOfNetwork
hw_mode=g
channel=7
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_passphrase=AardvarkBadgerHedgehog
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
```
<br/>
<br/><img src='/images/Raspbian_OS_auto_wifi_hotspot_guide/step17.PNG'><br/>

Step 18<br/>
```sudo systemctl reboot```<br/>
<br/><img src='/images/Raspbian_OS_auto_wifi_hotspot_guide/step18.PNG'><br/>

Step 19<br/>
After reboot, browser the wifi and connect it with your own password<br/>
<br/><img src='/images/Raspbian_OS_auto_wifi_hotspot_guide/step19.PNG'><br/>
