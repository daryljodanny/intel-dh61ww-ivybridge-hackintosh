# Intel DH61WW IvyBridge Hackintosh

This build is based on the vanilla installation guide found [here](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/). It requires a computer with macOS installed (either a virtual machine or a legit mac).
If you do not have a mac at hand, you can follow [this](https://internet-install.gitbook.io/macos-internet-install/) guide.
![About this Mac](https://imgur.com/3m5JwZR.png)
![Neofetch](https://imgur.com/33nx527.png)

## Hardware
* CPU - Intel Core i5 3330 (IvyBridge)
* Mobo - Intel DH61WW
	* LAN - Intel 82579V Gigabit Ethernet
	* Audio - ALC892
	* RAM - 2x Corsair Vengeance 4GB DDR3 1333MHz
* GPU - Sapphire RX580 Pulse 8GB DDR5
* SSD - AData SU650 240GB

### Working
* Audio
* Ethernet
* APFS
* Hardware Acceleration (offloaded to RX580. Haven't done any indepth testing though)
* Sleep / Wake
* AppStore
* iCloud

### Not working / Haven't tested
* DRM (Trailers work in TV.app. But while playing an actual episode the screen will be red).
* iMessage - Haven't tested (I don't use it) 
* AirDrop / HandOff - Haven't tested (No WiFi/Bluetooth card installed. Will work once a compatible card is installed)

## BIOS config
* Flash the latest BIOS
* Load Optimised Defaults
* Disable Legacy Boot
* Disable Secure Boot
* Disable iGPU (if your iGPU is incompatible under macOS)

## Drivers used
* ApfsDriverLoader
* FwRuntimeServices
* HfsPlus
* OcQuirks
* SMCHelper

## Kexts used
* Lilu
* WhateverGreen
* IntelMausi
* USBInjectAll
* FakeSMC
* AppleALC

## Tools used
* [Dids' Clover](https://github.com/Dids/clover-builder/releases)
* [gibMacOS](https://github.com/corpnewt/gibMacOS) - Python script for downloading macOS installers from the apple servers.
* [Clover Configurator](https://mackie100projects.altervista.org/download-clover-configurator/) - Tool used for editing config.plist.
* [USBMap](https://github.com/corpnewt/USBMap) - Python script for mapping out the USB ports post installation
* [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) - Python script for generating Serial Number, SmUUID, and MLB based on the given SMBIOS.
* [ssdtPRGen](https://github.com/Piker-Alpha/ssdtPRGen.sh) - Script for generating SSDT.aml which contains the required power management data for the CPU 

## Notes
* Always use the latest version of Clover (r5099 as of writing).
* **DO NOT** copy-paste this config.plist entirely. Use this only for reference purpose.
* While setting up Clover, use `OcQuirks` and `FwRuntimeServices` instead of `AptioMemoryFix`.
* Since the motherboard I'm using is 6-series and the CPU is Ivy Bridge, I've faked IMEI in the config.plist as per u/corpnewt guide. If both CPU and motherboard is based on IvyBridge, ignore it `config.plist -> Devices -> FakeID`.
* Since my iGPU (HD2500) is incompatible in macOS, I've disabled it under the BIOS and have ignored the framebuffer patching under config.plist -> Devices -> Properties. I have offloaded both H.264 and HEVC encoding to the GPU by using the boot args `shikigva=32 shiki-id=Mac-7BA5B2D9E42DDD94`.
	* shikigva=32 -> Changes the board ID.
	* shiki-id=Mac-7BA5B2D9E42DDD94 -> Uses the board id of iMacPro1,1.  
* If you're using a CPU with a compatible iGPU (HD4000) you can enable it under the BIOS and add the necessary framebuffer patches in `config.plist -> Devices -> Properties`. This way you can make use of Intel QuickSync. Remove shiki boot flags if you prefer to use both iGPU and dGPU.
* The config.plist provided here will not have any Serial Number, SmUUID, and MLB. It should be filled in using the GenSMBIOS (link above). Use either `iMac13,1` or `iMac13,2` SMBIOS.
* Generate SSDT.aml using ssdtPRGen (link above) and place it in `EFI -> Clover -> ACPI -> patched` after successful installation of macOS
* Map the USB ports using USBMap (link above) after successful installation of macOS. Verify that the USB power is also working as intended (can be identified using an option in USBMap script). After mapping the ports, remove the 'Port Limit Increase' patch from `config.plist -> KernelAndKextPatches -> KextsToPatch`.
* Make sure the serial number generated by GenSMBIOS is **INVALID** by using the 'Check Coverage' button under Clover Configurator. If the serial number is **VALID**, **GENERATE** another set of numbers using the script.
* If you're using a SATA SSD, enable TRIM. Otherwise APFS corruption might occur and you'll have to reinstall macOS again. You can check if TRIM is enabled by opening - About this Mac -> System Report -> SATA/SATA Express -> Your SSD -> Trim Support.  
If it's not enabled, enable it by running `sudo trimforce enable` under Terminal. I've enabled TRIM via config.plist so it should work.

## Credits
* Thanks [u/corpnewt](https://github.com/corpnewt) for the guides and tools for making it easier to NOOBS like us.
* Thanks to [r/hackintosh](https://www.reddit.com/r/hackintosh/) for the support
* Thanks to [HackintoshParadise Discord](https://discord.gg/u8V7N5C) for the support
* Thanks to the various developers who has contributed.
