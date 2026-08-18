# Firmware and OLED Iteration History

This is the concise history of the experiments and checkpoints that led to `sofle-performance-v1`. Commit and tag references make each durable state reproducible even after the old experiment branches are removed.

## 1. Original Upstream Configuration

The repository began from the Eyelash Sofle configuration. The inherited keyscan debounce was 8 ms by the time the OLED work started, and the keyboard already used the `eyelash_nano` board and split left/right shields.

## 2. Golden OLED Baseline

Commits `c3b51f9` and `ce6f01c` documented successful Nice!OLED builds for both halves.

- Left: Nice!OLED plus ZMK Studio over USB/UART
- Right: Nice!OLED verified as a peripheral build
- Tags retained: `golden-oled-v1`, `golden-oled-complete-v1`, and `golden-oled-v2`
- Durable branch: `main` at `ce6f01c`

This established a known-working display baseline before latency or layout tuning.

## 3. Latency and OLED Experiments

Several local builds explored progressively lower input and split latency, OLED animation choices, and left-side layout changes. Their build directory names preserve the sequence, including `seller_left_latency*`, `seller_right_latency*`, `seller_left_ble_latency`, `seller_left_1ms`, and OLED-specific tests.

The useful settings were consolidated rather than retaining every experimental branch:

- Split BLE preferred interval set to 6 (7.5 ms)
- Split/peripheral latency set to 0
- Supervision timeout set to 400
- Central, peripheral, and split-run queues set to 16
- Central Bluetooth transmit buffers set to 64

Commit `ee0ef43` created the first stable OLED/latency configuration. Commit `59f02f5` then removed a temporary backup configuration file so experiments did not remain as repository clutter. Tag `sofle-oled-stable-v1` marks that checkpoint.

## 4. Stable Build Documentation

Commit `16786b1` added the first stable build and recovery notes for both halves. Those notes captured the paired BLE tuning, clean left OLED, and right-side gem animation.

## 5. Left OLED Layout Refinement

The left OLED went through no-cat, no-WPM-cat, clean-layout, spacing, battery-widget, and balanced-layout builds. The final direction removed decorative animations from the left display and retained practical status information:

- Bluetooth or USB output
- Active profile and profile dots
- Battery percentage
- Modifier indicators
- Caps Lock/HID indicator
- Active layer

Commit `2999a79`, tagged `sofle-oled-layout-v1`, records the refined clean and balanced layout. The right display kept the gem animation to preserve a visual element without adding animation work to both halves.

The `happy-cat-oled` branch remains at the golden baseline as a future isolated test branch for revisiting the cat animation.

## 6. Matched Performance Baseline (v1)

Commit `35c5147`, tagged `sofle-performance-v1-checkpoint`, combined the final OLED choices with matched performance settings on both halves:

- Press debounce reduced from 8 ms to 2 ms
- Release debounce reduced from 8 ms to 2 ms
- Left/central and right/peripheral BLE interval and latency settings aligned
- Left/central and right/peripheral position queues set to 16
- Left clean status layout retained
- Right gem animation retained

The locally verified final test artifacts are:

- Left: `build/debounce_2ms_test/zephyr/zmk.uf2`
- Right: `build/debounce_2ms_right_test/zephyr/zmk.uf2`

## 7. Performance v2: Split BLE Queue Increase

- **Date:** 2026-08-18
- **Branch:** `sofle-performance-v1`
- **Commit:** `64cc709`
- **Checkpoint:** `sofle-performance-v2-checkpoint`

### Change

Split BLE event buffering was increased from 16 to 32:

| Queue | Before | After |
| --- | ---: | ---: |
| Central position queue | 16 | 32 |
| Central split-run queue | 16 | 32 |
| Peripheral position queue | 16 | 32 |

### Files Changed

- `boards/shields/eyelash_sofle/eyelash_sofle_left.conf`
- `boards/shields/eyelash_sofle/eyelash_sofle_right.conf`

### Validation

- BLE timing unchanged
- Debounce unchanged at 2 ms
- OLED unchanged
- Both halves built successfully

### Observed Result

- Improved typing consistency
- Smoother fast typing bursts
- No stability regression observed

## 8. Performance v3: KSCAN Event Queue Increase

- **Date:** 2026-08-18
- **Branch:** `sofle-performance-v1`
- **Commit:** `bfeb42d`
- **Checkpoint:** `sofle-performance-v3-checkpoint`

### Change

The KSCAN event queue was increased from 16 to 32 and placed in the shared configuration so both halves use the same value:

```ini
CONFIG_ZMK_KSCAN_EVENT_QUEUE_SIZE=32
```

### Files Changed

- `boards/shields/eyelash_sofle/eyelash_sofle_left.conf`
- `config/eyelash_sofle.conf`

### Validation

- Both halves built successfully
- Both compiled configurations use a KSCAN event queue of 32
- BLE interval remained 6, latency remained 0, and supervision timeout remained 400
- Split BLE queues remained at 32
- Debounce remained at 2 ms for press and release

### Observed Result

- Improved typing consistency
- Better responsiveness during fast input bursts

## 9. Performance v4: Behavior Queue Improvement

- **Date:** 2026-08-18
- **Branch:** `sofle-performance-v1`
- **Commit:** `e1ed60f`
- **Checkpoint:** `sofle-performance-v4-checkpoint`

### Change

The behavior queue was increased from 64 to 128:

```ini
CONFIG_ZMK_BEHAVIORS_QUEUE_SIZE=128
```

### Files Changed

- `config/eyelash_sofle.conf`

### Combined Performance State

- Split BLE queues: 32
- KSCAN event queue: 32
- Behavior queue: 128
- Press and release debounce: 2 ms
- BLE preferred interval: 6 (7.5 ms)
- BLE latency: 0

### Validation

- Both halves built successfully
- BLE settings unchanged
- Split BLE queues unchanged
- KSCAN event queue unchanged
- Debounce unchanged
- No stability regression observed

### Observed Result

- Additional improvement in fast typing consistency
- Smoother high-speed input bursts

## Retained Branch Policy

Only these working branches are intended to remain:

- `main`: golden two-half OLED baseline
- `sofle-performance-v1`: current matched OLED/performance baseline
- `happy-cat-oled`: future cat-animation testing

The removed experiment branches were `custom-firmware-dev`, `latency-tuning-v1`, `stable-v4-baseline`, and `stable-oled-latency-v1`. Their meaningful checkpoints remain recoverable through the tags and commit IDs listed above.
