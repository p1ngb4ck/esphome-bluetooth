# `pr/a2dp-source`

One part of the topic repo p1ngb4ck/esphome-bluetooth. The whole topic, with its description, is the branch `main`.

Change of this branch: `a2dp_source`: A2DP source with AVRCP (vendored ESP32-A2DP library) - speaker, device select, volume, microphone source, reconnect, `idle_timeout`

Based on these branches, whose changes it holds and builds on:

- `pr/bt-pairing-hid`: `bt_pairing` (pairing records, slots, device list, status events), `bt_hid` (Classic HID host), `ble_client_hid` (BLE HID), `ble_broadcast` (BLE advertisement data) with the ble_client / esp32_ble / api hooks they use; one branch because `bt_pairing` creates the slots of the other three
- `pr/esp32-ble-tracker-pause-scan`: `pause_scan` / `resume_scan`: BLE scanning paused while A2DP streams on the chip's own radio
