# Fix: Headset Microphone Detection (Acer Aspire V3-572G)

## The Problem
The Combo Jack on the Acer Aspire V3-572G (Realtek ALC283) failed to correctly detect external microphones. When a headset was plugged in, audio output worked correctly, but the system only recognized the internal microphone.

## Diagnostic Process
1. **Initial Workaround:** Discovered that adding `options snd-hda-intel model=auto,dell-headset-multi` to `/etc/modprobe.d/alsa-base.conf` enabled microphone detection, but created a "ghost" entry (showing 3 microphones instead of the 2 physical ones).
2. **Bug Reporting:** Since community forum solutions were incomplete, I opened a formal bug report on the [**Linux Kernel Bugzilla (#221075)**](https://bugzilla.kernel.org/show_bug.cgi?id=221075), providing system logs and codec information.
3. **Collaboration with Maintainers:** - [**Charalampos Mitrodimas (charmitro)**](https://github.com/charmitro) suggested testing the specific model parameter: `options snd-hda-intel model=auto,aspire-headset-mic`.
   - Testing confirmed this resolved the "ghost" microphone issue, correctly showing only the Internal and Headset microphones.
4. **Hardware Identification:** To facilitate a permanent kernel-level fix, I extracted the exact hardware IDs using `cat /proc/asound/card1/codec#0`.
   - **Codec:** Realtek ALC283
   - **Subsystem ID:** `1025:0943`

## The Solution (Upstream Kernel Patch)
Based on the provided Subsystem ID and successful test results, a formal patch was submitted to the Linux Kernel Mailing List (LKML) by [**Panagiotis Foliadis (panosfol)**](https://github.com/panosfol)

- **Quirk Line:**
```c
  SND_PCI_QUIRK(0x1025, 0x0943, "Acer Aspire V3-572G", ALC269_FIXUP_ASPIRE_HEADSET_MIC),
```

## Outcome & Contribution

The fix is now part of the official **Linux Kernel (Mainline)**. This ensures out-of-the-box audio support for this Acer model globally.
- [Official Lore Kernel Archive: [PATCH] ALSA: hda/realtek: Add quirk for Acer Aspire V3-572G](https://lore.kernel.org/all/20260221-fix-detect-mic-v1-1-b6e427b5275d@posteo.net/T/#u)

It is also in the **CachyOS Kernel**
- Official Commit made by [Eric Naim (1Naim)](https://github.com/1Naim) ===> [ALSA: hda/realtek: Add quirk for Acer Aspire V3-572G](https://github.com/CachyOS/linux/commit/bd318964ba38d38ceab1611d91084fc7e1a6d5ad)
