<!-----



Conversion time: 19.96 seconds.


Using this Markdown file:

1. Paste this output into your source file.
2. See the notes and action items below regarding this conversion run.
3. Check the rendered output (headings, lists, code blocks, tables) for proper
   formatting and use a linkchecker before you publish this page.

Conversion notes:

* Docs™ to Markdown version 2.0β2
* Thu Apr 09 2026 05:22:04 GMT-0700 (Pacific Daylight Time)
* Source doc: Basic Linux Concepts 
* Tables are currently converted to HTML tables.
----->


**<span style="text-decoration:underline;"> Administrative Tasks</span>** - are often done though terminal cli, which is more flexible, less resource intensive, faster and easier to automate. 

**<span style="text-decoration:underline;">Boot Process</span>**:

**Boot Process** - Is the steps that a computer takes to start up (boot)



1. ** Preboot Execution Environment PXE**
* Optional, used for remote booting / network boot
1. **Bootstrap phase**
* The first phase of the linux boot process
* Responsible for testing and bringing the hardware up
* BIOS & UEFI / POST which are what actually Initialize the hardware, and loading the bootloader / handing control to the bootloader   \

2. **Bootloader (ex. GRUB2)**
* Loads the OS and prepares the system
* Loads Initial RAM disc, and kernel at the same time
3. **Initial RAM Disk (initrd)**
* Essential drivers and tools are loaded to RAM, before switching to the real room filesystem
* A temp file system 
4. **Kernel**
* The core of the OS, managing system resources 
* The brain of the system that talks to the actual hardware 
* Initialize hardware, with drivers and such
* Mounts the real root filesystem

**<span style="text-decoration:underline;">Steps that PXE NIC to boot</span>**



1. The NIC starts up / activates 
2. **DHCP Request** - The NIC requests IP from DHCP Server
3. **TFTP Server Location** - The DHCP server gives where the NIC can find the TFTP server that has the boot files on it
4. Boot Files Transfer from the TFTP server to the computer
5. The Bootloader is executed to load the OS

**<span style="text-decoration:underline;">Bootloader</span>** - Is responsible for loading the OS by selecting and launching the Linux Kernel



1. **Computer Starts** - Firmware (BIOS / UEFI) hands control over to GRUB2 (ie. bootloader)
2. **GRUB2 Displays boot menu** - the user then selects the OS or kernel they want to boot into
3. **GRUB2 reads configs** - Loads /boot/grub/grub.cfg, which is a file that instructs the computer on how to display the boot menu, and how to load the kernel and initrd. It contains a bash script that defines menu items, set colors, set boot timeouts, and define partition locations. 
4. **GRUB2 loads initrd and kernel** - Loads the initrd and the selected kernel at the same time, and initrd then prepares the system for booting.

Administrators might want to modify the GRUB2 setting, to do that you can edit the /etc/default/grub file and then you use the update-grub command to actually put the new setting in place.

**<span style="text-decoration:underline;">Initial RAM Disk (initrd.img)</span>** - A temporary root filesystem that is loaded into memory / RAM before the real root filesystem is mounted. 

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

Admins can create or update it using tools like ***mkinitramfs*** or ***dracut*** 

**<span style="text-decoration:underline;">Kernel </span>**- The core of the OS that manages hardware, processes, memory, and system resources.

<span style="text-decoration:underline;">Kernel Process</span> 



1. Kernel Initializes System
2. Detects Hardware
3. Mounts Root Filesystem
4. Starts Essential Processes

<span style="text-decoration:underline;">Making changes to Kernel</span>



* Adjust Kernel behavior with ***sysctl*** - allows admins to modify kernel parameters at runtime (will only apply once at runtime **not permanent**). 	Parameters that control various system functions like networking, memory management, and security settings. 
* To make permanent changes to linux kernel - you have to edit the ***etc/sysctl.conf*** file
* Apply the changes made to the etc/sysctl.conf file with ***sysctl -p*** command

<span style="text-decoration:underline;">Example:</span> (To enable IP forwarding)

This allows the system to permanently route network traffic between interfaces, PC act like router  

Add - *net.ipv4.ip_forward=1*  to /etc/sysctl.conf 

And apply using *sysctl -p*  

**<span style="text-decoration:underline;">System Directories</span>**: 



* Linux system directories store files and provide a way to organize files
* Everything in linux is a file, applications, running processes, commands, etc

**<span style="text-decoration:underline;">System Directories</span>** - used to manage core functions such as booting, interacting with devices, sharing libraries, and processing info. 

Filesystem Hierarchy Standard (FHS) - Defines how files and directories are organized 

