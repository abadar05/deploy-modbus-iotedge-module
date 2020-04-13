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
