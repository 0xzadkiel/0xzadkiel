---
title: "A Comprehensive Guide to Linux Kernel Modules"
published: 2025-06-11
image: "./cover1.png"
tags: ["Linux"]
category: "Guide"
draft: false
---

## What are kernel modules ? 
Linux kernel modules aka (LKMS) are loadable pieces of code (think of them as plugins) that can be loaded/unloaded on demand without rebooting the linux kernel as well as allowing us to update drivers without restarting . Hence making the linux kernel light weight and faster to boot in comparison to them being complied into it as well as it makes it more memory sufficient because only loaded modules consume memory .

Linux kernel module reside in `/lib/modules/` and use `.ko` extension ,We are not required to have a module in this directory in order to load it.

- **Types of Modules**:

    - **Security**: AppArmor (to restrict application access to system resources.)
    - **Device Drivers**: NVIDIA GPU drivers.
    - **Filesystems**: `ntfs.ko` for NTFS support.
    - **Networking**: VPN modules like `tun.ko`.





## Interacting with kernel modules 

**Finding existing modules**
```bash
ls -l /lib/modules/$(uname -r)/*
```

**Viewing Loaded modules**
```bash
lsmod | less
```

![Cover image0](/images/lsmod.png)

**Inspecting module details**
 ```bash
 modinfo bluetooth
```

![Cover image1](/images/modinfo.png)





## Writing your own kernel module 
Although Linux kernel modules are written in C they are different then your user-space programs because linux kernel dosen't use standard `glibc`
library (which includes functions such as `printf`) due to it being not available in the kernel space .
However the naming convention is very similar e.g : 
`printf` is `printk`


Here's a example of a linux kernel module that prints our given text in `dmesg` : 

```c
#include <linux/module.h>
#include <linux/kernel.h>

static int __init mymodule_init(void) {
    printk(KERN_INFO "Hack the planet!\n");
    return 0;
}

static void __exit mymodule_exit(void) {
    printk(KERN_INFO "Your module is unloaded (:\n");
}

module_init(mymodule_init);
module_exit(mymodule_exit);
```


Easiest way to compile this into a kernel module is to make a `Makefile`:

```make 
obj-m += custommodule.o
all:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules
clean:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
```

We can now compile our code using `make` in the directory our code file resides in.



## Loading & Unloading modules 

To load and unload kernel modules we can use utilities such as :
1. **`insmod`**
2. **`rmmod`**
3. **`modprobe`**

The difference between `Insmod/rmmod` and `modprobe` is that modprobe can do dependency handling while `insmod/rmmod` can't . Dependency handling refers to how the linux kernel loads other modules that a module depends on in order to function properly . These can be `hardware`,`drivers` etc ...

So when we load a module using `insmod` it won't automatically load another module even if that is a dependency for the current module, Instead it will through and error . On the other side `modprobe` will do the opposite .

**Loading**
```bash
insmod custommodule.ko
```
Output in dmesg : 
![Cover image2](/images/insmod.png)

**Unloading**
```bash
rmmod custommodule
```
Output in dmesg : 
![Cover image3](/images/rmmod.png)








### Debugging issues 
Here we are common issues that we can face when dealing with linux kernel modules . 


**Unknown symbol in module**
This error accurs when a function or a variable is exported by another module and that module isn't loaded (dependency issue) . This can be resolved by simply using `modinfo` to check the dependency modules and loading them using either `modprobe` or `insmod` 

```bash
modprobe --show-depends bluetooth
```
Output:
![Cover image4](/images/modprobe.png)


####Module won't unload
Error such as  `Error : Module is in use`
This can be due to modules resources are still open (e.g /proc entries)

####Invalid module format
This happens due to kernel version mismatch or kernel abi mismatch .
We can simply recompile against the current kernel header by using `make` with our code again 




## Security tools in the linux kernel  
+ **`Selinux`**
+ **`Apparmor`**
+ **`EBPF`**

In order to understand these systems we have to understand different security levels we have when it comes to a systems security . We have levels 
1. **`DAC`** : User controlled permissions such as `chmod`
2. **`MAC`** : System wide policies inforced by the kernel e.g : `selinux`,`apparmor`
3. **`Rbac`**: role based permissions e.g : `kubernetes`



### Selinux
This is a (mac) system intergrated into the linux kernel and was developed by `NSA` , It enforces strict policies that define how processes and users interact with files , directories and network ports

**Modes**
1. `Enforcing` : Blocks unauthorized actions
2. `Permissive` : Logs but allows actions 
3. `Disabled` :. Turns off 


**Key-Features** : 
1. Labels (contexts) : Every file , process and port has a security label (e.g: `http_port_t`)
2. Policies : Rules define allowed interactions (e.g : can `httpd_t` read files labeld `httpd_content_t?`)>)


**Usage**
To check the current status of `selinux` we use `sestatus`
```bash
sestatus
```

To change the rules we use `setenforce`
```bash
setenforce 0 # permissive
setenforce 1 # Enforcing
```

Setting up rules with selinux can be done using `semanage`
```bash
semanage port -a -t http_port_t -p tcp 8080
```
Above we allow `apache` to access a non-standard port `8080`

Checking currently allowed ports 
```bash 
sudo semanage port -l
```




### Apparmor

This is a `Linux security module` (LSM) that provides Application-level `mac` via profiles and its a `kernel module.
Unlike `selinux` , it uses path-based restrictions rather than labels . 

**Key Features** : 
1. Define what files,capabilities and network access an application has 

**Modes** : 
1. `Enforce` : Blocks unauthorized actions 
2. `complain` : logs but allows violations 


**Usage**

*Check Status* : 
```bash
aa-status
```

*Putting a program in `complain mode`* : 
```bash
aa-complain /usr/sbin/apache2
```

*Generate a profile* : 
```bash
aa-genprof /usr/bin/chromium
```




### EBPF 
EBPF stands for Exnteded berkeley packet filter and is a kernel-level virtual machine allowing sandboxed programs to run without modifying the linux kernel . It also includes Dynamic tracing  (Monitor network traffic , function calls , syscalls)

For usage refer to  : https://ebpf.io/what-is-ebpf/ 





---


## Resources :
+ [Linux Kernel Modules](https://linux-kernel-labs.github.io/refs/heads/master/labs/kernel_modules.html )
+ [The LKM programming guide](https://sysprog21.github.io/lkmpg/)
+ [EBPF Guide](https://github.com/mikeroyal/eBPF-Guide )









