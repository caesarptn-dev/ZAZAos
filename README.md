# ZAZAos

> **complete by ZAZA but Tech**: a custom Android 14 ROM for the **Samsung Galaxy Note 7 / Note FE**, with its own launcher, terminal, browser and About screen.

![android](https://img.shields.io/badge/Android-14-3DDC84) ![base](https://img.shields.io/badge/base-LineageOS%2021-167C80) ![status](https://img.shields.io/badge/status-experimental%20%2F%20untested-orange) ![devices](https://img.shields.io/badge/devices-gracelte%20%7C%20gracerlte-blue)

## ⚠️ Read first

- **Experimental and untested on hardware.** Back up everything. Flashing can wipe your data and may brick your phone.
- **Battery safety:** the Galaxy Note 7 was recalled worldwide for battery fires. No ROM can fix a defective battery. Never use an original recalled battery.
- ZAZAos is built on someone else's work (see [Credits](#credits)). It is **not** affiliated with Samsung, Google or the LineageOS project.

## What is ZAZAos?

ZAZAos is an unofficial LineageOS 21 (Android 14) build for Exynos 8890 Note devices, re-branded and extended with four original apps. The base system, drivers (Wi-Fi, audio, camera, modem blobs) and kernel are the unmodified ones from the base ROM; the ZAZA apps, boot animation and branding are what this project adds.

| App | What it is |
|---|---|
| **ZAZA Launcher** | Original home screen: a honeycomb of 7 favourites, live battery / storage / memory rings, a big thin clock, and a scrollable **hexagonal app drawer** with search. Swipe up for apps, long-press the clock for wallpaper / settings / About. |
| **ZAZA Terminal** | Shell with command history and quick keys. Built-in `install <apk>` / `uninstall <pkg>` (silent when granted by the ROM, otherwise Android asks to confirm), `apps`, `root` and more. **Root mode** uses `su` when Magisk or KernelSU is installed. |
| **ZAZA Browser** | Tab-based browser with a floating bottom bar, honeycomb speed-dial start page, bookmarks, history, find-in-page, desktop mode, downloads, fullscreen video and a basic ad/tracker blocker. Engine: **Chromium** (the system WebView). |
| **About ZAZAos** | Shows ZAZAos version, device, kernel and the full credit list. Also reachable from the launcher. |

Also changed: ZAZAos boot animation, version strings shown in *Settings → About phone* (build number and the version row show ZAZAos), and the stock browser (Jelly) is replaced by ZAZA Browser.

## Supported devices

| Device | Model | Codename |
|---|---|---|
| Galaxy Note FE | SM-N935F/S | `gracerlte` (the build this ROM comes from) |
| Galaxy Note 7 (Exynos) | SM-N930F/S/K/L | `gracelte` (accepted by the installer, **not confirmed**) |

Snapdragon (US) Note 7 models are **not** supported.

## Installing

1. Unlock the bootloader and install a custom recovery (TWRP) for your model.
2. Copy `ZAZAos.zip` to the phone.
3. In recovery: **Wipe → Format data**, then **Install → ZAZAos.zip**.
4. Reboot. **The first boot takes several minutes** while Android optimises the apps.
5. If the home screen asks which launcher to use, choose **ZAZA Launcher** (*Settings → Apps → Default apps → Home app*).

> The installer logs an error about a missing `e2fsck_static`. This is expected and harmless. The ROM is written as a whole image.

### Root

ZAZAos does **not** ship an always-on `su`, because that would let any app take root silently. To get root, flash **Magisk** or **KernelSU** after installing the ROM, then tap the chip at the top of ZAZA Terminal (or run `root`). Without root, the terminal still works as a normal user and can install apps through Android's package installer.

### Kernel

The ROM boots with its own matched 3.18 kernel. Third-party kernels built for stock Samsung firmware (for example CronosKernel) may not boot Android 14, so flash them separately, only with a backup, and only if you accept the risk.

## Building the apps

The `source/` folder contains all four apps (plain Java, no Gradle).

```bash
cd source
tools/setup.sh                  # apt packages + android.jar (API 34)
python3 tools/mkicons.py        # optional: regenerate icons
tools/build.sh launcher 33
tools/build.sh terminal 29
tools/build.sh browser 33
tools/build.sh about 33         # APKs end up in build/<app>/<app>.apk
```

To put them in a ROM: copy `ZAZALauncher`, `ZAZABrowser`, `ZAZAAbout` into `/system/app/` and `ZAZATerminal` into `/system/priv-app/`, and add this allow-list (required by `ro.control_privapp_permissions=enforce`) as `/system/etc/permissions/privapp-permissions-zaza.xml`:

```xml
<permissions>
  <privapp-permissions package="com.zaza.terminal">
    <permission name="android.permission.INSTALL_PACKAGES"/>
    <permission name="android.permission.DELETE_PACKAGES"/>
  </privapp-permissions>
</permissions>
```

## Known limitations

- Not tested on a real device yet. Please report what works and what doesn't.
- The home screen, drawer and browser UI are new; *Settings* and the lock screen are still the stock LineageOS ones (only their text is branded).
- The terminal has no TTY, so full-screen tools (vi, top) don't work. `Ctrl-C` restarts the session.
- ZAZA Browser uses the system WebView: no extensions, and cookies are shared between tabs.
- The Lineage *Updater* app can't update ZAZAos.

## Credits

- **ZAZA but Tech**: creator of ZAZAos
- **rafa (Telegram)**: special thanks
- **ivanmeler**: unofficial LineageOS 21 build for the Exynos 8890 Note 7 / Note FE that ZAZAos is based on
- **LineageOS Project** and the **Android Open Source Project**: base system (Apache-2.0)
- **Chromium** project: rendering engine used by ZAZA Browser (via the system WebView)
- **Linux kernel** (GPL-2.0), Samsung and the community device kernel

## License

ZAZA apps and scripts: Apache-2.0 (add a `LICENSE` file to your repo). The Android/LineageOS components keep their own licenses; the kernel is GPL-2.0. Proprietary vendor files remain the property of their owners.

## Disclaimer

Provided "as is", without warranty. You are responsible for anything that happens to your device.
