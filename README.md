# Corne Wireless ZMK Config

ZMK firmware configuration for a [Typeractive Corne Wireless](https://typeractive.xyz) split keyboard.

## Hardware

- **PCB:** Typeractive Corne Wireless (partially assembled)
- **Controllers:** nice!nano v2.0
- **Displays:** nice!view (Sharp Memory LCD)
- **Connection:** Bluetooth (split halves + host)

## Keymap

Vim-optimized 3-layer layout with ESC on top-left and modifiers on thumbs.

### Base Layer

```
╭──────┬──────┬──────┬──────┬──────┬──────╮   ╭──────┬──────┬──────┬──────┬──────┬──────╮
│ ESC  │  Q   │  W   │  E   │  R   │  T   │   │  Y   │  U   │  I   │  O   │  P   │ BKSP │
├──────┼──────┼──────┼──────┼──────┼──────┤   ├──────┼──────┼──────┼──────┼──────┼──────┤
│ SHFT │  A   │  S   │  D   │  F   │  G   │   │  H   │  J   │  K   │  L   │  ;   │ SHFT │
├──────┼──────┼──────┼──────┼──────┼──────┤   ├──────┼──────┼──────┼──────┼──────┼──────┤
│ TAB  │  Z   │  X   │  C   │  V   │  B   │   │  N   │  M   │  ,   │  .   │  /   │ ENT  │
╰──────┴──────┴──────┼──────┼──────┼──────┤   ├──────┼──────┼──────┼──────┴──────┴──────╯
                     │ LWR  │ GUI  │ CTRL │   │ ALT  │ SPC  │ RSE  │
                     ╰──────┴──────┴──────╯   ╰──────┴──────┴──────╯
```

### Lower Layer (hold LWR)

Navigation, F-keys, Bluetooth, brightness, and volume.

```
╭──────┬──────┬──────┬──────┬──────┬──────╮   ╭──────┬──────┬──────┬──────┬──────┬──────╮
│      │      │      │ PGUP │      │      │   │BRI_UP│  F7  │  F8  │  F9  │ F10  │VOL_UP│
├──────┼──────┼──────┼──────┼──────┼──────┤   ├──────┼──────┼──────┼──────┼──────┼──────┤
│ CAPS │      │ HOME │ PGDN │ END  │      │   │BRI_DN│  F4  │  F5  │  F6  │ F11  │VOL_DN│
├──────┼──────┼──────┼──────┼──────┼──────┤   ├──────┼──────┼──────┼──────┼──────┼──────┤
│BTCLR │ BT0  │ BT1  │ BT2  │ BT3  │ BT4  │   │      │  F1  │  F2  │  F3  │ F12  │ MUTE │
╰──────┴──────┴──────┼──────┼──────┼──────┤   ├──────┼──────┼──────┼──────┴──────┴──────╯
                     │ LWR  │ GUI  │ CTRL │   │ ALT  │ SPC  │ SHFT │
                     ╰──────┴──────┴──────╯   ╰──────┴──────┴──────╯
```

### Raise Layer (hold RSE)

Numbers (numpad layout), symbols, and arrow keys.

```
╭──────┬──────┬──────┬──────┬──────┬──────╮   ╭──────┬──────┬──────┬──────┬──────┬──────╮
│  `   │  !   │  @   │  UP  │  $   │  %   │   │  &   │  7   │  8   │  9   │  ,   │ DEL  │
├──────┼──────┼──────┼──────┼──────┼──────┤   ├──────┼──────┼──────┼──────┼──────┼──────┤
│  _   │  (   │ LEFT │ DOWN │RIGHT │  )   │   │  ^   │  4   │  5   │  6   │  0   │  -   │
├──────┼──────┼──────┼──────┼──────┼──────┤   ├──────┼──────┼──────┼──────┼──────┼──────┤
│  '   │  [   │  \   │  #   │  /   │  ]   │   │  *   │  1   │  2   │  3   │  .   │  =   │
╰──────┴──────┴──────┼──────┼──────┼──────┤   ├──────┼──────┼──────┼──────┴──────┴──────╯
                     │ SHFT │ GUI  │ CTRL │   │ ALT  │ SPC  │ RSE  │
                     ╰──────┴──────┴──────╯   ╰──────┴──────┴──────╯
```

### Shifted Symbols

These symbols are available via SHFT + key in the Raise layer:

| Key | Shift + Key |
|-----|-------------|
| `   | ~           |
| '   | "           |
| ,   | <           |
| .   | >           |
| /   | ?           |
| \   | \|          |
| -   | _           |
| =   | +           |
| [   | {           |
| ]   | }           |

## Building

Push to `main` and GitHub Actions builds the firmware automatically using [ZMK's build workflow](https://github.com/zmkfirmware/zmk) pinned to `v0.3`.

Firmware artifacts are downloadable from the Actions tab.

## Flashing

1. Plug a half into USB
2. Double-tap the reset button on the nice!nano to enter bootloader (`NICENANO` drive appears)
3. Copy the matching `.uf2` file onto the drive:
   - Left half: `corne_left nice_view_adapter nice_view-nice_nano_v2-zmk.uf2`
   - Right half: `corne_right nice_view_adapter nice_view-nice_nano_v2-zmk.uf2`
4. Repeat for the other half

## Bluetooth Pairing

Supports 5 BT profiles (BT0-BT4). Switch profiles with LWR + BT key on the bottom row.

### Linux (bluetoothctl)

```bash
sudo systemctl start bluetooth
sudo rfkill unblock bluetooth
bluetoothctl
# power on → agent on → default-agent → scan on
# pair/trust/connect <MAC>
```

### macOS

System Settings > Bluetooth > Connect to "Corne"

## Troubleshooting

### Halves won't pair

Flash `settings_reset` firmware to both halves (add `settings_reset` shield to `build.yaml`, build, flash both, then flash real firmware).

### Displays garbled

Ensure `build.yaml` includes `nice_view_adapter nice_view` in the shield string. Firmware built without these shields uses the wrong display driver.

### Build fails with "No board named 'nice_nano_v2'"

The `config/west.yml` must pin to `revision: v0.3`. ZMK's `main` branch includes Zephyr 4.1 hwmv2 changes that rename boards and break `nice_view_adapter`.
