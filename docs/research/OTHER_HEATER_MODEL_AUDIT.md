# Other heater model and controller audit

Date: 2026-11-21. Research and offline simulation only; no heater commands, device changes, installs, or release compatibility claims.

## Result

There are useful, distinct local protocols worth implementing, but retail brand is not a protocol identifier. Sunster has particularly strong official model/controller/app evidence. MUDIRO has a precise community-tested model claim. HeatGenie and Webasto Cronus have public protocol implementations, but exact heater identification still needs care. LF Bros remains unresolved. AUTOTERM's documented QSTART path is a cellular accessory, not a direct Firestick BLE connection.

## Model-by-model evidence

| Product/controller | Official app and evidence | Local path and confidence | FireBridge decision |
|---|---|---|---|
| Sunster XMZ-TB10, controller 2409PRO; support page labels linked manual TB10Pro | Official manual version 2026.1.1, release June 24 2026, identifies model on PDF pp.1/4; controller and Bluetooth yes/Wi-Fi no on p.12; Sunster app on p.23 | Official Bluetooth confirmation. Community CBFF implementation is a plausible protocol candidate, not proven for every TB10 generation | Prioritize CBFF simulator; require controller/app/advertisement/status-frame confirmation before live enablement |
| Sunster SH W2 / SH W8 | Official catalog explicitly identifies A2409PW Wi-Fi/Bluetooth panel | Fits Sunster controller family; no claim that all firmware matches CBFF | Separate candidate entries, not an entire Sunster brand allowlist |
| Sunster TB10WF 2026 | Official catalog and separate model manual identify Wi-Fi/Bluetooth model | Do not assume same hardware/features as Bluetooth-only TB10Pro | Separate discovery/verification entry |
| Sunster S-DZ06 + A322 controller/motherboard set | Official catalog sells this separately and offers Sunster Neo app/category as distinct from Sunster app | Upstream has distinct DZ06/Sunster Neo documentation; sharing FFF characteristic UUIDs does not mean Warm Drive or CBFF compatibility | Keep separate protocol identity; no automatic UUID-only adoption |
| Sunster TP1 A2013SL + 433 remote | Official catalog names LCD/433 remote rather than Bluetooth | No direct BLE demonstrated in this review | Do not advertise support based on Sunster brand |
| MUDIRO M-5S EVO-H | Pinned upstream README specifically lists tested model; protocol documentation analyzes Warm Drive Android 1.0.3 | Community evidence: advertised name `Air Heater`, FFF1 write / FFF2 notify; direct BLE | Strong emulator target; upstream-tested is not FireBridge-tested |
| MUDIRO M-5S Ultra | Manufacturer-branded storefront advertises Bluetooth app but does not identify app in inspected text | Exact protocol remains unconfirmed; similar model name is insufficient | Separate unverified entry, not inherited EVO-H support |
| HeatGenie/Boygu controller family | Developer's App Store listing describes Bluetooth diesel-heater control; developer Wenzhou Bogu Intelligent Technology | Public HeatGenie register decoder; no exact retail heater model independently established here | Offline protocol candidate, not a model compatibility claim |
| Webasto installation already equipped with Cronus Smart | Official ThermoConnect app listing explicitly supports Cronus Smart via close-range Bluetooth | Public record protocol; actual attached heater model is not returned by decoder | Separate optional controller family; do not say all Webasto heaters work |
| Webasto ThermoConnect TCon1/TCon2 | Same official app, but mobile-network path | Not interchangeable with Cronus BLE | Outside direct BLE target |
| AUTOTERM with QSTART ASSY AT0060 | Official product identifies Autoterm Control app, GSM/LTE modem and microSIM | Requires QSTART accessory if absent; no direct BLE path established | Exclude from no-extra-hardware BLE claims |
| LF Bros N2 Pro / N3 Ice Fishing / N2 international | Manufacturer pages establish model names; exact official app/controller mapping not established | Regional seller/customer claims conflict across revisions; cannot assign Sunster/HeatGenie/ABBA based on appearance | Research hold: obtain manufacturer-specific manual/app evidence rather than asking user to buy/test blindly |

