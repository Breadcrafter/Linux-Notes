# Administrative Tasks
Administrative Tasks - are often done though terminal cli, which is more flexible, less resource intensive, faster and easier to automate. 

# Boot Process
Boot Process - Is the steps that a computer takes to start up (boot)

### Preboot Execution Environment PXE
* Optional, used for remote booting / network boot
* **Bootstrap phase:** The first phase of the linux boot process
* Responsible for testing and bringing the hardware up
* **BIOS & UEFI / POST:** which are what actually Initialize the hardware, and loading the bootloader / handing control to the bootloader 

### Bootloader (ex. GRUB2)
* Loads the OS and prepares the system
* Loads Initial RAM disc, and kernel at the same time

### Initial RAM Disk (initrd)
* Essential drivers and tools are loaded to RAM, before switching to the real room filesystem
* A temp file system 

### Kernel
* The core of the OS, managing system resources 
* The brain of the system that talks to the actual hardware 
* Initialize hardware, with drivers and such
* Mounts the real root filesystem

---

# Steps that PXE NIC to boot
1. The NIC starts up / activates 
2. **DHCP Request** - The NIC requests IP from DHCP Server
3. **TFTP Server Location** - The DHCP server gives where the NIC can find the TFTP server that has the boot files on it
4. **Boot Files Transfer** from the TFTP server to the computer
5. The Bootloader is executed to load the OS

---

# Bootloader
Is responsible for loading the OS by selecting and launching the Linux Kernel

* **Computer Starts** - Firmware (BIOS / UEFI) hands control over to GRUB2 (ie. bootloader)
* **GRUB2 Displays boot menu** - the user then selects the OS or kernel they want to boot into
* **GRUB2 reads configs** - Loads `/boot/grub/grub.cfg`, which is a file that instructs the computer on how to display the boot menu, and how to load the kernel and initrd. It contains a bash script that defines menu items, set colors, set boot timeouts, and define partition locations. 
* **GRUB2 loads initrd and kernel** - Loads the initrd and the selected kernel at the same time, and initrd then prepares the system for booting.

> Administrators might want to modify the GRUB2 setting, to do that you can edit the `/etc/default/grub` file and then you use the `update-grub` command to actually put the new setting in place.

---

# Initial RAM Disk (initrd.img)
A temporary root filesystem that is loaded into memory / RAM before the real root filesystem is mounted. 

* Contains essential drivers and kernel modules, and tools needed to initialize hardware and prepare the system for booting. 

**Example: On a system with an encrypted root partition it would go like the following**
1. First boot sequence starts
2. The Initrd and Kernel are both loaded
3. Initrd file contains the encryption modules needed to unlock the disk / encryption keys
4. At the same time the kernel is loading the storage controllers, disc encryption modules, or file system drivers are available. 
    * *This is important because systems with USB drives, encrypted discs, or specialized hardware that the kernel cannot directly access at boot time.*
5. Then it prepares the root filesystem in initrd 
6. Then it transitions from the temporary file system located in RAM to the kernel mounted file system. 

* Initrd typically located in `/boot/initrd.img` 
* Admins can create or update it using tools like `mkinitramfs` or `dracut` 

---

# Kernel
The core of the OS that manages hardware, processes, memory, and system resources.

### Kernel Process 
* Kernel Initializes System
* Detects Hardware
* Mounts Root Filesystem
* Starts Essential Processes

### Making changes to Kernel
* **Adjust Kernel behavior with sysctl** - allows admins to modify kernel parameters at runtime (will only apply once at runtime not permanent). Parameters that control various system functions like networking, memory management, and security settings. 
* To make permanent changes to linux kernel - you have to edit the `/etc/sysctl.conf` file
* Apply the changes made to the `/etc/sysctl.conf` file with `sysctl -p` command

**Example: (To enable IP forwarding)**
This allows the system to permanently route network traffic between interfaces, PC act like router  
* Add - `net.ipv4.ip_forward=1` to `/etc/sysctl.conf` 
* And apply using `sysctl -p`

---

# System Directories



Linux system directories store files and provide a way to organize files
Everything in linux is a file, applications, running processes, commands, etc

**System Directories** - used to manage core functions such as booting, interacting with devices, sharing libraries, and processing info. 

**Filesystem Hierarchy Standard (FHS)** - Defines how files and directories are organized 

* **(/) Root** - Serves as the top-level directory from which all other directories branch out, everything in the system is accessible from this directory. 
* **(/boot) Boot Loader Files** - Contains files needed to start the OS, holds Initrd (`/boot/initrd.img`), the Linux kernel slash (`/boot/vmlinuz`), and the boot loader config (`/boot/grub/grub.cfg`) 
* **(/bin) Essential User Binaries** - Stores executable programs (commands) that are required for basic system functionality. These Binaries are available to all users and are essential for both interactive use and scripts.
    * `/bin/ls` : Listing Files      
    * `/bin/cp` : Copying files    
    * `/bin/mv` : Moving files    
    * `/bin/cat` : displaying file contents     
