<h1 align="center">
  <br>ASUS N56JN Hackintosh<br>
</h1>

<div align="center">
   <img src="assets/about.png"> 
</div>


Configuration files, that allow me to run macOS on my laptop. Based on [OpenCore](https://github.com/acidanthera/OpenCorePkg).

- OpenCore [0.7.8](https://github.com/acidanthera/OpenCorePkg/releases/tag/0.8.4)
- macOS Monterey 12.6.7

---

### **¡Disclaimer!**

The files are meant for my personal reference. I've also modified my laptop firmware which can potentially cause the laptop to become unaccessible.
<span style="color:red"> You are free to use and modify these files, but do so at your own risk </span>
Just because this works for me doesn't guarantee it will work for you.

---

I own an _ASUS N56JN_, more specifically:
| Component | Description |
| --------: | :---------- |
| CPU | Intel Core i7 4700HQ (Haswell) |
| iGPU | Intel® HD Graphics 4600 |
| dGPU | Nvidia GeForce GT 840M |
| RAM | 16GB DDR3L 1600MHz |
| SSD | Samsung 850 Pro |
| ODD | populated with a HDD |
| WLAN & BT | Intel(R) Wireless 7260 |
| ETHERNET | RTL8168/8111 |
| AUDIO | ALC663 |

The subwoofer the laptop came with doesn't work and I haven't tried to make it work. I've replaced the optical drive with a hard drive to get more storage space. 
## What is working?

<span style="color:green;margin-left:1rem">&#9745;</span> Power management  
<span style="color:green;margin-left:1rem">&#9745;</span> iGPU[^1] (HDMI with audio[^2])  
<span style="color:green;margin-left:1rem">&#9745;</span> WiFi & Bluetooth  
<span style="color:green;margin-left:1rem">&#9745;</span> Battery read outs  
<span style="color:green;margin-left:1rem">&#9745;</span> Sleep  
<span style="color:green;margin-left:1rem">&#9745;</span> Audio[^3]  
<span style="color:green;margin-left:1rem">&#9745;</span> Touchpad with gestures  
<span style="color:green;margin-left:1rem">&#9745;</span> FN Keys  
<span style="color:green;margin-left:1rem">&#9745;</span> Keyboard backlight  
<span style="color:green;margin-left:1rem">&#9745;</span> Camera  
<span style="color:green;margin-left:1rem">&#9745;</span> USB3.0 Ports  
<span style="color:green;margin-left:1rem">&#9745;</span> Ethernet port  
<span style="color:green;margin-left:1rem">&#9745;</span> CFG register unlocked  
<span style="color:green;margin-left:1rem">&#9745;</span> NVRAM

<span style="color:red;margin-left:1rem">&#9746;</span> Nvidia dGPU (disabled as switchable graphics are not supported)  
<span style="color:red;margin-left:1rem">&#9746;</span> External subwoofer

<span style="color:orange;margin:0 .25rem 0 1.25rem">?</span> Card Reader ... _Detected as USB2.0-CRW, but not reading anything_  

- Booting with an external display plugged in turns of the internal display, so during boot I keep any display cables unplugged.

[^1]: Booting with an external display plugged in turns of the internal display, so during boot I keep any display cables unplugged.
[^2]: While I have seen it working, it is inconsisent. No idea why.
[^3]: ~~The audio jack sometimes produces static noise. A restart fixes the issue. ~~ I have broken the audio jack, need to resolder it.

## Starting from scratch? Here's how

1. [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/). Do not skip
   this, it is an amazing guide!
2. Clone this repo and modify config.plist slightly:
   - `AppleXcpmCfgLock` > Enable (I have it disabled because my CFG register is already unlocked)
   - Provide SMBIOS information ([GenSMBIOS](https://github.com/corpnewt/GenSMBIOS))
3. [Create a bootable usb](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/) and copy the EFI folder onto the EFI partition (It should be `/Volumes/EFI/EFI/OC/config.plist`).
4. Set these settings in BIOS:
   - **CSM** Enabled
   - **VT-d** Disabled
   - **SATA > SATA Mode Selection** AHCI
   - **Graphics > DVMT Pre-Allocated** 64M
5. Boot from USB and proceed to install. `HideAuxiliary` might be Enabled, so press Space during OpenCore boot picker to display all options. If USB isn't recognized check `ScanPolicy` in `config.plist` (set to 0 to allow discovering all drives). Make sure `HFSPlus.efi` driver is present.
6. After the installation copy the EFI folder from the USB partition to the drive.
   <h3>_Optional_:</h3>
7. [Fixing CFG Lock](https://dortania.github.io/OpenCore-Post-Install/misc/msr-lock.html)
8. I am yet to modify the firmware to replace CsmVideo with Hermit Crab Labs version to fix the resolution during first stages of boot

## Special Thanks

[**Apple**](http://apple.com/)

[**Dortania**](https://dortania.github.io/getting-started/)

[**Milad Tahanian**](https://github.com/mtahanian)
