# Haiku native fsn prototype repository

Repository base: https://markynose.github.io/haiku-repo/

This publishes an experimental original-SGI-fsn-derived native Haiku app and
a separate, newly authored NativePanels UI development package. It is not an
official SGI release, a 3D toolkit, or inclusion in the official Haiku/HaikuPorts
repositories. The existing Linux/SDL edition is separate.

## Install on Haiku x86_64

```sh
pkgman add-repo https://markynose.github.io/haiku-repo/
pkgman refresh
pkgman install fsn_native
# Optional reusable UI headers/static library/example:
pkgman install native_panels_devel
```

The same base URL can be added through Haiku Repositories preferences for
HaikuDepot. Runtime minimums are declared in the packages: Haiku beta6
hrev59866+79, GCC13 C++ runtime; the fsn app additionally requires FreeType2.14.3.

Launch **FSN Native** in Deskbar Applications to scan `/` automatically. The
initial view shows / and its direct children using the existing directory
warp view, avoiding an unreadable whole-tree overview. Select a child directory
to enter it; Escape then Overview shows the full scanned landscape. The
scan runs in a separate process with Cancel, retry and saved-scene fallback.
The initial snapshot is bounded to depth 4, 1500 nodes, 2000 entries per directory
and a cooperative 2-second traversal budget; a 15-second outer deadline applies.
Skipped entries and limits produce a visibly labelled partial tree. It is not a
complete volume inventory or a live file-manager view. To scan another directory: `fsn-scan-view /boot/home/small-folder`.
Version 0.2 uses checked uint64 file sizes and totals with exact decimal metadata.
Small scenes preserve recovered geometry; larger scenes use explicitly marked
normalized geometry (a modern extension). Open remains limited to 64 MiB.
Select a file/directory and choose Attributes… for bounded, read-only stored
attributes and MIME on Haiku. Values stay local; no MIME sniffing or attribute
writing occurs. Ordinary ext4/Btrfs/BeFS browsing relies on OS-mounted paths,
not new raw drivers. No actual ext4/Btrfs mount was available in this test setup.
Opening a supported file is explicit via Open selected or Enter; the viewer
does not copy, move or delete files. The default app receives the original
path; post-dispatch path/handler races are not eliminated.

## Packages and license boundaries

- `fsn_native-0.2.1-2-x86_64.hpkg`: bounded asynchronous root startup,
  cancellation/retry and saved-scene loading, plus exact uint64 sizes/totals and explicit
  read-only Haiku stored-attribute/MIME inspection. See
  [FSN-Prototype-Notice](FSN-Prototype-Notice.txt).
- `native_panels_devel-0.2.0-1-x86_64.hpkg`: independent PathBar, DetailsView and
  bounded PropertyList native controls, static library, headers, sources and
  standalone examples. MIT applies **only to this newly authored UI code**,
  not recovered fsn or the repository as a whole. See
  [MIT license](native-panels/LICENSE.txt) and [scope notice](NativePanels-NOTICE.txt).

Earlier app package files remain available for history; the index selects app 0.2.1
and NativePanels 0.2.0.

Original-SGI-derived redistribution clearance is not asserted. Project-owner
publication authorization does not grant third-party rights or convert recovered
code into MIT. The app has a separate scoped notice; the bundled font retains
its [DejaVu license](DejaVu-LICENSE.txt). No original SGI executable,
original screenshots, or disassembly corpus is included in this repository.

## Repository format

`repo` is the native HPKR index, `repo.sha256` its checksum, and `packages/` holds
real HPKG files. `SHA256SUMS` verifies deployed files, not publisher identity.
After package changes regenerate the index and checksum on Haiku; package
vendor must match `repo.info`. Preserve the canonical repository identifier
across mirrors. Never replace published package bytes under the same filename.

```sh
package_repo create repo.info packages/*.hpkg
sha256sum repo | cut -d ' ' -f1 > repo.sha256
```

Native UI, package installation, and an isolated HTTP repository were tested on
Haiku beta6 x86_64 in a VM. Public deployment availability is verified separately;
these artifacts do not claim physical hardware or older/32-bit Haiku coverage.

The 0.2 native installed-package, synthetic BFS and exact UINT64_MAX tests passed.
The permission-denial assertion was skipped under the VM root account. The
public-URL guest test remains limited by the environment HTTPS proxy CA; TLS
verification was not bypassed. Local native repository downloads/install were tested.

The 0.2.1 final native tests additionally passed actual `/` startup, controlled
Cancel/retry/close, saved-scene picker cancellation and refs loading, controller
and uint64 regressions, unchanged BFS/UI suites, local 0.2-to-0.2.1 upgrade,
installed no-argument/scene/wrapper launch and isolated repository reinstall.
The app and HPKR index were exported from Haiku and verified by SHA256 on the
host. A stale cached transaction copy was rejected and successfully downloaded
again during local repository testing; checksum verification was not bypassed.
