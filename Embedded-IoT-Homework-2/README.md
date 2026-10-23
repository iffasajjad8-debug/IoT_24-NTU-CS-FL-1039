# Embedded IoT Homework 2: Dual-LED PWM Control with OLED Display

Course: Embedded IoT Systems
Name: Iffa Sajjad | Roll No: 24-NTU-CS-FL-1039

## Hardware Connections

| Component | ESP32 Pin |
|---|---|
| LED 1 (with resistor) | GPIO 18 |
| LED 2 (with resistor) | GPIO 19 |
| OLED SDA | GPIO 21 |
| OLED SCL | GPIO 22 |
| OLED VCC / GND | 3V3 / GND |

Every LED is connected through its own current-limiting resistor, and both LED cathodes are tied to GND.

## PWM Configuration

| LED | LEDC Channel | Frequency | Resolution |
|---|---|---|---|
| LED 1 | 0 | 1000 Hz | 10-bit (0 to 1023) |
| LED 2 | 2 | 5000 Hz | 10-bit (0 to 1023) |

**Note on LEDC timers:** The ESP32 pairs its LEDC channels onto shared timers: channels 0 and 1 use one timer, channels 2 and 3 use the next, and so on. Any two channels on the same timer are forced to use the same frequency and resolution. To let the two LEDs run at different frequencies, LED 2 is assigned to channel 2, which places it on a separate timer. (In Arduino-ESP32 core 3.x, the timer assignment is handled automatically.)

## How the Duty Cycle Works

Both LEDs sweep their duty value from 0 up to 1023 and then back down to 0. LED 1 moves in steps of 4 and LED 2 in steps of 6, with an update every 20 ms. The two LEDs fade in opposite directions. The duty value is converted to a percentage using `duty × 100 / 1023`.

The duty cycle tells us what portion of each period the pin stays HIGH. A larger duty means a higher average voltage, so the LED looks brighter. The frequency, on the other hand, is how many ON/OFF cycles happen each second. Both frequencies used in this project are high enough that the human eye cannot detect any flicker.

## OLED Display

The OLED shows the heading **PWM MONITOR**, followed by one line for each LED showing its present duty percentage and its set frequency. The display reads from the very same variables that drive the PWM output (`duty1`, `duty2`, `LED1_FREQ`, `LED2_FREQ`), which guarantees that the numbers on screen always match what the pins are actually doing.

## Testing and Results

### Task 1: Two LEDs without OLED
- **Wokwi simulation:** Both LEDs faded smoothly, each at its own frequency.
- **Hardware:** Both LEDs faded on the real board, and the difference in their PWM frequencies was observable.
- **Evidence:** `screenshots/task1_wokwi_circuit_1039.png`, `task1_wokwi_running_1039.png`, `task1_hardware_greenfade_1039.jpeg`, `task1_hardware_yellowfade_1039.jpeg`

### Task 2: With OLED
- **Wokwi simulation:** The duty percentage on the OLED updated continuously as the LEDs faded.
- **Hardware:** The OLED showed the duty percentages changing live along with the configured frequencies.
- **Evidence:** `screenshots/wokwi_circuit_1039.png`, `wokwi_running_oled_1039.png`, `hardware_circuit_output_oled_1039.jpg`

## Project Link
- Wokwi project: https://wokwi.com/projects/478676421217347585