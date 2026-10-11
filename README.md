# ⚖️ STM32 Buck Converter

A step-down (Buck) DC-DC converter controlled by an **STM32F103C8T6 (Blue Pill)**. Developed as a **pair engineering project**, this system models, simulates, and implements a closed-loop controller that steps a **12V DC supply** down through an automated sequence (**10V → 9V → 6V → 3V**) using a 35 kHz interrupt-driven PI + Voltage Feedforward control loop.

---

## 🛠️ Tech & Tools Used

- **Programming Language**: C
- **Simulation & Control Tuning**: MATLAB / Simulink (modeling, closed-loop step-response verification, and PI gain tuning)
- **MCU Configuration & Code Generation**: STM32CubeMX
- **Flashing & Debugging**: STM32CubeProgrammer (via ST-Link V2)
- **Microcontroller**: STM32F103C8T6 (Blue Pill)

---

## ✨ Key Features

- **MATLAB-to-Hardware Workflow**: PI controller gains ($K_p = 0.01$, $K_i = 0.003$) were first designed and verified in MATLAB/Simulink before deploying to the physical buck converter stage.
- **35 kHz Real-Time Control Loop**
- **PI + Voltage Feedforward ($D_{ff}$)**: Combines theoretical duty-cycle feedforward ($V_{\text{target}} / V_{\text{in}}$) with closed-loop PI feedback.
- **Integrator Anti-Windup**: Dynamic duty-cycle clamping (**5% to 92%**) prevents integrator windup and ensures high-side bootstrap capacitor recharge time.
- **Finite State Machine (FSM)**: Automatically cycles through target output voltages (**10V → 9V → 6V → 3V**) every 3 seconds while maintaining a system heartbeat LED.

---

## 🔌 Hardware Pinout

| STM32 Pin | Peripheral | Function | Description |
| :--- | :--- | :--- | :--- |
| **PA8** | `TIM1_CH1` | PWM Output | 35 kHz PWM signal to Gate Driver (`HIN`) |
| **PA1** | `ADC1_IN1` | Analog Input | Output Voltage Feedback (4:1 Resistor Divider, 13.2V Max) |
| **PC13** | `GPIO_Output` | Status LED | Onboard Heartbeat LED |

---

## 🚀 How to Build & Flash

1. **Configure / Modify Peripherals in STM32CubeMX**
2. **Compile**: Build the project in VS Code (with Makefile) to generate the `.hex` / `.elf` binary.
3. **Flash**: Connect an **ST-Link V2** to the Blue Pill SWD pins (`SWDIO`, `SWCLK`, `GND`, `3.3V`) and flash the binary using **STM32CubeProgrammer**.

---

> 📌 **Note**:
> - **Source Code (`main.c`)**: This repository provides the core control code in [`main.c`](./main.c) only. To build and run it in your own environment, generate a base STM32F103C8 project in **STM32CubeMX / STM32CubeIDE** with your own project files and replace the generated `main.c` with this file.
> - **More Pictures & Results**: Please check the [`images/`](./images/) folder for additional circuit photos, MATLAB simulations, and oscilloscope waveforms.
---
