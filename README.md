# ESP32 FreeRTOS Industrial Equipment Health Monitoring System

An ESP32-based IoT system for monitoring the operating condition of industrial equipment such as motors, pumps, compressors, fans, gearboxes and conveyor systems.

The project uses FreeRTOS to run sensor acquisition, display updating and alarm monitoring as independent tasks. Sensor values are displayed locally on an OLED and remotely through the Blynk IoT dashboard.

## Project Features

- Temperature and humidity monitoring using DHT22
- Three-axis acceleration measurement using MPU6050
- Machine vibration calculation
- Simulated current monitoring using an ESP32 ADC input
- OLED display for local monitoring
- Blynk IoT dashboard
- Live acceleration charts
- Overtemperature detection
- Overcurrent detection
- Excessive-vibration detection
- Sensor failure detection
- Green and red status LEDs
- Buzzer fault alarm
- FreeRTOS multitasking
- Mutex protection for shared sensor data
- Wokwi simulation support

## Project Architecture

The ESP32 runs three FreeRTOS tasks:

| Task | Function | Interval/Priority |
|---|---|---|
| Sensor Task | Reads DHT22, MPU6050 and analog input | Every 2 seconds |
| Display Task | Updates the OLED display | Every 500 ms |
| Alarm Task | Controls LEDs and buzzer | Every 250 ms |

The Alarm Task has the highest priority so that faults are reported quickly.

## Components Required

| Component | Quantity |
|---|---:|
| ESP32 DevKit V1 | 1 |
| DHT22 sensor | 1 |
| MPU6050 module | 1 |
| SSD1306 128×64 OLED | 1 |
| Potentiometer | 1 |
| Active or passive buzzer | 1 |
| Green LED | 1 |
| Red LED | 1 |
| 220Ω resistors | 2 |
| 10kΩ resistor | 1 |
| Jumper wires | As required |

## ESP32 Pin Connections

| Component | ESP32 Connection |
|---|---|
| DHT22 DATA | GPIO 15 |
| Potentiometer SIG | GPIO 34 |
| Green LED | GPIO 2 |
| Red LED | GPIO 5 |
| Buzzer | GPIO 18 |
| MPU6050 SDA | GPIO 21 |
| MPU6050 SCL | GPIO 22 |
| OLED SDA | GPIO 21 |
| OLED SCL | GPIO 22 |

## DHT22 Connections

| DHT22 Pin | ESP32 |
|---|---|
| VCC | 3.3V |
| DATA | GPIO 15 |
| NC | Not connected |
| GND | GND |

Connect a 10kΩ pull-up resistor between DHT22 DATA and 3.3V.

## MPU6050 Connections

| MPU6050 | ESP32 |
|---|---|
| VCC | 3.3V |
| GND | GND |
| SDA | GPIO 21 |
| SCL | GPIO 22 |

## OLED Connections

| OLED | ESP32 |
|---|---|
| VCC | 3.3V |
| GND | GND |
| SDA | GPIO 21 |
| SCL | GPIO 22 |

The MPU6050 and OLED share the same I2C bus because they use different I2C addresses.

## Potentiometer Connections

| Potentiometer | ESP32 |
|---|---|
| VCC | 3.3V |
| SIG | GPIO 34 |
| GND | GND |

The potentiometer simulates a current sensor with the following range:

- ADC: 0–4095
- Sensor voltage: 0–3.3V
- Simulated current: 0–30A

## Fault Thresholds

| Parameter | Fault Condition |
|---|---:|
| Temperature | Above 50°C |
| Vibration | Above 1.5g |
| Simulated current | Above 20A |
| Sensor failure | Immediate fault |

## Output Conditions

### Normal Condition

- Green LED ON
- Red LED OFF
- Buzzer OFF
- OLED status: NORMAL

### Fault Condition

- Green LED OFF
- Red LED ON
- Buzzer ON
- OLED status: FAULT

## Blynk Datastreams

| Virtual Pin | Datastream | Type | Range |
|---|---|---|---|
| V0 | Temperature | Double | -40 to 100°C |
| V1 | Humidity | Double | 0 to 100% |
| V2 | Vibration | Double | 0 to 10g |
| V3 | Current | Double | 0 to 30A |
| V4 | Sensor Voltage | Double | 0 to 3.3V |
| V5 | ADC Value | Integer | 0 to 4095 |
| V6 | Fault State | Integer | 0 to 1 |
| V7 | Equipment Status | String | — |
| V8 | Acceleration X | Double | -20 to 20 m/s² |
| V9 | Acceleration Y | Double | -20 to 20 m/s² |
| V10 | Acceleration Z | Double | -20 to 20 m/s² |

Enable history for V8, V9 and V10 to display acceleration values in the Blynk chart.

## Required Libraries

Add the following libraries to the Wokwi `libraries.txt` file:

```text
Blynk
DHT sensor library for ESPx
Adafruit MPU6050
Adafruit Unified Sensor
Adafruit GFX Library
Adafruit SSD1306
