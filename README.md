## Introduction
The Pintry X2 is a two bay NAS (Network Attached Storage) based on the Raspberry Pi Compute Module 4. It uses the ASMedia AS1061 PCIe to Sata bridge to connect up to 2 drives (3.5'/2.5' HDD or 2.5' SSD) to the SoM running a Linux based OS (Raspberry Pi OS).

## Website
Here is a link for additional information: https://www.cezarchirila.com/projects/pintry-x2.html

## Features
* Supports both CM4 and CM4 lite variants
* 2 x SATA ports with power using ASM1061. It should work with both 2.5' and 3.5' HDDs/SSDs but so far it has only been tested with 2.5' SSDs (Western Digital WDS500G2B0A)
* On/Off switch - Can both wake up the CM4 or shut it down (see Device Tree Overlay section) 
* 2 x Fan headers (PWM pin connected, Tach pin unconnected)
* 12V Input Power (max 5A, required for 3.5' drives). 
* USB 2.0 Host port for connecting USB devices and a microUSB Device port for updating the eMMC on CM4. 
* Expander header for access to various GPIO

## Names have a meaning
Pi + Pantry = Pintry

## Device Tree Overlay
Add these changes at the end of the /boot/config.txt to enable specific functionality:
```
# Enable FAN control for FAN0
dtoverlay=gpio-fan,gpiopin=12

# Enable FAN control for FAN1
#dtoverlay=gpio-fan,gpiopin=13

# Enable full functionality of On/Off button
dtoverlay=gpio-shutdown,gpio_pin=21,active_low=0,gpio_pull=off

# Enable UART - UNTESTED
#dtoverlay=uart0,txd0_pin=14,rxd0_pin=15,pin_func=4
#enable_uart=1  
```
Remember to remove the '#' in from of options you want to enable. 

## Kernel
To be able to use SATA drives with this you need to recompile the kernel with added SATA support. See instructions here. 

## Enclosure
The enclosure supports 2.5' drives only. It does not have any mounting features for fans, but there is room for a 60mm fan on the bottom as shown in the images below. It does not need supports for printing. Designed in Fusion 360.

For assembly, you will need the following:
* 4 x M3 brass inserts (D0.46mm, L0.57mm)
* 4 x M3x18mm socket screws - for mounting the drives 
* 4 x M3x25mm socket screws - for mounting the drives
* 4 x M3x35mm socket screws - for joining the two halves of the enclosure

## Thermal management
To keep temperatures under control, I have used a RAM heatsink on the ASM1061 IC and a 60mm fan on the bottom of the enclosure tied to the FAN header and controlled via the 'gpio-fan' overlay. It is mounted in such way that it blows air over both the CM4 and the ASM1061.  

## Release
The current latest release is V1.1. This version has not been tested or manufactured yet, therefore I would suggest double checking it if you plan on bulding it!
All the necessary manufacturing and assembly files are in the 'OutputJob/Version' and 'Enclosure' folders.
