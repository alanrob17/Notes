# Linux Notes

These are notes I am keep that are based on what I am doing with my Linux netbook.

## Checking Memory

### 1. free

The free command is the most simple and easy to use command to check memory usage on linux. Here is a quick example.

```bash
 free -m
```

>    total       used       free     shared    buffers     cached  
> Mem:          7976       6459       1517          0        865       2248  
> -/+ buffers/cache:       3344       4631  
> Swap:         1951          0       1951

The ``m`` option displays all data in MBs. The total is 7976 MB is the total amount of RAM installed on the system, that is 8GB. The used column shows the amount of RAM that has been used by linux, in this case around 6.4 GB. The output is pretty self explanatory. The catch over here is the cached and buffers column. The second line tells that 4.6 GB is free. This is the free memory in first line added with the buffers and cached amount of memory.

Linux has the habit of caching lots of things for faster performance, so that memory can be freed and used if needed.
The last line is the swap memory.

```bash
 free -h
```

Shows memory in Gbs, better

### 2. /proc/meminfo

The next way to check memory usage is to read the /proc/meminfo file. Know that the /proc file system does not contain real files. They are rather virtual files that contain dynamic information about the kernel and the system.

```bash
 cat /proc/meminfo
```

Returns.

> MemTotal:        8167848 kB  
> MemFree:         1409696 kB  
> ...

Check the values of MemTotal, MemFree, Buffers, Cached, SwapTotal, SwapFree.
They indicate same values of memory usage as the free command.

### vmstat

The vmstat command with the s option, lays out the memory usage statistics much like the proc command. Here is an example

```bash
 vmstat -s
```

Returns.

> 8167848 K total memory  
> 7449376 K used memory  
> 3423872 K active memory  
> 3140312 K inactive memory  
>  718472 K free memory  
> ...
  
The top few lines indicate total memory, free memory etc and so on.

### 4. top command

The top command is generally used to check memory and cpu usage per process. However it also reports total memory usage and can be used to monitor the total RAM usage. The header on output has the required information.

```bash
 top
```

### 5. htop

Is on the Lubuntu menu and has a graphical interface. If you are using another distro then you may have to install this.

## Running Nodemon on Linux Mint

I created a project on Linux Mint using an Express server and wanted to use Nodemon to automatically compile on any JavaScript saves.

I used the local version of Neodemon in my project.

```bash
    npm install nodemon
```

When I went to run the server I got a "nodemon: command not found" error.

I found that I had to globally install Nodemon.

```bash
    npm install -g nodemon
```

This failed as well because I didn't have permission. I got around this with.

```bash
    sudo npm install -g nodemon
```

Now when I run Nodemon it works and is able to recompile on all JavaScript code saves.

## Changing the Linux swapfile size

I am having real problems keeping my ASUS netbook running. It doesn't have enough specs to run a lightweight version of Linux.

Increasing the size of the swapfile is supposed to improve the performance of low specced machines.

I have worked out how to change the swapfile size in the hope that it may help.

To check if you actually have a swapfile.

```bash
    swapon -s
```

This will either return nothing or detail the swapfiles statistics. In my case.

> /swapfile file    512M    0

I have a 512 Mb swapfile and 0 Mb is being used.

I need to increase the size of the swapfile so the first thing is to turn off the swapfile.

```bash
    sudo swapoff -a
```

Once I do this I have to remove the current swapfile.

```bash
    sudo rm -i /swapfile
```

In general you want to create a swapfile that is twice the size of your RAM. My RAM is 2 Gb so I need to create a 4 Gb swapfile.

```bash
    sudo dd if=/dev/zero of=/swapfile bs=1M count=4096
```

Set file permissions.

```bash
    sudo chmod 600 /swapfile
```

Verify the permissions.

```bash
    ls -lh /swapfile
```

> -rw------- 1 root root 4.0G Aug 27 16:59 /swapfile

We can see that only root user has the read and write flags enabled.

Mark the file as swap space by typing.

```bash
    sudo mkswap /swapfile
```

We then enable the swap file.

```bash
    sudo swapon /swapfile
```

Verify that the swap is available and confirm 4 GB RAM and 8 GB swap by typing.

```bash
    free -h
```

>               total        used        free      shared  buff/cache   available       
> Mem:          1.8Gi       436Mi       778Mi        30Mi       655Mi       1.3Gi
> Swap:         4.0Gi       284Mi       3.7Gi

Make the swap file permanent.

First back up the ``/etc/fstab`` file in case something goes wrong.

```bash
    sudo cp -pv /etc/fstab /etc/fstab.bak
```

Edit ``/etc/fstab`` in your text editor.

