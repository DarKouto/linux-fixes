# Linux Fixes

Personal log of Linux Contributions. Including hardware fixes, Nvidia boot issues, Kernel audio quirks, etc

## 🛠️ Contribution Index

| Device / System | Component | Status | Contribution Type |
| :--- | :--- | :--- | :--- |
| **Acer Aspire V3-572G** | Audio (ALC283) | **Merged Upstream** | Formal Kernel Patch (Bugzilla) |
| **Gigabyte G5 KF5** | Audio (ALC897) | **Merged (CachyOS)** | Kernel Quirk (SND_PCI_QUIRK) |
| **Nvidia (Laptops)** | Boot / Power Mgmt | **Resolved** | Bug Isolation & Package Revert |

---

## 📂 Documented Fixes

### 1. [Acer Aspire V3-572G: Headset Mic Detection](./fixes/acer-v3-572g-audio.md)
Fixed a long-standing issue where the combo jack failed to detect external microphones or created "ghost" devices.
* **Impact:** Global support in the Mainline Linux Kernel.
* **Key Tech:** HDA Codec, Bugzilla, SND_PCI_QUIRK.

### 2. [Gigabyte G5 KF5 (2023): Audio Jack Fix](./fixes/gigabyte-g5-kf5-audio.md)
Resolved microphone detection issues on the Realtek ALC897 codec by implementing a hardware-specific quirk.
* **Impact:** Out-of-the-box support for CachyOS users.
* **Key Tech:** Modprobe, Kernel Maintainer Collaboration.

### 3. [Nvidia: Boot Freeze (cachyos-settings)](./fixes/nvidia-cachyos-boot-freeze.md)
Identified and helped resolve a critical boot freeze caused by a regression in power management parameters.
* **Impact:** Fixed boot stability for Nvidia laptop users on CachyOS.
* **Key Tech:** System Snapshots, Regression Testing, Collaboration with Peter Jung.

---

## 💡 Why this exists?
Because Linux is about freedom and community. Documenting these "quirks" helps others with the same hardware and ensures that the "it just works" experience becomes a reality for more people.
