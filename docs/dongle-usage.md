# Dongle usage (receiver mode)

This keyboard is configured to run in **dongle mode**:

- The **dongle/receiver** is the ZMK “central” device.
- The **left + right halves** are ZMK “peripherals”.
- Keystrokes are sent from the halves to the dongle first, then forwarded to your computer via **USB** or **Bluetooth**.

## What changes in dongle mode

- The halves do **not** directly connect to the computer; they connect to the dongle.
- After the dongle is configured, power usage on the left half is reduced (per the vendor documentation) and the dongle can operate at a higher report rate.

## Hardware controls on the dongle

- **Battery toggle switch (ON/OFF)**: disconnects the internal battery.
  - If you always use the dongle connected to USB, you can leave the battery switch **OFF** to reduce battery wear.
- **Reset button**:
  - With the dongle connected via USB, **double-press reset** to enter bootloader mode.
  - A USB drive will appear on your computer. Copy the new firmware file (`.uf2`) onto it.

## Suggested placement

- Use the included stand to angle the dongle on the desk.
- Optionally mount it on top of your monitor (the dongle includes a magnet; a metal patch can be attached to the monitor).

## First-time setup / re-pairing (recommended reset sequence)

If you purchased the keyboard earlier and added a dongle later (or if pairing is broken), do a full settings reset:

1. Flash the **`settings_reset`** firmware to:
   - dongle/receiver
   - left half
   - right half
2. Then flash the normal firmware to each device:
   - dongle: the **dongle firmware**
   - left: **left firmware**
   - right: **right firmware**

Notes:
- The `settings_reset` firmware does not distinguish left/right; it’s just to clear stored pairing/settings.
- After flashing, power both halves and the dongle; they should pair automatically.

## Using ZMK Studio (live keymap changes)

If you are using ZMK Studio for live keymap updates, connect the **dongle** via USB and follow the ZMK Studio guide in [ZMK Studio](zmk-studio.md).

## Switching from dongle mode to direct keyboard mode

If you want to use the keyboard without the dongle (left half as USB/BT central):

1. **New shield definitions required**: This repo only defines dongle-mode shields (`_central_dongle`, `_peripheral_left`, `_peripheral_right`). You'd need to create `_left` (as central) and `_right` (as peripheral) shield variants.

2. **Update `build.yaml`**:
   ```yaml
   - board: nice_nano_v2
     shield: eyelash_sofle_left nice_view
   - board: nice_nano_v2
     shield: eyelash_sofle_right nice_view
   ```

3. **Create matching config files**:
   - `config/eyelash_sofle_left.keymap`
   - `config/eyelash_sofle_left.conf`

4. **Flash `settings_reset`** to both halves first, then flash the new firmware.

## Understanding ZMK shields

A "shield" in ZMK defines the hardware attached to the controller board (nice_nano). Each shield name corresponds to:
- An overlay file defining GPIO pins, matrix, sensors
- A default keymap
- Configuration defaults

ZMK looks for user config files matching the shield name exactly:
- Shield: `eyelash_sofle_central_dongle` → Config: `config/eyelash_sofle_central_dongle.keymap`

For more details, see the official ZMK documentation:
- [Shield development](https://zmk.dev/docs/development/hardware-integration/new-shield)
- [Customization](https://zmk.dev/docs/customization)