**<span style="text-decoration:underline;">(/) Root </span>**- Serves as the top-level directory from which all other directories branch out, everything in the system is accessible from this directory. 

**<span style="text-decoration:underline;">(/boot) Boot Loader Files</span>** - Contains files needed to start the OS, holds Initrd (/boot/initrd.img), the Linux kernel slash (/boot/vmlinuz), and the boot loader config (/boot/grub/grub.cfg) 

 

**<span style="text-decoration:underline;">(/bin) Essential User Binaries</span>** - Stores executable programs (commands) that are required for basic system functionality. These Binaries are available to all users and are essential for both interactive use and scripts.

All of the following are examples of commands that are in the /bin directory  



* /bin/ls	: Listing Files     
* /bin/cp	 : Copying files   
* /bin/mv : Moving files    
* /bin/cat : displaying file contents    

**<span style="text-decoration:underline;">(/sbin) System Binaries</span>** - Similar to /bin but contains commands primarily used by the root user, or sudo. These are system level modification commands. 

All of the following are examples of commands that are in the /sbin directory



* /sbin/fsck : Filesystem checks 
* /sbin/reboot : Restarting the system
* /sbin/iptables : configure & manage kernel built-in firewall 
* /sbin/mount : Mounting filesystems

**<span style="text-decoration:underline;">(/lib) Shared Libraries and Kernel Modules</span>** - Provides essential shared libraries required by the programs in /bin and /sbin 

All of the following are examples of files that are in the /lib directory



* /lib/libc.so.6 : Standard C library used by nearly all programs 
* /lib/ld-linux.so.2 : Dynamic linker that loads shared libraries 

**<span style="text-decoration:underline;">(/dev) Device Files</span>** - Contains special files that represent hardware devices, these files allow programs to interact with hardware as if they were normal files. ( Think of these files as interfaces, when a program wants to talk to your HD, it doesn’t send a signal directly to the metal; it writes that data to a file in /dev.) 

Also contains virtual devices that don’t actually exist but are useful for programmers. 

All of the following are examples of files that are in the /dev directory 



* /dev/sda : Your first HD or SSD
* /dev/nmve0n1 : Your first NVMe 
* /dev/tty1 : First terminal window 
* /dev/random : Generating random numbers (for encryption)
* /dev/null : Discard output (silence error messages / delete unwanted output) 

**<span style="text-decoration:underline;">(/proc) Process and System Information Directory</span>** - Virtual filesysem that is generated on-the-fly by the kernel that can provide real-time system information. Useful for monitoring, and troubleshooting system performance.  Contains dynamically generated files. 

All of the following are examples of files that are in the /proc directory



* /proc/cpuinfo : CPU details
* /proc/meminfo : memory usage 
* /proc/uptime : system uptime 
* /proc/[PID]/ : holds info about running processes

**<span style="text-decoration:underline;">(/etc) Configuration Files</span>** - Stores system-wide configuration files that determine how the system and applications behave. Sys admins modify these files to configure and secure the system. **You wouldn’t find the settings for a users program wont be found here cuz its not system wide its user specific. **

All of the following are examples of files that are in the /etc directory



* /etc/passwd : user account details 
* /etc/shadow : Password information
* /etc/hosts : Hostname-to-IP-mappings (DNS info)
* /etc/resolv.conf : DNS IP nameservers
* /etc/fstab : Defining filesystem mount points
* /etc/ssh/sshd_config : configuring ssh service 

**<span style="text-decoration:underline;">(/home) Home Directory</span>** - Contains personal files, settings, and data for each user of the system, each user has their own subdirectory, and uses a username for each user. 

Think of /home as a wall of lockers but each individual locker is a different person's own locker.

Examples of files you would see in /home directory



* /home/bob
* /home/sally  

**<span style="text-decoration:underline;">(/usr) User Programs and Application Data Directory</span>** - Contains system-wide user-installed programs, libraries, and documentation. Individual files aren't typically stored directly in this directory, but are organized in sub-directories based on purpose. 

**The user /bin and /sbin contains applications that are not critical to system startup, but are commonly used such as text editors, browsers, and compilers**. 

All of the following are examples of sub directories you find in /usr



* /usr/bin : Holds application binaries used by regular users 
* /usr/sbin : contains admin tools that require sudo privileges 
*  /usr/lib : stores shared libraries
* /usr/share : documentation and non-executable data

**<span style="text-decoration:underline;">(/var) Variable Data Directory</span>** - Holds variable fields, which are files that change frequently, such as system logs, cache, and db. Essential for storing dynamic content. 

