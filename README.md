# Haiku native fsn prototype repository

Repository base: https://markynose.github.io/haiku-repo/

This publishes an experimental original-SGI-fsn-derived native Haiku app and
a separate, newly authored NativePanels UI development package. It is not an
official SGI release, a 3D toolkit, or inclusion in the official Haiku/HaikuPorts
repositories. The existing Linux/SDL edition is separate.

## Install on Haiku x86_64

```sh
pkgman add-repo https://markynose.github.io/haiku-repo/
pkgman install fsn_native
# Optional reusable UI headers/static library/example:
pkgman install native_panels_devel
```

The same base URL can be added through Haiku Repositories preferences for
HaikuDepot. Runtime minimums are declared in the packages: Haiku beta6
hrev59866+79, GCC13 C++ runtime; the fsn app additionally requires FreeType2.14.3.

Launch **FSN Native** in Deskbar Applications to load a saved scene JSON. To
scan and view an actual modest directory: `fsn-scan-view /boot/home/small-folder`.
The current scanner retains the recovered signed32 aggregate-size limit.
Opening a supported file is explicit via Open selected or Enter; the viewer
does not copy, move or delete files. The default app receives the original
path; post-dispatch path/handler races are not eliminated.

## Packages and license boundaries

- `fsn_native-0.1.0-2-x86_64.hpkg`: metadata/notice revision; application executable
  unchanged from the tested native-UI build. See [FSN-Prototype-Notice](FSN-Prototype-Notice.txt).
- `native_panels_devel-0.1.0-1-x86_64.hpkg`: independent PathBar/DetailsView native
  controls, static library, headers, source/example. MIT applies **only to this
  newly authored UI component/example**, not to recovered fsn or the repository
  as a whole. See [MIT license](NativePanels-LICENSE.txt) and
  [scope notice](NativePanels-NOTICE.txt).

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
