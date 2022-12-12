Customized Linux Kernel
============

# Assignment 3 of CMPE 283

This assignment implemented modifications of cpuid leaf nodes:  
- Return the total number of exits - 0x4FFFFFFC 
- Return values are measured in processor cycles, across all VCPUs - 0x4FFFFFFD
- Return the number of exits for the exit number provided (on input) in %ecx. This value should be returned in %eax  - 0x4FFFFFFE  
- Return the time spent processing the exit number provided (on input) in %ebx(high 32 bits) and %ecx(low 32 bits) - 0x4FFFFFFF

1) I completed this homework alone.
2) To complete this assignment I used Ubuntu 20.04 on a Google cloud platform virtual machine with nested virtualization enabled.
3) From what I noticed the frequency of exits does not increase at a stable rate. Depending on running processes the rate for certain exit types would increase. One example was exit code 10 for CPUID which increased when I ran my tests that used cpuid. Using the information from leaf nodes C and D, after running a 'reboot' I got a count of 922582 exits and 13064155677 cpu cycles.
4) From the SDM defined exit types I saw that most frequent were exits 48 EPT_VIOLATION and 30 IO_INSTRUCTION. The least frequent of the exits that provided results was 29 MOV_DR.



Steps: 
* ### View ASMT2-README.md for instructions on setting up ubuntu environment in google cloud wuth nested virtualization. As I reused this VM.
* I used an Ubuntu 20.04 machine with 16 vCPU cores, 60 GB RAM and 100 GB SSD
* SSH into virtual machine
* Reusing environtment from Assignment 2 which already has functionality implemented for cpu leaf nodes 0x4FFFFFFC and 0x4FFFFFFD implemented
* Check kernel version: uname -r (I used 6.1.0-rc8+)
* Make changes to info gotten from 0x4FFFFFFE and 0x4FFFFFFF
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
* sudo kvm-ok
* sudo reboot (reboot command did not work well with google cloud so I had to restart the machine from the google cloud console)
* Here I used Google's https://remotedesktop.google.com/ to set up a UI connection to my virtual machine through Chrome browser
* Used the inner VM inside my Ubuntu VM from Assignment 2
* Launch virtual manager: sudo virt-manager
* Launch virtual machine running the Ubuntu iso
* Login to inner VM
* Run test code that uses cpuid
* After running my test script I get:  
![alt text](https://i.imgur.com/QZMf5Zk.png)