* **(/sbin) System Binaries** - Similar to /bin but contains commands primarily used by the root user, or sudo. These are system level modification commands. 
    * `/sbin/fsck` : Filesystem checks 
    * `/sbin/reboot` : Restarting the system
    * `/sbin/iptables` : configure & manage kernel built-in firewall 
    * `/sbin/mount` : Mounting filesystems
* **(/lib) Shared Libraries and Kernel Modules** - Provides essential shared libraries required by the programs in /bin and /sbin 
    * `/lib/libc.so.6` : Standard C library used by nearly all programs 
    * `/lib/ld-linux.so.2` : Dynamic linker that loads shared libraries 
* **(/dev) Device Files** - Contains special files that represent hardware devices, these files allow programs to interact with hardware as if they were normal files. ( Think of these files as interfaces, when a program wants to talk to your HD, it doesn’t send a signal directly to the metal; it writes that data to a file in /dev.) 
    * Also contains virtual devices that don’t actually exist but are useful for programmers. 
    * `/dev/sda` : Your first HD or SSD
    * `/dev/nmve0n1` : Your first NVMe 
    * `/dev/tty1` : First terminal window 
    * `/dev/random` : Generating random numbers (for encryption)
    * `/dev/null` : Discard output (silence error messages / delete unwanted output) 
* **(/proc) Process and System Information Directory** - Virtual filesysem that is generated on-the-fly by the kernel that can provide real-time system information. Useful for monitoring, and troubleshooting system performance. Contains dynamically generated files. 
    * `/proc/cpuinfo` : CPU details
    * `/proc/meminfo` : memory usage 
    * `/proc/uptime` : system uptime 
    * `/proc/[PID]/` : holds info about running processes
* **(/etc) Configuration Files** - Stores system-wide configuration files that determine how the system and applications behave. Sys admins modify these files to configure and secure the system. You wouldn’t find the settings for a users program wont be found here cuz its not system wide its user specific. 
    * `/etc/passwd` : user account details 
    * `/etc/shadow` : Password information
    * `/etc/hosts` : Hostname-to-IP-mappings (DNS info)
    * `/etc/resolv.conf` : DNS IP nameservers
    * `/etc/fstab` : Defining filesystem mount points
    * `/etc/ssh/sshd_config` : configuring ssh service 
* **(/home) Home Directory** - Contains personal files, settings, and data for each user of the system, each user has their own subdirectory, and uses a username for each user. 
    * Think of /home as a wall of lockers but each individual locker is a different person's own locker.
    * `/home/bob`
    * `/home/sally`  
* **(/usr) User Programs and Application Data Directory** - Contains system-wide user-installed programs, libraries, and documentation. Individual files aren't typically stored directly in this directory, but are organized in sub-directories based on purpose. 
    * The user /bin and /sbin contains applications that are not critical to system startup, but are commonly used such as text editors, browsers, and compilers. 
    * `/usr/bin` : Holds application binaries used by regular users 
    * `/usr/sbin` : contains admin tools that require sudo privileges 
    * `/usr/lib` : stores shared libraries
    * `/usr/share` : documentation and non-executable data
* **(/var) Variable Data Directory** - Holds variable fields, which are files that change frequently, such as system logs, cache, and db. Essential for storing dynamic content. 
    * `/var/log` : system logs
    * `/var/mail` : email
    * `/var/www` : temporary website data 
    * `/var/run` : are PID files that tell the system which PID a service is using
* **(/tmp) temporary Files Directory** - Used for storing temporary files created by applications as users work within them. This directory is cleared every time the system reboots. 
    * **/tmp vs /var/tmp** * `/tmp` : high turnover, usually wiped on reboot.
    * `/var/tmp` : more persistent intended for tmp files that must survive a reboot. 
    * Example: As a user edits a document, the system may create files to track changes, allowing the user to undo recent actions. Files like these in the /tmp directory are typically deleted automatically upon sys reboot or after a set period of inactivity. Usually has session files, cached downloads,and temporary logs. 
* **(/run) /var/run** - A temporary filesystem used for storing volatile information about the system since it was booted.  
    * Examples: “Lock files” (stored in /var/lock but often a symlink to /run/lock), PID, Unix Domain Sockets.
    * User Runtime Directories (`/run/user/<PID>`) provides a private runtime area for each logged-in user

---

# Some New commands ig: 
* `ls -la /directory path` - the -l flag lists files with extra info, -a flag lists all files including hidden files.
* `tail [file]` : Shows last 10 lines. : Quick check of recent events.
* `tail -f [file]` : Follows the file live.  : Live troubleshooting/monitoring.
* `tail -n [X] [file]` : Shows last X lines.    : Looking further back in time.
* `head [file]` : Shows first 10 lines.  : Checking file format or headers.
* `Dmesg` - reads from the `/var/log/dmesg` file or (kernel ring buffer) provides the hardware boot logs 

