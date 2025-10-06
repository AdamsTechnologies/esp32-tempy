# Tempy — ESP32 Tempy

**Tempy** is a friendly ESP32-powered temperature, humidity, and pressure sensor that adds a touch of personality to your environment.
It combines accurate environmental sensing with an expressive OLED “face” that blinks, smiles, and smoothly transitions between a face and live sensor data.

---

## Features

* **BME280 sensor** for temperature, humidity, and pressure.
* **OLED SSD1306 display (128×64)** with smooth animations and transitions.
* **Cute face expressions** that blink and change randomly — or manually from Home Assistant.
* **Automatic UI cycling** between face and sensor data screens.
* **Customizable timing** for face display, sensor display, and slide transitions.
* **Home Assistant integration** with secure API + OTA updates.
* **Efficient rendering** with a 400 kHz I²C bus and ~60 FPS draw rate.

---

## Hardware Requirements

| Component                 | Purpose                                | Notes                                                    |
| ------------------------- | -------------------------------------- | -------------------------------------------------------- |
| **ESP32 dev board**       | Main controller                        | e.g. ESP-WROOM-32 or compatible                          |
| **BME280 sensor**         | Temperature, humidity, pressure        | Set address to `0x76` or `0x77` depending on your module |
| **SSD1306 OLED (128x64)** | Display face + data                    | I²C, address `0x3C`                                      |

**I²C wiring (typical ESP32 dev board):**

| Pin | Device |
| --- | ------ |
| 21  | SDA    |
| 22  | SCL    |
| 3V3 | VCC    |
| GND | GND    |

---

## UI State Machine

| State       | Description                                         |
| ----------- | --------------------------------------------------- |
| `FACE`      | Shows Tempy’s animated face (blinks, mouth changes) |
| `SLIDE_F2S` | Transition from face to sensor screen               |
| `SENSOR`    | Displays temperature, humidity, and pressure        |
| `SLIDE_S2F` | Transition back to face view                        |

**Cycle timing** (customizable via globals):

* Face screen: 10 s
* Sensor screen: 5 s
* Slide transition: 800 ms

---

## Home Assistant Entities

After flashing, the device exposes:

* **Temperature**, **Humidity**, and **Pressure** sensors
* **Garage Temperature (F)** – derived Fahrenheit conversion
* **Face Mode** – `select` control to choose between expressions

---

## Security & Connectivity

* **API encryption** enabled (via `!secret api_encryption_key`)
* **Secure OTA updates** (via `!secret ota_password`)
* **Wi-Fi credentials** are stored securely using ESPHome secrets

---

## Deployment

1. **Connect** your ESP32 via USB.
2. **Save** this file as `esp32-tempy.yaml` in your ESPHome project folder.
3. **Add secrets** (`wifi_ssid`, `wifi_password`, `api_encryption_key`, `ota_password`).
4. **Run:**

   ```bash
   esphome run tempy.yaml
   ```
5. Once flashed, it will connect to your Wi-Fi and register with Home Assistant.

---

## Customization

* **Update display frequency:** adjust I²C `frequency` (e.g., `400kHz` → `800kHz`) if redraws appear choppy.
* **Change timings:** modify `face_show_ms`, `sensor_show_ms`, and `slide_ms` in the `globals:` section.
* **Adjust fonts:** replace `Roboto Mono` with any Google Font via `gfonts://`.
* **Modify scene text:** edit the `draw_sensor_scene` lambda for your preferred layout or labels.

---
