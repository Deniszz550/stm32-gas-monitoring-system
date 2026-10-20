# STM32 Nucleo - Real-Time Gas & Smoke Monitoring System

An embedded safety and environmental monitoring system built around the **STM32 Nucleo** development platform. The system continuously samples ambient air quality via an analog gas sensor (MQ series), displays real-time concentration levels and status indicators across two displays, and triggers audiovisual warnings upon detecting threshold breaches.

---

## Features

- **Continuous ADC Monitoring:** Real-time sampling of combustible gas/smoke concentrations via the internal ADC peripheral (10-bit / 12-bit).
- **Dual Display Interface:**
  - **16x2 I2C Character LCD:** Shows exact real-time ADC readings (`MONITORIZARE GAZ / NIVEL: XXX`).
  - **Secondary Status Display / Indicator:** Visual state reporting (`NORMAL` vs. `ALERTA!`).
- **Audiovisual Alert System:**
  - **Safe State:** Green status LED engaged.
  - **Alert State:** High-intensity Red alert LED triggered with acoustic buzzer alarm whenever concentration spikes above the safety baseline (e.g., test trigger up to 1023).
- **Automatic Recovery:** Returns to nominal mode once air clears and gas levels drop below threshold.

---

## Hardware Architecture

| Component | Interface / Protocol | Function |
| :--- | :--- | :--- |
| **STM32 Nucleo Board** | ARM Cortex-M | Main MCU, ADC conversion, state logic |
| **MQ Gas Sensor Module** | Analog Output (ADC In - PA0) | Detection of LPG, smoke, and flammable gases |
| **16x2 Character LCD** | I2C (PB8 - SCL, PB9 - SDA) | Numeric concentration readout (PCF8574) |
| **Secondary Status Display** | SPI / I2C | State indication (`NORMAL` / `ALERTA!`) |
| **Status LEDs (Red & Green)** | GPIO Out (PA5 / PA6) | Visual safety status indication |
| **Active Buzzer** | GPIO Out (PA7) | Acoustic alarm alert |
| **Breadboard & Passives** | - | Power distribution & current limiting |

---

## System Logic

```
   +--------------------+
   |   MQ Gas Sensor    |
   +---------+----------+
             | Analog Signal (ADC)
             v
   +--------------------+
   |    STM32 Nucleo    |
   +---------+----------+
             |
             +---> Level <= Threshold  --> [Green LED ON]  --> Status: NORMAL
             |
             +---> Level >  Threshold  --> [Red LED + Buzzer] --> Status: ALERTA!
```

---

## Project Structure

```text
├── Core/
│   ├── Inc/
│   │   ├── main.h
│   │   └── lcd_i2c.h
│   └── Src/
│       ├── main.c
│       └── lcd_i2c.c
├── .gitignore
└── README.md
```

---

## How to Build and Run

1. Clone or download this repository.
2. Open **STM32CubeIDE**.
3. Select `File` -> `Open Projects from File System...` and choose the project directory.
4. Connect the **STM32 Nucleo** board via USB (ST-LINK).
5. Build the project (`Ctrl + B`) and click **Run / Debug** (`F11`) to flash the firmware.