---

# Server Architecture
Architecture - The design of a computer’s processor, and how that design affects how the machine processes data. 

Typical linux server architecture is designed for: Reliability, Security, and Stability. To ensure stability, a server requires sufficient amount of RAM to handle multiple processes efficiently, and ample storage space for data management. 

### Most Common Server Architecture
* **x86** - An older 32-bit CPU design developed by intel, processing data in chunks of 32-bits or 4 bytes at a time. 
    * Limits amount of RAM it can access to 4GB
    * i386, i486, i586, i686 all refer to x86 CPU’s 
    * Not recommended for new server deployments 
* **x86_64 (AMD64)** - A 64-bit extension of x86
    * Typical Linux systems support up to 128 TB of RAM *although can be higher
    * Most common, and widely supported 
* **AArch64 (ARM64)** - The 64-bit version of ARM processors, designed for power, efficiency, and scalability 
    * Used mainly in mobile devices, cloud servers, and energy saving data centers
    * Many newer Linux servers use ARM64 to reduce power consumption, while still providing high performance 
* **Reduced Instruction Set Computing - 5th Iteration (RISC-V)** - An open-source CPU design that allows companies and developers to customize processors for their specific needs 
    * Great flexibility in processor design 
    * Used in things like edge computing devices (like AI-security cameras, Smart home Hubs & Gateways), and AI accelerators (Ghost Fish Accelerator tray) 
    * Allows for optimized performance in low-power environments
    * Open-Source instruction set architecture (ISA) 

---

# Linux Distributions
Linux - Open-Source OS, designed to be UNIX like but was made to run on personal computers. As it is open-source anyone is allowed to view, modify, and distribute its source code.  

Linux Distro - An OS, built on the Linux kernel, including system utilities, libraries, software packages, and a package management system. 

### Red Hat Package Manager (RPM-based) Distros 
* Includes Red Hat Enterprise Linux (RHEL), Fedora, CentOS, AlamaLinux, and openSUSE. 
* Will use the `dnf` command for software management (formerly yum) 
* Preferred in Enterprise environments due to their structured release cycles, stability, and strong security focus
* **Red Hat Enterprise Linux (RHEL)** - Offered as a commercial distro, typically requires a paid subscription for access. 
* **Fedora** - RHEL’s upstream counterpart, serves as a testing ground for new tech 
* **CentOS** - A free alternative to RHEL, has been replaced by CentOS Steam, functioning as a rolling preview of RHEL.
* **AlmaLinux & Rocky Linux** - Emerged as community-driven replacements for CentOS, offering a 1:1 binary-compatible, free version of RHEL. 

> RPM-based systems use both dnf, and zypper package managers in the linux terminal. 
> * `dnf` - `sudo dnf install firefox` (used on RHEL, CentOS, Fedora not Opensuse)
> * `zypper` - `sudo zypper install firefox` (used on opensuse not RHEL, CentOS, Fedora)

### dpkg-based 
* Derived from Debian, including Ubuntu, Linux Mint, and Kali Linux. 
* **Ubuntu** - Known for ease of use, frequent updates and excellent hardware compatibility.
* **Linux Mint** - User-friendly alternative to Ubuntu, windows-like interface.
* **Kali Linux** - Specialized distro tailored for cybersecurity professionals and pen testing. 

> These distros use the Debian package format or .deb and package management tools like `apt` or `dpkg`.
> * `dpkg` - A manual installer, you have to manually get the .deb install file before dpkg will be able to actually unpack it.
> * `apt` - Automatically fetches packages and their dependencies from online repositories.
> * `sudo apt-get install -f` to “fix” broken dependencies that dpkg couldn’t handle. 

| Feature | RPM-Based Distros | dpkg-Based Distros |
| :--- | :--- | :--- |
| Package Format | .rpm | .deb |
| Main Package Manager | dnf, yum, zypper | apt, dpkg |
| Update Model | Structured, stable releases | Frequent updates, community-driven |
| Distros | RHEL, Fedora, CentOS, openSUSE | Ubuntu, Linux Mint, Kali Linux |

---

# Display Protocols
* **X Server (X Window System)** : The software that implements the X11 display protocol. One key feature is Network Transparency. Acts as a middleman between applications and display handling. Contains Security vulnerabilities, and performance inefficiencies.
* **Wayland** : A modern display server protocol. Direct communication between applications and display reduces lag and glitches. Improved security by isolating applications.
* **Window Managers** : Control how Linux windows move, resize, and interact on the screen. 
    * **Floating Window Managers** : Allow users to freely move and resize windows. GNOME uses **Mutter**.
    * **Tiling Window Managers** : Automatically arrange windows in a grid. KDE Plasma uses **KWin**.
* **Display Managers** : Allow users to login and establish session control. 
    * **GNOME Display Manager (GDM)** : The graphical login manager for GNOME.
    * **SDDM (Simple Desktop Display Manager)** : Standard for KDE.
