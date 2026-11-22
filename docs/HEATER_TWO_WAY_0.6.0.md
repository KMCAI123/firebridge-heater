# Experimental two-way adapters — 0.6.0

Added AA55, AA66, AA55-XOR and AA66-XOR AirHeaterBLE command/status paths to the Android BLE transport, Node control path, TV controls and Homebridge switch exposure. Existing HeaterCC remains supported. No Hcalory, HeatGenie, Sunster, Warm Drive or Cronus power support is enabled by this change.

Identification requires owner selection, FFE0 service with FFE1 write/notify characteristics, bounded complete status frames, structural/range validation and two replies with the same protocol signature. Unknown or ambiguous services are rejected. Identity here means protocol classification, not cryptographic authentication or an exact retail model. No new brand-wide compatibility claim is made.

Only documented default app passkey 1234 is supported. Changed passkeys are not guessed. Four variants use the common documented AA55 explicit on/off command format. Native and Node both require fresh status; native independently checks identity, fault, operating phase and command age. Commands are sent once; reconnection does not replay a consumed command. Node requires subsequent matching telemetry before reporting success. Cooldown cannot be restarted by a new on request. No calibration, priming or safety-parameter commands are included.

Known limits: plaintext variants lack a cryptographic integrity check; classification uses structure/ranges and GATT context. Two valid frames reduce accidental misclassification but do not prove a genuine device. New hardware remains physically unvalidated. Hcalory connection/authentication variants need additional runtime work; other families remain research/offline labs.

Reference: MIT diesel-heater-ble source pinned to 22d973290f5e7148290a307379453785987a18bb. License retained under docs/licenses. Tests include independent upstream-derived vectors, native/Node parity, mock command/telemetry integration, cooldown/duplicate/stale status, safety gates and existing HeaterCC regressions.
