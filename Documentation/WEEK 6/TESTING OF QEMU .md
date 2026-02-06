# Yocto QEMU Image Creation  and Testing

## What is the Yocto Project?

The **Yocto Project** is an open-source collaboration that provides tools and processes to build custom Linux distributions tailored for embedded systems. It is not a Linux distribution itself, but a flexible build infrastructure that produces embedded Linux images using metadata and recipes.

The core of the Yocto Project uses:

- **BitBake** – the task scheduler and execution engine  
- **OpenEmbedded-Core** – core metadata for system builds  
- **Poky** – the reference distribution combining BitBake + OE-Core + build setup .


## What is QEMU?

**QEMU** (Quick Emulator) is an emulator that allows you to run a complete embedded Linux image on your development machine without physical hardware. Yocto supports building images that can run directly inside QEMU for testing and validation. 


## Step-by-Step Yocto Image Creation and Testing

### 1. Prepare Your Build Host

You must use a supported **Linux build host** with all required development packages installed. A typical supported host is Ubuntu or Debian. 

Install packages such as:
```py
sudo apt-get update 
sudo apt-get install gawk wget git diffstat unzip texinfo gcc-multilib \
build-essential chrpath socat cpio python3 python3-pip python3-pexpect \
xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa \
libsdl1.2-dev xterm
```
This ensures the Yocto build system and QEMU emulator can be used properly. 


---

### 2. Clone the Yocto Project (Poky) Repository

Download the Yocto Project reference distribution (Poky):
```py
git clone git://git.yoctoproject.org/poky
```
This contains:

- Build scripts  
- BitBake tool  
- Core metadata  

---

### 3. Initialize the Build Environment

Source the build environment script to create a build directory:
```py
source poky/oe-init-build-env
```

This sets up environment variables and creates the Yocto build directory where configuration and output files will be placed.:

---

### 4. Configure local.conf for QEMU

Open the `conf/local.conf` file inside your build directory and set the **machine target** to a QEMU architecture you want to test.

For example : MACHINE ?= "qemux86-64"

This tells the Yocto build system which emulator target it should build for. 

---

### 5. Build the Image

Run BitBake to build a Linux image:
```py 
bitbake core-image-minimal
```

This command instructs Yocto to compile all required components, such as:

- Linux kernel  
- Root filesystem  
- Bootloader  
- Other selected packages  

The result is a set of generated artifacts in the `tmp/deploy/images/<machine>/` directory. 

-This step may take several hours on first run depending on your system. 

---

### 6. Run the Image in QEMU

Once the build completes successfully, start the emulator using the `runqemu` script:
```py
runqemu qemux86-64
```
A new window or terminal will open showing the Linux image booting in QEMU.  

You can log in as user `root` 

![alt text](<../../ASSETS/QEMU TEST.png>)

