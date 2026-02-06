## Raspberry Pi Booting Process – Explained in Simple Language

The Raspberry Pi boot process is different from normal PCs.  
In Raspberry Pi, the **GPU starts first – not the CPU.**

Below is a detailed step-by-step explanation based on the diagram.
<div align="center">
  <img src="../../ASSETS/booting process .jpeg" alt="Booting Process" width="200">
</div>


---

### Step 1 – Power ON

* When power is applied to the Raspberry Pi, the board receives 5V from the power supply.
* At this stage:

  * The **ARM CPU is OFF**
  * The **GPU is ON**

This is a unique design of Raspberry Pi.

---

### Step 2 – GPU Starts First

* Inside the Raspberry Pi SoC, the VideoCore GPU is responsible for the early boot process.
* Immediately after power on:

  * GPU becomes active
  * CPU remains in reset (OFF state)
* At this moment:

  * **SDRAM (main memory) is disabled**
  * No Linux or kernel is running yet

---

### Step 3 – Execution of First Stage Bootloader (in ROM)

* The GPU has a small built-in bootloader stored in internal ROM memory.
* This is called the **1st stage bootloader**.
* It is hardcoded by the manufacturer and cannot be changed.

Function of this stage:
* Initialize minimal hardware
* Access the SD card
* Search for further boot files

---

### Step 4 – Loading Second Stage Bootloader (bootcode.bin)

* The first stage bootloader looks into the SD card.
* It searches for a file called: **bootcode.bin**

* This file is the **second stage bootloader**.

What happens here:
* bootcode.bin is loaded from SD card into L2 cache
* It is executed by the GPU
* Still at this point:

  * CPU is OFF
  * SDRAM is not yet enabled

---

### Step 5 – Enabling SDRAM and Loading Third Stage Bootloader

* Now the second stage bootloader (bootcode.bin):

  * Initializes the SDRAM (main RAM)
  * Makes the memory ready to use

* After enabling SDRAM, it loads the next file:

* This is called the **third stage bootloader**

Purpose:

* Prepare system for full firmware loading
* Move boot process from cache to actual RAM

---

### Step 6 – Loading GPU Firmware (start.elf)

* The third stage bootloader now loads the main GPU firmware file:

* This is one of the most important files in Raspberry Pi booting.

start.elf does the following:

* Fully initializes the hardware
* Reads configuration files
* Prepares the ARM CPU to run

---

### Step 7 – Reading Configuration and Kernel Files

Now start.elf performs these actions:

It reads:

* `config.txt` – system configuration parameters (optional)
* `cmdline.txt` – kernel boot parameters
* Kernel image file:

(or zImage in Yocto builds)

What happens here:

* Kernel is loaded into RAM
* Required parameters are passed
* System is prepared to start Linux

---

### Step 8 – ARM CPU is Released

* Up to now the ARM CPU was OFF.
* After kernel is loaded:

**GPU releases reset signal to ARM CPU**

This means:

* ARM CPU finally turns ON
* Control is handed over from GPU to CPU

---

### Step 9 – Kernel Starts Booting

Now normal Linux boot process begins:

* ARM CPU starts executing the Linux kernel
* Kernel initializes:

  * Drivers
  * Peripherals
  * Filesystem
* Finally the operating system boots completely.

---

## Boot Process Summary (Simple Flow)

Power ON  
⬇  
GPU starts (CPU OFF)  
⬇  
1st stage bootloader from ROM  
⬇  
bootcode.bin loaded from SD card  
⬇  
SDRAM enabled  
⬇  
loader.bin executed  
⬇  
start.elf firmware loaded  
⬇  
Kernel image loaded  
⬇  
ARM CPU turned ON  
⬇  
Linux Kernel starts  

---

## Important Files Involved

All these files must be present in the SD card boot partition:

| File                | Purpose                 |
| ------------------- | ----------------------- |
| bootcode.bin        | Second stage bootloader |
| loader.bin          | Third stage bootloader  |
| start.elf           | GPU firmware            |
| config.txt          | System configuration    |
| cmdline.txt         | Kernel parameters       |
| kernel.img / zImage | Linux kernel            |

---

## Conclusion

* Raspberry Pi boot process is **GPU-driven**, unlike PCs where BIOS/UEFI starts CPU first.
* Understanding this flow is very important for:

  * Yocto image building
  * Custom kernel development
  * Bootloader debugging
  * Embedded Linux work


