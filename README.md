# FireBridge Heater Preview for Fire TV



> **WARNING — USE AT YOUR OWN RISK.** Experimental heater-control software can fail and may contribute to fire, carbon monoxide poisoning, serious injury, death or property damage. **Never operate a heater through this project unattended or unmonitored. A responsible adult must be physically present, awake and watching throughout startup, operation, shutdown and cooldown, and must verify that the heater functions correctly.** Do not rely on an app, camera or notification as a substitute. Do not enable unattended starts or automations.

>

> **Before downloading or using the APK, read the [full safety and liability disclaimer](DISCLAIMER.md).** Software is supplied as-is, with warranty and liability exclusions only to the extent permitted by law. Non-waivable legal rights remain protected.



Connect one compatible HeaterCC Bluetooth diesel heater to Apple Home using an existing Android-based Fire TV. No Home Assistant or additional bridge computer is required.



**Experimental preview, not a finished or universally compatible product.** Discovery and live heater status have been tested on one Fire OS 7 / Android 9 Firestick. Apple Home pairing, heating commands, cooldown, standby, reboot recovery and extended streaming coexistence have not yet been validated in this preview.



## Download

**[Download v0.5.1-preview.1 with the in-app safety acknowledgment](https://github.com/KMCAI123/firebridge-heater/releases/download/v0.5.1-preview.1/firebridge-heater-preview.apk)** (approximately 102 MB).

**[Full step-by-step guide with Firestick screenshots](INSTALL.md)** · [Release and checksums](https://github.com/KMCAI123/firebridge-heater/releases/tag/v0.5.1-preview.1)

In Downloader, enter the direct APK URL above. **Legacy code 2819860 still downloads the older APK without an in-app disclaimer. Do not use that code for this update.** A replacement numeric code has not been published.

## Safety inside the app

On first launch or after an update requiring a new notice, open **Read full disclaimer**, return, check the acknowledgment box, then choose **Acknowledge and continue**. **Decline and exit** leaves setup and control locked. The full disclaimer is bundled offline and remains accessible through **Safety & Disclaimer** on the home screen and the local setup page.

Acknowledgment gates heater setup, native BLE commands, background startup and HomeKit power control. It does not detect whether an adult is actually supervising, does not make unattended operation safe, and does not establish a legally effective waiver of non-waivable rights.

![First-launch safety acknowledgment on the test Firestick](docs/images/safety-first-launch.png)

The safety UI was verified on the Firestick using an isolated lightweight test package; the complete distribution APK was built, signed and inspected but was not installed over the working heater app. Seven automated safety/regression tests pass. No heater was connected or commanded during these safety-screen tests.

## Install and set up



1. On a compatible Android-based Fire TV, install/open Downloader. Enable permission for Downloader to install unknown apps in the Fire TV developer settings. Names and availability vary by Fire TV model. This permission is required for Downloader; the APK cannot authorize itself.

2. Enter the direct APK URL above in Downloader and choose **Go**.

3. Install and open **FireBridge Heater Preview**. Read the offline disclaimer and complete the safety acknowledgment before setup becomes available.

4. Choose **Find my VEVOR / HeaterCC heater**. Allow Location on older Android versions, or Nearby devices on newer versions if prompted. Older devices may require location services enabled for BLE discovery. No geographic location is calculated by this app.

5. Close any vendor phone app or other bridge holding the heater's Bluetooth connection. Select your heater and choose **Check heater**. This sends status requests, not a heating command.

6. Wait for current heater status, then select **Show private Apple Home pairing code**.

7. On your iPhone, open Home, choose **+ â†’ Add Accessory**, and scan the displayed code or enter it manually via More Options. The iPhone and Fire TV must have local network discovery access to each other.



Keep the Fire TV powered and within Bluetooth range. The app supports an on/off heating accessory and ambient temperature sensor, not target-temperature or heat-level adjustment. Apple remote-access capability does not authorize unattended heating: an adult must still be physically present at the heater throughout operation.



![FireBridge reading live heater status](docs/images/live-status.png)



See the [illustrated setup guide](INSTALL.md) for every step and troubleshooting.



## Compatibility and evidence



- Android API 26 or newer; ARMv7 or ARM64; Bluetooth Low Energy.

- Not compatible with every Fire TV generation or non-Android Fire TVs.

- VEVOR branding alone is insufficient. The heater must speak the supported HeaterCC/ABBA protocol; discovery candidates are checked using live status.

- Tested: AFTSSS Firestick, Fire OS 7 / Android 9, 32-bit ARM. APK installation through PC ADB, remote-button navigation, heater discovery and valid status were successful. Homebridge reported reachable. The earlier v0.5.0 public GitHub APK also downloaded successfully through Downloader 2.0.4 on the Firestick and reached the Android installer; installation of that differently signed release was cancelled to preserve the existing test app.

- Not yet tested: Fresh-device Downloader permission workflow, pairing from iPhone, actual power operation, completed cooldown, cold boot, standby and prolonged operation.

- The APK is about 102 MB and expands its runtime on first launch; allow ample free storage beyond the download size.



Use only with a correctly installed heater and its original controller/power supply. Keep normal heater safety systems intact. Do not depend on this preview as a safety control. Every use must be continuously supervised in person, including shutdown and cooldown. See [the full disclaimer](DISCLAIMER.md).



## Privacy and local access



The app creates fresh local setup credentials and HomeKit pairing identity on the user's own Fire TV. The release contains no developer account credentials, heater pairing state or saved home configuration. Phone setup uses bearer-authenticated HTTP on a trusted LAN; keep the setup URL/QR private. Do not expose setup ports to the internet.



## Updates and signing



Package: `net.localbridge.tv.heaterpreview`. This public release uses a dedicated distribution signing key. Future APK updates must retain that key. The earlier privately sideloaded developer build used a different key and cannot be upgraded in-place to this public release. Back up that private build's state before replacing it. Normal new users do not have that development copy installed.



The v0.5.1 distribution APK retains the public signing certificate and includes the complete native/runtime assets plus the offline notice. Its signature, packaged notice and safety module were verified. Safety UI testing used an isolated lightweight package, not a full runtime installation. Checksums and certificate details are included with the release. A clean-machine reproducible build has not yet been established.



## Notices



This is an independent project, not an official VEVOR, Amazon, Apple or Homebridge product. Third-party software retains its own licenses; bundled runtime notices and an additional `THIRD-PARTY-NOTICES.zip` are supplied with the release. The repository currently distributes the preview APK and documentation; it is not a complete source/build repository.



Report your Fire TV model, Fire OS version and heater model when opening an issue. Do not post setup codes, private URLs, passwords or unredacted logs.

