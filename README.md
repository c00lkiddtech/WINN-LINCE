> **This project now lives in [c00lkiddtech/MACN-cheese](https://github.com/c00lkiddtech/MACN-cheese)** under the `desktop/` folder. New downloads and releases are published there. This repository is kept as a snapshot of the first preview; it is not archived or deleted.

# WINN & LINCE

**WINN** (Windows) and **LINCE** (Linux) are a shared [Tauri 2](https://tauri.app/) app for downloading App Store `.ipa` / `.pkg` files and Google Play `.apk` files. They are the Windows and Linux companions to [MACN-cheese](https://github.com/c00lkiddtech/MACN-cheese), a macOS port of [iPadn](https://github.com/mengangs/ipadn).

This is an unofficial companion. It isn't affiliated with iPadn, Apple, or Google.

## What it does

- **App Store tab** — sign in with an Apple ID (including 2FA), search, and download IPAs you already own. Sign-in, search and download are performed by a bundled copy of [ipatool](https://github.com/majd/ipatool) (MIT), running as a separate program. After login, the session is stored in the OS keyring.
- **Android tab** — search and download free Google Play apps with the bundled `macn-play` helper, using anonymous [Aurora OSS](https://gitlab.com/AuroraOSS/AuroraStore) sessions. Your Google account is never used. If Java is installed, split packages are merged into one `.apk` with APKEditor and signed with a local debug key. Without Java, the app saves `.apks` and tells you why.
- **Downloads only** — finished files go to your **Downloads** or **Desktop** folder. Nothing is ever installed.
- **No DRM stripping** — FairPlay-protected IPAs stay protected.

## Install

Get the latest build from [Releases](https://github.com/c00lkiddtech/WINN-LINCE/releases):

- Windows: `WINN_…_x64-setup.exe` (NSIS). Windows may warn about an unsigned app; that’s expected until it’s signed.
- Linux: `LINCE_….deb` or `.AppImage`.

Merging split Android packages needs a JRE or JDK (`java` on your `PATH`, or `JAVA_HOME`).

## Build from source

Requirements: Rust, Node 20+, and on Linux the usual Tauri packages (`libwebkit2gtk-4.1-dev`, `libgtk-3-dev`, `libssl-dev`, `patchelf`, and friends).

```bash
npm install
bash scripts/prepare-sidecars.sh          # official ipatool + macn-play
cargo build --release --manifest-path helpers/PlayHelper/Cargo.toml --target-dir .build/play-target
bash scripts/prepare-sidecars.sh          # copy the helper you just built
npm run tauri -- build
```

GitHub Actions builds LINCE on Ubuntu and WINN on Windows (see `.github/workflows/release.yml`). Tag a `v*` release to attach both artifacts.

## Licenses

The UI is **GPL-2.0-or-later**, same family as iPadn and MACN-cheese (see [LICENSE](LICENSE)). `macn-play` is a **separate program**, licensed **GPL-3.0-or-later**, because it embeds Aurora OSS device profiles. Credits and license texts are in the app’s Credits tab and in [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md):

- [iPadn](https://github.com/mengangs/ipadn) — GPL-2.0-or-later, mengangs and contributors
- [ipatool](https://github.com/majd/ipatool) — MIT, Majd Alfhaily
- [apkeep](https://github.com/EFForg/apkeep) — MIT, William Budington / EFF
- [rs-google-play](https://github.com/EFForg/rs-google-play) — MIT, Bill Budington / EFF and David Weinstein
- [Aurora Store](https://gitlab.com/AuroraOSS/AuroraStore) — GPL-3.0-or-later, AuroraOSS (profiles and anonymous dispenser)
- [APKEditor](https://github.com/REAndroid/APKEditor) — Apache-2.0, REAndroid
- [uber-apk-signer](https://github.com/patrickfav/uber-apk-signer) — Apache-2.0, Patrick Favre-Bulle

## Limits (MVP)

- iPadn’s version-history picker, SAP/StoreProof path, and TestFlight tab are not included. IPA downloads use ipatool’s public protocol.
- The app never auto-purchases paid App Store or Play apps.
- ipatool’s login flags can briefly expose a password in the process list; the lasting session lives in the OS keyring.
- Large IPAs stay FairPlay-encrypted. That is expected.
