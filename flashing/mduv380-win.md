# Windows - Flash OpenRTX to MD-UV380

This section describes how to flash OpenRTX onto the MD-UV380 series of hardware from Windows.

## Compatibility

MD-UV380 series radios running:

- Stock OEM firmware
- OpenGD77 firmware
- Existing install of OpenRTX firmware

## Firmware Upgrade Application

Refer to the hardware manufacturer for the recommended firmware upgrade utility.

For the purpose of this help file, Retevis technical support provided a FirmwareDownloadV3.04_EN.exedirect link to download *FirmwareDownloadV3.04_EN.exe*. 

For reference, md5 hash of file provided by Retevis:

```
md5sum -t FirmwareDownloadV3.04_EN.exe 
e561f018f1516c755741b15cdc1369fd  FirmwareDownloadV3.04_EN.exe
```

**Note**: Exercise caution when downloading anything from unknown sources.

# OpenRTX firmware

Building OpenRTX from source is outside the scope of this section. 

To download the latest release see [OpenRTX Github repo releases](https://github.com/OpenRTX/OpenRTX/releases) and select the 'openrtx_mduv380_v*x.x.x*.bin' file 

# Radio Upload/DFU mode

1. Turn off radio with power knob
2. Press and Hold the PTT button and the top button (above PTT) on the side of the radio
3. Turn on the radio with the power knob

The radio power LED (next to the Power knob) should slowly alternate between red & green to indicate the radio is in the proper mode for update.

The radio display is expected to be off and showing no information.

# Connect radio to PC

Using USB -> K1 connector adapter compatible with the radio, connect the radio to your PC.



# Firmware update process

## UpgradeDownload.exe - "DMR Download Software"

By default *FirmwareDownloadV3.04_EN.exe* provided by Retevis installs *DMR Download Software* in the file path *C:\Program Files (x86)\FirmwareDownloadV3.04\DMR Firmware DownLoad v3.04_En*.

### Software overview

The software is shown below.

![](../_media/uv380_flash_win_UpgradeDownloadSW_blank.jpg)

For flashing *OpenRTX*, only the User Program section is used.

### Execute firmware upgrade

Procedure for upgrade:

1. Ensure radio is in DFU mode using process and verifications above.
2. Click on *Open file upgrade' and select the firmware bin file compiled or downloaded previously.
3. Select *Download file of upgrade* as shown below.

![](../_media/uv380_flash_win_UpgradeDownloadSW_downloadFile.jpg)

## Troubleshooting

### DMR Download Software - Failed USB open

The error shown below could indicate an incorrect driver or incorrect radio mode.

![Error showing USB connection failure](../_media/uv380_flash_win_UpgradeDownloadSW_openfailed.jpg)

###  Windows Drivers & DFU Mode

Examples shown below of device manager showing radio in various modes.

#### Device Manager with radio in DFU/Update mode

The following image is the expected behavior to support firmware flashing.

![Windows Device Manager showing 'Digital Radio in USB mode' to indicate correct DFU/Update mode](../_media/uv380_flash_win_devicemanager_DFU.jpg)


#### Device Manager with radio in DFU/Update mode, incorrect driver

The following image shows the Windows Device Manager with the Radio connected using the wrong driver.

![Windows Device Manager showing 'Digital Radio in USB mode' to indicate incorrect DFU/Update driver](../_media/uv380_flash_win_devicemanager_incorrectDFU.jpg)

#### Device Manager with radio in normal operation mode

The follow image shows Windows Device Manager when the radio is in the operational mode and has correct drivers installed. This is the incorrect mode flashing firmware.

![Windows Device Manager showing 'USB Serial Device (COMx)'](../_media/uv380_flash_win_devicemanager_comport.jpg)

#### Device Manager with radio in normal operation mode, no device drivers

The following image shows Windows Device Manager when the radio firmware drivers have not been installed.

![Windows Device Manager showing 'Other Devices'->'STM32 Virtual ComPort in FS Mode' & 'Ports (COM & LPT)'->'USB Serial Device (COMx)' without drivers](../_media/uv380_flash_win_devicemanager_nodrivers.jpg)

Refer to Manufacturer's website for approved device drivers.