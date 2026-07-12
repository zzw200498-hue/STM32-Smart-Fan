# STM32-Smart-Fan
# Smart Temperature-Controlled Fan System (STM32F103C8T6)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Platform](https://img.shields.io/badge/platform-STM32-blue)](https://www.st.com/)

A dual-mode (Auto/Manual) intelligent fan control system based on STM32F103C8T6. It automatically adjusts fan speed according to ambient temperature, supports manual override via rotary encoder, and features multi-level temperature alarms with LED indicators and buzzer alerts.

---

## ✨ Features

- **Dual-Mode Control**:
  - **Auto Mode** (Temp > 25°C): Fan speed linearly increases with temperature (`Speed = Temperature`).
  - **Standby Mode** (20°C ≤ Temp ≤ 25°C): Fan stops to save energy.
  - **Manual Mode** (Temp < 20°C): User controls speed via rotary encoder (step = 4).
- **Multi-Level Alarms**:
  - **> 65°C**: Red LED on + Buzzer sounds (Central C tone via TIM2 interrupt).
  - **55°C ~ 65°C**: All LEDs off (silent warning).
  - **< 55°C**: Green LED on (safe status).
- **PWM Noise Elimination**: Adjusted timer prescaler and auto-reload to set PWM frequency to **25 kHz**, eliminating audible whining noise.
- **Real-Time Display**: OLED screen shows current motor speed and temperature value.
- **Hardware Debouncing**: Encoder input with software filtering for stable manual control.

---

## 🛠️ Hardware Requirements

| Component | Model/Pin |
| :--- | :--- |
| MCU | STM32F103C8T6 (Blue Pill) |
| Temperature Sensor | NTC Thermistor / Analog Output (ADC) |
| DC Motor | 3V~6V DC Motor with driver (MOSFET/L298N) |
| Rotary Encoder | EC11 (with push button) |
| OLED Display | 0.96" I2C OLED (SSD1306) |
| Buzzer | Passive Buzzer (PB12) |
| LEDs | Red (PA8), Green (PA9) |

---

## 🔌 Pin Mapping (Default Configuration)

> **⚠️ 重要提示**：以下引脚分配为推荐配置，请根据您的实际硬件接线修改对应的头文件（`Motor.h`、`Encoder.h`、`AD.h`、`OLED.h`）中的宏定义。

| Peripheral        | Pin   | Function                          | Note                         |
| :---------------- | :---- | :-------------------------------- | :--------------------------- |
| **Motor PWM**     | PA2   | PWM output (TIM2_CH3)             | 使用 TIM2 的 CH3 输出       |
| **ADC (Temp)**    | PA4   | Analog voltage reading            | ADC1 的通道 4               |
| **Encoder CLK**   | PA0   | Encoder phase A (TIM2_CH1)        | 与 PWM 共用 TIM2，无冲突    |
| **Encoder DT**    | PA1   | Encoder phase B (TIM2_CH2)        | 与 PWM 共用 TIM2，无冲突    |
| **Buzzer**        | PB12  | Timer2 interrupt driven           | 无源蜂鸣器，定时器翻转      |
| **Red LED**       | PA8   | Alarm indicator (high temp)       | 高电平点亮                  |
| **Green LED**     | PA9   | Safe indicator (normal temp)      | 高电平点亮                  |
| **OLED SCL**      | PB6   | I2C Clock                         | 硬件 I2C 或软件模拟        |
| **OLED SDA**      | PB7   | I2C Data                          | 硬件 I2C 或软件模拟        |

>  **Note**: The pin mapping above is a **recommended default**. If your hardware uses different pins, please modify the corresponding macro definitions in:
> - `Motor.h` – for PWM output pin and timer channel
> - `Encoder.h` – for encoder CLK/DT pins
> - `AD.h` – for ADC channel pin
> - `OLED.h` – for I2C SCL/SDA pins (if using software I2C)
>
> Always double-check for pin conflicts (e.g., do not use the same pin for two different peripherals).

---

## 🚀 How to Use

1.Open the project with Keil MDK (or your preferred IDE).

2.Compile and flash the firmware to STM32F103C8T6.

3.Power on the system. The OLED will display initial status.

4.Observe behavior:

Heat the temperature sensor → fan speed increases automatically.

When temp > 65°C, buzzer and red LED activate.

When temp < 20°C, rotate the encoder to manually adjust speed.