```bash
    sudo gedit /etc/fstab 
```

Add this line in ``/etc/fstab`` and confirm that there are no other **"swap"** lines.

```bash
    /swapfile   none    swap    sw  0   0
```

Now, reboot your system and then check that the swapfile is still there.

```bash
    free -h
```

>               total        used        free      shared  buff/cache   available       
> Mem:          1.8Gi       336Mi       878Mi        30Mi       655Mi       1.3Gi
> Swap:         4.0Gi       184Mi       3.8Gi

The swapfile has been permanently increased.

This is a copy of my ``/etc/fstab`` file.

```bash
    # /etc/fstab: static file system information.
    #
    # Use 'blkid' to print the universally unique identifier for a device; this may
    # be used with UUID= as a more robust way to name devices that works even if
    # disks are added and removed. See fstab(5).
    #
    # <file system>             <mount point>  <type>  <options>  <dump>  <pass>
    UUID=76EF-29F0                            /boot/efi      vfat    umask=0077 0 2
    UUID=046ce35e-f04a-41d8-ab8e-d5adcd58424e /              ext4    defaults   0 1
    /swapfile none swap  sw  0 0
```

## Moving files from multiple folders

I needed to move all *.jpg* files from sub folders to another folder. I couldn't do this in DOS but was able to find a linux command that could complete this task.

```bash
    mv -t /d/Projects/Soft.UX/Soft.UI/bin/Debug/images */*.jpg
```

This moves all *.jpg* files in the current folder structure to the *images* folder.

## Lightweight Distros

This is a list of distros that can be used on my netbook.

### 1. Lite Linux OS

<https://www.linuxliteos.com/>

Linux Lite is a popular and easy-to-use lightweight distribution. It is based on the latest Ubuntu LTS (Long Term Support) which provides regular updates. Apart from that it also has access to a vast collection of software repository.

Linux Lite usage XFCE desktop environment which is simple, lite, and user-friendly.

Requirements –

* 1 GHz CPU
* 786 MB RAM
* 8GB of Disk space
* 720p (1024 x 768) Display resolution

### 2. Zorin OS Lite

<https://zorinos.com/>

Zorin Lite is a lightning-fast lightweight Linux distribution. It runs snappy on computers as old as 15 years (Claimed by the developers).

This one again an Ubuntu-based distribution that means it has all the Ubuntu perks. It features XFCE desktop environment which mimics the Windows desktop layout, making it user-friendly for a lot of users.

**Note:** Zorin also has two different versions of the distro. A commercial Zorin Ultimate and free Zorin core build.

Requirements –

* 700 MHz CPU
* 512 MB RAM
* 8GB of Disk space
* <720p (640 x 480) Display resolution
* Has support for 32-Bit

### 3. Puppy Linux

<https://www.puppylinux.org/>

Puppy Linux is lightweight and come with different bases: Ubuntu, Slackware, etc. Users can can download different build-versions of Puppy Linux as per their preference.

It comes with lots of in-house puppy-specific applications and Joe’s windows manager.

Requirements –

* 600 MHz CPU
* 256 MB RAM
* 512 MB of disk space
* Has support for 32-Bit
* <300 MB ISO size

As of the latest release Linux lite 5.0, it is primarily targeting 64-bit systems only. This is because of Ubuntu which has recently dropped support for 32-bit.

### 4. Lubuntu

<https://www.lubuntu.net/>

Requirements –

* <1 GHz CPU
* 512 MB RAM
* 2GB of Disk space
* Has support For 32-Bit

### 5. Bodhi Linux

<https://www.bodhilinux.com/>

Bodhi Linux is specially designed to run on hardware with limited capabilities. It has a very minimal approach to everything it comes with except the performance.

Bodhi is also based on Ubuntu LTS so it gets all the updates and repository perks from Ubuntu.

It comes with Moksha desktop environment which can be customized with Compiz. The distro is available in four editions: Standard, Legacy, AppPack, and HWE.

Requirements –

* 500 MHz CPU
* 256 MB RAM
* 5GB of disk space
* 640 x 480 Display resolution
* Has support for 32-Bit

### 6. Ubuntu Mate

<https://www.ubuntu-mate.org/>

Ubuntu Mate is very impressive lightweight Linux distribution. It features MATE desktop environment and based on Ubuntu as the name gives that away.

It provides the best of both world’s, the lightweight and the regular flagship Linux distributions. This makes it a perfect choice for beginners.

Requirements –

* 1 GHz CPU
* 1 GB RAM
* 8GB of Disk space
* 720p (1024 x 768) Display resolution
* Has support for 32-Bit until April 2021

