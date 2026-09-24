# Install APK — macOS Finder service

A macOS Quick Action that installs an APK on a connected Android device or emulator, straight from Finder.

Right-click an `.apk` file in Finder and choose **Quick Actions › Install APK** (or **Services › Install APK**).

## Features

- **Device picker** — when several devices are connected, a picker lists physical devices first, then emulators (shown with their AVD name). Select one or more with ⌘-click. With a single device, the APK is installed straight away.
- **Reinstall** — the APK is installed with `adb install -r`, so an existing install is updated and its data kept.
- **Launch after install** — the app is started on each device where the install succeeded.
- **Uninstall on failure** — if the install fails (version downgrade, signature mismatch, …) and the app is already on the device, a dialog shows the error and offers to uninstall the app and install again. The app data is lost in that case.
- **Notification** — a notification shows the result for each device, for example `Pixel 8: Success`.

## Requirements

- macOS with Automator (built in).
- The Android SDK with `platform-tools` (for `adb`) and `build-tools` (for `aapt2`, used to read the package name to launch or uninstall the app).

The SDK is found in this order:

1. `$ANDROID_HOME`
2. `$ANDROID_SDK_ROOT`
3. `~/Library/Android/sdk` (the Android Studio default)

Services do not load your shell profile, so `ANDROID_HOME` is usually not set when the service runs. If your SDK is not in `~/Library/Android/sdk`, edit the `SDK=` line in the script (see [Customize](#customize)).

## Install

```bash
git clone git@github.com:LunabeeStudio/adb-service-macos.git
mkdir -p ~/Library/Services
cp -R "adb-service-macos/Install APK.workflow" ~/Library/Services/
/System/Library/CoreServices/pbs -flush
```

If **Install APK** does not appear in the Finder context menu:

- Open **System Settings › Keyboard › Keyboard Shortcuts… › Services** and make sure **Install APK** is enabled under **Files and Folders**.
- Relaunch Finder (hold ⌥ and right-click the Finder icon in the Dock, then choose **Relaunch**).

The first time the device picker or a dialog opens, macOS may ask you to allow it.

## Customize

Open `~/Library/Services/Install APK.workflow` in Automator (double-click it). The script is in the **Run Shell Script** action. Save with ⌘S.

## Uninstall

```bash
rm -r ~/Library/Services/"Install APK.workflow"
```

## Credits

Based on [robertocaldas/AdbInstallService](https://github.com/robertocaldas/AdbInstallService).
