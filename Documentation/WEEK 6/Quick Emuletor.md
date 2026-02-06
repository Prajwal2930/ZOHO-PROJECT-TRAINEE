# What is QEMU and How It Works

## 1. What is QEMU?

QEMU (Quick Emulator) is a free and open-source emulator that allows you to run an entire computer system — including CPU, memory, and devices — in software, even if the host hardware is different from the target architecture. QEMU translates machine code from one architecture to another through dynamic binary translation so that software compiled for a different CPU can run on your development machine.

There are two primary modes:

* **User-mode emulation**: Runs individual binaries compiled for another architecture.
* **System emulation**: Emulates a complete computer system — CPU, memory, peripherals — so an entire OS can boot. 


## 2. How QEMU Emulates ARM Hardware

QEMU can emulate both ARM architectures:

- `qemu-system-arm` → for 32-bit ARM systems  
- `qemu-system-aarch64` → for 64-bit ARM systems  

QEMU creates a virtual model of hardware in software, including:

- ARM CPU instructions  
- Memory  
- Storage devices  
- Network interfaces  
- Basic peripherals  

When QEMU runs, it:

1. Takes instructions from the guest system.  
2. Translates them to host machine instructions.  
3. Provides virtual hardware so the OS thinks it is running on real hardware.

---

## 3. Purpose of QEMU

### Main Uses

- **Testing Embedded Linux Images**  
  Run and test Linux images without using real hardware.

- **Cross-Architecture Development**  
  Run ARM software on an x86 PC for development and debugging.

- **Yocto Development Support**  
  Yocto uses QEMU to boot and test built images quickly without flashing them to real boards.

---
