
# Lenovo T490s macOS with OpenCore (Derived From yusifsalam/t490-macos)

### Status: WIP

This repo contains information for getting macOS 10.15.{6,7} Catalina working on a Lenovo **Thinkpad T490s** laptop.

I credit all my works to **yusifsalam/t490-macos** because most of the hacking is leveraged from t490-macos. 
So I am going to state a few important differences between t490 and my t490s.


Currently running:

| Component      | Version           |
| -------------- | ----------------- |
| macOS Catalina | 10.15.7 (19H2)    |
| OpenCore       | 0.6.1             |
| BIOS version   | 1.65              |
| ECP version    | 1.17              |

Note:  t490s's BIOS and ECP (and their versions) are very similar, but T490s is not exactly same as T490.
Luckily, BIOS and ECP version doesn't affect my catalina installation in T490s.

## Hardware info

| Component | Model                                     |
| --------- | ------------------------------------------|
| CPU       | i7-8565U Whiskey Lake                     |
| Memory    | 16GB/32GB 2400Mhz                         |
| Storage   | **WDC PC SN730 512GB**                    |
| Display   | 14" non-touch 1920x1080                   |
| GPU       | Intel UHD 620                             |
| Camera    | 720p (**without IR sensor**)              |
| WLAN      | Intel Wireless-AC 9560 2x2ac with BT5.0   |
| Battery   | Single 3-cell 57Wh                        |
| Touchpad  | ~~Synaptics TM3471-010~~**Elan Touchpad** |



## Status

### Working

- [x] Keyboard. Volume and brightness control keys;   **Also, added F11 key to bring up expose**
- [x] Battery indicator
- [x] Display auto brightness
- [x] Audio
- [x] Ethernet
- [x] iCloud services - iMessage, FaceTime, AppStore
- [x] GPU acceleration.  **My framebuffer is based on ig-platform-id: 0x90003EA5.  Please see additional remark on this**
- [x] Camera
- [x] Microphone
- [x] Bluetooth
- [x] Mac-like booting interface for multiboot **I am using "refind" as bookpicker instead of opencanopy since I am not sure opencanopy can shield multiboot windows and ubuntu.
- [x] Sleep/wake. **
- [x] Trackpad and gestures **Elan Touchpad without gesture was headache so far. Now dev version of voodooPS2 supports gesture in ELAN!!** 
- [x] Native CPU power management
- [x] HDMI video and audio up to 1440p **No monitor for this yet.   Not sure, but will check soon**
- [x] Handoff, continuity. **didn't check yetr**
- [x] AirPlay **didn't check yet**
- [x] FileVault  **didn't check yet**
- [x] DRM content playback (Netflix, Apple TV+). **didn't check yet**
- [x] Thunderbolt - works with Lenovo Thinkpad Thunderbolt 3 Dock (tested Ethernet, display over DisplayPort and HDMI, USB ports). **need to check this extensively**

Note:  I enabled the HDMI port instead of type-c port near LCD.   WIP, but it looks very promising.  Exact display configuration is:
- [x] Thinkpad LCD(LVDS)
- [ ] Type-c port(No Display) will act as power input and USB, but no DP display!
- [x] Thunderbolt 3 port(DP)  but I think it only acts as a usb port; no TB configuration yet.
- [x] HDMI port (HDMI) On-board HDMI is more convenient for me 

There are some issues related to this.  
- HDMI port has to be reconnected when reboot.  
- When DP-to-HDMI is used to connect your HDMI monitor with speaker, it won't stay on.  Quick fix is that make it sleep and wake. After that, it **stay** working.  (I don't know why?)

### Working, sort of

- [ ] Wifi works but not at full speeds
- [ ] Audio jack - glitches after wake from sleep. I don't use it so won't fix but will merge pull requests
- [ ] USB-C video output works, but no audio

### Not working at the moment

- [ ] HDMI video at 4K
- [ ] SD card reader - I don't use it so won't fix but will merge pull requests
- [ ] AirDrop

### Not tested

- [ ] Sidecar

## Kexts

