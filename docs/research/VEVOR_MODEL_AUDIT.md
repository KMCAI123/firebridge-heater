# VEVOR model and controller audit

Research date: 2026-11-21. Research only: no Bluetooth connections, heater commands, app installations, or production changes were made by this audit. Model/app evidence and protocol implementation evidence are deliberately separate. No newly researched heater is hardware-validated by this report.

## Main finding

VEVOR sells at least two materially different app/controller families. XMZ manuals specify **AirHeaterBLE**, while ZM2003/ZM8006 manuals specify **AirHeaterCC**. Supporting a VEVOR name, a nominal 8 kW rating, or a common BLE service is insufficient to select a command codec. The practical expansion order is AirHeaterBLE variants first, then a separately identified AirHeaterCC/ABBA implementation with toggle-safe control.

## Model-by-model evidence matrix

“Candidate” below means suitable for protocol investigation and offline emulation, not proven FireBridge compatibility.

| Exact model or SKU | Manufacturer evidence | Official controller app | Candidate protocol / confidence |
|---|---|---|---|
| XMZ-D1 | Original VEVOR manual cover | AirHeaterBLE | AA55/AA66 family candidate; exact variant unproven |
| XMZ-D2 | Same manual, separately named model | AirHeaterBLE | Same caveat |
| XMZ-L-D1, XMZ-L-D2, XMZ-L-D3, XMZ-L-D4 | New manual explicitly enumerates these four vertical-family models | AirHeaterBLE in shared manual | Shared family evidence, not a per-unit wire capture |
| XMZ-W-D1, XMZ-W-D2, XMZ-W-D3, XMZ-W-D4 | New manual enumerates these four horizontal-family models | AirHeaterBLE in shared manual | Shared family evidence only |
| XMZ-F-D1, XMZ-F-D2, XMZ-F-D3, XMZ-F-D4, XMZ-F-D5, XMZ-F-D6 | New manual enumerates these six models | AirHeaterBLE in shared manual | Shared family evidence only |
| XMZ-TB20 | VEVOR-authored model manual; official support page names its app-use tutorial | AirHeaterBLE | Family candidate; controller revisions require checking |
| XMZ-TB20 Pro | VEVOR-authored shared TB manual names model | AirHeaterBLE | Family candidate |
| XMZ-TB30 | Same shared TB manual | AirHeaterBLE | Family candidate |
| XMZ-TB40 | Same shared TB manual; vendor product identifies model | AirHeaterBLE | Family candidate |
| XMZ-HTB1 | Same shared TB manual | AirHeaterBLE | Family candidate |
| ZM2003 | VEVOR-authored manual explicitly identifies model | **AirHeaterCC** | ABBA candidate, not AA55/AA66; physical validation outstanding |
| ZM8006 | Same manual; official product identifies ZM8006 | **AirHeaterCC** | ABBA candidate; physical validation outstanding |
| SKU CYJRQDDKLS8KOS4H9V3 | Vendor Q&A explicitly answers an app question about “cy2005” | AirHeaterBLE per vendor answer | Lower-confidence SKU association; model name supplied by questioner, not verified nameplate |
| S590136 S1; SKU PPKCYJRQ25N18JNIP001V1 | Live vendor product specifications | Bluetooth advertised; app not established here | Unclassified: no command support should be inferred |
| SKU ZCJRQFT12V8KWKIL7V9 | Vendor 12 V 8 kW split-heater listing | Bluetooth advertised; app/model mapping not established here | Unclassified |

Primary/document sources for the table:

