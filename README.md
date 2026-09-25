# Arty A7 FPGA Flight Controller

This repository contains the hardware and software design files for an FPGA-based flight controller built on the Digilent Arty A7-100T. The system utilizes a MicroBlaze soft processor, an AXI UART16550 interface for high-speed ExpressLRS (CRSF) communication, and custom hardware-accelerated PWM generation for motor control.

## Team Roles

| Role | Member | Primary Responsibilities |
| :--- | :--- | :--- |
| **System Architect** | KSI | Vivado block design, clock/reset networks, pin mapping, bitstream generation, and hardware platform export (`.xsa`). |
| **Software Lead** | AMD | Vitis C firmware, AXI UART driver configuration, ExpressLRS CRSF packet parsing, and PID control loop implementation. |
| **Custom IP Developer** | ISHA | Verilog design for hardware-accelerated PWM generation, AXI4 packaging, and IP integration. |
| **QA & Integration** | JOEL | System-level testing, oscilloscope verification of PWM signals, and hardware-in-the-loop debugging. |

## Development Workflow & Milestones

| Phase | Title | Status | Owner | Key Tasks & Deliverables |
| :--- | :--- | :--- | :--- | :--- |
| **1** | **Hardware Foundation** | ✅ Completed | KSI | Instantiated MicroBlaze and AXI UART16550 (420,000 baud). Configured clock/reset networks. Routed physical `uart_tx`/`uart_rx` pins. Generated final `.xsa` handoff file. |
| **2** | **Software Communication** | 🔄 Active | AMD | Import `.xsa` to Vitis. Initialize UART driver. Write C parser for CRSF protocol to extract Roll, Pitch, Yaw, and Throttle data. Output verification to serial terminal. |
| **3** | **Custom Motor Control IP** | 🔄 Active | ISHA | Write Verilog multi-channel PWM logic. Package as an AXI4-Lite peripheral. Integrate into Vivado block design and map remaining PMOD pins. |
| **4** | **Flight Logic & Integration** | ⏳ Upcoming | Team Work | Link parsed CRSF variables to physical PWM IP memory addresses. Implement PID stabilization loops. Conduct hardware-in-the-loop testing and oscilloscope verification. |

## Hardware Pin Mapping Reference

The physical connections map directly to the Arty A7 constraints file (`arty_pins.xdc`). All I/O standards are set to `LVCMOS33`.

| Signal Name | Port / Pin Name | Board Location | Purpose |
| :--- | :--- | :--- | :--- |
| `uart_rx` | `G13` | PMOD JA (Top Row) | Listens to ELRS Receiver TX |
| `uart_tx` | `D13` | PMOD JA (Top Row) | Speaks to ELRS Receiver RX |
| `clk_in1_0` | `E3` | Main Chip Clock | 100MHz System Clock |
| `reset` | `C2` | Physical Reset Button | Hardware System Reset |
| `motor_pwm_*` | *TBD* | PMOD JB/JC | ESC Motor Control Signals |

---
**Current Action Items:** 
* **AMD:** Pull the latest `.xsa` hardware platform and begin Vitis BSP setup. 
* **ISHA:** Finalize Verilog PWM module for the upcoming AXI packaging milestone.
