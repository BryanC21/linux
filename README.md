Customized Linux Kernel
============

# Assignment 2 of CMPE 283

This assignment implemented modifications of cpuid leaf nodes:  
- Return the total number of exits - 0x4FFFFFFC 
- Return values are measured in processor cycles, across all VCPUs - 0x4FFFFFFD

![alt text](https://i.imgur.com/a36iYZ4.png)

1) I completed this homework alone.

2) To complete this assignment I used Ubuntu 20.04 on a Google cloud platform virtual machine with nested virtualization enabled.

Steps: 

* Fork linux kernel repository from https://github.com/torvalds/linux
* Create a google cloud platform virtual machine with nested virtualization enabled. I followed these steps: https://cloud.google.com/compute/docs/instances/nested-virtualization/enabling
* I used an Ubuntu 20.04 machine with 16 vCPU cores, 60 GB RAM and 100 GB SSD
* SSH into virtual machine
* Git clone this forked repository 
* sudo apt-get update
* To install necessary packages: sudo apt-get install git fakeroot build-essential ncurses-dev xz-utils libssl-dev bc flex libelf-dev bison zstd
* Copy kernel config: cp -v /boot/config-$(uname -r) .config
* Leave default options and run: make menuconfig
* Avoid Certificate errors: 
*   scripts/config --disable SYSTEM_TRUSTED_KEYS 
*   scripts/config --disable SYSTEM_REVOCATION_KEYS
* Replace number afetr -j with your cpu count
* Build the kernel
* sudo make -j16
* sudo make -j16 INSTALL_MOD_STRIP=1 modules_install
* sudo make install
* Check kernel version: uname -r
* Apply changes to kernel: sudo reboot
* Check kernel version: uname -r
* Now that kernel is compiled I make changes to cpuid
* Make changes to info gotten from 0x4FFFFFFC and 0x4FFFFFFD
* Edited linux/arch/x86/kvm/cpuid.c
* Edited linux/arch/x86/kvm/vmx/vmx.c
* Rebuild kernel with my changes
* sudo make modules
* sudo make INSTALL_MOD_STRIP=1 modules_install
* sudo make install
* sudo rmmod kvm_intel
* sudo rmmod kvm
* sudo modprobe kvm_intel
* sudo modprobe kvm
* 
