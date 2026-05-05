# ZMK Sofle (dongle mode) config

This repository has the ZMK configuration for an **Eyelash Sofle** split keyboard running in **dongle/receiver mode**. It includes the shield definitions, keymap, and a GitHub Actions workflow that builds the firmware automatically.

## Hardware

- Controller board: `nice_nano_v2`
- SoC: Nordic nRF52840

## What this repo builds

GitHub Actions builds these firmware files (see [build.yaml](build.yaml)):

| Firmware file | Description |
|---|---|
| `eyelash_sofle_central_dongle_oled` | The **dongle/receiver** (ZMK central, with OLED display) |
| `eyelash_sofle_peripheral_left` | Left half |
| `eyelash_sofle_peripheral_right` | Right half |
| `settings_reset` | Clears stored settings and pairings |

## Quick start (GitHub Actions)

1. Fork this repository.
2. Go to **Actions** and enable workflows if GitHub asks.
3. Edit the keymap or use the [Keymap Editor](https://nickcoutsos.github.io/keymap-editor/) web UI (requires GitHub login).
4. Commit your changes. GitHub Actions will build the firmware automatically.
5. Open the latest **Build ZMK firmware** run and download the artifact.
6. Flash the `.uf2` file to the dongle, left half, or right half as needed (see below).

## How to flash firmware

1. Connect the target device (dongle, left half, or right half) to your computer via USB.
2. Press the reset button **twice quickly**. A USB drive will appear on your computer (usually named `NICENANO`).
3. Copy the correct `.uf2` file to the root of that drive. Do not put it inside a folder.
4. The device restarts automatically once the file is copied.

For first-time setup or if the devices stopped pairing with each other, follow this order:

1. Flash `settings_reset.uf2` to **all three** devices: dongle, left half, right half.
2. Then flash the normal firmware to each device: dongle firmware to the dongle, left firmware to the left half, right firmware to the right half.
3. Power on all three devices. They will pair automatically.

## Key files

- [build.yaml](build.yaml): build list (which boards and shields get built)
- [config/eyelash_sofle_central_dongle.keymap](config/eyelash_sofle_central_dongle.keymap): your keymap
- [config/eyelash_sofle_central_dongle.conf](config/eyelash_sofle_central_dongle.conf): config options
- [boards/shields/](boards/shields/): shield definitions (pin assignments, matrix, default keymap)
- [config/west.yml](config/west.yml): pins the ZMK version and adds extra modules
- [zephyr/module.yml](zephyr/module.yml): registers this repo as a Zephyr module

## Live keymap changes (ZMK Studio)

This repo supports **ZMK Studio**, which lets you change key assignments live without rebuilding or flashing firmware.

- Guide: [docs/zmk-studio.md](docs/zmk-studio.md)
- Official docs: https://zmk.dev/docs/features/studio

Note: once you start managing your keymap through ZMK Studio, future edits to the `.keymap` file will not take effect until you use **”Restore Stock Settings”** in ZMK Studio.

## Documentation

- [docs/dongle-usage.md](docs/dongle-usage.md): dongle setup, pairing, and placement
- [docs/keymap.md](docs/keymap.md): changing the keymap, flashing, and using the Keymap Editor

## Keymap diagram

<img src="keymap-drawer/eyelash_sofle_central_dongle.svg">
