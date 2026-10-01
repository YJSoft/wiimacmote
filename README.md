# About fork

Fork to enable CI to build app

# WiiMacMote

WiiMacMote is a macOS utility for pairing Nintendo Wii controllers over Bluetooth.

![Screenshot](screenshot.png "Screenshot")

## Features

- Pairs new Wii peripherals using the red SYNC button.
- Reconnects saved devices from macOS Bluetooth.
- Shows active controllers first, including buttons, battery, player LEDs, rumble, MotionPlus capability, and extension status.

## Requirements

- macOS 14 or newer.
- Xcode 16 or newer.
- Bluetooth-capable Mac.
- Nintendo Wii Remote, Wii Remote Plus, Wii Fit Balance Board, or compatible Wii extension.

## Build

- Open `WiiMacMote.xcodeproj`.
- Select the `WiiMacMote` scheme and `My Mac` destination.
- Build and run from Xcode.
- Or run `./Scripts/build.sh`.
- To build only for Apple Silicon, run `./Scripts/build.sh --arm64`.
- Run source checks with `./Scripts/verify-source.sh`.

Local copied apps may need fresh ad-hoc signing for macOS Bluetooth permission:

```sh
codesign --force --deep --sign - /Applications/WiiMacMote.app
```

You can also run:

```sh
./Scripts/build.sh --sign-installed
```

## CI builds (arm64)

The `Build macOS arm64 app` GitHub Actions workflow runs on pushes, pull requests,
and manual runs from the **Actions** tab. It uses macOS 15 and Xcode 16.4 to run
source checks and core tests, build the Release app for arm64, and ad-hoc sign it.
The workflow verifies the architecture and signature before packaging the app.

To download a build, open a successful workflow run under **Actions** and download
the **WiiMacMote-arm64** artifact. Extract the artifact ZIP, then extract the
included `WiiMacMote-arm64.zip` and move `WiiMacMote.app` to `/Applications`.
Artifacts are retained for 30 days. The app requires Apple Silicon and macOS 14
or newer.

No signing secrets are required. CI builds are ad-hoc signed, not Developer ID
signed or notarized, so macOS may require approval in **System Settings > Privacy
& Security** before opening the downloaded app. If Bluetooth permission does not
work after copying the app, use the local ad-hoc signing command above.

## Pairing

- Open the Bluetooth section.
- Turn on `Scan`.
- For a new Wii peripheral, press the red SYNC button behind the battery cover.
- For a saved Wii remote, press a face button such as `1` or `2` while scanning is on.
- For a saved Wii Fit Balance Board, press its power button while scanning is on.
- Connection can take a moment while macOS exposes the HID service.

## References

- https://github.com/wiiuse/wiiuse
