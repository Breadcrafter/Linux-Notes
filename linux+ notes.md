
**Boot Process:**

  

**Boot Process** - Is the steps that a computer takes to start up (boot)

  

1.  **Preboot Execution Environment PXE**
    

    -   Optional, used for remote booting / network boot
    

1.  **Bootstrap phase**
    

  -   The first phase of the linux boot process
    
  -   Responsible for testing and bringing the hardware up
    
  -   BIOS & UEFI / POST which are what actually Initialize the hardware, and loading the bootloader / handing control to the bootloader  
      
    

2.  **Bootloader (ex. GRUB2)**
    

  -   Loads the OS and prepares the system
    
  -   Loads Initial RAM disc, and kernel at the same time
    

  

3. ** Initial RAM Disk (initrd)**
    

  -   Essential drivers and tools are loaded to RAM, before switching to the real room filesystem
    
  -   A temp file system
    

  

4. ** Kernel**
    

  -   The core of the OS, managing system resources
    
  -   The brain of the system that talks to the actual hardware
    
  -   Initialize hardware, with drivers and such
    
  -   Mounts the real root filesystem
    

  

Steps that PXE NIC to boot

  

1.  The NIC starts up / activates
    
2.  DHCP Request - The NIC requests IP from DHCP Server
    
3.  TFTP Server Location - The DHCP server gives where the NIC can find the TFTP server that has the boot files on it
    
4.  Boot Files Transfer from the TFTP server to the computer
    
5.  The Bootloader is executed to load the OS
    

  
  

Bootloader - Is responsible for loading the OS by selecting and launching the Linux Kernel

  

1.  Computer Starts - Firmware (BIOS / UEFI) hands control over to GRUB2 (ie. bootloader)
    
2.  GRUB2 Displays boot menu - the user then selects the OS or kernel they want to boot into
    
3.  GRUB2 reads configs - Loads /boot/grub/grub.cfg, which is a file that instructs the computer on how to display the boot menu, and how to load the kernel and initrd. It contains a bash script that defines menu items, set colors, set boot timeouts, and define partition locations.
    
4.  GRUB2 loads initrd and kernel - Loads the initrd and the selected kernel at the same time, and initrd then prepares the system for booting.
    

  

Administrators might want to modify the GRUB2 setting, to do that you can edit the /etc/default/grub file and then you use the update-grub command to actually put the new setting in place.

  

**Initial RAM Disk (initrd.img)** - A temporary root filesystem that is loaded into memory / RAM before the real root filesystem is mounted.

  

Contains essential drivers and kernel modules, and tools needed to initialize hardware and prepare the system for booting.

Example: On a system with an encrypted root partition it would go like the following

  

First boot sequence starts

  

The Initrd and Kernel are both loaded

  

Initrd file contains the encryption modules needed to unlock the disk / encryption keys

  

At the same time the kernel is loading the storage controllers, disc encryption modules, or file system drivers are available.

  

* This is important because systems with USB drives, encrypted discs, or specialized hardware that the kernel cannot directly access at boot time.

  

Then it prepares the root filesystem in initrd

  

Then it transitions from the temporary file system located in RAM to the kernel mounted file system.

  

Initrd typically located in /boot/initrd.img

Admins can create or update it using tools like mkinitramfs or dracut

**Kernel** - The core of the OS that manages hardware, processes, memory, and system resources.

  

**Kernel Process**

  

1.  Kernel Initializes System
    
2.  Detects Hardware
    
3.  Mounts Root Filesystem
    
4.  Starts Essential Processes
    

  

Making changes to Kernel

  

  -   Adjust Kernel behavior with sysctl - allows admins to modify kernel parameters at runtime (will only apply once at runtime not permanent). Parameters that control various system functions like networking, memory management, and security settings.
    

  

  -   To make permanent changes to linux kernel - you have to edit the etc/sysctl.conf file
    

  

  -   Apply the changes made to the etc/sysctl.conf file with sysctl -p command
    

  

Example: (To enable IP forwarding)

This allows the system to permanently route network traffic between interfaces, PC act like router

  

Add - net.ipv4.ip_forward=1 to /etc/sysctl.conf

And apply using sysctl -p
