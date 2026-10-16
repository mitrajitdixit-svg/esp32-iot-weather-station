[README (3).md](https://github.com/user-attachments/files/33567658/README.3.md)
# ESP32 IoT Weather Station

An ESP32-based environment monitor that reads temperature and humidity from a DHT22 sensor, shows the values on an OLED display, and sounds a buzzer when the temperature goes above a set threshold. Simulated in Wokwi. WiFi/MQTT publishing is planned as the next stage.

![Circuit screenshot](docs/circuit.png)
![Demo](docs/demo.gif)

## Simulation

Run it in your browser, no hardware needed:
**[Open in Wokwi](PASTE_YOUR_WOKWI_SHARE_LINK_HERE)**

Click the DHT22 in the diagram and change the temperature to test the alert.

## Features

- Reads temperature and humidity every 2 seconds
- Live readings on a 128x64 SSD1306 OLED
- Buzzer and on-screen warning when temperature is 35 °C or higher
- Serial output for debugging
- "Sensor error" message if the DHT22 returns invalid data
- Non-blocking timing with `millis()` (no `delay()` in the main loop)

## Components

| Qty | Part |
|---|---|
| 1 | ESP32 DevKit V1 |
| 1 | DHT22 temperature and humidity sensor |
| 1 | SSD1306 0.96" I2C OLED (128x64) |
| 1 | Buzzer |

## Wiring

| Component | Component Pin | ESP32 Pin |
|---|---|---|
| DHT22 | VCC (pin 1) | 3V3 |
| DHT22 | Data (pin 2) | GPIO 15 |
| DHT22 | NC (pin 3) | not connected |
| DHT22 | GND (pin 4) | GND |
| OLED | VCC | 3V3 |
| OLED | GND | GND |
| OLED | SCL | GPIO 22 |
| OLED | SDA | GPIO 21 |
| Buzzer | Pin 1 (+) | GPIO 2 |
| Buzzer | Pin 2 (-) | GND |

Notes:
- The OLED uses the ESP32's default I2C pins (SDA = 21, SCL = 22).
- On a real build with a bare 4-pin DHT22, add a 10 kΩ pull-up resistor between the data pin and 3V3. Many 3-pin DHT22 modules already include it.
- GPIO 2 and 15 are boot strapping pins. If uploading fails on real hardware, unplug those wires while flashing, or move them to other free pins and change `DHT_PIN` / `BUZZER_PIN` in the code.

## Libraries

- Adafruit GFX Library
- Adafruit SSD1306
- DHT sensor library

## How to run

**In Wokwi:** open the simulation link above and press the green play button.

**On real hardware:**
1. Wire the parts as in the table above.
2. Install the libraries listed above in the Arduino IDE.
3. Select the **ESP32 Dev Module** board and the correct port.
4. Open `src/sketch.ino` and upload.
5. If the OLED stays blank, change `OLED_ADDR` from `0x3C` to `0x3D`.

## Configuration

Set at the top of `sketch.ino`:

| Setting | Default | Meaning |
|---|---|---|
| `TEMP_ALERT_C` | 35.0 | Alert threshold in °C |
| `READ_INTERVAL_MS` | 2000 | Time between readings |
| `BUZZER_FREQ_HZ` | 2000 | Buzzer tone frequency |

## Project structure

```
/src
  sketch.ino
/docs
  circuit.png
  demo.gif
README.md
```

## Roadmap

- [x] Stage 1: DHT22 readings over Serial
- [x] Stage 2: OLED display and buzzer temperature alert
- [ ] Stage 3: WiFi and MQTT publishing
- [ ] Stage 4: Dashboard, moving-average smoothing, WiFi reconnect handling
- [ ] Optional: heat index calculation, real outdoor weather comparison via API

## Background

This is my second hardware project. The first was an obstacle alert system built in Tinkercad. This one moves to the ESP32 and Wokwi, adding a display, sensor error handling, and (next) networking.

## License

MIT
