# Golden OLED Firmware State

Tag:
golden-oled-v1

Known working hardware:
- Eyelash Sofle left half
- Nice OLED display

Working build:

west build \
  -s zmk/app \
  -b eyelash_nano \
  -d build/seller_left \
  -- \
  -DBOARD_ROOT=$PWD \
  -DZMK_CONFIG=$PWD/config \
  -DSHIELD="eyelash_sofle_left nice_oled" \
  -DCONFIG_ZMK_STUDIO=y \
  -DCONFIG_ZMK_STUDIO_LOCKING=n \
  -DSNIPPET=studio-rpc-usb-uart

Important components:
- eyelash_nano board
- eyelash_sofle_left shield
- nice_oled shield
- CONFIG_ZMK_DISPLAY=y
- SSD1306 OLED defined in eyelash_sofle.dtsi
- zmk-nice-oled west module

OLED path:

eyelash_sofle.dtsi
 -> SSD1306 I2C device
 -> ZMK display subsystem
 -> nice_oled widgets
 -> OLED output

## Right side verified

Working build:

west build \
  -s zmk/app \
  -b eyelash_nano \
  -d build/seller_right \
  -- \
  -DBOARD_ROOT=$PWD \
  -DZMK_CONFIG=$PWD/config \
  -DSHIELD="eyelash_sofle_right nice_oled"

Result:
- Build successful
- OLED shield enabled
- UF2 generated
