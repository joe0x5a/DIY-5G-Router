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





## Configure OpenWrt

## Configure Wireguard

## Force Android USB tethering 
