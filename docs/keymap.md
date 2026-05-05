# Keymap changes

There are two main ways to change keys:

1) **ZMK Studio** (live changes; no re-flash for keymap updates)
- Recommended for most key changes.
- See: [ZMK Studio](zmk-studio.md)

2) **Rebuild + re-flash firmware** (for structural/advanced changes)
- Required when you change features/settings beyond what ZMK Studio can edit.
- Examples: enabling/disabling features, changing devicetree/overlays, changing what behaviors are available, changing sensors/encoders configuration, etc.

## File naming convention

ZMK requires config files to match the **shield name** exactly:

| Shield (in build.yaml) | Config file |
|------------------------|-------------|
| `eyelash_sofle_central_dongle` | `config/eyelash_sofle_central_dongle.keymap` |
| `eyelash_sofle_central_dongle` | `config/eyelash_sofle_central_dongle.conf` |

The shield's default keymap (`boards/shields/eyelash_sofle/eyelash_sofle.keymap`) is used as a fallback if no matching config file is found.

## Editing the keymap in this repo

- Primary keymap file: [config/eyelash_sofle_central_dongle.keymap](../config/eyelash_sofle_central_dongle.keymap)
- Configuration options: [config/eyelash_sofle_central_dongle.conf](../config/eyelash_sofle_central_dongle.conf)

Tip: Only the **central device** (dongle) needs a keymap. The left/right peripherals just send key positions to the central.

## Useful options in `.conf`

The file `config/eyelash_sofle_central_dongle.conf` overrides defaults for the dongle. Common things to adjust:

| Option | Default | What it does |
|--------|---------|--------------|
| `CONFIG_ZMK_IDLE_SLEEP_TIMEOUT` | `3600000` (1 h) | How long before the dongle sleeps, in ms |
| `CONFIG_ZMK_MOUSE` | `y` | Enables mouse/pointer support |
| `CONFIG_ZMK_DISPLAY` | `y` | Enables the OLED display |
| `CONFIG_ZMK_DONGLE_DISPLAY_DONGLE_BATTERY` | `y` | Shows battery levels on OLED |
| `CONFIG_ZMK_DONGLE_DISPLAY_MAC_MODIFIERS` | commented out | Shows macOS modifier logo on OLED |
| `CONFIG_BT_CTLR_TX_PWR_PLUS_8` | `y` | Increases BLE TX power (+8 dBm) |

## Build with GitHub Actions (recommended)

1. Fork this repository on GitHub.
2. In your fork, go to **Actions** and enable workflows if GitHub prompts you.
3. Modify the keymap:
   - Edit the keymap file directly in GitHub, or
   - Use the [Keymap Editor](https://nickcoutsos.github.io/keymap-editor/) (log in with GitHub, select your fork, edit, and save)
4. Commit your changes.
5. GitHub Actions will run automatically (or use **Run workflow**).
6. Download the build artifact from the Actions run.

> **Security note:** Authorizing Keymap Editor grants it access to one or more of your GitHub repositories. Only do this if you are comfortable with that access and your repo does not contain sensitive information.

## Flashing the firmware (`.uf2`)

1. Connect the target device (dongle, left, or right) via USB.
2. Double-press the reset button to enter bootloader mode.
3. A USB drive will appear.
4. Copy the correct `.uf2` file to the root of that drive.

Important:
- Don't delete files from the bootloader drive.
- After flashing the dongle, use **"Restore Stock Settings"** in ZMK Studio to load the new keymap (if you've used Studio before).

## When to flash each device

| Change | Dongle | Left | Right |
|--------|--------|------|-------|
| Keymap changes | ✅ | ❌ | ❌ |
| Display settings (OLED) | ✅ | ❌ | ❌ |
| RGB / backlight config | ✅ | ✅ | ✅ |
| Peripheral config (sleep timeout, etc.) | ❌ | ✅ | ✅ |
| Bluetooth/pairing issues | ✅ | ✅ | ✅ |

## When to use `settings_reset`

Flash `settings_reset.uf2` **only** when:
- Bluetooth pairing is broken or devices won't connect
- Switching between dongle mode and direct keyboard mode
- First-time setup after receiving hardware

**Not needed** for normal keymap updates.

## If a build fails

- Check the **Actions** log: open the failed run, expand the failing step, and read the error line. it usually points to a syntax error in the keymap file.
- Undo the last keymap change and try again.
- If the Keymap Editor produced the error, try making the change manually in the `.keymap` file instead. the editor occasionally generates invalid syntax.
- If needed, re-fork the repo to return to a known-good state, then re-apply your changes with the Keymap Editor.

## Keymap diagram

The keymap diagram is auto-generated and shown in the [README](../README.md#keymap-diagram). It is rebuilt automatically on each push via the `draw.yml` workflow. To preview it locally, open `keymap-drawer/eyelash_sofle_central_dongle.svg` in a browser.

For a full breakdown of every file and folder in the repo, see the [Project structure](../README.md#project-structure) section in the README.
