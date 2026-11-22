# Direct Android app inspection

Date: 2026-11-22. Static analysis only. Neither app was installed or executed. No radio capture, heater command, login, firmware operation or production change occurred.

## Downloads and provenance

| App | Inspected artifact | Source and qualification |
|---|---|---|
| AirHeaterBLE | com.clj.airheater, 2.1.5, versionCode 215, minSdk 21, targetSdk 35 | [Manufacturer-hosted APK](https://www.booyood.com/app/airHeaterByBLE.apk), linked in heater manuals. Download size 37,326,184 bytes. This is not necessarily the latest Play version. |
| HCALORY | com.hcalory.client, 3.3.2, versionCode 30302, minSdk 24, targetSdk 36 | [APKPure download listing](https://apkpure.net/hcalory/com.hcalory.client/download), XAPK base inspected. Mirror-sourced, not directly authenticated against a Play-installed copy. Cached pages reported older versions; actual manifest governs this report. |

Official identities: [AirHeaterBLE Google Play](https://play.google.com/store/apps/details?id=com.clj.airheater), [HCALORY Google Play](https://play.google.com/store/apps/details?id=com.hcalory.client). Manufacturer domain [Booyood](https://www.booyood.com/).

Both base APKs passed Android apksigner verification. A valid signature proves package integrity under that certificate, not manufacturer trust by itself. HCALORY certificate SHA1 matches the mirror's historical listing; independent Play-certificate verification remains undone.

| Artifact | SHA256 |
|---|---|
| AirHeaterBLE APK | 24910b30e896cbd4398e1f72db663f98c86a50bdd433b20f534256f960be7e66 |
| AirHeaterBLE signing certificate | df61f99e45c6084563390fc8d1ac93f87acaf9f32536879759ea967a483300c0 |
| HCALORY base APK | 4a69aaec71d542d3e27d00ec58d4d005808685a43a7f0643e3f4a2183100879c |
| HCALORY XAPK container | 82b03e41135e346a91b025ac00d9708d74c090409b0dae196ed88e05bf563e57 |
| HCALORY signing certificate | 9fa6ef9e4107604e5be4956402daf842834fcf70f35e95537ad2e497e6894f6f |

## AirHeaterBLE: direct findings

Source asset: `assets/apps/__UNI__9A3735B/www/app-service.js`, SHA256 `d2100fedd392cc30c7fc8e22a105f8f6cfddfd88a352f1640261195870beeeef`. Character offsets below refer to its UTF-8-decoded text, not byte offsets.

- Around 689540: normal transport FFE0 service, FFE1 notification and write characteristic. Separate OTA transports also exist; those are not normal heater control and were not used.
- Around 792851, logged locator `common/bluetooth/useBluetooth.js:2929`: eight-byte AA55 command builder. Bytes 2/3 split the four-digit controller password by hundreds/remainder; byte 4 is the command; bytes 5/6 are little-endian data; byte 7 sums bytes 2–6 modulo 256.
- Around 1085803/1086248 and 1137735/1137992: on uses command 3/data 1, off command 3/data 0. Status requests use command 1/data 0. These support explicit target-state commands, not a blind power toggle.
- Around 730639 and 777202: AA55/AA66 identification, XOR decoding and the default eight-character XOR mask used by our older-family decoder.
- Around 721986, `parseBluetoothData.js:294`: legacy status parser reads operating state, step, temperature and voltage in the expected positions. Native case and room temperatures are signed little-endian values. XOR-family room temperature uses signed big-endian tenths.
- Around 812969: a newer AA77 path and handshake exists. This is **not** covered by our four AA55/AA66 adapters. The app also handles additional running/mode values that our conservative decoder rejects. Rejecting these should remain an unavailable/unsupported result, not be interpreted as off.

Offline check `AppAudit/check-air.cjs` asserted the exact relevant source expressions and compared a separately transcribed packet layout against FireBridge for six PIN values and three commands: **18 comparisons passed**. Proprietary JavaScript was not evaluated.

## HCALORY: direct findings

Portable JADX 1.5.7 was downloaded from its original GitHub release into the research folder, with archive digest checked against GitHub release metadata. Archive SHA256: `1755e4242ade71a077438723689f404079592ec7764a3a0572f98fef94ebcbf3`. Decompilation reported 43 errors; the relevant methods below were readable, but this is not a complete successful decompilation of every function.

- `q2/h0.java:47–52, e()`: FFF0 service writes FFF2 and reads/notifies FFF1; BD39 writes BDF7 and reads/notifies BDF8. Shared FFF0 UUID alone cannot distinguish HCALORY from HeaterCC.
- `h9/u.java:M0()` around line 283: twelve-character ECB-prefixed identity maps to MVP1; manufacturer data 06BD01 maps to MVP2; prefixes 0102/0103, 0202/0203 and 0301/0302/0303 distinguish later MVP variants. This establishes how the app classifies parsed advertisement data, not that FireBridge already implements the entire discovery pipeline.
- `r2/h.java:x(), y(), j()` around lines 999, 1003 and 899: distinct on, off and legacy-query packet builders. `check-hcalory.py` extracted their literal bytes, appended the app checksum and compared them with our offline codec: **all three matched**.
- `r2/h.java` around lines 743 and 953: PIN verification command and clock/status request. No user PINs were accessed; only public algorithm structure was inspected.
- `q2/e0.java`: checksum covers bytes starting at offset 8. `p()` chooses BD39; `k()` builds hour/minute/second/ISO-weekday; the other path uses a timezone-adjusted epoch. The request updates time, so it must not be called passive telemetry collection.
- `r2/h.java` around lines 178–211: sign/magnitude temperature fields and big-endian voltage scaling agree with our offline interpretation.

These findings materially improve the basis for a HCALORY adapter. They do not finish connection/authentication sequencing, notification reassembly, all model classifications or physical validation.

## Conclusions and next validation

1. The AirHeaterBLE explicit on/off implementation now has direct manufacturer-app corroboration, beyond the earlier community reference. Keep AA77 excluded.
2. HCALORY's core command encoders agree with a downloaded app, and its classifier/transport split is now independently located. Build and test the complete connection state machine before enabling it live.
3. A radio capture requires the corresponding physical heater and a compatible Android host with usable Bluetooth logging. No radio capture was performed here. A Firestick-only static analysis cannot substitute for that evidence.
4. Capture only owner-authorized traffic, avoid account/login traffic, keep raw captures private and bounded, and redact device identifiers/PIN fields before creating fixtures. Never infer physical heater behavior from write acceptance alone.

The downloaded apps remain proprietary. No decompiled source or APK is copied into FireBridge or published with this report. Only protocol facts, source locators and independently written checks are retained. This inspection is not a legal opinion about redistribution or vendor terms. Existing MIT reference code remains governed by its own license.

Local evidence directory: `HeaterProtocolResearch/AppAudit`. Official-store installation, signed-app provenance comparison, real Bluetooth traffic and new-model heater operation remain unverified.
