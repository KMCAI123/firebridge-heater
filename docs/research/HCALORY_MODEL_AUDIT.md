# Hcalory model and protocol audit

Research date: 2026-11-21. Read-only research; no BLE commands, heater operations, package installation, or production changes performed by this audit.

## Decision

HBU1S is the strongest concrete next candidate: manufacturer explicitly identifies its HCALORY phone app, and an original integration explicitly associates HBU1S with BD39/MVP2. This is enough to build a defensible offline protocol emulator, not enough to declare FireBridge hardware compatibility. The same brand and app span several controller generations; service FFF0 alone cannot select a command protocol.

## Model-by-model evidence

All rows remain **unverified on FireBridge hardware**. “Protocol unknown” means no specific controller revision was tied to that model by the primary evidence inspected.

| Exact model | Manufacturer/store app evidence | Controller evidence and development disposition |
|---|---|---|
| HBU1S, HC1-U2 / HC1-U5 variants | [Manufacturer explicitly names HCALORY app](https://hcalory.com/collections/diesel-heater/products/hbu1s-diesel-heater-unit-1s-12v-higher-fuel-efficient) | Pinned integration names HBU1S as BD39/MVP2. Highest-priority emulator target; do not assume every retail revision identical. Product page has conflicting 5KW/8KW labels, so wattage is a poor identity key. |
| TB2S / TB2-SE | [Manufacturer names HCALORY Bluetooth app](https://hcalory.com/pl/products/tb2s-diesel-heater-toolbox-2s-6l-app-control-upgraded) | Protocol unknown; advertised upgrade from TB2. Do not silently reuse TB2 protocol. |
| TB2S+ / TB2 Plus | [Manufacturer names HCALORY app](https://hcalory.com/collections/diesel-heater/products/tb2s-diesel-heater-toolbox-2s-plus-6l-wireless-remote-upgraded) | Protocol unknown; wireless temperature-sensing remote is a product distinction, not evidence of a BLE protocol variant. |
| TC2S | [Manufacturer explicitly names HCALORY app](https://hcalory.com/cs/collections/all/products/tc2s-diesel-heater-toolbox-compact-2s-5l-higher-fuel-efficient) | Protocol unknown. Common remote compatibility with TB2S, TB2S+, TB MAX does not prove common BLE commands. |
| TB MAX / Toolbox MAX | [Manufacturer confirms app, remote and panel](https://hcalory.com/products/tb-max-diesel-heater-toolbox-7-5l-co-alarm-last-up-to-62-5-hrs) | Exact app identity not stated in inspected product text; HCALORY family is plausible, not established here. Protocol unknown. CO remote functions require separate validation; do not expose threshold writes. |
| TB MAX2 / Toolbox Max 2 | [Manufacturer confirms app control](https://hcalory.com/products/tb-max2-diesel-heater-7-9l-app-carbon-clean-65h-runtime) | Current page markets a 2027 upgrade. Exact controller unknown. Added carbon-cleaning feature must not be inferred from older MVP commands. |
| TB3 / Toolbox 3 | [Manufacturer confirms app control](https://hcalory.com/products/tb3-diesel-heater-6l-anti-leak-with-high-altitude-waterproof) | Exact controller/app package not identified by inspected product text. Defer live command support. |
| F1-S | Official Hcalory app supported-model list below | Exact BLE generation unknown; offline family classification only. |
| L1-S | Same official app list | Exact BLE generation unknown. |
| L2-S | Same official app list | Exact BLE generation unknown. |
| W1-S | Same official app list | Exact BLE generation unknown. |
| W2-S | Same official app list | Exact BLE generation unknown. |
| TB1 | Same official app list | Exact BLE generation unknown. |
| TB2 | Same official app list | Exact BLE generation unknown; keep distinct from TB2S. |
| TC1SE | Same official app list | Exact BLE generation unknown; do not conflate TC1SE, TC2-SE and TC2S. |

The publisher's [Google Play listing](https://play.google.com/store/apps/details?hl=en_CA&id=com.hcalory.client) identifies Android package **com.hcalory.client**, publisher **HONG KONG TING HUI LIMITED**, and the eight older models listed above. Its displayed update date is July 9, 2026. The [Apple listing](https://apps.apple.com/us/app/hcalory/id6484163464) is **6484163464**, same publisher and older model list. Newer manufacturer product pages expand the app's practical product coverage beyond that list; the old list is not an exhaustive compatibility boundary.

The manufacturer's [manual index](https://hcalory.com/pages/how-to-use) also lists HBS2, HBH2, HBS1S, HBH1S, HBU-3, UNIT 2, HBS-3/S and HBH-3/S. Their appearance in the index alone does not establish BLE support or protocol. They are research leads, not enabled models. Some manufacturer PDFs are image pages/watermarked and were not treated as parsed protocol evidence.

## Pinned original implementation sources and licenses

1. [Spettacolo83/homeassistant-diesel-heater](https://github.com/Spettacolo83/homeassistant-diesel-heater/tree/22d973290f5e7148290a307379453785987a18bb), commit **22d973290f5e7148290a307379453785987a18bb**, committed **2026-11-19T16:10:08+01:00**. Inspected existing local checkout. Root LICENSE: MIT, copyright 2025 Sebastian Majchrzak. Bundled diesel_heater_ble/LICENSE: MIT, copyright 2024 Spettacolo83. Retain applicable notices if copying code.
   - [Protocol implementation](https://github.com/Spettacolo83/homeassistant-diesel-heater/blob/22d973290f5e7148290a307379453785987a18bb/diesel_heater_ble/src/diesel_heater_ble/protocol.py)
   - [Advertisement classifier](https://github.com/Spettacolo83/homeassistant-diesel-heater/blob/22d973290f5e7148290a307379453785987a18bb/custom_components/diesel_heater/hcalory.py)
   - [Connection coordinator](https://github.com/Spettacolo83/homeassistant-diesel-heater/blob/22d973290f5e7148290a307379453785987a18bb/custom_components/diesel_heater/coordinator.py)
   - [Captured-frame regression tests](https://github.com/Spettacolo83/homeassistant-diesel-heater/blob/22d973290f5e7148290a307379453785987a18bb/tests/test_protocol.py)
2. [mSoftMS/AirHeater-BLE protocol notes](https://github.com/mSoftMS/AirHeater-BLE/blob/7a946bef3d8baa43ae94b150c6d786d0a7616476/PROTOCOL.md), HEAD **7a946bef3d8baa43ae94b150c6d786d0a7616476**, GitHub commit date **2026-01-18T08:53:14Z**, repository MIT. Notes claim original app BLE capture analysis but do not tie the protocol to a sufficiently specific retail model. Useful separate-family evidence, not HBU1S proof.

The original [issue #34 discussion](https://github.com/Spettacolo83/homeassistant-diesel-heater/issues/34), opened February 11, 2026, contains real notifications and corrections from reverse engineer Xev. It also contains erroneous interim maintainer explanations; use pinned executable behavior and captured regression frames rather than copying every comment. Real reports show handshake and mode-mapping bugs, so existence of the integration is not blanket reliability proof.

## Protocol crosswalk

| Family | Identification and transport | Offline emulation scope |
|---|---|---|
| Hcalory MVP1 | FFF0, notify FFF1, write FFF2; specific short manufacturer record | Legacy 0E04 query and documented commands, synthetic peripheral responses; not all FFF0 heaters. |
| Hcalory MVP2 | BD39, notify BDF8, write BDF7; HBU1S specifically named by upstream | PIN verification, time-sync/status query, power/mode/setpoint/gear framing and captured telemetry. |
| MVP1_2/1_3, MVP2_2/2_3, MVP3/3_2/3_3 | Classifier distinguishes manufacturer records; newer authenticated command family may also use FFF0 | Classifier and command-family tests possible. No retail-model mapping proven here. |
| AirHeater/ABBA-related FFF0 | mSoftMS documents BA AB command frames plus AA55 extended commands | Separate emulator only. Shared FFF0 is insufficient to try these commands on HBU1S. |

Pinned classifier reconstructs manufacturer bytes from sorted company IDs (little-endian two-byte ID plus payload). Legacy MVP1 is a six-byte record whose hex begins ECB. Other recognized records are exactly 16 bytes: bytes 2:5 equal 06 BD 01 for MVP2, or bytes 2:4 equal 01 02 / 01 03 / 02 02 / 02 03 / 03 01 / 03 02 / 03 03 for later variants. A HeaterNNNN-MAC name is explicitly excluded into the ABBA-related path. Unknown/mixed records must remain unknown; this classifier describes upstream app analysis, not a manufacturer standard.

## Critical packet findings independently checked

- **Telemetry headers:** pinned parse() checks byte 8 == 03 and checksum, not a fixed 0002 prefix. Real issue #42 regression frames in tests use **0001 and 0003** prefixes. A FireBridge decoder accepting only 0002 will reject real hardware frames.
- **Voltage:** stale parse docstring says little-endian. Executable implementation and captured test assertions agree on **big-endian bytes 24-25 divided by 10**; 00 78 is 12.0 V. Prefer observed fixture and executable logic.
- **Native temperature:** sign/magnitude, big-endian values divided by 10; case sign at26/value27:29, cabin sign29/value30:32, unit at37. Fahrenheit must not be interpreted as Celsius.
- **Length caveat:** upstream accepts len>=38; byte37 overlaps checksum at exactly38 and unit is only read if len>38. Do not claim full-unit telemetry from an incomplete frame.
- **State:** byte20 high nibble differentiates off, stopping, heating, ventilation, error; low nibble differentiates ignition/cooldown/running/standby. Cooling is not “heater safely cold/off”. Byte23 auto-start-stop is 1 enabled, 2 disabled.
- **Checksum:** sum of bytes8 through the byte before checksum, modulo256, for this native family. This is not the checksum rule of every other heater protocol.
- **MVP2 auth request:** exactly18 bytes, four individual numeric PIN digits, command0A0C, verification operation01. **Auth notification:** exactly19 bytes, prefix0003, bytes7:9=0B0C, byte11=06, auth status byte17, checksumlast. Pinned validator treats1 as success, any other well-formed status as false, malformed as None. Upstream comments disagree on failure values; only1 should enable command flow.
- **Handshake ordering:** Xev explains the heater can notify PIN/auth state first, then the app verifies. An emulator that only replies to a client-first request misses this path. Never log or echo PIN data in public fixtures; use explicit synthetic digits.
- **Query has a side effect:** MVP2 “query” is also **clock synchronization**, exactly18 bytes. BD39 carries numeric hour, minute, second, ISO weekday. The newer FFF0 transport carries a big-endian four-byte timezone-shifted epoch. It is not passive observation.
- **Legacy query:** MVP1 uses0E04 with a nine-byte payload; generic envelope makes22 bytes. Temperature packet15 bytes, gear14 bytes, auth/time query18 bytes. Packet sizes should be tested independently of encoder output.

## Emulation limits and next implementation gates

Offline tests can verify frame parsing, advertisements, checksum rejection, truncated frames, units, big-endian voltage, startup/cooldown/error transitions, unsolicited auth notification, bad PIN, disconnect/re-auth, and clock-query variants without operating a heater. Use captured upstream telemetry as independent expected fixtures; testing an encoder exclusively against a decoder designed from it can preserve the same mistake in both.

A deterministic software peripheral is not evidence of real BLE connection stability, controller timing, heater safety transitions, remote sensor behavior, Bluetooth range, phone contention, or command acknowledgment. Do not label any untested model “supported” merely because its synthetic tests pass.

Prioritize: (1) source-backed HBU1S/MVP2 offline tests; (2) strict manufacturer-record classification with unknown-device rejection; (3) later obtain authorized, sanitized advertisements and GATT identity from a real second model; (4) supervised physical validation before enabling live writes. Keep prime, calibration, CO thresholds, carbon-clean cycles and unknown commands out of generic control. Shut down through the heater controller and preserve its cooling cycle.

No additional model purchase or user model-name lookup is necessary to continue offline development. Physical confirmation remains necessary before claiming that a particular retail heater revision works.
