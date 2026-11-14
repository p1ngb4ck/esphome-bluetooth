# `pr/esp32-ble-start-order`

One part of the topic repo p1ngb4ck/esphome-bluetooth. The whole topic, with its description, is the branch `main`.

Change of this branch: Bluedroid starts once WiFi reports its start-up done and, with a usb_bluetooth bluedroid dongle, once the dongle is ready; `restart()`

Based on these branches, whose changes it holds and builds on:

- `pr/wifi-startup-done-callback` of p1ngb4ck/esphome-network: `add_on_startup_done_callback` / `is_startup_done`: once per boot
