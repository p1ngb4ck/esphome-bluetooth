# `pr/bt-offload`

One part of the topic repo p1ngb4ck/esphome-bluetooth. The whole topic, with its description, is the branch `main`.

Change of this branch: `bt_offload`: Bluetooth HID input and speaker offloaded to the C6 (custom firmware), the 16 peer data handlers of esp32_hosted on the P4

Based on these branches, whose changes it holds and builds on:

- `pr/esp32-ble-start-order`: Bluedroid starts once WiFi reports its start-up done and, with a usb_bluetooth bluedroid dongle, once the dongle is ready; `restart()`
- `pr/usb-bluetooth` of p1ngb4ck/esphome-usb-drivers: `usb_bluetooth`: Bluetooth dongles as HCI controller - Bluedroid on the dongle, firmware download
