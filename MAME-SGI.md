# Experimental MAME SGI 0.289-1 for Haiku x86_64

**Unofficial modified test build. IRIX 5.3 installation and two cold desktop launches verified in a Haiku VM; warm restart failed.**
This is MAME 0.289 with local package revision 1, not a stock upstream release,
not upstream 0.289.1, and not a replacement for full official MAME.

## Download and install

- [Tested HPKG](packages/mame_sgi-0.289-1-x86_64.hpkg)
- [Complete corresponding source ZIP](https://github.com/Markynose/haiku-repo/releases/download/mame-sgi-0.289-1-experimental/mame-sgi-haiku-corresponding-source.zip)
- [Experimental release and status](https://github.com/Markynose/haiku-repo/releases/tag/mame-sgi-0.289-1-experimental)

Download the HPKG and install locally from Terminal:

```sh
pkgman install ./mame_sgi-0.289-1-x86_64.hpkg
mame_sgi
```

A separate MAME repository is required because this tested package's vendor
is `Unofficial local build`, whereas the existing FSN repository has a different
vendor. The root FSN repository and all old packages are preserved unchanged.
The MAME-only index was generated natively and selects the exact tested package. Use:

```sh
pkgman add-repo https://markynose.github.io/haiku-repo/mame/
pkgman refresh
pkgman install mame_sgi
```

Do not use the root FSN repository URL for this package.

Run the `mame_sgi` launcher, not the inner executable. It scopes LIBRARY_PATH
to its application-private SDL and does not replace system SDL. It provides
only `mame_sgi` / `cmd:mame_sgi`; no global SDL provider or `mame` command.
Official MAME coexistence is structurally isolated but was not directly tested
because the VM had no official MAME installed.

No ROMs, firmware, IRIX media, guest disk images or save states are included.
Supply media you are authorized to use and explicit ROM, NVRAM and disk paths.
Example after supplying required files:

```sh
mame_sgi indy_4610 -bios b10 -window -video soft
```

Use a clean boot; do not rely on Indy save-state input continuation.
Runtime minimum: Haiku beta6 x86_64 hrev59866+79 with compatible package
runtime dependencies (including SDL2_ttf, GCC C++ runtime, fontconfig,
FreeType, iconv and GL). Tested in a VM, not on a physical T460.

## Changes and evidence

Three Newport corrections: two-bit underlay pixel advancement, inclusive
right edge, and TOPSCAN circular scanout. Private SDL 2.32.10 carries a
Haiku relative-mouse active-view lifecycle fix. No speedup is claimed.

208 native actual-function renderer regression cases passed. The final native
HPKG was created, installed, launched via canonical/PATH launcher, exercised
through selector window/fullscreen and clean exit, then uninstalled. Native
loader evidence confirmed app-private SDL; system SDL2 and SDL2_ttf hashes
were unchanged after install and uninstall.

### Desktop follow-through — 2026-11-02 UTC

The unchanged MAME build and app-private SDL completed a fresh IRIX 5.3
installation with a FAT32-backed CHD in the Haiku VM. Inst explicitly reported
success at 15:44; finalization completed. A cold MAME launch reached the
populated desktop. Console `uname -a` returned
`IRIX IRIS 5.3 11091812 IP22 mips`. Clean guest shutdown reached the safe-to-power-off
screen, followed by normal MAME exit at 18:46. Original input defaults were
restored; a second cold launch reached a fully populated desktop at 19:36.

**Known failure:** the installation's warm restart raised a UTLB-miss exception.
Cold launch success does not establish working warm restart or general stability.
Keyboard/menu interaction and emulated PS/2 pointer movement were exercised;
pointer testing temporarily changed MAME Digital Speed settings, then restored
the original defaults. Normal physical mouse behavior remains unverified;
host synthetic motion tests were inconclusive.

The earlier BFS-backed CHD attempt stalled in a Haiku page-cache quota wait
(`ModifiedPageQueueWaitIfOverQuota`), with another process waiting on the BFS
journal. The clean FAT-backed reinstall succeeded, but this is a workaround
trial, not an established root cause or OS fix. No performance, physical-hardware,
complete emulation-correctness or long-term stability claim is made.

[Detailed dated QA and selected UI screenshots](mame/DESKTOP-QA.md).
The binary, complete corresponding-source archive and native repository index
are unchanged. Their earlier README/metadata warnings are historical and are
superseded only by the specific dated results here; remaining limitations still
apply. No installed OS disk, ROM, firmware, ISO, RAM snapshot, NVRAM or private
checkpoint is distributed with this evidence update.

## Licensing and corresponding source

[MAME notice](MAME-SGI-COPYING.txt), [GPL version 2](MAME-SGI-GPL-2.0.txt),
[SDL zlib licence](MAME-SGI-SDL-LICENSE.txt). Individual files and third-party
components retain their own notices. Complete notices are included in both
package and source. The unrelated NativePanels MIT licence does not cover MAME.

The attached corresponding-source ZIP includes complete pinned MAME archives,
all three patches, SDL release source, HaikuPorts/local patches, build and
package recipes, licence texts and QA. Use that ZIP, **not GitHub's automatic
repository source archive**, to reproduce the modified program.

SHA256:

```text
0c4b05dd9c9df787d00d02b96e5ed505828fdee4d5c2eae9855f2b020c40244a  mame_sgi-0.289-1-x86_64.hpkg
d38c21531995b007284857d9ed048e444c44d9ddca70c82926dcd9b243845c71  mame-sgi-haiku-corresponding-source.zip
```
