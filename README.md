# esphome-bluetooth

## Purpose
Bluetooth host and HID for ESPHome: one host component (`bt_host`: Classic GAP, pairing records and slots, Bluedroid
data persistence), Classic and BLE HID, BLE advertisement data, an A2DP source, the Bluedroid start order and the
offload of Bluetooth HID and audio to an ESP32-C6 coprocessor.

## How it works
`bt_host` runs on Bluedroid and keeps one pairing database for every Bluetooth driver; HID devices paired at runtime
fill its slots (no YAML entry per device). The drivers (`bt_hid` Classic HID, `ble_client_hid` BLE HID,
`ble_broadcast`, `a2dp_source`) do discovery, pairing and bond removal through it. Classic runs on the chip (ESP32,
ESP32-S31) or on a USB dongle; BLE runs on the chip, on the ESP32-P4 through the ESP32-C6, where `bt_offload` can move
HID input and the speaker to the C6.

## Quick start
Chips: ESP32, ESP32-S3

Needs: [esphome-esp32-platform](https://github.com/p1ngb4ck/esphome-esp32-platform),
[esphome-hid](https://github.com/p1ngb4ck/esphome-hid), [esphome-network](https://github.com/p1ngb4ck/esphome-network),
[esphome-storage-interface](https://github.com/p1ngb4ck/esphome-storage-interface)

```yaml
external_components:
  - source: github://p1ngb4ck/esphome-bluetooth@main
    components: [a2dp_source, ble_broadcast, ble_client, ble_client_hid, ble_nus, bt_hid, bt_host, bt_offload,
                 esp32_ble, esp32_ble_tracker]
  - {source: github://p1ngb4ck/esphome-esp32-platform@main, components: [esp32]}
  - {source: github://p1ngb4ck/esphome-hid@main, components: [hid]}
  - {source: github://p1ngb4ck/esphome-storage-interface@main, components: [storage]}
  - {source: github://p1ngb4ck/esphome-network@main, components: [wifi]}
storage:
esp32_ble:
esp32_ble_tracker:
bt_host: {pairing: {max_devices: 4}}
```

Dongles, `bt_offload`: esphome-usb-drivers, esphome-usb-host; `ble_nus`: esphome-uart-tcp; P4: BLE via the C6.

## Main options
| Option | What it does | Default |
|---|---|---|
| `bt_host` `pairing:` [`max_devices`][o-pair] | paired devices kept, 1-32 | `8` |
| `bt_host` `pairing:` [`max_hid_devices`][o-pair] | HID slots for runtime pairing, 0-8 | `0` |
| `bt_host` `pairing:` [`file`][o-pair] | pairing database as a file on a storage | preferences |
| `bt_host` `persistence:` [`backend`][o-pers] | Bluedroid's bonds in a `storage` file or `nvs` | `storage` |
| `bt_host` `persistence:` [`gatt_cache`][o-pers] | persist the GATT client cache | unchanged |
| `bt_hid` [`pin`][o-hid] | PIN for devices without Secure Simple Pairing | `0000` |
| `ble_client_hid` [`encryption`][o-bhid] | bond and encrypt | `true` |
| `a2dp_source` [`target_name`][o-a2dp] | pair with this name; empty = strongest device | `""` |
| `a2dp_source` [`reconnect`][o-a2dp] | `listen` or `paging` | `listen` |
| `a2dp_source` [`idle_timeout`][o-a2dp] | suspend the stream without audio | `1s` |

CONFIGURATION.md still documents the pairing and persistence options under `bt_pairing` / `bt_persistence`; with
`bt_host` they are its `pairing:` / `persistence:` blocks.

[o-pair]: CONFIGURATION.md#bt_pairing
[o-pers]: CONFIGURATION.md#bt_persistence
[o-hid]: CONFIGURATION.md#bt_hid
[o-bhid]: CONFIGURATION.md#ble_client_hid
[o-bc]: CONFIGURATION.md#ble_broadcast
[o-a2dp]: CONFIGURATION.md#a2dp_source
[o-up]: CONFIGURATION.md#changes-to-upstream-components

## Catalog
Components (folder on `main` - options):
- [`a2dp_source`][c-a2dp_source] - [a2dp_source][o-a2dp]
- [`ble_broadcast`][c-ble_broadcast] - [ble_broadcast][o-bc]
- [`ble_client`][c-ble_client] - [changes to upstream components][o-up]
- [`ble_client_hid`][c-ble_client_hid] - [ble_client_hid][o-bhid]
- [`ble_nus`][c-ble_nus] - [changes to upstream components][o-up]
- [`bt_hid`][c-bt_hid] - [bt_hid][o-hid]
- [`bt_host`][c-bt_host] - [pairing][o-pair], [persistence][o-pers]
- [`bt_offload`][c-bt_offload] - [changes to upstream components][o-up]
- [`esp32_ble`][c-esp32_ble] - [changes to upstream components][o-up]
- [`esp32_ble_tracker`][c-esp32_ble_tracker] - [changes to upstream components][o-up]

[c-a2dp_source]: https://github.com/p1ngb4ck/esphome-bluetooth/tree/main/esphome/components/a2dp_source
[c-ble_broadcast]: https://github.com/p1ngb4ck/esphome-bluetooth/tree/main/esphome/components/ble_broadcast
[c-ble_client]: https://github.com/p1ngb4ck/esphome-bluetooth/tree/main/esphome/components/ble_client
[c-ble_client_hid]: https://github.com/p1ngb4ck/esphome-bluetooth/tree/main/esphome/components/ble_client_hid
[c-ble_nus]: https://github.com/p1ngb4ck/esphome-bluetooth/tree/main/esphome/components/ble_nus
[c-bt_hid]: https://github.com/p1ngb4ck/esphome-bluetooth/tree/main/esphome/components/bt_hid
[c-bt_host]: https://github.com/p1ngb4ck/esphome-bluetooth/tree/main/esphome/components/bt_host
[c-bt_offload]: https://github.com/p1ngb4ck/esphome-bluetooth/tree/main/esphome/components/bt_offload
[c-esp32_ble]: https://github.com/p1ngb4ck/esphome-bluetooth/tree/main/esphome/components/esp32_ble
[c-esp32_ble_tracker]: https://github.com/p1ngb4ck/esphome-bluetooth/tree/main/esphome/components/esp32_ble_tracker

Branches:
- [main][b-main] (the whole topic)
- [pr/a2dp-source][b-1]
- [pr/ble-nus][b-2]
- [pr/bt-classic][b-3]
- [pr/bt-host][b-4]
- [pr/bt-offload][b-5]
- [pr/bt-pairing-hid][b-6]
- [pr/bt-persistence][b-7]
- [pr/esp32-ble-bt-logs][b-8]
- [pr/esp32-ble-nvs-ready][b-9]
- [pr/esp32-ble-start-order][b-10]
- [pr/esp32-ble-tracker-classic-scan-window][b-11]
- [pr/esp32-ble-tracker-pause-scan][b-12]

[b-main]: https://github.com/p1ngb4ck/esphome-bluetooth/tree/main
[b-1]: https://github.com/p1ngb4ck/esphome-bluetooth/tree/pr/a2dp-source
[b-2]: https://github.com/p1ngb4ck/esphome-bluetooth/tree/pr/ble-nus
[b-3]: https://github.com/p1ngb4ck/esphome-bluetooth/tree/pr/bt-classic
[b-4]: https://github.com/p1ngb4ck/esphome-bluetooth/tree/pr/bt-host
[b-5]: https://github.com/p1ngb4ck/esphome-bluetooth/tree/pr/bt-offload
[b-6]: https://github.com/p1ngb4ck/esphome-bluetooth/tree/pr/bt-pairing-hid
[b-7]: https://github.com/p1ngb4ck/esphome-bluetooth/tree/pr/bt-persistence
[b-8]: https://github.com/p1ngb4ck/esphome-bluetooth/tree/pr/esp32-ble-bt-logs
[b-9]: https://github.com/p1ngb4ck/esphome-bluetooth/tree/pr/esp32-ble-nvs-ready
[b-10]: https://github.com/p1ngb4ck/esphome-bluetooth/tree/pr/esp32-ble-start-order
[b-11]: https://github.com/p1ngb4ck/esphome-bluetooth/tree/pr/esp32-ble-tracker-classic-scan-window
[b-12]: https://github.com/p1ngb4ck/esphome-bluetooth/tree/pr/esp32-ble-tracker-pause-scan

Needed topic repos:
- [esphome-esp32-platform](https://github.com/p1ngb4ck/esphome-esp32-platform): `esp32` (`nvs_ready`).
- [esphome-hid](https://github.com/p1ngb4ck/esphome-hid): `hid` (HID input of the drivers and slots).
- [esphome-storage-interface](https://github.com/p1ngb4ck/esphome-storage-interface): `storage` (pairing file,
  persistence backend).
- [esphome-network](https://github.com/p1ngb4ck/esphome-network): `wifi` (Bluedroid starts after WiFi's start-up),
  `esp32_hosted` (P4 with C6).
- For dongles and `bt_offload`: [esphome-usb-drivers](https://github.com/p1ngb4ck/esphome-usb-drivers)
  (`usb_bluetooth`), [esphome-usb-host](https://github.com/p1ngb4ck/esphome-usb-host) (`usb_host`).
- For `ble_nus`: [esphome-uart-tcp](https://github.com/p1ngb4ck/esphome-uart-tcp) (`uart`).

All topics together: `github://p1ngb4ck/esphome@dev`.

---

# Detailed version

ESPHome `dev` plus the Bluetooth host and HID parts of the project: Classic GAP dispatch, pairing records and slots,
Bluedroid data persistence, Classic and BLE HID, BLE advertisement data, Bluetooth audio (A2DP source), the Bluedroid
start order, the C6 offload. One branch per change; `main` contains every branch. Base: ESPHome `dev` at
`a5a75738ad`. The USB dongle (`usb_bluetooth`) is in
[esphome-usb-drivers](https://github.com/p1ngb4ck/esphome-usb-drivers).

`main` holds the rework into one host component, `bt_host` (scan, pairing, security, bonding for BLE and Classic),
with the profiles as drivers; its own branch is `pr/bt-host`. The branches `pr/bt-classic`, `pr/bt-pairing-hid` and
`pr/bt-persistence` are the state it starts from (`bt_classic`, `bt_pairing`, `bt_persistence` as separate
components, which `main` no longer has).

## Branches

Per branch: change; chips; needs.

- `pr/esp32-ble-start-order`: Bluedroid starts once WiFi reports its start-up done and, with a usb_bluetooth
  bluedroid dongle, once the dongle is ready; `restart()`; ESP32 with BLE; needs `pr/wifi-startup-done-callback`.
- `pr/esp32-ble-bt-logs`: `disable_bt_logs` also quiets the Bluedroid layers the build keeps (ERROR instead of their
  defaults); ESP32 with BLE; needs -.
- `pr/esp32-ble-nvs-ready`: esp32_ble waits for `nvs_ready`; ESP32 with BLE; needs `pr/esp32-nvs-ready`.
- `pr/bt-classic`: `bt_classic`: one Classic GAP callback with handlers and fallbacks, the shared IO capability and
  PIN type, BTDM dual mode on the ESP32-S31; S31 on chip, others with a dongle; needs -.
- `pr/bt-persistence`: `bt_persistence`: Bluedroid's pairing data and GATT cache in PSRAM, saved to a storage file
  (`backend: storage`) or NVS; with esp32_ble or usb_bluetooth; needs `pr/storage`.
- `pr/bt-pairing-hid`: `bt_pairing` (pairing records, slots, device list, status events), `bt_hid` (Classic HID
  host), `ble_client_hid` (BLE HID), `ble_broadcast` (BLE advertisement data) with the ble_client / esp32_ble / api
  hooks they use; one branch because `bt_pairing` creates the slots of the other three; chips see below; needs
  `pr/bt-classic`, `pr/hid`, `pr/storage`.
- `pr/esp32-ble-tracker-classic-scan-window`: the defaulted BLE scan window stays 30 ms when Bluetooth Classic runs
  on the same radio; ESP32, S31; needs -.
- `pr/esp32-ble-tracker-pause-scan`: `pause_scan` / `resume_scan`: BLE scanning paused while A2DP streams on the
  chip's own radio; ESP32, S31; needs -.
- `pr/a2dp-source`: `a2dp_source`: A2DP source with AVRCP (vendored ESP32-A2DP library) - speaker, device select,
  volume, microphone source, reconnect, `idle_timeout`; ESP32 on chip; S31 on chip (with esp32_ble); others with a
  usb_bluetooth bluedroid dongle; needs `pr/bt-pairing-hid`, `pr/esp32-ble-tracker-pause-scan`.
- `pr/bt-host`: `bt_host`: `bt_classic`, `bt_pairing` and `bt_persistence` as one host component (`pairing:` /
  `persistence:` blocks); the drivers' discovery, SSP, bond removal and scan mode through it; `ble_controller` /
  `classic_controller` per role; Classic HID on the chip's own Classic (ESP32, ESP32-S31); every chip with Bluedroid;
  needs `pr/bt-classic`, `pr/bt-offload`, `pr/esp32-ble-bt-logs`, `pr/esp32-ble-nvs-ready`,
  `pr/esp32-ble-tracker-classic-scan-window`, `pr/esp32-ble-tracker-pause-scan`, `pr/hid`, `pr/esp32-exfat-fat-volumes`.
- `pr/bt-offload`: `bt_offload`: Bluetooth HID input and speaker offloaded to the C6 (custom firmware), the 16 peer
  data handlers of esp32_hosted on the P4; P4 with C6; needs `pr/esp32-ble-start-order`, `pr/usb-bluetooth`.
- `pr/ble-nus`: `ble_nus` with the UART debug callback's prefix argument; chips as `ble_nus`; needs
  `pr/uart-debugger-prefix-settings`.

Chips as the code declares today: `ble_client_hid` and `ble_broadcast` on every chip with BLE (the P4 through the
C6); `bt_hid` (Classic HID) needs a `usb_bluetooth` dongle with `bluedroid: true` on every chip, the ESP32 and
ESP32-S31 included, although both have Classic on chip [seen bt_hid/__init__.py:106, bt_pairing/__init__.py:313].
That is one of the points of the planned rework.

`pr/storage` and `pr/esp32-exfat-fat-volumes` are in
[esphome-storage-interface](https://github.com/p1ngb4ck/esphome-storage-interface), `pr/hid` in
[esphome-hid](https://github.com/p1ngb4ck/esphome-hid), `pr/esp32-nvs-ready` in
[esphome-storage-drivers](https://github.com/p1ngb4ck/esphome-storage-drivers), `pr/wifi-startup-done-callback` in
[esphome-network](https://github.com/p1ngb4ck/esphome-network), `pr/usb-bluetooth` in esphome-usb-drivers,
`pr/uart-debugger-prefix-settings` in [esphome-uart-tcp](https://github.com/p1ngb4ck/esphome-uart-tcp).

## Contributions

- [NonaSuomy](https://github.com/NonaSuomy): ideas behind the Bluetooth work, among them the C6 offload firmware
  (`bt_offload`).

## Use

On `main` (with `bt_host`; the other components from their owner repos):

```yaml
external_components:
  - source: github://p1ngb4ck/esphome-bluetooth@main
    components: [esp32_ble, esp32_ble_tracker, ble_client, ble_client_hid, ble_broadcast, bt_hid, bt_host,
                 a2dp_source]
  - {source: github://p1ngb4ck/esphome-esp32-platform@main, components: [esp32]}
  - {source: github://p1ngb4ck/esphome-hid@main, components: [hid]}
  - {source: github://p1ngb4ck/esphome-storage-interface@main, components: [storage]}
  - {source: github://p1ngb4ck/esphome-network@main, components: [wifi]}

esp32_ble:
  enable_on_boot: true
  io_capability: display_only

esp32_ble_tracker:

bt_host:
  classic_controller: usb
  ble_controller: c6
  pairing:
    id: bt_pairing_db
    file: /sdcard/bluetooth/bt_pairing.bin
    max_devices: 32
    max_hid_devices: 7
    max_broadcast_devices: 1
    layout: de
  persistence:
    id: bt_store
    backend: storage
    path: /sdcard/bluetooth/bt_store.bin
    write_delay: 2s
    storage_timeout: 20s
    gatt_cache: true
```

`classic_controller: usb` needs `usb_bluetooth` ([esphome-usb-drivers](https://github.com/p1ngb4ck/esphome-usb-drivers))
and `usb_host` ([esphome-usb-host](https://github.com/p1ngb4ck/esphome-usb-host)); `ble_controller: c6` needs
`esp32_hosted` (esphome-network).

On the branches before `bt_host` (`pr/bt-pairing-hid`, `pr/bt-persistence`) the same as separate components (the
preference keys are unchanged, so stored pairings stay when moving to `bt_host`):

```yaml
bt_pairing:
  id: bt_pairing_db
  file: /sdcard/bluetooth/bt_pairing.bin
  max_devices: 32
  max_hid_devices: 7
  max_broadcast_devices: 1
  layout: de

bt_persistence:
  id: bt_store
  backend: storage
  path: /sdcard/bluetooth/bt_store.bin
  write_delay: 2s
  storage_timeout: 20s
  gatt_cache: true
```

Actions `bt_pairing.*` are `bt_host.*`, `bt_persistence.set_path` is `bt_host.persistence_set_path`, the select
platform `bt_pairing` is `bt_host`, `bt_pairing_id` is `bt_host_id`; `external_components` lists `bt_host` instead of
`bt_classic`, `bt_pairing`, `bt_persistence`.

From the Waveshare ESP32-P4 Pi board's config (BLE on the C6, Classic on a USB dongle). `external_components`
replaces components only; the ESPHome core of the build host stays as installed. With the other topics together
use `github://p1ngb4ck/esphome@dev`.

## Branch structure

Each `pr/` branch holds one part of this topic on top of upstream and says in its `BRANCH.md` what it is based on.
`main` holds all of them.

- `pr/a2dp-source`: `pr/bt-pairing-hid`, `pr/esp32-ble-tracker-pause-scan`
- `pr/ble-nus`: `pr/uart-debugger-prefix-settings` of p1ngb4ck/esphome-uart-tcp
- `pr/bt-classic`: upstream only
- `pr/bt-host`: `pr/bt-classic`, `pr/bt-offload`, `pr/esp32-ble-bt-logs`, `pr/esp32-ble-nvs-ready`,
  `pr/esp32-ble-tracker-classic-scan-window`, `pr/esp32-ble-tracker-pause-scan`, `pr/hid` of p1ngb4ck/esphome-hid,
  `pr/esp32-exfat-fat-volumes` of p1ngb4ck/esphome-storage-interface
- `pr/bt-offload`: `pr/esp32-ble-start-order`, `pr/usb-bluetooth` of p1ngb4ck/esphome-usb-drivers
- `pr/bt-pairing-hid`: `pr/bt-classic`, `pr/hid` of p1ngb4ck/esphome-hid, `pr/storage` of
  p1ngb4ck/esphome-storage-interface
- `pr/bt-persistence`: `pr/storage` of p1ngb4ck/esphome-storage-interface
- `pr/esp32-ble-bt-logs`: upstream only
- `pr/esp32-ble-nvs-ready`: `pr/esp32-nvs-ready` of p1ngb4ck/esphome-storage-drivers
- `pr/esp32-ble-start-order`: `pr/wifi-startup-done-callback` of p1ngb4ck/esphome-network
- `pr/esp32-ble-tracker-classic-scan-window`: upstream only
- `pr/esp32-ble-tracker-pause-scan`: upstream only
