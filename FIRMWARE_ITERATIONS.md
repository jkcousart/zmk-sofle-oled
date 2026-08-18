# Firmware and OLED Iteration History

This is the concise history of the experiments and checkpoints that led to `sofle-performance-v1`. Commit and tag references make each durable state reproducible even after the old experiment branches are removed.

## 1. Original upstream configuration

The repository began from the Eyelash Sofle configuration. The inherited keyscan debounce was 8 ms by the time the OLED work started, and the keyboard already used the `eyelash_nano` board and split left/right shields.

## 2. Golden OLED baseline

Commits `c3b51f9` and `ce6f01c` documented successful Nice!OLED builds for both halves.

- Left: Nice!OLED plus ZMK Studio over USB/UART
- Right: Nice!OLED verified as a peripheral build
- Tags retained: `golden-oled-v1`, `golden-oled-complete-v1`, and `golden-oled-v2`
- Durable branch: `main` at `ce6f01c`

This established a known-working display baseline before latency or layout tuning.

## 3. Latency and OLED experiments

Several local builds explored progressively lower input and split latency, OLED animation choices, and left-side layout changes. Their build directory names preserve the sequence, including `seller_left_latency*`, `seller_right_latency*`, `seller_left_ble_latency`, `seller_left_1ms`, and OLED-specific tests.

The useful settings were consolidated rather than retaining every experimental branch:

- Split BLE preferred interval set to 6 (7.5 ms)
- Split/peripheral latency set to 0
- Supervision timeout set to 400
- Central, peripheral, and split-run queues set to 16
- Central Bluetooth transmit buffers set to 64

Commit `ee0ef43` created the first stable OLED/latency configuration. Commit `59f02f5` then removed a temporary backup configuration file so experiments did not remain as repository clutter. Tag `sofle-oled-stable-v1` marks that checkpoint.

## 4. Stable build documentation

Commit `16786b1` added the first stable build and recovery notes for both halves. Those notes captured the paired BLE tuning, clean left OLED, and right-side gem animation.

## 5. Left OLED layout refinement

The left OLED went through no-cat, no-WPM-cat, clean-layout, spacing, battery-widget, and balanced-layout builds. The final direction removed decorative animations from the left display and retained practical status information:

- Bluetooth or USB output
- Active profile and profile dots
- Battery percentage
- Modifier indicators
- Caps Lock/HID indicator
- Active layer

Commit `2999a79`, tagged `sofle-oled-layout-v1`, records the refined clean and balanced layout. The right display kept the gem animation to preserve a visual element without adding animation work to both halves.

The `happy-cat-oled` branch remains at the golden baseline as a future isolated test branch for revisiting the cat animation.

## 6. Matched performance baseline

Commit `35c5147`, on branch and tag `sofle-performance-v1`, combined the final OLED choices with matched performance settings on both halves:

- Press debounce reduced from 8 ms to 2 ms
- Release debounce reduced from 8 ms to 2 ms
- Left/central and right/peripheral BLE interval and latency settings aligned
- Left/central and right/peripheral position queues set to 16
- Left clean status layout retained
- Right gem animation retained

The locally verified final test artifacts are `build/debounce_2ms_test/zephyr/zmk.uf2` for the left half and `build/debounce_2ms_right_test/zephyr/zmk.uf2` for the right half.

## Retained branch policy

Only these working branches are intended to remain:

- `main`: golden two-half OLED baseline
- `sofle-performance-v1`: current matched OLED/performance baseline
- `happy-cat-oled`: future cat-animation testing

The removed experiment branches were `custom-firmware-dev`, `latency-tuning-v1`, `stable-v4-baseline`, and `stable-oled-latency-v1`. Their meaningful checkpoints remain recoverable through the tags and commit IDs listed above.

## Performance v2 - Split BLE Queue Increase

Date:
2026-08-18

Branch:
sofle-performance-v1

Change:

Increased split BLE event buffering:

Before:
- Central position queue: 16
- Central split run queue: 16
- Peripheral position queue: 16

After:
- Central position queue: 32
- Central split run queue: 32
- Peripheral position queue: 32

Files changed:
- boards/shields/eyelash_sofle/eyelash_sofle_left.conf
- boards/shields/eyelash_sofle/eyelash_sofle_right.conf

Validation:
- BLE timing unchanged
- Debounce unchanged at 2ms
- OLED unchanged
- Both halves built successfully

Observed result:
- Improved typing consistency
- Smoother fast typing bursts
- No stability regression observed

Checkpoint:
sofle-performance-v2-checkpoint
