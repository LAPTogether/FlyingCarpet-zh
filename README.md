## Version 8 adds the option to send folders from Android and iOS

Android version is available [here](https://play.google.com/store/apps/details?id=dev.spiegl.flyingcarpet), or if you prefer to sideload, `android_FlyingCarpet.apk` is available on the [releases](https://github.com/spieglt/FlyingCarpet/releases) page.

iOS version [here](https://apps.apple.com/us/app/flying-carpet-file-transfer/id1637377410) or search the App Store for "Flying Carpet File Transfer".

Linux, macOS, and Windows versions are available on the [releases](https://github.com/spieglt/FlyingCarpet/releases) page. Installers and standalone executable versions available.

# Flying Carpet

Send and receive files between Android, iOS, Linux, macOS, and Windows over ad hoc WiFi. No shared network or cell connection required, just two devices with WiFi chips in close range.

Don't have a flash drive? Don't have access to a wireless network? Need to move a file larger than 2GB between different filesystems but don't want to set up a network share? Try it out!

[Demo video](https://youtu.be/-RKiSmo-Bns)

## Screenshots:

<img src="screenshots/android.png" height=500> <img src="screenshots/ios.png" height=500> <img src="screenshots/linux.png" height=500> <br> <img src="screenshots/mac.png" height=500> <img src="screenshots/windows.png" height=500>

## Use:

**Linux:** Download the `.AppImage` file from the [releases](https://github.com/spieglt/FlyingCarpet/releases) page for a standalone version, or if you're on a Debian-based distribution, download the `.deb` file and install it with `apk` or `dpkg`.

**macOS:** Download the `.dmg` disk image file from the [releases](https://github.com/spieglt/FlyingCarpet/releases) page. Double-click to mount it and drag the `.app` bundle inside to your Applications folder. Or if you use Homebrew, run `brew install flying-carpet`.

**Windows:** Download the `.msi` installer from the [releases](https://github.com/spieglt/FlyingCarpet/releases) page, or `FlyingCarpet.exe` for a standalone version.

## Compilation Instructions:

+ Install [Rust](https://www.rust-lang.org/tools/install).

+ Run `cargo install tauri-cli` to install Tauri.

+ Mac only: Install XCode. Open `FlyingCarpetMac/FlyingCarpetMac/FlyingCarpetMac.xcodeproj` and build it.

+ For Linux, install dependencies. Ubuntu 20 example:
```
sudo apt install libsoup2.4* libjavascriptcoregtk* libgdk-pixbuf2.0* librust-pango-sys-dev libgdk3.0* librust-atk-dev librust-atk-sys-dev librust-gdk* libwebkit2gtk* librsvg2-dev
```

+ Run `cargo tauri dev` to run a development version or `cargo tauri build` to create release artifacts.

## Restrictions:

+ Apple devices can only transfer to/from Android, Linux, and Windows as they can no longer programmatically run hotspots. Use AirDrop instead for Apple-to-Apple transfers.

+ Disables your wireless internet connection while in use. (Does not apply to Windows or Android when hosting the hotspot.)

+ macOS sometimes switches back to a wireless network with internet connectivity during particularly long transfers.

+ The Android version requires at least Android 11/API level 30. There is a version requiring only Android 4.4/SDK 19 on the releases page, but I've heard two reports of it not working and I am not able to test or debug. The Android version does not work on some Xiaomi, MIUI, or HarmonyOS devices, and possibly other Android-like OSes. I don't own these devices and so can't test, but it seems like this is due to lack of support for the [LocalOnlyHotspot](https://developer.android.com/develop/connectivity/wifi/localonlyhotspot) API. It has been confirmed to work on at least one Xiaomi phone.

+ Requires Windows 10 or later.

## Planned Features

+ Bluetooth for connection negotiation (instead of QR code scanning or manual entry)?

+ Add Flying Carpet shortcut to iOS Share menu.

## Questions That Could Be Asked at Some Point:

+ **Wasn't this a Go repo?** Yes, carcinization has come for the gopher. There were several issues I didn't know how to solve in the Go/Qt paradigm, especially with Windows: not being able to make a single-file executable, needing to Run as Administrator, and having to write the WiFi Direct DLL to a temp folder and link to it at runtime because Go doesn't work with MSVC. Plus it was fun to use `tokio`/`async` and `windows-rs`, with which the Windows networking portions are written. The GUI framework is now Tauri which gives a native experience on all platforms with a very small footprint. The Android version is written in Kotlin and the iOS version in Swift. Neither mobile codebase is in this repository.

+ **You're using SHA-256 to derive the key from a password. Isn't that bad? Shouldn't you be using a Password-Based Key Derivation Function like Scrypt or Argon2?** I was doing this before, but it wasn't strictly necessary because these keys are only used during the file transfer. For an attacker to intercept the data in transit, they'd need to be on the hotspot network, which is protected by WPA2, so they'd need to shoulder-surf the password or QR code. The change was made because I couldn't find a good Scrypt or Argon2 implementation on all platforms.

+ **Why are you using AES-GCM at all if there's already WPA2 then?** When I started working on this project in 2017, I was trying to allow for IBSS WiFi networks on macOS that didn't use authentication. I was using the wrong encryption (and incorrectly) then, and later I added AES-GCM because it's the only good and official-ish AEAD implementation I could find in all of Go, Swift, Kotlin, and now Rust. If any cryptographers read this and find that I'm still being dumb, please let me know.

## Questions for Hypothetical Users:

+ Did anyone use the CLI version? Is there need for one now? With the original Go version, I wrote the CLI first and then learned how to make a GUI. For this version, with Tauri requiring Tokio, the async stuff is pretty deeply hooked in, so it didn't make sense to do the CLI first. It may be relatively straightforward to make a CLI version, but I might not do so unless people will use it. Please let me know: theron@spiegl.dev.

If you've used Flying Carpet, please send feedback to theron@spiegl.dev. Thanks for your interest! Please also check out https://github.com/spieglt/cloaker, https://cloaker.mobi, and https://github.com/spieglt/whatfiles.
