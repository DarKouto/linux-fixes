# Fix: Headset Microphone Detection (Acer Aspire V3-572G)

## The Problem
The Combo Jack on the Acer Aspire V3-572G (Realtek ALC283) failed to correctly detect external microphones. Standard workarounds enabled the microphone but resulted in "ghost" entries, showing 3 microphones instead of the 2 physical ones.

## Diagnostic Process
1. **Initial Troubleshooting:** Identified that the system was using the `snd_hda_codec_alc269` driver family.
2. **Bug Reporting:** Since community forum solutions were incomplete, I opened a formal bug report on the **Linux Kernel Bugzilla (#221075)**, providing system logs and codec information.
3. **Collaboration with Maintainers:** - **Charalampos Mitrodimas** suggested testing the specific model parameter: `options snd-hda-intel model=auto,aspire-headset-mic`.
   - Testing confirmed this resolved the "ghost" microphone issue, correctly showing only the Internal and Headset microphones.
4. **Hardware Identification:** To facilitate a permanent kernel-level fix, I extracted the exact hardware IDs using `cat /proc/asound/card1/codec#0`.
   - **Codec:** Realtek ALC283
   - **Subsystem ID:** `1025:0943`

## The Solution (Upstream Kernel Patch)
Based on the provided Subsystem ID and successful test results, a formal patch was submitted to the Linux Kernel Mailing List (LKML) by **Panagiotis Foliadis**.

- **Quirk Line:**
```c
  SND_PCI_QUIRK(0x1025, 0x0943, "Acer Aspire V3-572G", ALC269_FIXUP_ASPIRE_HEADSET_MIC),
```

## Outcome & Contribution

The fix is now part of the official Linux Kernel (Mainline). This ensures out-of-the-box audio support for this Acer model globally.

Resources:

[Bugzilla Ticket: Kernel Bug 221075](https://bugzilla.kernel.org/show_bug.cgi?id=221075)

[Official Patch: Lore Kernel Archive - Patch Submission](https://lore.kernel.org/all/20260221-fix-detect-mic-v1-1-b6e427b5275d@posteo.net/T/#u)
