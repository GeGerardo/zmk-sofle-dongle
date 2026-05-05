# ZMK Studio (live keymap changes)

ZMK Studio lets you change key assignments at runtime, without rebuilding or flashing firmware (for supported capabilities).

Official docs: https://zmk.dev/docs/features/studio

## What you can (and can't) do

ZMK Studio is great for:
- Editing key bindings while the keyboard is in use
- Renaming layers and enabling extra layers
- Assigning predefined behaviors to keys

ZMK Studio does **not**:
- Define brand new behaviors that are not already in the firmware/devicetree
- Define new physical layouts

If you need those, you must rebuild + flash firmware.

## Requirements (already set up in this repo)

To use ZMK Studio over USB, the firmware needs:
- the `studio-rpc-usb-uart` snippet
- `CONFIG_ZMK_STUDIO=y`

**These are already configured in this repo** - no changes needed. The [build.yaml](../build.yaml) enables those for the dongle/central build only (the recommended setup: only the central should have Studio enabled).

Your keymap also needs a `&studio_unlock` binding to unlock the Studio interface. In this repo it is already present in the **Sys+RGB** layer (`config/eyelash_sofle_central_dongle.keymap`).

## Using Studio

> ZMK Studio works over **USB only**. The dongle must be plugged into your computer.

1. Connect the **dongle/receiver** via USB.
2. Open ZMK Studio:
   - Web: https://zmk.studio/ (Chrome or Edge only. Firefox does not support WebSerial)
   - Native app downloads: https://zmk.studio/download
3. Select the keyboard/serial port that corresponds to the dongle.
   - If it fails to connect, close the page/app and try a different listed port.
4. Activate the `&studio_unlock` binding (on the **Sys+RGB** layer) to unlock editing.

Tip: If you are connected to the computer over both USB and BLE endpoints, set the keyboard output to USB (`&out OUT_USB`) before using Studio.

## Important: interaction with `.keymap` files

From the official ZMK docs:
- Once you start using ZMK Studio to manage your keymap, later edits to the `.keymap` file will **not** apply unless you perform **"Restore Stock Settings"** in ZMK Studio.

### When to use "Restore Stock Settings"

| Scenario | Action needed |
|----------|---------------|
| Changed `.keymap` file and reflashed | **Restore Stock Settings** in ZMK Studio |
| Made changes only in ZMK Studio | No action needed |
| Fresh flash on new device | No action needed |

### Practical guidance

- If you intend to use ZMK Studio long-term, treat `.keymap` as the "baseline/stock" layout.
- Use `.keymap` edits mainly to:
  - add `&studio_unlock`
  - add extra empty `reserved` layers (so Studio can use more layers)
  - make changes that Studio cannot do (new behaviors, encoder bindings, etc.)

## Troubleshooting

**The keyboard doesn't appear in ZMK Studio / no port listed**
- Make sure the dongle is connected via a **data-capable USB cable** (not a charge-only cable).
- Try a different USB port.
- Make sure you're using Chrome or Edge. Firefox does not support WebSerial.

**ZMK Studio connects but changes don't take effect**
- Confirm the keyboard output is set to USB (`&out OUT_USB`). Studio does not work over Bluetooth.
- Try pressing `&studio_unlock` again to re-unlock the interface.

**After reflashing, my Studio layout is gone**
- This is expected. ZMK Studio stores its layout in the dongle's flash memory, separate from the `.keymap` file. After flashing new firmware, that stored layout may reset.
- Use **"Restore Stock Settings"** to re-read the `.keymap` baseline, then re-apply your Studio changes on top.
