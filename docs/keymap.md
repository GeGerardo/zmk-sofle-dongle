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

## Build with GitHub Actions (recommended)

1. Fork this repository on GitHub.
2. In your fork, go to **Actions** and enable workflows if GitHub prompts you.
3. Modify the keymap:
   - Edit the keymap file directly in GitHub, or
   - Use the Keymap Editor: https://nickcoutsos.github.io/keymap-editor/
4. Commit your changes.
5. GitHub Actions will run automatically (or use **Run workflow**).
6. Download the build artifact from the Actions run.

## Alternative: Keymap Editor (web UI)

Keymap Editor is a convenient way to change key bindings (and some simple configuration) using a graphical UI:

- https://nickcoutsos.github.io/keymap-editor/

How it works (high level):

1. Log in with GitHub.
2. Authorize the Keymap Editor app to access your GitHub repositories.
3. Select your fork of this ZMK config repo from the list.
4. Make changes and click **Save**.
5. GitHub Actions builds new firmware automatically.

Security note:

- Authorizing Keymap Editor grants it access to one or more of your GitHub repositories (depending on what you approve).
- Only do this if you are comfortable with that access and your repo does not contain sensitive information.

## Flashing the firmware (`.uf2`)

1. Connect the target device (dongle, left, or right) via USB.
2. Double-press the reset button to enter bootloader mode.
3. A USB drive will appear.
4. Copy the correct `.uf2` file to the root of that drive.

Important:
- Don’t delete files from the bootloader drive.
- After flashing the dongle, use **"Restore Stock Settings"** in ZMK Studio to load the new keymap (if you've used Studio before).

## When to flash each device

| Change | Dongle | Left | Right |
|--------|--------|------|-------|
| Keymap changes | ✅ | ❌ | ❌ |
| Display settings (nice_view/OLED) | ✅ (OLED) | ✅ | ✅ |
| Peripheral config (sleep timeout, etc.) | ❌ | ✅ | ✅ |
| Bluetooth/pairing issues | ✅ | ✅ | ✅ |

## When to use `settings_reset`

Flash `settings_reset.uf2` **only** when:
- Bluetooth pairing is broken or devices won't connect
- Switching between dongle mode and direct keyboard mode
- First-time setup after receiving hardware

**Not needed** for normal keymap updates.

## If a build fails

- Undo the last keymap change and try again.
- If needed, re-fork the repo to return to a known-good state, then re-apply your changes with the Keymap Editor to reduce syntax errors.
