# `pr/bt-pairing-hid`

One part of the topic repo p1ngb4ck/esphome-bluetooth. The whole topic, with its description, is the branch `main`.

Change of this branch: `bt_pairing` (pairing records, slots, device list, status events), `bt_hid` (Classic HID host), `ble_client_hid` (BLE HID), `ble_broadcast` (BLE advertisement data) with the ble_client / esp32_ble / api hooks they use; one branch because `bt_pairing` creates the slots of the other three

Based on these branches, whose changes it holds and builds on:

- `pr/bt-classic`: `bt_classic`: one Classic GAP callback with handlers and fallbacks, the shared IO capability and PIN type, BTDM dual mode on the ESP32-S31
- `pr/hid` of p1ngb4ck/esphome-hid: `hid`: report maps, keyboard layouts (also as files), keyboards, mice, gamepads, remotes; key events carry their device; `on_key`, `on_text`, `on_pointer`, `on_control`, API actions
- `pr/storage` of p1ngb4ck/esphome-storage-interface: the `storage` component: registry, path / filesystem / network / raw storages, the asynchronous worker (jobs, streams), `storage.*` actions, PSRAM transfer buffer, change feed, ISO 9660 mount
