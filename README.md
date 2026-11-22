# FireBridge Heater Preview for Fire TV

Connect one compatible HeaterCC Bluetooth diesel heater to Apple Home using an existing Android-based Fire TV. No Home Assistant or additional bridge computer is required.

**Experimental preview, not a finished or universally compatible product.** Discovery and live heater status have been tested on one Fire OS 7 / Android 9 Firestick. Apple Home pairing, heating commands, cooldown, standby, reboot recovery and extended streaming coexistence have not yet been validated in this preview.

## Download

**In Downloader, enter code `2819860` and choose Go.**

**[Full step-by-step guide with Firestick screenshots](INSTALL.md)**

Short link: [aftv.news/2819860](https://aftv.news/2819860). This code downloads v0.5.0-preview.1.


**[Download the Fire TV APK](https://github.com/KMCAI123/firebridge-heater/releases/download/v0.5.0-preview.1/firebridge-heater-preview.apk)** (approximately 102 MB).

[Release and checksums](https://github.com/KMCAI123/firebridge-heater/releases/tag/v0.5.0-preview.1)

## Install and set up

1. On a compatible Android-based Fire TV, install/open Downloader. Enable permission for Downloader to install unknown apps in the Fire TV developer settings. Names and availability vary by Fire TV model. This permission is required for Downloader; the APK cannot authorize itself.
2. Enter **2819860** in Downloader and choose **Go**. Alternatively use the direct APK link above.
3. Install and open **FireBridge Heater Preview**.
4. Choose **Find my VEVOR / HeaterCC heater**. Allow Location on older Android versions, or Nearby devices on newer versions if prompted. Older devices may require location services enabled for BLE discovery. No geographic location is calculated by this app.
5. Close any vendor phone app or other bridge holding the heater's Bluetooth connection. Select your heater and choose **Check heater**. This sends status requests, not a heating command.
6. Wait for current heater status, then select **Show private Apple Home pairing code**.
7. On your iPhone, open Home, choose **+ → Add Accessory**, and scan the displayed code or enter it manually via More Options. The iPhone and Fire TV must have local network discovery access to each other.

Keep the Fire TV powered and within Bluetooth range. The app supports an on/off heating accessory and ambient temperature sensor, not target-temperature or heat-level adjustment. Apple remote access depends on your own Apple Home configuration and home hub.

![FireBridge reading live heater status](docs/images/live-status.png)

See the [illustrated setup guide](INSTALL.md) for every step and troubleshooting.

## Compatibility and evidence

- Android API 26 or newer; ARMv7 or ARM64; Bluetooth Low Energy.
- Not compatible with every Fire TV generation or non-Android Fire TVs.
- VEVOR branding alone is insufficient. The heater must speak the supported HeaterCC/ABBA protocol; discovery candidates are checked using live status.
- Tested: AFTSSS Firestick, Fire OS 7 / Android 9, 32-bit ARM. APK installation through PC ADB, remote-button navigation, heater discovery and valid status were successful. Homebridge reported reachable. The public GitHub APK also downloaded successfully through Downloader 2.0.4 on the Firestick and reached the Android installer; installation of that differently signed release was cancelled to preserve the existing test app.
- Not yet tested: Fresh-device Downloader permission workflow, pairing from iPhone, actual power operation, completed cooldown, cold boot, standby and prolonged operation.
- The APK is about 102 MB and expands its runtime on first launch; allow ample free storage beyond the download size.

Use only with a correctly installed heater and its original controller/power supply. Keep normal heater safety systems intact. Do not depend on this preview as a safety control. Initial heating tests should be supervised.

## Privacy and local access

The app creates fresh local setup credentials and HomeKit pairing identity on the user's own Fire TV. The release contains no developer account credentials, heater pairing state or saved home configuration. Phone setup uses bearer-authenticated HTTP on a trusted LAN; keep the setup URL/QR private. Do not expose setup ports to the internet.

## Updates and signing

Package: `net.localbridge.tv.heaterpreview`. This public release uses a dedicated distribution signing key. Future APK updates must retain that key. The earlier privately sideloaded developer build used a different key and cannot be upgraded in-place to this public release. Back up that private build's state before replacing it. Normal new users do not have that development copy installed.

The distribution APK has identical non-signature content to the device-tested developer APK. The published checksum and signing certificate details are included with the release. A clean-machine reproducible build has not yet been established.

## Notices

This is an independent project, not an official VEVOR, Amazon, Apple or Homebridge product. Third-party software retains its own licenses; bundled runtime notices and an additional `THIRD-PARTY-NOTICES.zip` are supplied with the release. The repository currently distributes the preview APK and documentation; it is not a complete source/build repository.

Report your Fire TV model, Fire OS version and heater model when opening an issue. Do not post setup codes, private URLs, passwords or unredacted logs.
