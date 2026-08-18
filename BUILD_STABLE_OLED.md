# Sofle Performance v1 Build and Recovery

This document records the matched, known-good firmware baseline for both halves of the Eyelash Sofle.

## Checkpoint

- Branch: `sofle-performance-v1`
- Tag: `sofle-performance-v1`
- Commit: `35c5147` (`Create matched split performance baseline`)
- Hardware: Eyelash Sofle, `eyelash_nano`, Nice!OLED displays

The branch and tag intentionally share a name at this checkpoint. When a Git command needs an unambiguous reference, use `refs/heads/sofle-performance-v1` for the branch or `refs/tags/sofle-performance-v1` for the tag.

## Build the left half

From the repository root:

```sh
west build \
  -s zmk/app \
  -b eyelash_nano \
  -d build/sofle_performance_v1_left \
  -- \
  -DBOARD_ROOT=$PWD \
  -DZMK_CONFIG=$PWD/config \
  -DSHIELD="eyelash_sofle_left nice_oled" \
  -DCONFIG_ZMK_STUDIO=y \
  -DCONFIG_ZMK_STUDIO_LOCKING=n \
  -DSNIPPET=studio-rpc-usb-uart
```

Output: `build/sofle_performance_v1_left/zephyr/zmk.uf2`

## Build the right half

From the repository root:

```sh
west build \
  -s zmk/app \
  -b eyelash_nano \
  -d build/sofle_performance_v1_right \
  -- \
  -DBOARD_ROOT=$PWD \
  -DZMK_CONFIG=$PWD/config \
  -DSHIELD="eyelash_sofle_right nice_oled"
```

Output: `build/sofle_performance_v1_right/zephyr/zmk.uf2`

The same left, right, and settings-reset variants are declared in `build.yaml` for automated builds.

## Verified baseline

### Input latency

- Key press debounce: 2 ms
- Key release debounce: 2 ms
- Keyscan event queue: 16

### Split BLE

- Preferred connection interval: 6 (7.5 ms)
- Preferred peripheral latency: 0
- Preferred supervision timeout: 400
- Left/central position queue: 16
- Left/central split-run queue: 16
- Right/peripheral position queue: 16
- Bluetooth transmit buffers (`CONFIG_BT_CONN_TX_MAX`): 64 on the left/central half

### Left OLED

- Clean, balanced status layout
- Bongo Cat, Luna, and responsive Bongo Cat disabled
- Bluetooth/output, profile, battery, modifier, Caps Lock/HID, and layer status retained

### Right OLED

- Peripheral animation enabled
- Gem animation enabled
- Cat animation disabled

## Known-good recovery artifacts

The locally verified 2 ms builds that produced this baseline are:

- Left: `build/debounce_2ms_test/zephyr/zmk.uf2`
- Right: `build/debounce_2ms_right_test/zephyr/zmk.uf2`

Build output is ignored by Git and is not a durable backup. For recovery on another machine, check out tag `refs/tags/sofle-performance-v1` and rebuild using the commands above. Keep separately named copies of the resulting UF2 files if binary recovery artifacts are required.

## Recovery procedure

1. Check out the tag with `git switch --detach refs/tags/sofle-performance-v1`.
2. Build the left and right firmware using the commands above.
3. Flash the left UF2 to the left half and the right UF2 to the right half.
4. If split pairing is stale, build and flash the `settings_reset` shield once, then reflash the correct firmware to each half and re-pair.

See `FIRMWARE_ITERATIONS.md` for the path from the original golden OLED firmware to this baseline.
