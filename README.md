Connecting modbus devices to azure iot hub

# Install ThingsPro Edge v2.0.0

## Default settings 
  - username/password:  moxa/moxa
  - IP LAN1 192.168.3.127 
  - IP LAN2 192.168.4.127

## Set to Default

Reset to factory default

If the unit has been installed prior, we should reset it back to default before installing ThingsPro Edge. The commands listed below is for `armhf` and UC-8100A series gateway

```sh
mx-set-def
```

> Note: Make sure to have a console cable conected to the device or login via ssh

Remove docker folder
```sh
rm -rf /overlayfs/docker /overlayfs/working/docker
```

> Note: This will wipe out all the data on the device!

## Configure Network
```sh
dhclient eth0
```
> Note: Make sure there is a dhcp server on LAN1

## Download and Install ThingsPro with Internet 
- armhf:
    ```sh
    wget https://thingspro.blob.core.windows.net/software/edge/V2.0.0/update_2.0.0-1424-uc-8112a-me-iotedge_armhf.deb
    dpkg -i ./update_2.0.0-1424-uc-8112a-me-iotedge_armhf.deb
    ```
## Alternatively Download and Install ThingsPro without Internet
Click on the url below teh package will be download on your PC
Copy package file (update_2.0.0-1424-uc-8112a-me-iotedge_armhf.deb) on UC gateway via FTP

- Hint: On Windows10 use WinSCP tool to transfer file from PC to UC gateway
 ```sh
https://winscp.net/eng/download.php
 ```

## Track Installation Progress
```sh
journalctl -u update -f
