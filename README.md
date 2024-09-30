# Gigabyte H270N-WIFI 
# macOS Sonoma 14.6.1, OpenCore 1.0.1

> [!NOTE]
> Remember to fill SMBIOS info with GenSMBIOS

**CPU:** Intel Core i3-7100; Kaby Lake

**GPU:** Intel HD Graphics 630

**RAM:** 16GB 2133MHz DDR4

**Motherboard Model:** Gigabyte GA-H270N-WIFI

**BIOS revision:** F8d

**Motherboard Model Website:** https://www.gigabyte.com/Motherboard/GA-H270N-WIFI-rev-10

**Audio Codec:** Realtek ALC1220

**Ethernet Card:** Intel Ethernet Dual GbE LAN I219-V (both ports works)

**Wifi/BT Card:** Intel Wireless 8265/ 8275 (MB stock card)

**Other installed OSs:** Windows 11 23H2, Ubuntu 24.04 LTS

## Guides used:

Dortania’s OpenCore Install Guide.

## What's working:

- iGPU with full acceleration.

- Sound.

- Ethernet

- Wifi and bluetooth. (I’m using the stock builtin card that comes with the motherboard)

- --AirPort, AirDrop--

- iMessages, Facetime

- All USB ports; USB3, USB-C, USB2 (You'll need Hackintool to assign USB ports properly)

## What isn’t working:

- DRM in Apple TV application. (Only trailers works fine)
- Sleep
      - To avoid problems with this issue, disable all switches in Energy Saver section of Settings
      - In Lock Screen, set 'Turn display off when inactive' to 'Never'

## Notes:

### SMBIOS info:
I set SMBIOS to iMacPro1,1.

### Intel HD Graphics 630

In OpenCore config.plist, in DeviceProperties section, I added this:

    PciRoot(0x0)/Pci(0x2,0x0)
    layout-id                 Data         <00001259>

### Fixing the ethernet connection

Both ethernet ports work fine, the main port needs the kext `IntelMausi.kext` the other one uses `SmallTree-Intel-211-AT-PCIe-GBE.kext`.

### Fixing sound:

By following the guide of fixing sound, I got sound. I use the green aux output, and I choose Internal Speaker as my output device in Sound Preferences.

In OpenCore `config.plist`, in `DeviceProperties` section, I added this:
    
    PciRoot(0x0)/Pci(0x1F,0x3)
    layout-id                 Number         <21>

### Wifi and Bluetooth:

I only needed to add three kexts files in the Kexts folder.

    AirportItlwm.kext, IntelBluetoothFirmware.kext and BlueToolFixup.kext

### USB Mapping

I used Hackintool to map my USB ports. Then added SSDT-EC-USBX.aml and SSDT-UIAC.aml in ACPI folder. Also USBPorts.kext (which is created by Hackintool).



### SMBIOS info:

You'll need to use GenSMBIOS to get your SMBIOS information.


## Getting the required files

**ACPI files:** Check SSDTs section in Dortania guide.

**Kexts:** Mark a note of the Kexts mentioned in the `Kernel - Add` section of config.plist file.
       I've added `RestrictedEvents.kext` to have access to the new os updates, but keep in mind you'll need to set `SecureBootModel` to `Disabled` to install them.

**Picker Variant:** Check the guide **'OpenCore beauty treatment'** in **Dortania** page.

**Tools and Drivers:** These are taken from OpenCorePKG package.

## Installing Sonoma 14.6.1

> [!NOTE]
> There is a fix to get the updates listed, this is how:
> 
> You'll need the latest `RestrictedEvents.kext` in **Kexts** folder.
> 
> Also in `boot-args` you'll need to add `revpatch=sbvmm`

You'll need to set `SecureBootModel` to `Disabled` during the installtion process. You can change it back to `Default`, after the installtion process is complete.
You have to make sure that you are using `AirportItlwm.kext` that is made for Sonoma 14.4 or later.
