Customized Linux Kernel
============

Implemented modifications of cpuid leaf nodes:  
- Return the total number of exits - 0x4FFFFFFC 
- Return values are measured in processor cycles, across all VCPUs - 0x4FFFFFFD
- Return the number of exits for the exit number provided (on input) in %ecx. This value should be returned in %eax  - 0x4FFFFFFE  
- Return the time spent processing the exit number provided (on input) in %ebx(high 32 bits) and %ecx(low 32 bits) - 0x4FFFFFFF

* Example outputs:  
![alt text](https://i.imgur.com/a36iYZ4.png)
![alt text](https://i.imgur.com/QZMf5Zk.png)
