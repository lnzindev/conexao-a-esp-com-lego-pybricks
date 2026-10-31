# ESP32 × LEGO Pybricks

**Experiments with Bluetooth Low Energy communication between an ESP32 and LEGO SPIKE hubs running Pybricks.**

This repository collects small hardware and BLE experiments. The main idea is to let a LEGO hub publish a state or sensor event and have an ESP32 scan for the corresponding BLE manufacturer data and react to it.

## Communication concept

    LEGO SPIKE Hub (Pybricks)
      ├─ reads a sensor
      └─ broadcasts a BLE message
                 |
                 v
           Bluetooth LE
                 |
                 v
    ESP32 DevKit
      ├─ scans advertisements
      ├─ inspects manufacturer data
      └─ reacts to recognized ON / OFF patterns

## Experiments in this repository

### 1. Pybricks broadcaster

The Pybricks examples use the LEGO hub's BLE broadcast channel and can publish state changes from a force sensor.

- PrimeHub example: force sensor on port B, BLE broadcast channel 1.
- EssentialHub example: force sensor on port B, BLE broadcast channel 1.
- A separate proximity experiment uses an ultrasonic sensor on port B and a motor on port A to trigger an event when an object is detected nearby.

The examples are prototypes; check the selected file before running it on a hub, because each program has different hardware assumptions.

### 2. ESP32 BLE receiver

The ESP32 sketch uses the ESP32 BLE Arduino APIs to scan advertisements and inspect manufacturer data. The current receiver recognizes the captured ON and OFF patterns and toggles the LED connected to GPIO 13.

### 3. BLE discovery scanner

A separate scanner prints nearby device names, addresses, RSSI values, and manufacturer data in hexadecimal. It is useful for observing the actual advertisements before writing a receiver filter.

## Hardware and software

- ESP32 DevKit board supported by the Arduino ESP32 core.
- Compatible LEGO SPIKE Prime or SPIKE Essential hub.
- Pybricks firmware and the Pybricks messaging APIs.
- Force sensor or ultrasonic sensor, depending on the selected example.
- Arduino IDE for the ESP32 sketches.

## How to experiment

1. Install the ESP32 board support package in Arduino IDE.
2. Open the ESP32 receiver sketch and select the correct board and serial port.
3. Flash the selected Pybricks program to the LEGO hub.
4. Open Serial Monitor at 115200 baud.
5. Trigger the sensor and observe whether the expected BLE state is received.

For discovery work, run the BLE scanner first and inspect the raw manufacturer-data bytes. BLE advertisement contents can vary with firmware, channel, and message type, so do not assume every Pybricks message matches the current filter.

## Important notes

- This is an experimental collection, not a finished communication library.
- The receiver currently matches specific packet patterns observed during development; it is not a universal Pybricks BLE decoder.
- GPIO 13 is the LED output in the current ESP32 example. Change it to match your board and wiring.
- Sensor ports, hub type, and motor connections must match the selected Pybricks script.
- Test with motors unloaded and keep the robot's movement safe while debugging.
- No automatic hardware test suite is included; validate on the actual hub and ESP32.

## Next improvements

- Rename and separate the current code fragments into clearly named Python and Arduino files.
- Define a documented message format with explicit message types and versioning.
- Add connection-loss handling and a safe default state.
- Add optional outputs for servo and LED-strip control.
- Record tested hub firmware, ESP32 board package, and hardware combinations.
