# ZAZAos

> **complete by ZAZA but Tech**: a custom Android 14 ROM for the **Samsung Galaxy Note 7 / Note FE**, with its own launcher, panels, notes, terminal and browser.

![android](https://img.shields.io/badge/Android-14-3DDC84) ![base](https://img.shields.io/badge/base-LineageOS%2021-167C80) ![status](https://img.shields.io/badge/status-experimental-orange) ![devices](https://img.shields.io/badge/devices-gracelte%20%7C%20gracerlte-blue)

## ⚠️ Read first

- **Experimental.** Back up everything. Flashing can wipe your data and may brick your phone.
- **Battery safety:** the Galaxy Note 7 was recalled worldwide for battery fires. No ROM can fix a defective battery. Never use an original recalled battery.
- ZAZAos is built on other people's work (see [Credits](#credits)). It is **not** affiliated with Samsung, Google or the LineageOS project.

## What's new in r3

| | |
|---|---|
| **Orbit launcher** | No dock. Your apps sit on a ring: **swipe to spin it** (it keeps spinning, then snaps), the app that reaches the top moves **into the middle**. Tap the middle to open it, **hold the middle to shuffle** to a random app. Swipe up for a rounded app grid with search. Up to 10 apps on the orbit. |
| **Themes** | Liquid Glass (blur is rendered once, so it doesn't lag), Midnight, Paper, Neon, Candy, Forest. Three icon styles, four clock styles. |
| **New icon pack** | ZAZA glyphs on rounded tiles replace the LineageOS-looking icons in the launcher. |
| **Volume panel on the left** | Replaces the stock panel. Slim glass panel, expands to media / ring / alarm. |
| **Bluetooth popup** | Connect headphones, earbuds, a watch, speaker or car and a popup shows a picture of the device. |
| **Overheat warning** | "Your phone is overheating. Do you want to keep playing?" with *Keep playing* / *Cool down*. Threshold is adjustable. |
| **ZAZA Notification Style** | Replaces the stock heads-up popup with a ZAZA one: Bubble / Banner / Minimal, top or bottom, opacity, corner radius, **upload your own frame**, choose the sound. |
| **ZAZA Notes** | Colored notes, pin, search, reminders, share. Columns, text size, corner radius, sort and default color are all customizable. |
| **ZAZA Feedback** | Send a bug / idea with device info to an email or any app. |
| **Boot animation packs** | Pick ZAZA Hex, Pulse, Minimal, your own `.zip`, or stock. **Needs root** (applied as a Magisk module). |
| **ZAZA easter egg** | Tap the Android / LineageOS version in Settings. |
| **Settings entry** | A "ZAZAos" entry appears in Settings that opens ZAZA Style. |

Also included from r2: **ZAZA Terminal** (shell, `install`/`uninstall`, root mode), **ZAZA Browser** (Chromium WebView), **About ZAZAos**.

## First-run setup (important)

1. Install, boot, open **ZAZA Style** (or *Settings → ZAZAos*).
2. Tap **Enable ZAZA layer**. The ROM gives ZAZA Core the system permission it needs; if Android blocks anything, tap **Grant root** and try again.
3. Tap **Permissions** and allow Bluetooth and notifications.
4. In **ZAZA Notification Style**, turn on *Use ZAZA popups* and allow *Notification access* when asked.

If something can't be done because the system is read-only or protected, ZAZA asks for **root (Magisk)** instead of silently failing.

## How the "hide system, show ZAZA" parts work

Android's own volume panel and heads-up popups live inside SystemUI, which can't be edited without rebuilding it. So ZAZA hides them and draws its own:

- **Volume:** ZAZA Panels (an accessibility service) takes the volume keys and shows the ZAZA panel; the stock panel never appears.
- **Notifications:** the system heads-up popup is switched off (a normal setting) and ZAZA draws its own popups from the notification feed.
- It uses the accessibility-overlay window type, so no "display over other apps" permission is needed. It does **not** read screen content.

Not changed: the notification shade, quick settings and lock screen are still the stock LineageOS ones.

## Supported devices

| Device | Model | Codename |
|---|---|---|
| Galaxy Note FE | SM-N935F/S | `gracerlte` (the build this ROM comes from) |
| Galaxy Note 7 (Exynos) | SM-N930F/S/K/L | `gracelte` (accepted by the installer, **not confirmed**) |

Snapdragon (US) Note 7 models are **not** supported.

## Installing

1. Unlock the bootloader and install TWRP for your model.
2. Join the downloaded parts into `ZAZAos.zip` (see `JOIN.txt`) and copy it to the phone.
3. In recovery: **Wipe → Format data**, then **Install → ZAZAos.zip**. (Updating from r2 without wiping usually works, but wiping is safer.)
4. Reboot. **The first boot takes several minutes.**
5. If asked which launcher to use, choose **ZAZA Launcher**.

> The installer logs an error about a missing `e2fsck_static`. This is expected and harmless.

### Root

ZAZAos does **not** ship an always-on `su` (any app could take root silently). Flash **Magisk** after the ROM, before the first reboot. Root is used by ZAZA only for things Android blocks (boot animation, and as a fallback for settings changes).

## Building the apps

`source/` holds all seven apps (plain Java, no Gradle).

```bash
cd source
tools/setup.sh
tools/build.sh launcher 33
tools/build.sh core 33
tools/build.sh notes 33
tools/build.sh feedback 33
tools/build.sh about 33
tools/build.sh browser 33
tools/build.sh terminal 29
```

To put them in a ROM: `ZAZALauncher`, `ZAZACore`, `ZAZATerminal` go in `/system/priv-app/`; `ZAZABrowser`, `ZAZAAbout`, `ZAZANotes`, `ZAZAFeedback` in `/system/app/`. Privileged permissions must be allow-listed in `/system/etc/permissions/privapp-permissions-zaza.xml` (the ROM enforces this):

```xml
<permissions>
  <privapp-permissions package="com.zaza.terminal">
    <permission name="android.permission.INSTALL_PACKAGES"/>
    <permission name="android.permission.DELETE_PACKAGES"/>
  </privapp-permissions>
  <privapp-permissions package="com.zaza.launcher">
    <permission name="android.permission.READ_WALLPAPER_INTERNAL"/>
  </privapp-permissions>
  <privapp-permissions package="com.zaza.core">
    <permission name="android.permission.WRITE_SECURE_SETTINGS"/>
    <permission name="android.permission.START_ACTIVITIES_FROM_BACKGROUND"/>
  </privapp-permissions>
</permissions>
```

## Known limitations

- Experimental; please report what works and what doesn't (ZAZA Feedback).
- Overheat popup watches battery temperature, not the SoC.
- The volume panel only handles the hardware keys; volume changes from headset buttons use the stock behavior.
- Bluetooth pictures are drawn art picked by device type/name, not real photos.
- ZAZA popups replace heads-up popups only; the lock screen and shade are stock.
- The lock-screen clock style can't be changed (it lives in SystemUI). Clock styles apply to the ZAZA home screen.
- Phone, Messages and Camera are still the LineageOS apps (planned).
- The terminal has no TTY; full-screen tools won't work.

## Credits

- **ZAZA but Tech**: creator of ZAZAos
- **rafa (Telegram)**: special thanks
- **ivanmeler**: unofficial LineageOS 21 build for the Exynos 8890 Note 7 / Note FE that ZAZAos is based on
- **LineageOS Project** and the **Android Open Source Project**: base system (Apache-2.0)
- **Chromium** project: rendering engine behind ZAZA Browser (via the system WebView)
- **Feather icons** (MIT): glyphs in the ZAZA icon pack
- **Linux kernel** (GPL-2.0), Samsung and the community device kernel

## License

ZAZA apps and scripts: Apache-2.0 (add a `LICENSE` file to your repo). Android/LineageOS components keep their own licenses; the kernel is GPL-2.0. Feather icons are MIT. Proprietary vendor files remain the property of their owners.

## Disclaimer

Provided "as is", without warranty. You are responsible for anything that happens to your device.
