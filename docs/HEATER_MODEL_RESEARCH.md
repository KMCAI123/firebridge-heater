# Diesel heater model and app research

**Release update (2026-11-22):** v0.6.0 adds four experimental AA55/AA66 command/status adapters. The following research records pre-release experiments; see [current adapter limits](HEATER_TWO_WAY_0.6.0.md). Hcalory and other families remain research-only.

Research date: 2026-11-21. Three independent model audits plus offline protocol tests. **This is a research roadmap, not the released APK compatibility list.** No additional heater was operated or physically validated during this work.

## Specific models and apps

| Manufacturer / exact model(s) | Phone app | Evidence and implementation status |
|---|---|---|
| VEVOR XMZ-D1, XMZ-D2 | AirHeaterBLE | Manufacturer manual; four AA55/AA66 protocol variants simulated. Exact model-to-wire variant requires discovery/status confirmation. |
| VEVOR XMZ-L-D1, L-D2, L-D3, L-D4 | AirHeaterBLE | Shared manufacturer manual; same qualification. |
| VEVOR XMZ-W-D1, W-D2, W-D3, W-D4 | AirHeaterBLE | Shared manufacturer manual; same qualification. |
| VEVOR XMZ-F-D1, F-D2, F-D3, F-D4, F-D5, F-D6 | AirHeaterBLE | Shared manufacturer manual; same qualification. |
| VEVOR XMZ-TB20, TB20 Pro, TB30, TB40, HTB1 | AirHeaterBLE | Model manuals, some hosted on document mirrors; not independently hardware-tested. |
| VEVOR ZM2003, ZM8006 | AirHeaterCC | Manual evidence; candidate for existing ABBA/HeaterCC family, not automatic proof of every firmware revision. |
| VEVOR S590136 S1 | Unresolved | Bluetooth advertised; no reliable app/protocol mapping established. |
| Hcalory HBU1S / HC1-U2 / HC1-U5 | HCALORY | Official app plus specific upstream HBU1S BD39/MVP2 evidence. Strongest next Hcalory target. |
| Hcalory TB2S / TB2-SE, TB2S+ / TB2Plus, TC2S | HCALORY | Official model/app evidence; exact protocol revision unresolved. |
| Hcalory F1-S, L1-S, L2-S, W1-S, W2-S, TB1, TB2, TC1SE | HCALORY | Official app compatibility listing; no exact MVP generation assignment. |
| Hcalory TBMAX, TBMAX2, TB3 | App advertised; mapping unresolved | Do not inherit another Hcalory controller's support. |
| Sunster XMZ-TB10 / support-index TB10Pro, controller 2409PRO | Sunster | Official model/controller/manual evidence. CBFF is a candidate; not yet emulated here. |
| Sunster SH W2, SH W8 / A2409PW; TB10WF 2026 | Sunster family candidate | Separate Wi-Fi/Bluetooth revisions; see detailed audit. |
| Sunster S-DZ06 / A322 | Sunster Neo | Separate protocol family from Sunster CBFF. |
| MUDIRO M-5S EVO-H | Warm Drive | Exact model reported tested upstream. Separate BLE direction/protocol; no FireBridge hardware test. |
| MUDIRO M-5S Ultra | Unresolved | Bluetooth advertised; cannot inherit EVO-H mapping. |
| HeatGenie / Boygu controllers | HeatGenie | App and protocol evidence; exact retail heater model unresolved. Offline status parser tested. |
| Webasto with existing Cronus Smart controller | ThermoConnect | Official local Bluetooth path; controller matters, not heater brand alone. |
| LF Bros N2 Pro, N3 Ice Fishing, N2 international | Unresolved | Insufficient authoritative model/app evidence. |
| AUTOTERM with QSTART | Autoterm Control | Cellular accessory, outside direct Firestick BLE scope. |

## Source audits

Each audit preserves original links, uncertainties, controller distinctions and source licensing:

- [VEVOR model-by-model evidence](research/VEVOR_MODEL_AUDIT.md)
- [Hcalory model-by-model evidence](research/HCALORY_MODEL_AUDIT.md)
- [Sunster, MUDIRO, HeatGenie, Webasto, AUTOTERM and LF Bros](research/OTHER_HEATER_MODEL_AUDIT.md)

Protocol reference: [MIT-licensed upstream at commit 22d973290f5e7148290a307379453785987a18bb](https://github.com/Spettacolo83/homeassistant-diesel-heater/tree/22d973290f5e7148290a307379453785987a18bb). Home Assistant is not required for these finite protocol codecs. Upstream functionality is evidence, not independent FireBridge validation or manufacturer endorsement.

## What was built and checked

- In-memory VEVOR AA55, AA66 and both XOR variants: status, modeled startup, heating, cooldown, fault and connection-loss scenarios.
- In-memory Hcalory authentication/query/power flows for MVP1/MVP2 and FFF0/BD39 branches. MVP1/BD39 is explicitly artificial branch coverage, not a confirmed hardware combination.
- Hcalory command bytes compared against a separate pinned Python reference; status decoder checked against two upstream reported captures.
- All 768 AA66 fault-family combinations compared against the pinned implementation.
- HeatGenie complete-frame decoder: CRC known-answer check, independent CRC producer, 400 single-bit corruptions, all truncations, 2,000 seeded malformed inputs and 50/52-byte variants.
- **34 test cases passed: 24 Node tests and 10 Python tests**, including existing safety and heater regressions. Cases contain many parameterized inputs.

Independent review caught and corrected a synthetic Hcalory frame-length error, missing real header variants, overly broad startup-state interpretation, and AA66/TWX fault decoding. These are offline codec fixes; no new power-control capability was enabled on a Firestick.

## Important limits

Simulated startup/cooldown timing is invented for testing. Simulators cannot establish BLE range, pairing reliability, real command acceptance or physical heater behavior. HeatGenie lab accepts complete reports and rejects fragments; it is not a BLE stream assembler. Sunster, Warm Drive and Cronus implementations have been researched, not completed in FireBridge.

Do not choose a protocol by brand or shared service UUID alone. Require advertisement identity, characteristic direction, validated telemetry, and an exact controller revision before enabling writes. Hcalory MVP2 status requests also set the controller clock, so they are not passive reads. ABBA power is a toggle and must never be blindly retried. Unknown state remains unavailable, not assumed off. No password guessing is proposed.

The published v0.5.1 APK remains unchanged. This research does not expand its advertised compatibility. Existing safety acknowledgments and continuous-supervision requirements remain in force.

## Ranked next targets

1. VEVOR AirHeaterBLE XMZ family: strongest manufacturer model/app coverage and four protocol simulations already available.
2. Hcalory HBU1S: exact retail model plus upstream BD39/MVP2 evidence and an offline emulator.
3. Sunster TB10Pro / 2409PRO: strong official identification; build a dedicated CBFF simulator next.
4. MUDIRO M-5S EVO-H / Warm Drive: specific upstream-tested model; dedicated parser and acknowledgment simulation next.
5. HeatGenie and Webasto Cronus Smart: credible distinct protocols, but retail mapping or installation/controller requirements need separate treatment.

Next real-device work should start with owner-authorized discovery and telemetry validation on an identified controller. It must not jump directly from a passing simulator to automatic power commands. The user does not need to research model/app names; the exact candidates and evidence are recorded here.
