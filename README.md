### Lab 8 — BLE Automation Input/Output Service
## Authors
- Daryna Shevchuk (part 1 and part 4)
- Oleksandr Lykhanskyi (part 2 and part 3)

### Hardware Requirements

- PSoC Creator 4.4

- Development kit CY8CKIT-042 BLE (PSoC 4 BLE)

- USB-UART connection

- On-board components:

 -- CapSense linear slider

 -- LEDs D1–D4

 -- Pushbuttons SW2 and Shield Buttons

 -- 7-segment display

-- PWM

-- SPI Master

## Main features of the system:

### 1. BLE Stack Initialization

BLE component is started and the AppCallBack() function is registered.

The callback processes key BLE events:

advertising start

device connected/disconnected

connection parameter updates

pairing and bonding

SMP events (Passkey Entry, Numeric Comparison)

AIOS characteristic read/write operations

### 2. CapSense Slider

Reads the position of the CapSense linear slider.

Sends slider value over BLE as AIOS Analog Characteristic (0x0036).

Displays the measured value on the 7-segment display.

Sends debug output via UART:

```bash
capsense_pos = XX
```

### 3. 7-Segment Display

Updated using SPI Master with the help of a WDT timer interrupt (100 Hz).

Display codes stored in the lookup table `KOD7[]`.

Function `out7seg()` generates the display buffer `indyk[]`.

### 4. PWM Control

Device receives an analog value via BLE and updates a PWM output.

The value is obtained using:

```bash
CyBle_AiossGetCharacteristicValue(...)
```

PWM may be used to control LED brightness or another analog output.

### 5. Digital Output (LED Control)

Four on-board LEDs (D1–D4) are controlled remotely using BLE Digital Characteristics.

LED states are updated in:

```bash
OutputLedHandler()
```

### 6. Button Handling

Physical pushbuttons generate an interrupt.

Their state is read and can be sent to the BLE client as a digital input value.

### 7. UART Debug Output

UART prints:

BLE state changes

CapSense slider values

AIOS read/write events

PWM updates

Button interrupts
This helps with debugging and verification during testing.

### Workflow Summary

The system operates as:

```bash
BLE Event → Process IO → Update Display/PWM/LED → Optional UART Log
```

and continuously updates:

sensor inputs (CapSense, buttons)

digital outputs (LEDs)

analog outputs (PWM)

BLE communication

### Results

BLE Automation IO Service correctly implemented.

CapSense slider position successfully transmitted via BLE.

7-segment display continuously updated through SPI + WDT.

LED control from BLE confirmed.

PWM value successfully received and applied.

Button interrupts correctly handled.

UART output provides a complete trace of system operation.
