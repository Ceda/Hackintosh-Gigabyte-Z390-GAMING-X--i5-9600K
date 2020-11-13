# Hackintosh Gigabyte GAMING X i9-9900k 5700XT

## Verified working with 10.15.7. and Big Sur 11.0.1 Final
## Update: Beta bios from Gigabyte resolves the Apple Watch unlock issue and provides the CFG Unlock in the bios!!!!!! CFG Unlock is required for this EFI to work properly. Unlock either with the BETA Bios or the method described below
## Update 2: I migrated to a 1TB Samsung NVME drive so I added the recommended NVMeFix.kext to fix power and energy consumption on these drives. You can disable the kext in the config.plist if you don't use NVME but only SATA drives.    

![](https://github.com/extric99/Hackintosh-Gigabyte-Z390-GAMING-X-i7-9900k-5700XT/blob/master/screenshot/Screenshot_Info.png)
![](https://github.com/extric99/Hackintosh-Gigabyte-Z390-GAMING-X-i7-9900k-5700XT/blob/master/screenshot/Screenshot_BigSur.png)

## Configuration
```
- Motherboard: Gigabyte Gaming X
- BIOS: f10g (BETA)
- CPU: i9-9900K  
- RAM: 4x 8GB Crucial Ballistix Sport LT BLS2K8G4D30BESBK 3000 MHz, DDR4
- Storage: Samsung 970 EVO M.2 1TB  
- dGPU: ASUS 5700XT (Reference)  
- WIFI/BT: FV-T919  
- SMIBIOS 19,1
- OpenCore 0.6.3
```
## Confirmed working
```
- Quick boot into MacOS and rock solid
- NVRAM if CFG Lock is disabled (see below)
- Fan and CPU temp information
- iMessage,Handoff and Approve & Unlock with Apple Watch
- Sleep and Wake from bluetooth mouse or keyboard
- iGPU Framebuffer for hardware acceleration (encoding/decoding/preview) including Apple TV DRM movies (shikivga=80) and SideCar
(note: Big Sur seems to break the support for Apple TV DRM, no issues on Catalina)
```
![](https://github.com/extric99/Hackintosh-Gigabyte-Z390-GAMING-X-i7-9900k-5700XT/blob/master/screenshot/Screenshot_temp.png)
![](https://github.com/extric99/Hackintosh-Gigabyte-Z390-GAMING-X-i7-9900k-5700XT/blob/master/screenshot/Screenshot_Hackintool_1.png)
![](https://github.com/extric99/Hackintosh-Gigabyte-Z390-GAMING-X-i7-9900k-5700XT/blob/master/screenshot/Screenshot%20Framebuffer.png)

- Improved OpenCL [here](https://browser.geekbench.com/v5/compute/1264374) and Metal performance [here](https://browser.geekbench.com/v5/compute/1264376) thanks to Radeon optimisations

## Known Issues
- Unlock with Apple Watch does not work with current stable bios due to enabled serial port. (use the Beta one or F7)
- Multiple key press to wake from sleep with bluetooth (known issue with Gigabyte Gaming X or M boards)


## Bios Setup:
```
- Make sure the IGP is set to Enabled for the Framebuffer to be recognized (Auto will not work)
- Disable CFG Lock 
- Disable Serial Port (Beta Bios only)
- Advanced Mode > Settings > Above 4G Decoding > Enabled
- Advanced Mode > Settings > USB Configuration > XHCI Hand-off > Enabled
- Advanced Mode > Boot > CSM Support > Disabled
```
NOTE: Bios has shared a BETA! BIOS that adresses the CFG Unlock, reverting to French issue and allows to disable the serial port which will make the Apple Watch Unlock work again. [>More info here<](https://www.tonymacx86.com/threads/ssdt-or-clover-patch-to-disable-super-i-o-serial-port-on-gigabyte-z390-m-gaming.287471/page-8). Important is to use a screen without USB/SD hub when configuring the bios for the first time otherwise you just have a black screen getting into the bios. Read the link for more info.
USE THIS AT YOUR OWN RISK!

## USB Setup:

Some ports have been disabled to stay below the 15 port limit
Ignore the Thunderbolt controller, this has been removed from this EFI.

![](https://github.com/extric99/Hackintosh-Gigabyte-Z390-GAMING-X-i7-9900k-5700XT/blob/master/screenshot/Screenshot_USB_Layout.png)

![](https://github.com/extric99/Hackintosh-Gigabyte-Z390-GAMING-X-i7-9900k-5700XT/blob/master/screenshot/Screenshot_USB.png)

## Post Installation

1. Setup Bios as per above
2. Open your config.plist, go to PlatformInfo -> Generic and populate the Serial, Board Serial (MLB), SystemUUID and ROM (ethernet MAC address in Base64 [more info](https://dortania.github.io/OpenCore-Post-Install/universal/iservices.html#fixing-en0)). Make sure to edit the config.plist only with ProperTree.
3. Go to System Preferences > Startup Disk and select your startup disk.
4. Disable CFG Lock and save profile in Bios (see below).
5. [Enable Trim](https://www.howtogeek.com/222077/how-to-enable-trim-for-third-party-ssds-on-mac-os-x/).
6. Done.

If you get stuck at OpenCore boot, try to clear nvram via OpenCore settings  

![](https://github.com/extric99/Hackintosh-Gigabyte-Z390-GAMING-X-i7-9900k-5700XT/blob/master/screenshot/Screenshot_MAC.png)

## Disable CFG Lock (only needed with the current stable version)

Disable CFG Lock (MSR 0x5C1) in BIOS via modified GRUB Shell, follow the guide [HERE] (https://www.tonymacx86.com/threads/gigabyte-z390-m-gaming-build-with-working-nvram.291193)

Credits pastrychef

Unlock MSR (NVRAM will not work with locked MSR) <-This value is ONLY for the Gigabyte Z390 M Gaming or Gaming X!!
1. Download the EFI Shell and unZip it.
2. Copy the EFI folder to the EFI partition of your USB flash drive. (EFI Agent can help with mounting of EFI partitions.)
3. Boot from the USB flash drive.
4. At the Grub prompt, enter:

setup_var_3 0x5C1 0x0

5. Reboot in to BIOS.
6. Save your BIOS settings profile to USB flash drive. If your BIOS ever resets itself, you won't have to manually unlock MSR again, just load your BIOS setting profile and MSR will be unlocked.
7. Done.
* Note/Warning: The MSR address is the same for every BIOS version for this motherboard, so far. This MSR address is only for this motherboard. If you are on another motherboard, you will need to find what your MSR address is.

## Tips
- User [EFI-shell] (https://www.tonymacx86.com/attachments/efi-shell-zip.448321)
- Use [Hackintool](http://headsoft.com.au/download/mac/Hackintool.zip) to validate correct implementation of Framebuffer and USB
- Use [Macinfo](https://github.com/acidanthera/MacInfoPkg) to generate your S/N