- [VEVOR XMZ-D1/D2 original manual](https://d2v0huudrf11kh.cloudfront.net/vevor-center-goods/VEVOR-Diesel-Heater_1691547766381.pdf): cover identifies both models; Bluetooth section names AirHeaterBLE. It describes BYD-prefixed discovery names and matching the configurable app/controller password.
- [VEVOR XMZ-L/W/F family manual](https://mdm-goods.vevor.net/mdm-goods-service-prod/DieselHeater%28M%2920260723_1784812895122.pdf): cover/model table enumerates the fourteen model suffixes above; Bluetooth instructions at PDF pages 48–51 name AirHeaterBLE. This is one shared document, not fourteen independent compatibility tests.
- [VEVOR XMZ-TB20 support page](https://www.vevor.ca/pages/contact-us/CYJRQPPKGJXSC4ODWV3) and [VEVOR-authored TB20 manual, mirrored](https://device.report/manual/21003917): support index identifies model-specific app tutorial; manual names AirHeaterBLE.
- [VEVOR-authored TB20 Pro/TB40/HTB1/TB30 manual, mirrored](https://device.report/m/8b29052d64a3959dd026304e5bc82010fba773bf41c48056f32354abba31467a): lists the four models and AirHeaterBLE. These mirrored manuals are manufacturer-authored primary documents, but their host is not VEVOR; retain that provenance distinction.
- [VEVOR-authored ZM2003/ZM8006 manual PDF, mirrored](https://device.report/m/355abf7367f8cb9bae816dd8a3e40d8dd2640bb84e2948a1285391adfa770920.pdf): cover identifies both models; Bluetooth section specifies AirHeaterCC, Heater-plus-machine-code discovery, and only one connected phone at a time. App UI includes heating, ventilation, fault/state, voltage and temperature. This is not proof of FireBridge wire compatibility.
- [Manufacturer-hosted older ZM2003/ZM8006 manual](https://d37keo26p536wj.cloudfront.net/mdm-goods-service-prod/DIESELHEATER%EF%BC%88M%29_1731488031193.pdf): located, but browser extraction failed on its 22.9 MB size. Do not claim its complete contents were inspected; the accessible manufacturer-authored revision above was inspected instead.
- [Official ZM8006 listing](https://www.vevor.com/diesel-heater-c_10321/vevor-diesel-air-heater-12v-24v-8kw-bluetooth-app-remote-control-for-outdoors-p_010619482651).
- [Official XMZ-TB40 listing](https://www.vevor.com/category-c_10000/product-p_010454359204), SKU DDKCYJRQ25N16A38N001V1.
- [Official vertical-heater app Q&A](https://www.vevor.com.au/goods/faq/inquiry?goodSn=CYJRQDDKLS8KOS4H9V3).
- [Official S590136 S1 listing](https://www.vevor.com/diesel-heater-c_10321/vevor-diesel-heater-8kw-dc-12v-all-in-one-portable-diesel-air-heater-with-bluetooth-app-control-remote-control-and-lcd-screen-1-2-gal-fuel-tank-for-vehicles-vans-rvs-campers-and-garages-p_010458340725).
- [Official legacy split-heater listing](https://www.vevor.com/calentador-de-diesel-c_10321/vevor-bluetooth-app-control-diesel-air-heater-12v-8kw-diesel-heater-with-automatic-altitude-adjustment-remote-control-and-lcd-diesel-parking-heater-for-rv-trailer-camper-van-boat-and-indoors-p_010679003839).

Retail URLs and cached titles can select different variants. The live S590136 S1 listing differed from its cached generic suitcase title; record nameplate, controller photo and SKU together before claiming a specific variant works.

## Independent controller-family corroboration

[VEVOR replacement switch XMZ-F-D7 manual](https://d37keo26p536wj.cloudfront.net/mdm-goods-service-prod/SKU3-DIESELHEATERPARTS%EF%BC%88M%EF%BC%89_1751994744503.pdf), PDF page 8, explicitly lists XMZ-TB20, TB20 Pro, TB30, TB40, HTB1 and the fourteen F/W/L variants as supported host models and excludes others. This strengthens the shared XMZ controller-family hypothesis. It does not establish identical Bluetooth firmware or prove the ZM family compatible. XMZ-F-D7 is a replacement controller/accessory here, not another heater model.

## Protocol source audit

Inspected local clean checkout of [Spettacolo83/homeassistant-diesel-heater](https://github.com/Spettacolo83/homeassistant-diesel-heater/tree/22d973290f5e7148290a307379453785987a18bb), exact HEAD `22d973290f5e7148290a307379453785987a18bb`. Source carries MIT licensing; preserve applicable copyright/license notices for copied code. No local changes in that checkout were reported by git status.

Relevant pinned files:

- [Protocol codecs](https://github.com/Spettacolo83/homeassistant-diesel-heater/blob/22d973290f5e7148290a307379453785987a18bb/diesel_heater_ble/src/diesel_heater_ble/protocol.py)
- [BLE constants](https://github.com/Spettacolo83/homeassistant-diesel-heater/blob/22d973290f5e7148290a307379453785987a18bb/custom_components/diesel_heater/const.py)
- [Connection/discovery coordinator](https://github.com/Spettacolo83/homeassistant-diesel-heater/blob/22d973290f5e7148290a307379453785987a18bb/custom_components/diesel_heater/coordinator.py)

These are developer implementation claims, not VEVOR's published protocol specification or tests of every matrix model.

### AirHeaterBLE family

The implementation supports plain AA55, plain AA66, and XOR-obfuscated variants. Its VEVOR command frame is eight bytes: AA 55, two base-100 PIN bytes, command, little-endian 16-bit argument, then sum of bytes 2–6 modulo 256. The same command construction is used for plain and XOR-status variants. A factory-default PIN in code is not proof that a particular heater uses it.

Plain AA55 accepts 18–20-byte status, while plain AA66 uses 20 bytes. Both carry state, step, mode, voltage, temperature and altitude. AA66's error field must be masked with 0x3F; its upper two bits encode an error-family discriminator. Treating its raw byte as an error code can generate a false fault.

The code provides FFE0/FFE1 and alternate characteristic definitions. Discover actual properties and protocol framing; do not assume every notify characteristic equals the write characteristic. XOR status support must be selected by verified framing, not a product-name guess. XOR obfuscation is not cryptographic authentication.

### AirHeaterCC / ABBA family

The pinned source assigns service FFF0, **FFF1 notify and FFF2 write** (Bluetooth base UUID suffix). Other families can use the same service with opposite directions, so the service UUID alone is not identification.

Status parsing requires AB BA 11 CC and at least 21 bytes. Separate clock/configuration messages must not become heater status. Commands start BA AB and end with an additive checksum. Crucially, the source implements heating power as a **toggle**, not an idempotent on/off operation. Retrying an uncertain power write can reverse the intended result. Current status must be fresh before deciding whether one toggle is needed, with no blind retry after timeout/disconnection.

The source also includes a connection-time clock command. Do not treat that as a harmless status read. The ABBA command builder does not use the generic PIN parameter; this alone does not establish that every controller lacks any pairing/access restriction. No authentication-bypass conclusion follows.

## Offline emulation plan

Emulation can exercise codecs, controller/session state and HomeKit translation without ignition. It cannot establish radio range, physical heating safety, actual temperature calibration or every firmware revision's compatibility.

1. Create separate simulated AA55, AA66, AA55-XOR, AA66-XOR and ABBA peripherals/transports. Keep model catalogue metadata separate from protocol selection.
2. Include cold/off, starting, heating, cooldown, fault and stale/disconnected states. Never collapse cooldown into safely off or an acknowledged request into confirmed heating.
3. Test malformed/truncated/coalesced notifications, non-status frames, incorrect lengths/checksums, signed temperatures, Celsius/Fahrenheit, endian conversion and AA66 family flags with zero fault.
4. Test rejection/no-response for wrong PIN and unknown frame types. Do not probe or brute-force real PINs.
5. ABBA-specific tests: a duplicate power request does not cause two toggles; disconnect after write becomes uncertain; reconnect obtains fresh state before any action; clock/config messages cannot report a new state; notification and write UUIDs cannot be swapped.
6. Keep fuel priming, pump calibration, CO thresholds, automatic restart/timers and factory settings outside the initial generic adapter. They need distinct evidence and deliberate UI design.
7. Label outcomes “offline simulated” until a specific controller/firmware has supplied real read-only identification and supervised functional validation.

## Recommended delivery order

1. Finish and test all four AirHeaterBLE codec variants behind explicit protocol identification. Catalogue the XMZ models above as researched candidates, not universally supported heaters.
2. Add an independent AirHeaterCC/ABBA candidate for ZM2003 and ZM8006, with toggle-aware command handling and separate range/unit constraints. This is a meaningful second VEVOR family, not a cosmetic brand alias.
3. Hold S590136 S1 and unidentified retail SKUs in “protocol unknown.” Do not send commands based on their Bluetooth marketing text.
4. Use model-specific observed GATT properties and valid status frames to promote compatibility. A newly discovered heater can be listed without enabling its power button.

No further user lookup is needed to identify the next models to research: the exact model list and applications are now established to the evidence levels above. Actual physical validation still requires access to each relevant controller revision; a simulator cannot replace it.

## Independent FireBridge decoder review

Read-only reviewed `HeaterPreview/bridge/heater-protocols.cjs` after the parent agent's AA66 masking change against the clean pinned checkout above. The decoded VEVOR voltage, cabin temperature, XOR key, accepted lengths, state/step values, and command construction match the inspected fields. Plain AA55 accepts 18 or 20 bytes specifically, not 19, as the upstream coordinator shows. AA66-XOR correctly requires byte35 masking except TWX motherboard byte44==6, which uses the raw fault value. One non-critical diagnostic difference remains: upstream AA55-XOR also identifies TWX when byte44==6, while the FireBridge decoder currently labels that family `aa55`; its numeric fault remains correct. This review did not execute live traffic or claim exhaustive hardware conformance.
