# Supervised heater test — 2026-11-22

Selected Firestick .93, installed 0.5.1 prototype, existing VEVOR HeaterCC controller. Owner confirmed notice read and readiness to supervise in the garage. Safety acknowledgment completed through the app UI.

Observed sequence:

1. Fresh UI status: off, 66 F, 13 V.
2. One Start heating click. Controller status changed to heating; UI reported confirmation. Owner independently reported normal startup.
3. One Stop heating click. Controller status changed to cooldown; UI reported confirmation. Both power buttons disabled during cooldown. Owner independently reported normal cooling.
4. Subsequent read-only UI check: off, 66 F, 13 V. Start enabled, Stop disabled.

No repeated power commands, power interruption, tuning or calibration changes were used. Heating telemetry during startup does not establish sustained combustion or warm-air performance. Final physical fan-stop confirmation was requested and is still pending in this record. Apple Home pairing/control, extended running, recovery under connection loss and other heater models were not tested.

Storage preparation: owner-authorized adoption of the 123,048,296,448-byte USB disk succeeded. Its Alpine configuration overlay was preserved privately and SHA256 verified before formatting. Adopted volume mounted with 116,457,864 KiB available. Disney+ moved successfully; FireBridge remained internal. USB adoption adds storage, not RAM.
