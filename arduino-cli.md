# arduino-cli cheat sheet

Commands for the Feather nRF52840 Sense, as an alternative to the Arduino IDE. Run them from the repo root.

* Board (FQBN): `adafruit:nrf52:feather52840sense`
* Port: `/dev/cu.usbmodemXXXX` (macOS), take the real name from `arduino-cli board list`

## Daily use
Find the board and its port:

    arduino-cli board list

Compile a sketch (pass the sketch folder, not the `.ino` file):

    arduino-cli compile --fqbn adafruit:nrf52:feather52840sense 05/Arduino/nRF52840Sense_Blink

Compile and upload:

    arduino-cli compile --fqbn adafruit:nrf52:feather52840sense --upload -p /dev/cu.usbmodemXXXX 05/Arduino/nRF52840Sense_Blink

Upload only (after a compile):

    arduino-cli upload --fqbn adafruit:nrf52:feather52840sense -p /dev/cu.usbmodemXXXX 05/Arduino/nRF52840Sense_Blink

Serial monitor (baud rate must match `Serial.begin()`, quit with Ctrl+C):

    arduino-cli monitor -p /dev/cu.usbmodemXXXX -c baudrate=115200

## Save board and port per sketch
Store FQBN and port in the sketch folder (creates `sketch.yaml`), then drop `--fqbn` and `-p`:

    arduino-cli board attach -b adafruit:nrf52:feather52840sense -p /dev/cu.usbmodemXXXX 05/Arduino/nRF52840Sense_Blink
    arduino-cli compile --upload 05/Arduino/nRF52840Sense_Blink

## Libraries
    arduino-cli lib search sht31
    arduino-cli lib install "Adafruit SHT31 Library" "Adafruit NeoPixel"
    arduino-cli lib list
    arduino-cli lib upgrade

## Boards and cores
    arduino-cli core list
    arduino-cli core update-index
    arduino-cli core upgrade
    arduino-cli board listall feather

## New sketch
    arduino-cli sketch new MySketch

## Troubleshooting
* No port in `board list`: use a USB data cable (not charge-only), plug in without a hub.
* Port gone after an upload: the sketch has no `#include "Adafruit_TinyUSB.h"`, so the board has no USB serial. Double-press the reset button to enter the bootloader (drive `FTHRSNSBOOT` appears), then upload again.
* The port name can differ between bootloader and running sketch, check `board list` before each upload.
* More output: add `-v` to `compile` or `upload`.

## One-time setup
    brew install arduino-cli
    arduino-cli config init
    arduino-cli config add board_manager.additional_urls https://adafruit.github.io/arduino-board-index/package_adafruit_index.json
    arduino-cli core update-index
    arduino-cli core install adafruit:nrf52
    arduino-cli lib install "Adafruit SHT31 Library" "Adafruit NeoPixel"
