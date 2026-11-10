# SpellForce GOG / 1.54 — Missing Health Bar Hex Patch

A historical hex patch for missing health bars in **SpellForce's GOG / 1.54 package**. It was created by editing the game binary directly in a hex editor. This repository preserves the resulting executable and the accompanying Direct3D wrapper.

**Format:** binary hex patch · **Platform:** Windows · **Game executable:** x86 (32-bit)

> **Known limitation:** avoid opening the in-game **Options** menu when using this patch. This instruction comes from the original repository description; the exact failure mode was not documented.

## What is included

| File | Architecture | Size | Embedded DLL description |
| --- | --- | ---: | --- |
| [SpellForce 1.54.exe](<SpellForce 1.54.exe>) | x86 | 10,358,864 bytes | — |
| [SysWOW64/v3d9.dll](SysWOW64/v3d9.dll) | x86 | 455,168 bytes | dgVoodoo 2.78.1 — Direct3D9 |
| [System32/v3d9.dll](System32/v3d9.dll) | x64 | 530,432 bytes | dgVoodoo 2.78.1 — Direct3D9 |

Both DLLs report file version `4.9.0.904`. The executable has no embedded file version; **1.54 is the original filename's version label**, not a version detected from its resources.

There is no separate source project or compilation step: the patch was made through hex edits. The original unmodified executable and a byte-offset change log are not included, so the complete edit sequence cannot be reconstructed from this package alone.

## Package layout and use

An existing game installation matching the intended GOG / 1.54 build is required. This is not a standalone game installation.

1. Close the game and back up the original executable and any DLL you intend to replace.
2. Verify the downloaded files with the command below.
3. The original packaging places the patched executable in the game directory. The two DLL folders record the original system-wide destinations on **64-bit Windows**: `SysWOW64` contains the x86 DLL, while `System32` contains the x64 DLL.
4. Keep the architecture distinction intact: the **32-bit game requires the x86 wrapper**.
5. Keep the original Options-menu limitation in mind. To undo the installation, restore your backed-up files and remove only files you added.

The folder names follow [Windows' system-directory architecture convention](https://learn.microsoft.com/en-us/windows/win32/winprog64/file-system-redirector); this repository contains no automated installer.

An x86 wrapper placed beside the game executable may allow the patch to stay within the game directory, because Windows normally searches there before the system directory. This is an inference from the [documented DLL search order](https://learn.microsoft.com/en-us/windows/win32/dlls/dynamic-link-library-search-order). **App-local installation has not been tested with this package.**

## Verify the files

From the extracted repository in PowerShell:

```powershell
powershell -NoProfile -File .\Verify-Package.ps1
```

[Verify-Package.ps1](Verify-Package.ps1) checks all three binaries against [SHA256SUMS](SHA256SUMS). Missing or modified files cause it to fail. It reads the package without executing the game, loading DLLs, or installing files.

The hashes identify this archive's files; they do not establish game compatibility.

## Status and related version

The documentation refresh inspected file sizes, PE architectures, embedded DLL metadata, and SHA-256 hashes. **The game, Options-menu behavior, and health bar fix were not run or retested.** The original binaries are unchanged.

For the separate 1.61 package, see [SpellForce Platinum Edition 1.61](https://github.com/Rick-laboratory/Spellforce-Patch-1.61-MISSING-HEALTHBAR-Fix). Keep the two packages separate; they contain different executables and wrapper versions.

The DLL resources identify [dgVoodoo 2 by Dege](https://github.com/dege-diosg/dgVoodoo2). Game and wrapper components retain their respective ownership; this archive does not add a new license to those binaries.
