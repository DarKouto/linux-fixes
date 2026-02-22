# Fix: Headset Microphone Detection (Gigabyte G5 KF5 2023)

## The Problem
The Combo Jack on the Gigabyte G5 KF5 (Realtek ALC897) was failing to detect external microphones. When a headset was plugged in, audio output worked correctly, but the system only recognized the internal microphone.

## Diagnostic Process
1. **Initial Workaround:** Discovered that adding `options snd-hda-intel model=dell-headset-multi` to `/etc/modprobe.d/alsa-base.conf` enabled microphone detection, but created a "ghost" entry (showing 3 microphones instead of the 2 physical ones).
2. **Community Collaboration:** Reported the issue on the CachyOS forums. After providing the output of `aplay -l` (which identified the behavior as related to the ALC256 family logic), a maintainer suggested a more specific fixup.
3. **Refined Solution:** Testing `options snd-hda-intel model=alc2xx-fixup-headset-mic` successfully enabled only the 2 correct microphones (Internal + Headset).

## The Solution (Kernel Level)
To make the fix permanent and "Out of the Box" (OOTB) for all users, a Kernel Quirk was added to the CachyOS hardware-specific patches.

- **Hardware ID:** `0x1458:0x900e`
- **Quirk Line:**
```c
SND_PCI_QUIRK(0x1458, 0x900e, "Gigabyte G5 KF5 (2023)", ALC2XX_FIXUP_HEADSET_MIC),
```

## Outcome & Contribution

The fix was merged into the CachyOS Linux Kernel. Users of this laptop model no longer need to manually create modprobe configuration files.

Official Commit by [Eric Naim (1Naim)](https://github.com/1Naim): => [Gigabyte G5 KF5 (2023) Audio Quirk](https://github.com/CachyOS/linux/commit/dd90b60b63e88586f1298d8ed1156e94631bf2aa)