All of the following are examples of files you would find in the /var directory



* /var/log : system logs
* /var/mail : email
* /var/www : temporary website data 
* /var/run : are PID files that tell the system which PID a service is using

**<span style="text-decoration:underline;">(/tmp) temporary Files Directory</span>** - Used for storing temporary files created by applications as users work within them. This directory is cleared every time the system reboots. 

**/tmp vs /var/tmp**  

/tmp :  high turnover, usually wiped on reboot.

/var/tmp : more persistent intended for tmp files that must survive a reboot. 

Example: 

As a user edits a document, the system may create files to track changes, allowing the user to undo recent actions. Files like these in the /tmp directory are typically deleted automatically upon sys reboot or after a set period of inactivity. 

Usually has **session files**, **cached downloads**,and **temporary logs**. 

**<span style="text-decoration:underline;">(/run) /var/run</span>** - A temporary filesystem used for storing volatile information about the system since it was booted.  

Examples



* “Lock files” (stored in /var/lock but often a symlink to /run/lock)
* PID 
* Unix Domain Sockets (communication endpoints used by processes to talk to one another)
* User Runtime Directories (/run/user/&lt;PID> provides a private runtime area for each logged-in user

**<span style="text-decoration:underline;">(/srv)</span>** - **Site-specific data** served by this system, such as data and scripts for web servers, data offered by FTP servers, and repositories for version control systems. (Site-specific data that the system serves to users.)

Examples of files you might see in the /srv directory



* /srv/ftp : FTP server data files that can be downloaded via an FTP service 
* /srv/git : Centralized repositories for systems like Git, SVN, or CVS can be stored here for shared access.
* /srv/docker : Modern admins may use /srv for persistent data for Docker or Podman containers 

Some New commands ig: 



* ls -la /*directory path*        - the -l flag lists files with extra info, -a flag lists all `                                        files including hidden files.

`tail [file] : `Shows last 10 lines. : Quick check of recent events.

<code>tail -f [file] : </code>**Follows** the file live.  : Live troubleshooting/monitoring.

`tail -n [X] [file] : `Shows last X lines.   : Looking further back in time.

`head [file] : `Shows first 10 lines.  : Checking file format or headers.

**Dmesg** - reads from the /var/log/dmesg file or (kernel ring buffer) provides the hardware boot logs 

**<span style="text-decoration:underline;">Server Architecture</span>**

**<span style="text-decoration:underline;">Architecture</span>** - The design of a computer’s processor, and how that design affects how the machine processes data. 

Typical linux server architecture is designed for: **Reliability**, **Security**, and **Stability**

To ensure stability, a server requires sufficient amount of RAM to handle multiple processes efficiently, and ample storage space for data management. 

**<span style="text-decoration:underline;">Most Common Server Architecture</span>**



* **x86** - An older 32-bit CPU design developed by intel, processing data in chunks of 32-bits or 4 bytes at a time. 
    * Limits amount of RAM it can access to 4GB
    * i386, i486, i586, i686 all refer to x86 CPU’s 
    *  Not recommended for new server deployments  \

* **x86_64 (AMD64)** - A 64-bit extension of x86
    * Typical Linux systems support up to 128 TB of RAM *although can be higher
    * Most common, and widely supported  \

* **AArch64 (ARM64)** - The 64-bit version of ARM processors, designed for power, efficiency, and scalability 
    * Used mainly in **mobile devices**, **cloud servers**, and **energy** **saving data centers**
    * Many newer Linux servers use ARM64 to reduce power consumption, while still providing high performance  \

* **Reduced Instruction Set Computing - 5th Iteration(RISC-V)** -  An open-source CPU design that allows companies and developers to customize processors for their specific needs 
    * Great flexibility in processor design 
    * Used in things like **edge computing devices** (like AI-security cameras, Smart home Hubs & Gateways), and AI accelerators (Ghost Fish Accelerator tray) 
    * Allows for optimized performance in low-power environments
    * Open-Source instruction set architecture (ISA) 

**<span style="text-decoration:underline;">Linux Distributions</span>**

**Linux** - Open-Source OS, designed to be UNIX like but was made to run on personal computers.

As it is open-source anyone is allowed to view, modify, and distribute its source code.  

**Linux Distro **- An OS, built on the Linux kernel, including system utilities, libraries, software packages, and a package management system. 

**Red Hat Package Manager (RPM-based) Distros** - Includes Red Hat Enterprise Linux (RHEL), Fedora, CentOS, AlamaLinux, and openSUSE. 



* Will use the *dnf* command for software management (formerly yum) 
*  Preferred in Enterprise environments due to their **structured release cycles**, **stability**, and **strong security focus**
* **Red Hat Enterprise Linux (RHEL)** - Offered as a commercial distro, typically requires a paid subscription for access. 
* **Fedora** - RHEL’s upstream counterpart, serves as a testing ground for new tech 
* **CentOS** - A free alternative to RHEL, has been replaced by **CentOS Steam**, functioning as a rolling preview of RHEL.
* **AlmaLinux & Rocky Linux** - Emerged as community-driven replacements for CentOS, offering a 1:1 binary-compatible, free version of RHEL. 

RPM-based systems use both dnf, and zypper package managers in the linux terminal. 

Example: 

dnf                        - sudo dnf install firefox **(used on RHEL, CentOS, Fedora not Opensuse)**

zypper                  - sudo zypper install firefox **(used on opensuse not RHEL, CentOS, Fedora)**

**dpkg-based** - ** **Derived from Debian, including Ubuntu, Linux Mint, and Kali Linux. 



* **Ubuntu** - Known for ease of use, frequent updates and excellent hardware compatibility and the support of a large software ecosystem. 
* **Linux Mint** - An even more user-friendly alternative to Ubuntu, designed for desktop users who prefer a familiar, windows-like interface with additional customization options. 
* **Kali Linux** - A specialized, Debian-based distro tailored for cybersecurity professionals, and pen testing. 

These distros use the Debian package format or .deb and package management tools like apt or dpkg.

**dpkg** - A manual installer, you have to manually get the .deb install file before dpkg will be able to actually unpack, move binaries, libraries, and manual pages into their correct system directories, and finally configures the package. Now it is installed. 

**apt (advanced Package Tool)** - Automatically fetches packages and their dependencies from online repositories. (Meaning you don’t have to manually install anything when using apt)

Example: 

apt               : sudo apt install firefox

dpkg            : sudo dpkg -i firefox.deb

The problem with dpkg is that it wont automatically get dependencies that aren’t already installed, so the installl will fail and leave the package in an "unconfigured / broken” state. To fix this you would run ***sudo apt-get install -f*** to “fix” the broken dependencies that dpkg couldn’t handle. 

**Package** - A compressed archive file that serves as a complete unit for distributing and installing software. 

**Package Management System** - A collection of tools that automates the process of installing, upgrading, configuring, and removing software. 



* **Manages / automates the process of finding, installing, updating, and removing software.** 
* Works with software dependencies to automate the process of including, updating, and managing third-party code in a project. 

**Software Dependencies** - A component such as a library, module, or another program that an application requires to install or run correctly. 

Example: when a user wants to install firefox, they can use a package manager, which automatically downloads and installs the latest version, while also identifying and installing any dependencies that the web browser needs to function properly. 


<table>
  <tr>
   <td><strong>Feature </strong>
   </td>
   <td><strong>RPM-Based Distros</strong>
   </td>
   <td><strong>dpkg-Based Distros</strong>
   </td>
  </tr>
  <tr>
   <td>Package Format
   </td>
   <td>.rpm
   </td>
   <td>.deb 
   </td>
  </tr>
  <tr>
   <td>Main Package Manager
   </td>
   <td>dnf, yum, zypper 
   </td>
   <td>apt, dpkg  
   </td>
  </tr>
  <tr>
   <td>Update Model 
   </td>
   <td>Structured, stable releases (enterprise-focused)
   </td>
   <td>Frequent updates, community-driven 
   </td>
  </tr>
  <tr>
   <td>Distros 
   </td>
   <td>RHEL, Fedora, CentOS, openSUSE
   </td>
   <td>Ubuntu, Linux Mint, Kali Linux
   </td>
  </tr>
</table>


**<span style="text-decoration:underline;">Display Protocols</span>**



* **X Server(X Window System) :** The software that implements the X11 display protocol. 
    * One key feature is **Network Transparency** meaning users can run applications on one system and display them on another computer connected to the same network.
    * Allows applications to display graphics and enables window movement and resizing. 
    * It acts as a middleman between the applications and display handling 
    * Window rendering 
    * Keyboard input
    * Mouse movements 
    * Contains Security vulnerabilities, and performance inefficiencies
* **Wayland :** A modern display server protocol that specifies the communication between a display server (**Wayland compositor**) and its clients (**your applications**). 
    * Direct communication between applications and display reduces lag and glitches ( like screen tearing, and artifacts)
    *   Improved security by isolating applications from one another, preventing them from interfering with one another 
        * Example: **Each application is rendered in its own separate process**, meaning a misbehaving or crashed application cannot directly interfere with or capture input from other applications, **improving security and stability**. 
    * Not all applications fully support Wayland, Like some screen recording and remote desktop applications in Ubuntu and Fedora still rely on X server for compatibility

**Window Managers :** Control how Linux windows move, resize, and interact on the screen. 



* **Floating Window Managers :** Allow users to freely move and resize windows
    * GNOME is a floating window manager by default 
    * Using **Mutter **: The default **window manager** and **Wayland compositor** for the GNOME desktop environment. 
        * **The Compositor Role :** In a modern Wayland session, Mutter is the display server. It talks directly to the hardware and tells each application exactly when and where to draw its pixels. 
        * **The Window Manager Role :** It handles the “physics” of your desktop-minimizing, maximizing, tiling, and moving windows
        * **GTK Integration :** Since it is the heart of GNOME, it is built specifically to work with the **GTK toolkit**. 
        * ***It is the “backend” of the GNOME shell. If Mutter crashes, your entire graphical desktop disappears. ***
* **Tiling Window Managers :** Automatically arrange windows in a grid. 
    * KDE Plasma is a Tiling Window Manager by default
    * Using **KWin :** The default **window manager ** and **Wayland compositor** for the KDE Plasma desktop environment.
        * **Dual-Protocol Support :** KWin acts as a traditional Window Manager when running on the older X11 protocol, and as a full compositor when running on the modern Wayland Protocol.
        * **The Qt Toolkit :** KWin is built using the Qt toolkit. This is why KDE apps often KDE apps look and behave differently than GNOME apps
        * **Extreme Customization :** KWin allows users to script its behavior, you can change how windows snap, add complex animations, or even make it behave like a tiling window manager (like i3 or Sway)
        * **Compositing Effects : ** KWin handles hardware acceleration, It talks to your GPU (using OpenGL or Vulkan) to make sure that moving a window doesn't stutter or “tear”

**Display Managers :** Allow users to login and establish session control 



* Provides a graphical login screen and starts the desktop environment or window manager 
* **GNOME Display Manager (GDM) :** The graphical login manager (also known as a display manager) for the GNOME desktop environment.  
    * Provides a graphical interface for users to authenticate (login) and then start a desktop session (like GNOME or KDE) using either the Wayland or X11 protocol. 
* **SDDM (Simple Desktop Display Manager) :** It is the default login manager for KDE Plasma and LXQt. 
    * Built using the Qt toolkit, making it the perfect architectural match for the KDE ecosystem.
    * SDDM is famous for being extremely visually customizable, using **QML** for its themes, allowing for logins that include animations, videos, and complex layouts. 
    * Once you provide the correct login info, SDDM terminates its “greater” and launches the Plasma Shell and the KWin window manager/compositor. 
    * Modern versions of SDDM are designed to run without root access where possible, improving the overall security of the boot process. 

**<span style="text-decoration:underline;">Software Licensing:</span>** 

**Software Licensing :** Defines the Rules and permissions that govern how software can be used, modified, and shared. 

Linux is built on the principles of Free and Open-Source software (FOSS), allowing users to modify, share, and distribute it freely. 

**Free Software :** Refers to software that provides users the rights to use, modify, share, and distribute the software without restrictions.

	Think of free as in freedom, not free as in free drinks (ie. not price wise)

While Free software and Open Source software overlap, Free software is more focused on the ethical and philosophical aspects of users rights rather than just the practical benefits of open-source software. 

Examples:



* GNU	
    * Linux Kernel
    * WordPress
    * LibreOffice
    * MySQL/MariaDB

**Open Source Software :** Related to Free Software but focuses more on collaboration, transparency,, and security benefits than user rights. 

Open Source software source code is publicly available, allowing anymore to view, modify, and contribute to its development. 

<span style="text-decoration:underline;">*While all Free Software is Open Source, not all Open Source Software fully aligns with the ethical principles of Free Software \
</span>

**Proprietary Software :** Restricts user freedoms by keeping the source code closed. Users cannot view, modify, or distribute the software.

Companies that develop proprietary software retail full control, often requiring users to purchase a license and agree to strict terms of use. 

Examples: 



* Windows
* macOS
* Adobe software 

**Copyleft Software :** A specific licensing method used in FOSS to ensure the software remains free and open-source in the future versions. It mandates that if anyone modifies and redistributes the software they **must** release those changes under the same license. 

Copyleft guarantees long-term software freedom, but some developers and businesses find it restrictive, as it limitless their ability to incorporate copyleft licensed code into their proprietary projects. 
