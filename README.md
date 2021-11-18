# IoT Central & Arduino PRO: Factory 5.0: ML-Powered Manufacturing Workshop

Thank you for registering for [Edge Impulse's Factory 5.0: ML-Powered Manufacturing Workshop with IoT Central & Arduino Pro](https://register.gotowebinar.com/register/4284862102840899596?source=IoTCMC)!

First, please read the *[Workshop Pre-requisites](https://edgeimpulse.notion.site/IoT-Central-Arduino-PRO-Factory-5-0-ML-Powered-Manufacturing-Workshop-Participant-Pre-requisites-637bd86b3e654470a6ad2473193bdd2a)*.

Glass Breaking Public Edge Impulse Project: https://studio.edgeimpulse.com/public/55141/latest

Food Quality Assurance Public Edge Impulse Project: https://studio.edgeimpulse.com/public/55159/latest

## Requirements

### Hardware

* [Arduino Portenta H7 / H7 Lite](https://store.arduino.cc/portenta-h7) development board.
* [Arduino Portenta Ethernet Vision Shield](https://store.arduino.cc/products/arduino-portenta-vision-shield-ethernet).

### Tools

The arduino-cli tool is used to build and upload the Edge Impulse firmware to the Arduino Portenta H7 board. View installation steps at the link below:

* [Arduino CLI](https://arduino.github.io/arduino-cli/installation/).

The Edge Impulse firmware depends on some libraries and the Mbed core for Arduino. These will be automatically installed if you don't have them yet during the build script.

## Building the application

> **Note**: If you replaced the `edge-impulse-sdk` with a new SDK from an exported `Arduino library` from the [Edge Impulse Studio](https://studio.edgeimpulse.com) then you *must* run the `src/edge-impulse-sdk/create-arduino-library.sh` script prior to building the application.

1. Build the application:

    ```
    ./arduino-build.sh --build
    ```

1. Flash the application:

    ```
    ./arduino-build.sh --flash
    ```

## Using your own exported `Arduino library` from Edge Impulse Studio

Extract the contents of the exported `Arduino library` and replace `edge-impulse-sdk`, `tflite-model` and `model-parameters` in the `src` directory. Then run the `create-arduino-library.sh` script prior to building the application.

```
cd src/edge-impulse-sdk/
sh create-arduino-library.sh
```

## Flashing the pre-compiled firmware

Enter the directory for either the Food Quality Assurance (`pre-compiled/food-quality`) or Glass Breaking (`pre-compiled/glass-breaking`) use case, then use the arduino-cli to flash the firmware to the Portena H7, for example:

```
cd pre-compiled/glass-breaking/
arduino-cli upload -p $(arduino-cli board list | grep Arduino | cut -d ' ' -f1) --fqbn arduino:mbed_portenta:envie_m7:split=100_0 --input-dir .
```

Then, start inferencing on the edge with: `edge-impulse-run-impulse` (or `edge-impulse-run-impulse --continuous` for the glass breaking use case)

## Known Issues

- Image corruption after audio capture when frame buffer is placed in SDRAM (`EI_CAMERA_FRAME_BUFFER_SDRAM`).

A simple work-around is to avoid image capture after audio capture, otherwise reset the board.

## Troubleshooting

* Not flashing? You can double tap the button on the board to put it in bootloader mode.
* Invalid DFU suffix signature?

    ```
    dfu-util: Invalid DFU suffix signature
    dfu-util: A valid DFU suffix will be required in a future dfu-util release!!!
    dfu-util: Cannot open DFU device 2341:035b
    dfu-util: No DFU capable USB device available
    Upload error: Error: 2 UNKNOWN: uploading error: uploading error: exit status 74
    ```

    Having the above issues? Then copy `20-arduino.rules` to `/etc/udev/rules.d/` and try again.
