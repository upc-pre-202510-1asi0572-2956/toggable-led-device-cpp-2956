# User Stories for ToggableLedDevice Example (C++ Edition)

This document contains user stories for the `ToggableLedDevice-Example-Cpp` project, which demonstrates the `ModestIoT` (C++ Edition) Arduino library. 
The User Stories are defined for two personas: the Device User (interacting with the device) and the Device Maker (integrating the framework into the sketch). 
Each story includes acceptance criteria, using the **Given-When-Then** format where applicable.

## Device-User Stories

### US01: Toggle LED with Button
- **As a** Device User, **I want** to press a button to toggle a LED between on and off states, **so that** I can physically control a simple IoT device.
- **Acceptance Criteria**:
  - **Given** the device is powered on and the LED is initially off, **when** I press the button once, **then** the LED turns on.
  - **Given** the device is powered on and the LED is on, **when** I press the button once, **then** the LED turns off.
  - **Given** the device is powered on, **when** I press and hold the button, **then** the LED toggles only once per press (debouncing ensures no rapid toggling).

## Device-Maker Stories

### US02: Monitor LED State
- **As a** Device Maker, **I want** to see the LED's current state printed to the Serial Monitor, **so that** I can verify the device operates correctly during development and testing.
- **Acceptance Criteria**:
  - **Given** the device is powered on and connected to the Serial Monitor at 115200 baud, **when** the device starts, **then** "LED initial state: 0" is printed.
  - **Given** the LED is off and the Serial Monitor is open, **when** the button is pressed to turn the LED on, **then** "LED state: 1" is printed.
  - **Given** the LED is on and the Serial Monitor is open, **when** the button is pressed to turn the LED off, **then** "LED state: 0" is printed.

### US03: Integrate Framework for Button and LED
- **As a** Device Maker, **I want** to integrate the `ModestIoT` framework to handle button presses and LED control, **so that** I can build the device sketch with minimal low-level coding.
- **Acceptance Criteria**:
  - **Given** I include `<ModestIoT.h>` in the sketch, **when** I instantiate `ToggableLedDevice` with GPIO 26 (button) and GPIO 27 (LED), **then** the button triggers `BUTTON_PRESSED_EVENT` and the LED responds to `TOGGLE_LED_COMMAND` without additional hardware setup code.
  - **Given** I extend `Device` with `ToggableLedDevice`, **when** a button press occurs, **then** the LED toggles and the state is logged to Serial using framework components.
  - The sketch leverages `Button` and `Led` from the framework without reimplementing sensor or actuator logic.

### US04: Use Interrupt-Driven Detection
- **As a** Device Maker, **I want** to use an interrupt-driven approach to detect button presses, **so that** the device responds quickly and efficiently without polling in the main loop.
- **Acceptance Criteria**:
  - **Given** the sketch attaches an interrupt to GPIO 26, **when** I press the button, **then** the LED toggles within a 200ms debounce period, without `loop()` polling the button state.
  - **Given** no button press occurs, **when** I inspect the `loop()` function, **then** it remains empty, relying on interrupts for responsiveness.
  - The interrupt service routine (ISR) calls `triggerButtonEvent` to integrate with the framework.

---

*Created: March 23, 2025.*
*Last Updated: March 26, 2025*