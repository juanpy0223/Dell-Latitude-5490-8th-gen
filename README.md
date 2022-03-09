# Dell Latitude 5490 OpenCore 0.7.8

 Dell Latitude 5490

Tested to clean Install Mojave, Catalina, need some tweaking for Big Sir up, when Imhave time, will update
my wifi card is BCM94360ng, I havent tried any other.

please add your own PLATFORM INFO, with https://github.com/corpnewt/GenSMBIOS, im using MacBookPro15,2 SMBIOS. But you can use any SMBIOS from MBP14,1 through MBP16,1


# Guides and sources:

I followed [Dortania's OpenCore Guide](https://dortania.github.io/)

Then also took some files from [franklin-gedler repo](https://github.com/franklin-gedler/Dell-Latitude-7490-OpenCore-Big-Sur)

Last I followed [OC Little Translated by 5T33Z0](https://github.com/5T33Z0/OC-Little-Translated)


# My Specs:

- Model Make: Dell Latitude 5490

- CPU/GPU: Intel i5-8350u/Intel UHD Graphics 620

- DRAM: 24GB (16GB+8GB) 2400MHZ DDR4 RAM

- Storage: 1TB MX500 Crucial SSD 2.5" (Files) (sata port) + 256GB WD SN520 NVME 2242 (Windows) (via wwan slot) + 1TB WD SN530 2280 (nvme slot) (MacOS-Catalina and Big Sur)

- Connections: BCM94360ng BT+Wifi card - I219-LM Intel Ethernet

- 3x USB 3.1 Gen1 (one with PowerShare) - DisplayPort over USB Type-C(optional Thunderbolt 3(1)


# What works on MacOS:

- KeyBoard+Trackpad full (SSDT-GPI0.aml + AlpsHID.kext,VooDooI2C.kext,VooDooI2cHID.kext, VooDooPS2Controller.kext, NoTouchID.kext)

- Graphics Full Aceleration, VGA and HDMI (Plug.aml, Lilu.kext, VirtualSMC.kext, WhateverGreen.kext)

- A type USB USB Port Map kext, USB-C | DP DisplayPort - (USB Port Map kext + SSDT-EC-USBX-LAPTOP.aml + Graphic Patch)

- Brightness+Audio keys (SSDT-PNLF.aml + BrightnessKeys.kext)

- Battery Read-Outs (SSDT-AC.aml, ECEnabler.kext, SMCBatteryManager.kext, ACPIBatteryManager.kext) it actually worked without any patch or kext, but battery life was very poor, I went on with patching, now it lasts 5 hours.

- Power Management (SSDT-EC-USBX-LAPTOP.aml, SSDT-HPET.aml, SSDT-PLUG.aml, CPUFriend+CPUFriendDataProvider kexts)

- Wifi + Bluetooth no kext needed

- Conexion LAN (IntelMausi.kext)

- SDCard Reader (RealtekCardREader+RealtekCardReaderFriend kexts)

- Audio: Speakers and Microphone (AppleAlc.kext alcid11 + HPET aml) ComboJack (PostInstall\ComboJack-fixforDell + VerbStub.kext) it fixes the audio distortion via wired headphones and aux audio. (I had to disable SIP in order to install, then enabled back).

# I included a file (pcidevices.aml) wich has all pci devices config needed for DeviceProperties



# This is a WORK IN PROGRESS, please let me know any problem you may have, or any suggestions.

There are other patches for correcting sleep issues, and making macOS believe that os running on a real MacBookPro, like ALSD, DMAC, EXT3-WAKESCREEN, GPIO, GPRW, MCHC, MEM2, PCMR, PTWAKKTS, and so on.

# Note: when connected to the charger it wakes up right after sleep, but if I detach charger, sleep and waking up works ok, just by closing and opening the laptop.

# Before installation you must do 2 things: 1st. Bios Config, 2nd. Config Modification via GrubShell: (Credits to franklin-gedler)


# Bios Config:

Boot mode: UEFI

Fast Boot: Minimal

SecureBoot = Disable

SATA Mode: AHCI 

Intel SGX: Software Controlled

# Now we start the pendrive and select modGRUBShell.efi or OpenShell.efi (Be carefull with the commands):

CFG Lock = Disable
    (VarOffset/VarName): 0x52D
    VarStore: 0x1
    Name: Setup
    
    Command for modGRUBShell.efi: setup_var_cv Setup 0x52D 0x1 0x0
____________________________________________________________________

DVMT Pre-Allocated = 64MB
    (VarOffset/VarName): 0x804
    VarStore: 0x1
    Name: Setup
    
    Command for modGRUBShell.efi: setup_var_cv Setup 0x804 0x1 0x2
____________________________________________________________________

DVMT Total Gfx Mem = Max
    (VarOffset/VarName): 0x805
    VarStore: 0x1
    Name: Setup
    
    Command for modGRUBShell.efi: setup_var_cv Setup 0x805 0x1 0x3
____________________________________________________________________

VT-d = Disable
    (VarOffset/VarName): 0x808
    VarStore: 0x1
    Name: Setup
    
    Command for modGRUBShell.efi: setup_var_cv Setup 0x808 0x1 0x0
    
    
    After this we can reboot and then install.
