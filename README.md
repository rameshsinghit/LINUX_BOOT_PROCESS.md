# 🐧 Linux Boot Process: Technical Deep Dive

A comprehensive guide explaining the journey from hardware power-on to the user login prompt.

---

## 🏗️ The Full Flow
**Power ON** ➔ **BIOS/UEFI** ➔ **MBR/GPT** ➔ **GRUB2** ➔ **Kernel** ➔ **initramfs** ➔ **systemd** ➔ **Login**

---

## 🔍 Stage-by-Stage Breakdown

### 1. Power ON (Hardware Initialization)
* **Execution:** Power reaches the motherboard, CPU resets, and jumps to a fixed firmware address containing BIOS or UEFI code.
* **Control:** CPU starts first, then BIOS/UEFI takes control automatically.
* **Failure Signs:** System beeps, black screen, or “No boot device found”.

### 2. BIOS / UEFI (The Boot Manager)
* **Role:** Stored in a non-volatile chip on the motherboard, it decides which disk to boot from.
* **BIOS (Legacy):** Works with MBR; loads the first 446 bytes of the disk.
* **UEFI (Modern):** Works with GPT; boots from the **EFI System Partition (ESP)** usually at `/boot/efi`.

### 3. MBR or GPT (Locating the Bootloader)
* **MBR:** The first 512-byte sector of the disk containing the Bootloader Stage-1 (446 bytes), Partition Table (64 bytes), and Magic Number (2 bytes).
* **GPT:** Used in UEFI systems; utilizes an EFI binary (e.g., `grubx64.efi`) to move control to GRUB2.

### 4. GRUB2 (The Bootloader)
* **Responsibilities:** Displays the boot menu, allows kernel selection, and passes boot parameters.
* **Handover:** Loads the **Kernel** (`vmlinuz-*`) and **initramfs** into memory.
* **Failure Signs:** `grub rescue>` prompt appears.

### 5. Kernel (The OS Heart)
* **Actions:** Detects hardware, loads essential drivers, starts memory management, and initializes **PID 1 (systemd)**.
* **Failure Signs:** Kernel panic or “Unable to mount root fs”.

### 6. initramfs (Temporary Root FS)
* **Role:** A helper that provides necessary drivers (LVM, RAID, Multipath, Encryption) to access the real root `/` filesystem.
* **Handover:** Switches from temporary root to the real root and returns control to the Kernel.

### 7. systemd (Service Manager)
* **PID 1:** The first process that starts all system services.
* **Tasks:** Mounts filesystems, activates networking, and starts services like SSH and firewalld.
* **Failure Signs:** Boot drops to emergency mode (often due to `/etc/fstab` issues).

### 8. Login Prompt
* **Final Targets:** Reaches `multi-user.target` for CLI or `graphical.target` for GUI.

---
# LINUX_BOOT_PROCESS.md
