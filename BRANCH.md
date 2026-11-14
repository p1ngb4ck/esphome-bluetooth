# `pr/bt-host`

One part of the topic repo p1ngb4ck/esphome-bluetooth. The whole topic, with its description, is the branch `main`.

Change of this branch: `bt_host`: `bt_classic`, `bt_pairing` and `bt_persistence` as one host component (`pairing:` / `persistence:` blocks); the drivers' discovery, SSP, bond removal and scan mode through it; `ble_controller` / `classic_controller` per role; Classic HID on the chip's own Classic (ESP32, ESP32-S31)

Based on these branches, whose changes it holds and builds on:

- `pr/bt-classic`: `bt_classic`: one Classic GAP callback with handlers and fallbacks, the shared IO capability and PIN type, BTDM dual mode on the ESP32-S31
- `pr/bt-offload`: `bt_offload`: Bluetooth HID input and speaker offloaded to the C6 (custom firmware), the 16 peer data handlers of esp32_hosted on the P4
- `pr/esp32-ble-bt-logs`: `disable_bt_logs` also quiets the Bluedroid layers the build keeps (ERROR instead of their defaults)
- `pr/esp32-ble-nvs-ready`: esp32_ble waits for `nvs_ready`
- `pr/esp32-ble-tracker-classic-scan-window`: the defaulted BLE scan window stays 30 ms when Bluetooth Classic runs on the same radio
- `pr/esp32-ble-tracker-pause-scan`: `pause_scan` / `resume_scan`: BLE scanning paused while A2DP streams on the chip's own radio
- `pr/hid` of p1ngb4ck/esphome-hid: `hid`: report maps, keyboard layouts (also as files), keyboards, mice, gamepads, remotes; key events carry their device; `on_key`, `on_text`, `on_pointer`, `on_control`, API actions
- `pr/esp32-exfat-fat-volumes` of p1ngb4ck/esphome-storage-interface: `esp32` `advanced: enable_exfat` (patched FatFs copy), FAT volume count from the configured mounts, VFS max count
