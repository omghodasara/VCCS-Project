# README

## Project Overview

I designed and simulated a Voltage-Controlled Current Source (VCCS) using a Sallen-Key low-pass filter followed by a Howland current pump stage. The primary goal was to convert a pulse-width modulated (PWM) input voltage into a precise and constant output current across a range of load resistances, suitable for biomedical applications.

***

## Key Features

- The circuit produces an output current independent of load resistance within the 1 kΩ to 10 kΩ range, confirming stable current source behavior.
- The circuit output current varies with PWM frequency due to the fixed “on-time” setting in the PWM input, causing the average voltage fed into the current source to fluctuate. This frequency-dependency is explained by the pulse definition: the pulse’s rise, on, and fall times remain constant, while the period changes with frequency, leading to varying duty cycles and average voltages.

***

## Usage Instructions

- The simulation can be run using the included LTspice schematic file (`.asc`).
- Transient analysis (`.tran`) is used to observe the filtered output voltage and resulting current through the load.
- Users may test different PWM frequencies, duty cycles, and load resistances by modifying the PWM source parameters and load resistor values within the schematic.

***

## Important Notes on Design

- The 600 Ω resistor in the Howland current pump stage is chosen considering a nominal 60% duty cycle PWM input. If the duty cycle changes, the resistor value should be adjusted proportionally to maintain the target output current range (0–5 mA).
- The frequency-dependent output current behavior arises due to the PWM signal configuration, not the circuit design itself.

***



