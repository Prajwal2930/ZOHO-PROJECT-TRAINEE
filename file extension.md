# Understanding Yocto Build Output for Raspberry Pi and  file extension

After building an image with Yocto using the command:

 
```python
bitbake core-image-minimal 
```

many files are generated in the directory:
```python
 build/tmp/deploy/images/raspberrypi3/
```

---

## 1. Big Picture – What Yocto Actually Creates

When Yocto builds an image, it does not create only one file.  
Instead, it generates several components required to boot a complete Linux system:

- Linux Kernel  
- Device Tree files  
- Root filesystem  
- Bootloader and firmware files  
- Helper archives  

All these parts together form a bootable Raspberry Pi operating system.

---

## 2. What Does Raspberry Pi Need to Boot?

To boot successfully, Raspberry Pi requires:

- Kernel image  
- Device Tree file  
- Root filesystem  
- Boot configuration files  

Yocto automatically builds all these file.

---

# Classification of Build Output Files

All files in the output directory can be grouped into clear categories.

---

## CATEGORY A – Kernel Files

Example files:
```py
zImage  
zImage-raspberrypi3.bin  
zImage-1-5.15.92+git0....bin 
``` 

### What is zImage?

- zImage is the Linux kernel binary.  
- It is the core of the operating system.  
- It is a compressed ARM kernel format.

For Raspberry Pi booting:

**zImage is the main kernel that actually runs.**


## CATEGORY B – Device Tree Files (.dtb)

Example files:
```py
bcm2710-rpi-3-b.dtb  
bcm2711-rpi-4-b.dtb  
bcm2708-rpi-b.dtb 
``` 

### What are .dtb files?

DTB means **Device Tree Blob**.

A DTB file tells the Linux kernel:

- What hardware is present  
- Which peripherals exist  
- GPIO configuration  
- Memory layout  
- CPU information  

### Why so many DTB files?

The meta-raspberrypi layer supports many boards such as:

- Raspberry Pi 1  
- Raspberry Pi 2  
- Raspberry Pi 3  
- Raspberry Pi 4  

So Yocto builds DTB files for all supported models.

### Which DTB file is important?

For Raspberry Pi 3, the required file is:

```py
bcm2710-rpi-3-b.dtb
```

All others are not used for this specific board.

---

## CATEGORY C – Device Tree Overlays (.dtbo)

Example files:
```py
i2c-rtc.dtbo  
hifiberry-dac.dtbo  
gpio-ir.dtbo  
disable-bt.dtbo 
``` 

### What are DTBO files?

DTBO means **Device Tree Overlay**.

These files are optional configurations used to:

- Enable extra hardware  
- Configure add-on boards  
- Enable or disable peripherals  

### Examples

disable-bt.dtbo → disables onboard Bluetooth  
i2c-rtc.dtbo → enables external RTC module  

### Are DTBO files mandatory?

No.  
They are used only when enabled in the file:

config.txt

Otherwise they are ignored.

---

## CATEGORY D – Root Filesystem Images

Important files:
```py
core-image-minimal-raspberrypi3.ext3  
core-image-minimal-raspberrypi3.wic.bz2  
core-image-minimal-raspberrypi3.tar.bz2  
```

These represent the same root filesystem in different formats.

---

### A) .ext3 File

Example:
```py
core-image-minimal-raspberrypi3.ext3
```
This is:

- A raw Linux filesystem image  
- Formatted as ext3  
- Contains directories such as:

/bin  
/etc  
/lib  
/usr  

Use cases:

- Running in QEMU  
- Mounting on Linux  
- Using as a root partition  

---

### B) .tar.bz2 File

Example:
```py
core-image-minimal-raspberrypi3.tar.bz2
```
This is:

- A compressed archive of the root filesystem  
- Not directly bootable  

Used for:

- Manual extraction  
- NFS booting  
- Testing in chroot  

---

### C) .wic.bz2 File – Most Important

Example:
```py
core-image-minimal-raspberrypi3.wic.bz2
```
This is the real bootable SD card image.

WIC means **OpenEmbedded Image Creator**.

It contains:

- Partition table  
- Boot partition  
- Root filesystem  
- Kernel  
- Device tree files  

#### Simple comparison:

| File Type | Purpose |
|---------|---------|
| .ext3 | only root filesystem |
| .tar.bz2 | compressed archive |
| .wic.bz2 | full bootable SD card image |

For flashing to Raspberry Pi, the main file to use is:

**core-image-minimal-raspberrypi3.wic.bz2**

---

### D) .wic.bmap File

Example:
```py
core-image-minimal-raspberrypi3.wic.bmap
```
This is a helper file used for fast flashing.

Used with the command:

bmaptool copy image.wic.bz2 /dev/sdX

It speeds up writing the image to SD card.

---

## CATEGORY E – Kernel Modules Archive

File:
```py
modules-raspberrypi3.tgz
```
This contains:

- Kernel modules  
- Drivers  
- Network modules  
- Filesystem modules  

Usually these modules are already included in the root filesystem.  
This archive is useful when modules are needed separately.

---

## CATEGORY F – Boot Files

The build output also includes boot-related files such as:
```py
config.txt  
cmdline.txt  
start.elf  
bootcode.bin  
````
These are Raspberry Pi firmware and configuration files.  
They are required for booting on real Raspberry Pi hardware.

---

# Final Understanding

Although the output folder contains many files, logically there are only a few important components:

- Kernel (zImage)  
- Device Tree (DTB)  
- Root filesystem  
- Boot firmware  
- Optional overlays  

All other files are just different formats or helper files.

---
