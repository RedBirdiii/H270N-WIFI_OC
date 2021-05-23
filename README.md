# Gigabyte H270N-WIFI - Big Sur 11.3.1, OpenCore 0.6.9

CPU: Intel Core i3-7100; Kaby Lake

GPU: Intel HD Graphics 630

RAM: 8GB 2133MHz DDR4

Motherboard Model: Gigabyte GA-H270N-WIFI

BIOS revision: F8d

Motherboard Model Website: https://www.gigabyte.com/Motherboard/GA-H270N-WIFI-rev-10

Audio Codec: Realtek ALC1220

Ethernet Card: Intel Ethernet Dual GbE LAN I219-V (only one of the two ports works, the one beside the USB-C port doesn’t work)

Wifi/BT Card: Intel Wireless 8265/ 8275 (comes with the MB)

Other installed OSs: Windows 10 21H1, Ubuntu 20.04.2

Guides used:

Dortania’s OpenCore Install Guide.

What's working:

- iGPU with full acceleration.

- Sound.

- Ethernet

- Wifi and bluetooth. (I’m using the stock builtin card that comes with the motherboard)

- AirPort, AirDrop

- iMessages, Facetime

- All USB ports; USB3, USB-C, USB2

What isn’t working:

- Sleep

    Notes:

    Troubleshooting iGPU with full acceleration

I followed the Dortania’s guide to the letter, then checked the USB I made by that guide. Everything went great except when it reaches the recovery setup ‘language choice’ screen, the screen goes black. I found that the SMBIOS is the problem. iMac18,1 caused me troubles.

I changed that to MacPro6,1 and the setup goes very smooth.

In OpenCore config.plist, in DeviceProperties section, I added this:

    PciRoot(0x0)/Pci(0x2,0x0)
    layout-id                 Data         <00001259>

Fixing the ethernet connection

After finishing installing Big Sur 11.3.1, I found that I don’t have internet connection. After trying the other port, the internet is on. The motherboard has two ports, only one port works. The one that is beside the USB-C is not working. The kext IntelMausi.kext is used for the ethernet connection Intel Ethernet I219-V.

Fixing sound:

By following the guide of fixing sound, I got sound. I use the green aux output, and I choose Internal Speaker as my output device in Sound Preferences.

In OpenCore config.plist, in DeviceProperties section, I added this:
    
    PciRoot(0x0)/Pci(0x1F,0x3)
    layout-id                 Data         <21>

Wifi and Bluetooth:

I only needed to add three kexts files in the Kexts folder.

    AirportItlwm.kext, IntelBluetoothFirmware.kext and IntelBluetoothInjector.kext

Sidenote: Ubuntu grub menu

When I put the driver file CrScreenshotDxe.efi in Drivers folder of OpenCore to screenshots of the OS picker, keyboard keys are disabled at the grub menu of Ubuntu. I removed the file and got the grub menu working normally.

USB Mapping

I used Hackintool to map my USB ports. Then added SSDT-EC-USBX.aml and SSDT-UIAC.aml in ACPI folder. Also USBPorts.kext.

boot args:

    keepsyms=1 debug=0x100 alcid=21
