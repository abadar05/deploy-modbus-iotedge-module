Connecting modbus devices to azure iot hub

# Install ThingsPro Edge v2.0.0

## Default credential and IP address
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
## Download and Install ThingsPro without Internet
Click on the url below which start downloading thingspro software, it takes 5~10 minutes depending the network speed of your internet connection. 

After the download is completed, copy .deb package file on the UC gateway via FTP

> Hint: On Windows10 use WinSCP tool. It is free tool available on the url below. 
 ```sh
https://winscp.net/eng/download.php
 ```
 - armhf:
     ```sh
     dpkg -i ./update_2.0.0-1424-uc-8112a-me-iotedge_armhf.deb
     ```

## Update the Installation Process
```sh
journalctl -u update -f
```

## Track Installation Progress
   ![](Media/track-installation-progress.png)

> Note: The entire process will take about ~11 minutes depends on the hardware/model. When it shows "Stopped MOXA ThingsPro Updater", you can reboot the device by command listed below

## Reboot Device
```sh
sudo reboot
```

# Deploy Module from Azure Portal

## thingspro-agent
- Select your iotedge device from the list
 ![](Media/select-iotedge-device.png)

- Set Modules
 ![](Media/set-modules.png)
 
 - IoT Edge Module
 ![](Media/iotedge-module.png)
 
 - IoT Edge Module Name & Image URI
   ![](Media/thingspro-agent-module.png)
   - Image URI:
       - armhf:
            ```
            moxa2019/thingspro-agent:2.0.0-528-armhf 
            ```
- Container Create Options 
   ![](Media/thingspro-agent-container-create-options.png)
    - Container Create Options:
    ```
    {
         "HostConfig": {
             "Binds": [
                 "/var/thingspro/apps/cloud/data/setting/:/var/thingspro/cloudsetting/",
                 "/run/:/host/run/",
                 "/var/thingspro/data/:/var/thingspro/data/"
              ]
          }
    }
    ```
## Modbus

- IoT Edge Module Name & Image URI
   ![](Media/modbus-module.png)
   - Image URI:
              ```
              mcr.microsoft.com/azureiotedge/modbus:1.0 
              ```
            
- Module Twin Settings
   ![](Media/modbus-module-twin-settings.png)
    - Module Twin Settings:
    ```
    {
          "PublishInterval": 1000,
          "SlaveConfigs": {
              "Slave01": {
                  "SlaveConnection": "192.168.2.200",
                   "HwId": "GasTank-34:48:ED:98:9B:7C",
                   "Operations": {
                       "Op01": {
                           "PollingInterval": "1000",
                           "UnitId": "1",
                           "StartAddress": "400001",
                           "Count": "1",
                           "DisplayName": "Temp"
                       }
                  }
             }
        }
   }
   ```

- Routes
    ![](Media/routes.png)
    - NAME:
        ```
        route
        ```
    - VALUE:
        ```
        "FROM /messages/* INTO $upstream"
        ```
        
   - NAME:
        ```
        modbusToIoTHub
        ```
    - VALUE:
        ```
        "FROM /messages/modules/Modbus/outputs/modbusOutput INTO $upstream"
        ```
        
- Review + create
   ![](Media/review-create.png)
  
 ## Modules Runtime Status
   ![](Media/modules-runtime-status.png)
   
 > Note: After successfull deployment the iotedge device pull the module images specified in the deployment manifest and start the conatiner from the image. This takes 4~5 minutes depending on the network speed. The iot edge device sends the status updates via reported properties to azure iot hub.  
    
# Verify Modules on UC-8112A IIoT Gateway

## Connect the gateway via Local Web GUI 

Default credentials 
  - username/password:  admin/admin@123

![](Media/local-webgui.png)

## Azure IoT Edge
  ![](Media/tpe-aie-modulelist.png)
