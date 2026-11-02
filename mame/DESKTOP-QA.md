# IRIX 5.3 desktop follow-through — 2026-11-02 UTC

## Verified scope

This is a documentation/evidence update for the unchanged experimental MAME SGI
0.289-1 Haiku x86_64 build, not a new binary release or upstream certification.
The trial ran native MAME in a Haiku VM, with a fresh IRIX installation on a
FAT32-backed CHD. No personal files or shared host folders were used.

- 15:44: Inst explicitly reported successful installation. Finalization reached
  100% ELF requickstart and system reconfiguration, ready to restart at 16:41.
- 16:45: warm restart failed with a UTLB-miss exception. This is a known failure,
  not fixed by the subsequent cold-launch result.
- A fresh MAME process, without save-state restoration, reached graphical login
  and a populated desktop by 17:36.
- 18:36: Console `uname -a` returned `IRIX IRIS 5.3 11091812 IP22 mips` and the
  shell prompt returned. An earlier malformed synthetic-input command was
  corrected; both attempts remain visible in the screenshot.
- 18:45: guest shutdown explicitly reached the safe-to-power-off screen.
  18:46: MAME closed normally; no crash dialog and no remaining MAME process.
- Original input defaults were restored after MAME closed; native sync returned.
- A second fresh MAME process, no save state, reached graphical login and then a
  fully populated desktop at 19:36: Toolchest, Console icon, Icon Catalog and
  desktop icons. Cold return-to-desktop persistence passed.

## Input limitations

Guest keyboard and window menus worked. Emulated PS/2 pointer motion was
exercised using temporary MAME Analog Input Adjustments Digital Speed settings
(X=43, Y=25); these QA-only settings were reverted to the original defaults.
An unintended panel binding was also removed. This was not a source fix.

Normal physical mouse behavior remains unverified. Programmatic host motion
was recentered by SDL and did not yield a useful guest-motion test; this does
not establish either production success or a physical-mouse defect.

## Host/storage and other limitations

The original BFS-backed CHD attempt stalled in Haiku page-cache quota wait
(`ModifiedPageQueueWaitIfOverQuota`), with another process waiting on the BFS
journal. Root cause and lock ownership remain unresolved. A read-only BFS
check found no allocation errors, but the guest filesystem was damaged after
unclean shutdown; that partial install was not used for the successful trial.

The fresh FAT-backed install completed without recurrence during this trial.
This is not proof of a general Haiku fix. Warm restart remains an observed
failure. No long-term stability, performance, physical-T460 coverage or complete
hardware/emulation-correctness claim is made.

## Selected UI-only evidence

- [Second cold desktop](irix53-second-cold-desktop.png)
- [Console uname result](irix53-uname.png)
- [Safe-to-power-off screen](irix53-clean-halt.png)

These are diagnostic UI captures, not OS installation media. No ROM, firmware,
ISO, guest disk, RAM snapshot, NVRAM, input configuration or private checkpoint
is included. Underlying UI/trademarks retain their owners' rights; no endorsement
or blanket MIT relicensing is asserted.

## Artifact continuity

The published HPKG, corresponding-source ZIP and native package indexes remain
byte-for-byte unchanged. Earlier embedded READMEs/index summaries reflect the
initial release's QA state; only the specific dated results above supersede them.

```text
0c4b05dd9c9df787d00d02b96e5ed505828fdee4d5c2eae9855f2b020c40244a  mame_sgi-0.289-1-x86_64.hpkg
d38c21531995b007284857d9ed048e444c44d9ddca70c82926dcd9b243845c71  mame-sgi-haiku-corresponding-source.zip
e09b84cffb95ff5b1bab991f1e286483d29c9d4e4bc94c1b19f928675b30cab3  mame/repo
```
