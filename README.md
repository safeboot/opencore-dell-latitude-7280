# Dell Latitude 7280 Hackintosh
Hello! This is my personal EFI setup for OpenCore. Please note that your hardware may differ from mine so beware of some issues that you may experience.

![Dell Latitude 7280 Front](https://i.dell.com/das/xa.ashx/global-site-design%20WEB/5dcd4ce2-0508-7eec-6fe8-5984ea791e5c/1/OriginalPng?id=Dell/Product_Images/Dell_Client_Products/Notebooks/Latitude_Notebooks/12_7280/Non_Touch/Hero/laptop-latitude-12-7280-non-touch-pol-hero-504x350.psd) ![Dell Latitude 7280 Back](https://www.computerslaptops.co.uk/media/inv/i/512/Dell-Latitude-7280-Core-i7-7600U-8GB-DDR4-02810277025_3V83HM2-1633511635.jpg)

## Tables of Contents
- [Dell Latitude 7280 Hackintosh](#dell-latitude-7280-hackintosh)
  - [Tables of Contents](#tables-of-contents)
  - [System Specs](#system-specs)
  - [Setup](#setup)
    - [Getting macOS](#getting-macos)
    - [Adding the EFI](#adding-the-efi)
    - [Preparing the BIOS](#preparing-the-bios)
  - [Results](#results)
  - [Credits](#credits)

---

## System Specs
| Component | Specification |
| ----------- | ----------- |
| Processor | Intel Core i5-7300U |
| RAM | 8GB DDR4 2133MHz |
| iGPU | Intel HD 620 Graphics |
| Storage | WD SN530 512GB SSD |
| Audio | Realtek ALC3246 (or ALC256) |
| Ethernet | Intel Ethernet I219-LM |
| Wireless | Intel Dual Band Wireless AC 8265 |
| Bluetooth | Intel Wireless Bluetooth|
| SMBios | MacBookPro14,1 |
| OpenCore | v0.7.8 |
| macOS | v12.4 |

---

## Setup
### Getting macOS
To start, you are going to need macOS. Specifically we are going to start creating the USB installer. Follow the link below:
https://dortania.github.io/OpenCore-Install-Guide/installer-guide/

### Adding the EFI
After the USB installer has been created, simply download the EFI provided in this repository and add it. Make sure to put it in the right place (aka the right partition).

Please note that this EFI is tailored to the system specifications and components [seen above](#system-specs).

### Preparing the BIOS
This part is very specific and you should be very careful when proceeding. There are things we need to enable and disable to get OpenCore to boot.

**Enable:**
- System Configuration -> **SATA Operations** -> ACHI
- System Configuration -> **Integrated NIC** -> Enabled
- POST Behavior -> **Fastboot** -> Minimal

**Disable:**
- Secure Boot -> **Secure Boot Enable** -> Disable
- Security -> **Computrace/Absolute** -> Disable (this cannot be undone, be warned!)
- Intel software guards extensions -> **Intel SGX Enable** -> Disable
- System Configration -> **Integrated NIC** -> Enable UEFI network stack
- Power Management -> **Wake on AC** -> Disable
- Power Management -> **Wake on Dell USB-C Dock** -> Disable
  
---

***Now there is one more thing that needs to be done. This part is dangerous and may lead to the bricking of your device if done wrongly. I am not responsible for any damages! Do this at your own risk!!***

Essentially we are going to disable the CFG Lock and DVMT.

Reboot and on the menu picker, select "modGRUBShell.efi". The following is extracted from Lorys89's guide.

![CFG-Lock](https://raw.githubusercontent.com/Lorys89/DELL_LATITUDE_7280/main/Screenshot/CFG-LOCK.png)

To disable CFG Lock:
`setup_var 0x4ED 0x0`

*After this mod, set the quirk "AppleXcpmCfgLock" to false in your config.plist*

![Set DVMT PRE to 64MB](https://raw.githubusercontent.com/Lorys89/DELL_LATITUDE_7280/main/Screenshot/DVMT-PRE.png)

To set DVMT PRE Allocated to 64MB:
`setup_var 0x795 0x2`

![Set DVMT Total GFX Mem](https://raw.githubusercontent.com/Lorys89/DELL_LATITUDE_7280/main/Screenshot/DVMT-TOT.png)

To set DVMT Total GFX Mem to MAX
`setup_var 0x796 0x3`

*After this mod, remove the strings "framebuffer-fbmem" and "framebuffer-stolenmem" from iGPU Patch*

---

## Results
My config is different from Lorys89's due to hardware differences. Your hardware may also be different to mine so I recommend checking everything. If something, e.g. Wi-Fi card doesn't work, then the config may not be compatible with the Kext inside this config.

![Screenshot](Screenshots/Hackintosh%20Complete.png)

My setup works perfectly. Audio, keyboard shortcuts, backlight, webcam, Bluetooth audio are working as they should. I also have a Windows 10 Dual-booted and I have installed [Blackosx's BsxM1 OpenCore theme](https://github.com/blackosx/BsxM1).

If you have any questions, feel free to message me on Discord (**safeboot#9005**).

---

## Credits
This guide was inspired by [Lorys89's guide](https://github.com/Lorys89/DELL_LATITUDE_7280)!

- [Apple](https://apple.com) for macOS.
- [Acidanthera](https://github.com/acidanthera) for OpenCore and all the lovely hackintosh work.
- [Dortania](https://dortania.github.io/OpenCore-Install-Guide/config-laptop.plist/icelake.html) For great and detailed guides.
- [Juico](https://github.com/juico) for fix Alps i2c touchpad.
- [Hackintoshlifeit](https://github.com/Hackintoshlifeit) Support group for installation and post installation.