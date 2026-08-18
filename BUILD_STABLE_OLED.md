# Stable OLED Latency Firmware

Tag:
sofle-oled-stable-v1

Branch:
stable-oled-latency-v1

## Hardware

- Eyelash Sofle
- eyelash_nano
- nice_oled displays

## Left build

    west build \
      -s zmk/app \
      -b eyelash_nano \
      -d build/stable_left_v1 \
      -- \
      -DBOARD_ROOT=$PWD \
      -DZMK_CONFIG=$PWD/config \
      -DSHIELD="eyelash_sofle_left nice_oled"

## Right build

    west build \
      -s zmk/app \
      -b eyelash_nano \
      -d build/stable_right_v1 \
      -- \
      -DBOARD_ROOT=$PWD \
      -DZMK_CONFIG=$PWD/config \
      -DSHIELD="eyelash_sofle_right nice_oled"

## Verified configuration

### Split BLE

- Split interval: 6
- Split latency: 0
- Central queue: 16
- Peripheral queue: 16
- Split run queue: 16

### Left OLED

- Bongo Cat disabled
- Luna disabled
- Responsive Bongo Cat disabled

### Right OLED

- Gem animation enabled

## Recovery UF2

- eyelash_sofle_left_stable_oled_v1.uf2
- eyelash_sofle_right_stable_oled_v1.uf2
