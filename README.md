# DIY 5G Router Build
For a while I've been looking for a mobile router solution to replace my TP-Link M7650 MiFi. The MiFi has been great, but its:
- only 4g and I didnt acquire 5g capabilities with my covid jabs.
- only WiFi connectivity, no wired capability

I looked at what I could buy, but were either:
- looked like the perfect solution but beyond what I was prepared to pay
- only capable of 4G or didnt have Gigabit ethernet.

Some of the routers I looked at were based on OpenWrt. 
I have more Raspberry Pis than fingers and only 3 are in use, so it would nice to put another in to acive service, and not have to hide it from the wife :)
So I thought, could I build my own 5g router?
*Spoiler:  I wrote this while connected to  my DIY router. :)

## Kit List
- 8GB Raspberry Pi4.
- Micro SD Card
- Mobile Phone 
- USB Battery 
- Ethernet cable


## Flash SD Card with OpenWrt
1) Go to `https://openwrt.org/toh/raspberry_pi_foundation/raspberry_pi#installation` and download the firmware for your Raspberry Pi.  Im using an 8GB Pi4 so downloaded:
```
https://downloads.openwrt.org/releases/21.02.3/targets/bcm27xx/bcm2711/openwrt-21.02.3-bcm27xx-bcm2711-rpi-4-squashfs-factory.img.gz
```

I used the Raspberry Pi Imager to image the Micro SD card

![image](https://user-images.githubusercontent.com/53142047/185110492-a9c4ff2f-e593-419c-aa79-117b38f42d5b.png)

2) For the Operating system, choose `Use Custom` and select the OpenWrt firmware.
![image](https://user-images.githubusercontent.com/53142047/185111932-c69b6eab-88e3-4456-bc91-f9d0d829726f.png)

3) Under Storage select the SD card to image.

![image](https://user-images.githubusercontent.com/53142047/185112494-19edbebc-2898-4e10-8b91-6da691d93e26.png)

4) Click the `Write` button.

5) Click `YES` to confirm you are about to write to the correct location.

![image](https://user-images.githubusercontent.com/53142047/185113070-a340ac91-37dc-4b82-b27a-8c9341ec7ed3.png)

6) When completed, click `CONTINUE` remove the SD Card and insert it in the Raspberry Pi.
![image](https://user-images.githubusercontent.com/53142047/185113760-b779b663-fd75-41b5-a802-87959fbbeab4.png)


## OpenWrt: Configure USB Tethering
1) Boot the Raspberry Pi and connect an ethernet cable from your computer to the Raspberry Pi.

2) The default IP address of OpenWrt is `192.168.1.1/24`

