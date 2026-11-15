# ViveStream-Next Engines ⚙️

Automated high-efficiency repackager and distribution hub for the core binary engines powering [ViveStream Next](https://github.com/Vivestream-Next/ViveStream-Next).

## 🚀 Overview

ViveStream Next requires a specialized suite of runtime binaries for media acquisition, stream remuxing, dynamic extraction, and transcript alignment. This repository automatically pulls, strips, optimizes, and bundles these dependencies every 12 hours via GitHub Actions.

### 📦 Included Engines

| Binary | Source | Purpose | Optimization |
|---|---|---|---|
| **FFmpeg** | `yt-dlp/FFmpeg-Builds` (Master GPL) | Audio/video remuxing, conversion & HLS streaming | Stripped PE/ELF debug symbols, 7-Zip max Deflate / LZMA2 |
| **FFprobe** | `yt-dlp/FFmpeg-Builds` (Master GPL) | Media stream inspection & stream analysis | Stripped PE/ELF debug symbols, 7-Zip max Deflate / LZMA2 |
| **yt-dlp** | `yt-dlp/yt-dlp-nightly-builds` | YouTube and media stream extractor | Latest Nightly build with verified checksums |
| **QuickJS (`qjs`)** | `quickjs-ng/quickjs` | High-performance JS engine for YouTube EJS challenge solving | Lightweight standalone binary (<1.5 MB) |

---

## ⚡ Artifacts & Downloads

Every automated build publishes the following release assets:

### 1. Unified Engine Bundles (Recommended)
Contains `ffmpeg`, `ffprobe`, `yt-dlp`, and `qjs` packaged together for single-request client setup.
- **Windows x64**: `vivestream-engines-win64.zip`
- **Linux x64**: `vivestream-engines-linux64.tar.xz`

### 2. Standalone FFmpeg Lite Builds
Backwards-compatible drop-in archives containing stripped `ffmpeg` and `ffprobe`.
- **Windows x64**: `ffmpeg-win64-lite.zip`
- **Linux x64**: `ffmpeg-linux64-lite.tar.xz`

### 3. Checksums
- `SHA256SUMS`: Cryptographic manifest verifying all engine artifacts.

---

## 🛡️ Optimization Pipeline

1. **Symbol Stripping**:
   - `llvm-strip --strip-all` removes non-essential relocation notes, `.debug_*` sections, and symbol tables from Windows PE executables.
   - `strip --strip-all` strips ELF binaries on Linux.
2. **Deflate & LZMA2 Compression**:
   - Windows archives use `7z a -tzip -mx=9 -mfb=258 -mpass=15` for maximum standard Deflate compression (100% compliant with standard ZIP readers and Rust's `zip` crate).
   - Linux archives use `XZ_OPT="-9e -T0"` extreme preset multi-threaded LZMA2 compression.
3. **Automated Sync**:
   - Runs automatically on a 12-hour cron schedule (`0 */12 * * *`) and on manual `workflow_dispatch`.

---

## 🔗 Links

- **Main Repository**: [ViveStream-Next](https://github.com/Vivestream-Next/ViveStream-Next)
- **Website**: [vivestream-next.vercel.app](https://vivestream-next.vercel.app/)
