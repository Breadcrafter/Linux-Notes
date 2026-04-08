# Linux System Administration & Architecture Notes (Security+)

---

## 1. Administrative Tasks
* **CLI Advantage:** Most administrative tasks are done via terminal/CLI.
    * **Flexibility:** More options than GUI.
    * **Efficiency:** Lower resource overhead and faster execution.
    * **Automation:** Easier to script and automate repetitive tasks.

---

## 2. The Linux Boot Process
The sequence of steps a computer takes to start up.



### Phase 1: Pre-Boot & PXE
* **PXE (Preboot Execution Environment):** Used for network booting. 
    1. **NIC Activates:** Starts up and sends a **DHCP Request**.
    2. **TFTP Server:** DHCP provides the location of the TFTP server containing boot files.
    3. **Transfer:** Boot files are sent to the local machine.
* **Firmware (BIOS/UEFI):** Performs **POST** (Power-On Self-Test) and hands control to the bootloader.

### Phase 2: Bootloader (GRUB2)
* **Role:** Loads the OS, Kernel, and Initial RAM Disk simultaneously.
* **Config:** Reads settings from `/boot/grub/grub.cfg`.
* **Modification:** Admins edit `/etc/default/grub` and run the `update-grub` command to apply changes.

### Phase 3: Initial RAM Disk (initrd)
* **Definition:** A temporary root filesystem in RAM.
* **Function:** Contains essential drivers/modules needed to mount the *real* root filesystem (e.g., encryption drivers for LUKS, RAID drivers).
* **Location:** `/boot/initrd.img`.

### Phase 4: Kernel
* **Role:** The "Brain" of the OS. Manages hardware, memory, and processes.
* **Execution:** Initializes hardware, mounts the real root filesystem, and starts system processes.
* **Kernel Tuning:**
    * `sysctl`: Modify parameters at runtime.
    * `/etc/sysctl.conf`: Permanent changes (e.g., `net.ipv4.ip_forward=1` to make the PC act as a router).
    * `sysctl -p`: Apply changes from the config file.

---

## 3. Filesystem Hierarchy Standard (FHS)
Everything in Linux is treated as a file.

| Directory | Purpose | Key Content / Examples |
| :--- | :--- | :--- |
| `/` | **Root** | Top-level directory; start of the tree. |
| `/boot` | **Static Boot Files** | Kernel (`vmlinuz`), Grub, and `initrd.img`. |
| `/bin` | **User Binaries** | Common commands: `ls`, `cp`, `mv`, `cat`. |
| `/sbin` | **System Binaries** | Root-level commands: `fsck`, `reboot`, `iptables`. |
| `/lib` | **Shared Libraries** | Code modules used by `/bin` and `/sbin`. |
| `/dev` | **Device Files** | Hardware interfaces: `/dev/sda` (Disk), `/dev/null` (Black hole). |
| `/proc` | **Process/Kernel** | Virtual info: `/proc/cpuinfo`, `/proc/meminfo`. |
| `/etc` | **Config Files** | System settings: `passwd`, `shadow`, `hosts`. |
| `/home` | **User Data** | Personal folders: `/home/bob`. |
| `/usr` | **User Applications** | `/usr/bin` (installed apps like Firefox). |
| `/var` | **Variable Data** | Logs (`/var/log`), mail, and spool files. |
| `/tmp` | **Temporary** | Wiped on reboot; transient application data. |
| `/run` | **Runtime Data** | PIDs and lock files; cleared on reboot. |

---

## 4. Server Architecture
The design of the CPU determines how data is processed.

* **x86:** 32-bit (i386, i686). Max 4GB RAM. Legacy.
* **x86_64 (AMD64):** Standard 64-bit architecture. Supports vast RAM (128TB+).
* **AArch64 (ARM64):** Power-efficient. Used in mobile, cloud, and modern data centers.
* **RISC-V:** Open-source ISA. Highly customizable for AI and edge devices.

---

## 5. Linux Distributions & Package Management

### RPM-Based (Enterprise Focused)
* **Distros:** RHEL, Fedora, CentOS, AlmaLinux, openSUSE.
* **Tools:** `dnf` (or older `yum`), `zypper` (openSUSE).
* **Format:** `.rpm`.

### Dpkg-Based (Community/User Focused)
* **Distros:** Debian, Ubuntu, Linux Mint, Kali Linux.
* **Tools:** * `apt`: Handles repositories and dependencies automatically.
    * `dpkg`: Low-level installer for `.deb` files (doesn't handle dependencies).
* **Dependency Fix:** `sudo apt-get install -f`.

---

## 6. Display Protocols & Managers

### Protocols
* **X11 (X Server):** Older, supports network transparency (run app here, display there). High overhead.
* **Wayland:** Modern, secure, and smooth. Better application isolation.

### Managers & Compositors
* **Mutter (GNOME):** Handles window physics (GTK-based).
* **KWin (KDE):** Highly customizable window management (Qt-based).
* **Display Managers:** Graphical login screens like **GDM** (GNOME) or **SDDM** (KDE).

---

## 7. Useful Administration Commands
* `ls -la`: List all files including hidden (`-a`) with details (`-l`).
* `tail -f /var/log/syslog`: Monitor logs in real-time.
* `dmesg`: View the kernel ring buffer (hardware/boot logs).
* `sysctl -p`: Reload kernel parameters from config.
