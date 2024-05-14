# Gigabyte H270N-WIFI 
# macOS Sonoma 14.5, OpenCore 1.0.0

[Remember to fill SMBIOS info with GenSMBIOS]


CPU: Intel Core i3-7100; Kaby Lake

GPU: Intel HD Graphics 630

RAM: 16GB 2133MHz DDR4

Motherboard Model: Gigabyte GA-H270N-WIFI

BIOS revision: F8d

Motherboard Model Website: https://www.gigabyte.com/Motherboard/GA-H270N-WIFI-rev-10

Audio Codec: Realtek ALC1220

Ethernet Card: Intel Ethernet Dual GbE LAN I219-V (both ports works)

Wifi/BT Card: Intel Wireless 8265/ 8275 (comes with the MB)

Other installed OSs: Windows 11 23H2, Ubuntu 24.04 LTS

Guides used:

Dortania’s OpenCore Install Guide.

What's working:

- iGPU with full acceleration.

- Sound.

- Ethernet

- Wifi and bluetooth. (I’m using the stock builtin card that comes with the motherboard)

- AirPort, AirDrop

- iMessages, Facetime

- All USB ports; USB3, USB-C, USB2 (You'll need Hackintool to assign USB ports properly)

What isn’t working:

- DRM in Apple TV application. (Only trailers works fine)
- Sleep
      - To avoid problems with this issue, disable all switches in Energy Saver section of Settings
      - In Lock Screen, set 'Turn display off when inactive' to 'Never'

Notes:

Troubleshooting iGPU with full acceleration

I set SMBIOS to iMacPro1,1.

In OpenCore config.plist, in DeviceProperties section, I added this:

    PciRoot(0x0)/Pci(0x2,0x0)
    layout-id                 Data         <00001259>

Fixing the ethernet connection

Both ethernet ports work fine, the main port needs the kext IntelMausi.kext the other one uses Intel Ethernet I219-V kext.

Fixing sound:

By following the guide of fixing sound, I got sound. I use the green aux output, and I choose Internal Speaker as my output device in Sound Preferences.

In OpenCore config.plist, in DeviceProperties section, I added this:
    
    PciRoot(0x0)/Pci(0x1F,0x3)
    layout-id                 Data         <21>

Wifi and Bluetooth:

I only needed to add three kexts files in the Kexts folder.

    AirportItlwm.kext, IntelBluetoothFirmware.kext and IntelBluetoothInjector.kext

USB Mapping

I used Hackintool to map my USB ports. Then added SSDT-EC-USBX.aml and SSDT-UIAC.aml in ACPI folder. Also USBPorts.kext (which is created by Hackintool).

boot args:

    keepsyms=1 debug=0x100 alcid=21



[Remember to fill SMBIOS info]
