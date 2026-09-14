# Agent notes

ZMK config for the Forager, a 34-key split keyboard on a Seeed XIAO nRF52840. It's pinned to ZMK v0.3 in `config/west.yml` and `.github/workflows/build.yml`. GitHub Actions builds the firmware on every push.

## Keep keymap.html in sync

`keymap.html` is a standalone page that shows the layout. It doesn't read the keymap file, so it goes out of date unless you update it by hand.

**Whenever you change `config/forager.keymap` (or the physical layout in `forager.dtsi`), update `keymap.html` in the same change.** Match each change to the data in the page's `<script>`:

| Keymap change | Update in `keymap.html` |
| --- | --- |
| A key binding in a layer | That layer's string in `SRC`. Copy the bindings exactly and keep them in key-position order (0–33). |
| A combo added, removed or changed | The `COMBOS` array: `keys`, `out`, `name`, plus `layers` and `ms` when they aren't the defaults (all layers, 30 ms). |
| A layer added, removed or renamed | `LAYER_IDS`, `LAYERS` (name and "how to reach" text), the `--l-<id>` color tokens in the CSS, and `SRC`. |
| A new behavior or macro | A `case` in `parse()` and an entry in `BEH`. Add timing values if it has any. |
| A new keycode or shortcut | `KEY` / `NAME` for plain keys, `ACT` for shortcuts that should get a word label (e.g. `LG(C)` → Copy). |
| Timing (`tapping-term-ms`, `quick-tap-ms`, flavor) | The matching entry in `BEH`. |
| Key positions in `forager.dtsi` | `PHYS`. |

Also update the ASCII diagram comment above the layer in `forager.keymap`, and the combo count and commit reference in the page header/footer if they change.

A published copy of the page also exists on claude.ai. Update that copy only when the user asks.

## Other notes

- Board name is `seeeduino_xiao_ble` on ZMK v0.3 (Zephyr 3.5). If ZMK is ever bumped to a Zephyr 4.x release, it becomes `xiao_ble//zmk`, and `zmk-rgbled-widget` needs a matching version.
- Each half wakes from soft off with its outer top key, set in the shield overlays. The right half often doesn't wake because its wake key is part of the soft-off combo; the owner uses the reset button instead.