| Kext                   | Version | Remark                                   |
| ---------------------- | ------- | ---------------------------------------- |
| AirportItlwm           | 1.1.0   | Integrates Itlwm with native WiFi picker |
| AppleALC               | 1.5.0   | Fixes onboard audio                      |
| CPUFriend              | 1.2.2   | Power management                         |
| CPUFriendDataProvider  | -       | Frequency vector for CPUFriend           |
| IntelBluetoothFirmware | 1.1.2   | Fixes bluetooth                          |
| IntelBluetoothInjector | 1.1.2   | Companion for IntelBluetoothFirmware     |
| IntelMausiEthernet     | 2.5.1d1 | Fixes ethernet                           |
| itlwm                  | 1.1.0   | WiFi kext                                |
| Lilu                   | 1.4.8   | Kext patcher                             |
| NoTouchID              | 1.0.4   | Disable TouchID                          |
| NVMEFix                | 1.0.4   | Fix for NVME SSDs                        |
| SMCBatteryManager      | 1.1.7   | Battery indicator                        |
| SMCLightSensor         | 1.1.7   | Ambient light sensor                     |
| SMCProcessor           | 1.1.7   | CPU temp monitoring                      |
| SMCSuperIO             | 1.1.7   | Monitor fan speed, not working           |
| USBInjectAll           | 0.7.5   | Inject all USB, only for troubleshooting |
| USBMap                 | -       | Inject only mapped USB                   |
| VirtualSMC             | 1.1.7   | SMC chip emulation                       |
| ~~VoodooRMI              | 1.1     | Trackpad driver                        |
| VoodooSMBUS            | 3.0 dev | SMBUS driver~~                           |
| VoodooPS2Controller    | **2.1.8**| keyboard, Trackpoint + **Trackpad**     |
| WhateverGreen          | 1.4.3   | Graphics                                 |

## ACPI patches

| Patch                 | Remark                         |
| --------------------- | ------------------------------ |
| SSDT-ALS0             | Fix display auto brightness    |
| SSDT-AWAC             | Fix AWAC                       |
| SSDT-BAT              | Fix battery indicator          |
| SSDT-EXT1-FixShutdown | Fix shutdown on reboot         |
| SSDT-EXT3-LedReset-TP | Fix LED not working after wake |
| SSDT-EXT4-WakeScreen  | Fix screen not waking          |
| SSDT-GPIO             | Trackpad fix                   |
| SSDT-GPRW             | Fix immediate wake after sleep |
| SSDT-HPET             | Fix irq conflicts              |
| SSDT-PLUG             | x86 plugin injection fix       |
| SSDT-PNLF-CFL         | Backlight fix                  |
| SSDT-PTSWAK           | Fix sleep issues               |
| SSDT-SBUS-MCHC        | SBUS fix                       |
| SSDT-USBX             | USBX patch                     |
| **SSDT-KBRD**         | Volume, Brightness Up/Down etc |

## Pre-Install & BIOS settings

First, read the [Dortania OC guide](https://dortania.github.io/OpenCore-Install-Guide/). The guide will take you through the creation of installation USB and drive formatting. Update to the latest firmware (the easiest way is to use fwupd on linux).

Then do the following BIOS settings:

- Disable secure chip
- Enable Intel Virtualization and VT-d
- Disable secure boot and fast boot
- Disable Intel SGX control
- Disable Device Guard
- Disable wake on LAN/thunderbolt
- Set boot mode to UEFI only, disable CSM support

Now you can install macOS on your APFS or HFS+ formatted drive.

## Post-install

- [Fix iServices](https://dortania.github.io/OpenCore-Post-Install/universal/iservices.html#generate-a-new-serial) if you want to use iMessage or FaceTime.
- Disable hibernation, since it doesn't work properly on hackintoshes
- Make your own USB map kext
- Generate your own CPU frequency vectors using [CPUFriendFriend](https://github.com/corpnewt/CPUFriendFriend). The one included here is set to Balance power and CPU lowest frequency set to 500 MHz
- (Optional) [Rectangle](https://github.com/rxhanson/Rectangle) for window management
- (Optional) [LuLu](https://github.com/objective-see/LuLu) for network traffic control
- (Optional) [Monitor Control](https://github.com/MonitorControl/MonitorControl) to adjust brightness/contrast controls for internal+external displays (DP/USB-C/HDMI
- (Optional) [Karabiner-Elements](https://github.com/pqrs-org/Karabiner-Elements) to rebind key presses

## CREDITS

- [Acidanthera](https://github.com/acidanthera)
- [Dortania OC guide](https://dortania.github.io/OpenCore-Install-Guide/)
- [Rehabman's battery patch guide](https://www.tonymacx86.com/threads/guide-how-to-patch-dsdt-for-working-battery-status.116102/) and [Rehabman's ACPI hotpatching guide](https://www.tonymacx86.com/threads/guide-using-clover-to-hotpatch-acpi.200137/)
- [CorpNewt's tools](https://github.com/corpnewt)
- [OpenWireless and itlwm](https://github.com/OpenIntelWireless/itlwm)
- [VoodooRMI](https://github.com/VoodooSMBus/VoodooRMI)
- [Daliansky's OC-little repo](https://github.com/daliansky/OC-little)
- [Tyler Nguyen's x1c-hackintosh repo](https://github.com/tylernguyen/x1c6-hackintosh)
- [Vojtěch Jungmann's T480-OpenCore-Hackintosh repo](https://github.com/EETagent/T480-OpenCore-Hackintosh)