3) Go to your Ethernet adapter settings on your computer and configure a Static IP address of 192.168.1.2/24.
![image](https://user-images.githubusercontent.com/53142047/185200972-3ba9abb2-1d62-462b-ba78-b49698b9e438.png)

4) Ping the Pi to test IP Connectivity
![image](https://user-images.githubusercontent.com/53142047/185201340-5297f2d9-5047-4506-9da1-e212ea99fede.png)

5) In a web browser, go to `https://192.168.1.1`.  Ignore the cert warning for now and login as root with no password.
![image](https://user-images.githubusercontent.com/53142047/185201920-dae5c8a5-623f-4f7e-b538-306997c5d080.png)

6) The router will remind you to set a password.
![image](https://user-images.githubusercontent.com/53142047/185202069-3e98e55b-3414-4db1-803d-9aea8b03bbed.png)

7) To set a password go to  `System > Administration > Router Password`.
![image](https://user-images.githubusercontent.com/53142047/185202411-958fa7f5-178b-42b5-9f6a-09bc0487f6e9.png)

8) OpenWrt does not have internet access at the minute becuase it is only connection is direct to your computer. To update OpenWrt and install the modules required for USB tethering, we need a temporary connection to the Internet.  We can achieve this by connecting to a WiFi network. Go to  `Network > Wireless` For `radio0` click `Scan`.
![image](https://user-images.githubusercontent.com/53142047/185203426-962b4cff-e88a-4734-99ae-e468b0a0ba39.png)

A list of available Wireless networks will appear. Choose one you have access to.
![image](https://user-images.githubusercontent.com/53142047/185204384-ec30e068-a3a2-4d02-98b4-df0151f73379.png)
- Enter the Passphrase
- Set it to the  Public Internet Facing interface `WAN`
Click `Submit, then `Save and Apply`

9) Next. Configure an upstream DNS Server. Go to  `System > DHCP and DNS` and add the ip address of the  DNS server you want to use.
![image](https://user-images.githubusercontent.com/53142047/185206305-6e9a9adb-3f30-4ca7-864e-f5dd6f7c01eb.png)

Click `Save and Apply`.

10) The Raspberry Pi should now have access to the internet. This can be test by going to  `Network > Diagnostics` and test connectivity with ping/traceroute and nslookup.

![image](https://user-images.githubusercontent.com/53142047/185206998-6d52899f-1b09-46d4-9fb6-7936fe22df0d.png)

11) Go to `System > Software` and click the `Update List` button to refresh the  list of available software.

![image](https://user-images.githubusercontent.com/53142047/185207498-ac461110-5e47-4cb6-8eaf-a3de325ed5b5.png)

If it works, the status popup window for Package Manager should not have any errors.

![image](https://user-images.githubusercontent.com/53142047/185207756-850d224e-0fa9-4e1a-84dd-a488ef6ee37f.png)

Click `Dismiss`.

12) Select the `Installed` tab and use the `Filter` box to look for `usb-net`.  If any of the following are missing. Go to the `Available` Tab and install the missing packages.
```
kmod-usb-net
kmod-usb-net-cdc-eem
kmod-usb-net-cdc-ether
kmod-usb-net-rndis
```

13) When finished, the following USB packages should be installed.

![image](https://user-images.githubusercontent.com/53142047/185208986-e916a6e6-585c-4c81-ac5f-472c3e21e022.png)

14) Connect your phone to a USB3 port on the Rasberry Pi. 
The phone will detect it has been plugged in using USB.  Double tap the Alert Message on the Phone and chose USB Tethering.

15) Back in the OPenWrt Web Portal. Select `Network > Interfaces >  A16)dd new interface`

![image](https://user-images.githubusercontent.com/53142047/185210376-a1f77189-7b95-45d7-a0d4-150208b924d9.png)

16) In the Add new interface window. Give the interface a:
- name
- Set the  Protocol to `DHCP Cient`. The Mobile Phone will give out a DHCP address
- Select the  Device as `usb0`
Click `Create Interface`

17) From the list of interfaces, select `edit` for the  new USB0  interface. Select the `Firewall Settings` tab and assign it to the `WAN` Zone.

![image](https://user-images.githubusercontent.com/53142047/185211677-209319db-6917-4f8d-b65f-46fa7504f4e9.png)

The Interface settings should now look like:

![image](https://user-images.githubusercontent.com/53142047/185212438-61daba0f-fec1-4e20-995a-87a3c22bbb02.png)


Click `Save`, then `Save and Apply`.

18) Once the settings have been applied.  The `Phone/usb0` interface should come up and be assigned and IP address.

![image](https://user-images.githubusercontent.com/53142047/185212819-153e941e-b6f7-475b-9237-f7009fc098a3.png)

19) The temporary WWAN interface, is now no longer required. Edit the settings and untick the `Bring up on Boot` option.

![image](https://user-images.githubusercontent.com/53142047/185213149-d42547cc-95a8-4b89-b130-4c184deb5174.png)

Click `Save`. Then `Save & Apply`

The `WWAN` interface can now be stopped, by pressing the `Stop` button

![image](https://user-images.githubusercontent.com/53142047/185213503-d8795677-0705-4f29-8414-4607c5ad4dd4.png)

20) Go back to the Diagnostic Page and test Network connectivity to the Internet

![image](https://user-images.githubusercontent.com/53142047/185214200-fa1fb6a7-f12e-45d2-9284-b070770d03e1.png)

If all is good, reboot the router by going to  `System > Reboot`.

![image](https://user-images.githubusercontent.com/53142047/185214451-7a2fea45-fcf4-45b9-abae-cfee0983ba6e.png)

Then click `Perform Reboot`

![image](https://user-images.githubusercontent.com/53142047/185214601-a52e90bd-708e-4a2f-9a6c-8188fa7ae324.png)

21) When the Rasberry Pi reboots the USB interface will go down and you will have to  chose USB tethering on the phone.

In the next section we will set Android to always chose USB tethering when USB is connected so the usb0 interface comes up with having to manually set USB Tethering.

But before going any further.... .take a backup.

22) Go to `System > Backup/Flash Firmware.

![image](https://user-images.githubusercontent.com/53142047/185215565-e149a3cd-bab6-45d1-984e-1b89c4543a44.png)

From the `Flash OPerations` page. Select `Generate Archive` 

![image](https://user-images.githubusercontent.com/53142047/185215965-b0521154-53f5-4629-9752-6bc3df7a0346.png)


## Default to USB Tethering 
Mileage might vary on this, depending on the version of Adroid you are using, but on my phone I could access the `Developer Options` and  force the phone to always use USB Tethering when a USB cable is connected.

To Access the Developer Options
1) Go to `Setting`

2) Select `About Phone`

3) Scroll to the bottom and tap the `Build Number` seven times.

4) In Settings, search for USB and look for the `Default USB Configuration` option. 

5) Select the  Default USB Configuration to be  USB Tethering.

This should have forced the phone to always chose USB Tethering.

At this point in the configuration you  should have an OpenWrt router which will work via the Ethernet interface of the  Raspberry Pi.  To  have more than one wire device  connected to the OpenWrt router, connect a switch to the Raspberry Pi ethernet interface.

## Configure WiFi Access Point

## Configure Wireguard