## Original sources

- [Sunster official support and model-specific manual index](https://sunsterdirect.com/pages/customer-support-business-inquiries). This source distinguishes Sunster versus Sunster Neo app links and separate controller categories.
- [Sunster official XMZ-TB10 manual](https://cdn.shopify.com/s/files/1/0721/5611/9264/files/XMZ-TB10.pdf?v=1788775484), version/date as above. Naming discrepancy between support index and PDF is preserved, not silently normalized.
- [Sunster official Google Play app](https://play.google.com/store/apps/details?id=com.booyood.www.sunsterApp).
- [Sunster S-DZ06 controller](https://sunsterdirect.com/products/s-dz06-lcd-switch-bluetooth-remote-control-12-24v).
- [MUDIRO M5S Ultra product](https://mudiro.net/products/m5s-ultra-diesel-heater-12v-24v-110v-8kw-with-auto-shut-off-alarm-thermostat-bluetooth-remote-auto-altitude-adjustment-for-camping-rv-tent). App protocol not specified here.
- [HeatGenie app developer listing](https://apps.apple.com/gb/app/heatgenie/id6471930955).
- [Webasto official ThermoConnect app listing](https://play.google.com/store/apps/details?hl=en&id=embelin.webasto).
- [Webasto ThermoConnect description](https://www.webasto.com/en-int/heating/controls/thermoconnect.html).
- [Webasto official privacy document](https://my.webastoconnect.com/docs/privacy_policy_thermoconnect.pdf), distinguishes mobile-network ThermoConnect from Bluetooth Cronus Smart.
- [AUTOTERM official QSTART](https://autoterm.com/product/701-modem-qstart-4g-lte-version): includes panel compatibility restrictions; PU22 is excluded unless changed to a supported panel.
- [AUTOTERM original QSTART manual](https://cdn.autoterm.com/Manuals/Modem%20QSTART/2020%20January%20version/QSTART%20EN%20v09.2019.pdf), historical 2019/2020 document, not proof of current BLE features.
- LF Bros manufacturer: [N2 Pro](https://www.lfbros.com/cpzfl1/13.html), [N3 Ice Fishing](https://www.lfbros.com/cpzfl2/11.html), [N2 international](https://www.lfbros.com/cpzfl3/22.html).

No popularity ranking is claimed. No commercial heating performance, Bluetooth range or CO safety claim was validated.

## Inspected implementation and licensing

Repository: [Spettacolo83/homeassistant-diesel-heater](https://github.com/Spettacolo83/homeassistant-diesel-heater/tree/22d973290f5e7148290a307379453785987a18bb), pinned commit `22d973290f5e7148290a307379453785987a18bb` verified in the local research clone.

Inspected `diesel_heater_ble/src/diesel_heater_ble/protocol.py`, README and `docs/protocols/HEATGENIE.md`, `WARM_DRIVE.md`, `CBFF.md`, `CRONUS.md`. Root license is MIT copyright 2025 Sebastian Majchrzak; decoder subtree license is MIT copyright 2024 Spettacolo83. Both scope and notices matter when adapting code. There is no need to install Home Assistant to reimplement these finite byte decoders. Upstream documentation and code are evidence of an implementation, not manufacturer approval or independent hardware validation.

### HeatGenie

Service 181A alone is generic Environmental Sensing and cannot safely identify this heater. Upstream requires recognized advertisement, suitable write/notify characteristics and a valid report. Status is exactly 50 or 52 bytes, prefix AA, register-count byte 09, type F2 at offset 7, 40-byte register area at offset 8, CRC-16 polynomial 1021 initial zero, big-endian appended CRC. Probe sentinel 32760 must clear prior reading. Measured temperatures remain Celsius when display is Fahrenheit; target follows display unit. Fault mask lowest set bit has priority. Cooldown is distinct from active heating. Commands have acknowledgment nonce; an eventual live integration must correlate acknowledgments rather than infer success from write completion.

### Warm Drive / MUDIRO

Advertised identity `Air Heater` with FFF1 write/FFF2 notify is a distinct direction from ABBA variants. Reports/requests use 29 0D and Modbus CRC with 5C/5A/5D/A5 subtypes. Manufacturer data indicates air versus water type. The app's air-heater manual level is represented by a specific temperature ladder, not a generic AA55 gear write. Changed controller code requires owner input; never brute-force. Configuration writes require cached full snapshots and can affect calibration; omit them from an initial adapter. Preserve cooldown, cleaning, ventilation and standby distinctions.

### Sunster CBFF

Upstream describes a locked AA77 path, double-XOR obfuscation using controller constant and MAC representation, FEAA command envelopes and a 47-byte CBFF status. This is not Bluetooth security authentication and is not an AA55 command clone. PIN handling and per-controller settings snapshots need dedicated tests. Sunster Neo/DZ06 remains separate even with similar GATT UUIDs. Do not expose advertised CO fields as a certified safety alarm.

### Cronus Smart

Notify UUID `4229d528-33d0-4aa7-9af1-00b03fc128f9`, write UUID `2ed964d9-1371-45ee-a84b-1d134d5ce7b7`, plus Cronus identity. This is a record protocol: read 55, response 56/57, write 50/51, acknowledgment 52. Continuations must remain separate ATT frames. Air and water records differ; controller type does not identify attached heater model. Tests must cover partial records, continuation ordering, wrong records, timeouts, acknowledgment failure and bounded cache size. Do not infer pump/glow status from unrelated heater protocols.

## Offline lab completed

New files only under `HeaterPreview/scripts/other-heater-lab/`:

- `heatgenie.py`: bounded complete-frame status decoder; no network, BLE, command builder, controller writes or runtime integration.
- `test_heatgenie.py`: independent nibble-wise reference CRC and synthetic frame producer; ten unittest cases.
- `THIRD_PARTY_LICENSE.txt`: decoder MIT notice.
- `README.md`: scope and reproduction instructions.

Run: `python -B -m unittest discover -s C:/Users/fital/Documents/FireTV-Homebridge/HeaterPreview/scripts/other-heater-lab -v`.

Observed result: **10 tests passed**. Includes standard CRC known-answer 123456789 -> 31C3, 53 seeded reference comparisons, both frame lengths, negative temperature, Fahrenheit target, unsupported probe clearing, cooldown/unknown state, all 400 single-bit corruptions of the base frame, all shorter lengths, concatenated/oversized/type-invalid input, three wrong-protocol frames with valid CRC, and 2,000 seeded malformed inputs.

Limits: synthetic tests validate our interpretation and rejection logic only. They do not prove actual heater support, radio reliability, pairing, controller revision, command acceptance, range or physical safety. Fragmented input is deliberately rejected because this lab accepts complete reports; no stream reassembly or BLE transport is packaged. No packet was sent to a heater.

## Recommended next work

1. Keep live discovery and writes disabled for unverified families. Detection should combine owner-visible model/controller/app, advertisement, GATT direction and validated frame identity.
2. Continue Sunster CBFF and Warm Drive synthetic emulators with exact captured upstream fixture provenance where available; label all invented samples synthetic.
3. HeatGenie parser is ready for offline captured-frame comparison. Add acknowledgment correlation/state-machine testing before even proposing live command tests.
4. Cronus is technically practical but separate enough to deserve its own record assembler, type model and tests. Only existing Cronus-equipped installations meet the no-added-hardware condition.
5. LF Bros needs authoritative app/controller evidence; AUTOTERM QSTART is not the same architecture and should remain excluded from direct BLE compatibility claims.
