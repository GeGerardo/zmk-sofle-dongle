# Dongle usage (receiver mode)

This keyboard is configured to run in **dongle mode**:

- The **dongle/receiver** is the ZMK "central" device.
- The **left + right halves** are ZMK "peripherals".
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

## OLED display

The dongle has a small OLED display. It shows:
- Battery levels for the dongle, left half, and right half
- Current connection state (USB or Bluetooth)
- Active layer

> The macOS modifier logo can be enabled by uncommenting `CONFIG_ZMK_DONGLE_DISPLAY_MAC_MODIFIERS=y` in `config/eyelash_sofle_central_dongle.conf`.

## Suggested placement

- Use the included stand to angle the dongle on the desk.
- Optionally mount it on top of your monitor (the dongle includes a magnet; a metal patch can be attached to the monitor).

## First-time setup / re-pairing

See the [README](../README.md#how-to-flash-firmware) for the full reset and re-pairing sequence.

If pairing is broken or devices won't connect, always do the full sequence: flash `settings_reset` to all three devices first, then flash the normal firmware to each.

## Live keymap changes (ZMK Studio)

Connect the **dongle** via USB and follow the [ZMK Studio guide](zmk-studio.md).

## Troubleshooting

**Halves won't pair with the dongle after flashing**
- Make sure you flashed `settings_reset` to all three devices before flashing the normal firmware.
- Power cycle all devices after flashing.

**Dongle connects to the computer but keypresses aren't registering**
- Check that both halves are powered on.
- Move the halves closer to the dongle; BLE range can be limited by desk obstacles.

**OLED is blank**
- The display turns off when the keyboard is idle. Press any key to wake it.
- If it stays blank after activity, reflash the dongle firmware.

**One half is not responding**
- Try power cycling that half (flip its power switch off and back on).
- If it still doesn't respond, flash `settings_reset` to that half only, then reflash its normal firmware.

---

<details>
<summary>Switching from dongle mode to direct keyboard mode</summary>

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

</details>
