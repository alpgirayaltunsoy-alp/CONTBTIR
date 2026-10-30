CONTBTIR
A Universal PC Remote Control Board

<p align="center"> <img src="3d1.png" width="45%" alt="CONTBTIR PCB 3D View"> <img src="3d2.png" width="45%" alt="CONTBTIR PCB 3D View - Alternate Angle"> </p>

<p align="center"> <b>One Board. One USB Connection. Complete PC Control.</b> </p>

📌 Overview

CONTBTIR is a compact, multifunctional hardware project designed to control a personal computer remotely using a single USB-connected board.

The main goal of this project is to provide a centralized solution for controlling a PC through different wireless and infrared interfaces without requiring multiple separate adapters or controllers.

The board combines two microcontrollers:

CH552G – Handles USB communication with the computer.
ESP32-C3 – Provides wireless connectivity and additional control capabilities.

The intended control methods include:

Infrared (IR) remote control.
Bluetooth wireless control.
USB communication with the host computer.

The ultimate goal is to allow users to control their PC remotely using a conventional IR remote or a Bluetooth-compatible device.

✨ Features
Single-board PC remote control solution.
USB connectivity.
CH552G-based USB interface.
ESP32-C3-based wireless functionality.
IR remote control support.
Bluetooth Low Energy (BLE) capability through ESP32-C3.
Compact PCB design.
Dedicated programming access points.
Easy firmware uploading without directly touching small IC pins.
🧠 Hardware Architecture

The project uses two main microcontrollers, each responsible for different tasks.

1. CH552G – USB Controller

The CH552G is responsible for USB communication with the computer.

Its primary responsibilities include:

USB communication.
Interfacing with the PC.
Handling the USB-side functionality of the board.
Receiving and forwarding control commands.

The CH552G also has a dedicated USB bootloader programming interface.

2. ESP32-C3 – Wireless Controller

The ESP32-C3 provides the wireless capabilities of the board.

Its intended responsibilities include:

Bluetooth Low Energy communication.
Wireless command reception.
Communication with the CH552G.
Handling wireless control functionality.

The ESP32-C3 can be programmed independently of the CH552G.

🔧 PCB Design

The repository contains the following hardware design files:

File	Description
3d1.png	PCB 3D visualization
3d2.png	PCB 3D visualization from another angle
schem.png	Electrical schematic
BOM_Board1_PCB1_2026-10-30.xlsx	Bill of Materials
PickAndPlace_PCB1_2026-10-30.xlsx	Component placement coordinates
Gerber_PCB1_2026-10-30.zip	PCB manufacturing files

These files can be used to inspect, manufacture and assemble the PCB.

⚡ Firmware Programming Guide

One of the most important design considerations of CONTBTIR is making firmware programming as easy as possible.

Both microcontrollers have accessible programming points, so users do not need to touch their tiny IC pins directly.

IMPORTANT: A pair of fine-tipped tweezers is recommended for activating the programming modes.

There are two separate programming procedures.

1. CH552G Firmware Programming
Programming Mode Activation

The CH552G uses its USB bootloader to receive firmware.

On this PCB, the programming activation point is conveniently accessible through capacitor C1.

Instead of trying to reach the small CH552G pins, you can use the two pads of C1.

📍 Where Should You Touch?

Locate capacitor C1, positioned on the right-hand side of the board.

You will see two small solder pads.

These are the programming activation points.

The procedure:

Locate capacitor C1 on the PCB.
Prepare a pair of conductive tweezers.
Place the tweezer tips on the two opposite terminals of C1 simultaneously.
This temporarily connects the two pads.
While maintaining this connection, connect the board to your computer using USB.
Release the tweezers once the board has entered its USB bootloader mode.
Your computer should now be able to detect the CH552G bootloader.
Upload the firmware using a compatible CH552G programming tool.
Why Does This Work?

The CH552G USB bootloader requires its USB data-line programming condition to be activated.

In this PCB design, one terminal of C1 is connected to the V33 supply, while the other terminal is connected to the filtered UDP (USB D+) programming-related line.

By temporarily bridging these two terminals, the USB data line is pulled to the required logic level.

This makes it possible to activate the bootloader without directly accessing the CH552G's small physical pins.

In short:

C1 Pad 1 + C1 Pad 2 → Temporary Short → Connect USB → Release → Flash Firmware

⚠️ Important: Perform this procedure only during the USB connection/bootloader-entry sequence. Do not leave the pads permanently shorted. Confirm the actual PCB net connections before performing the procedure. Never bridge unrelated power or ground pads.
2. ESP32-C3 Firmware Programming

The ESP32-C3 has a separate programming interface.

Unlike the CH552G, the ESP32-C3 enters its download mode by holding GPIO9 LOW (GND) during reset.

The programming point is also accessible without touching the ESP32-C3 module's tiny pins.

📍 Where Should You Touch?

Locate the ESP32-C3 module, identified as U2.

Immediately to the upper-left side of the module, you will find resistor R3.

The lower terminal of R3 (the terminal closer to the module, according to the PCB orientation) is used as the accessible GPIO9 programming point.

You will also need a GND connection.

Convenient GND locations include:

The large exposed GND copper area on the PCB.
The metal shielding of the USB connector, provided it is electrically connected to GND.
The Procedure
Locate the ESP32-C3 module (U2).
Locate resistor R3 immediately to its upper-left.
Identify the lower terminal of R3, closer to the module.
Take a pair of conductive tweezers.
Touch one tweezer tip to the lower terminal of R3.
Touch the other tweezer tip to the PCB's GND plane or USB connector metal shell.
This temporarily pulls GPIO9 LOW.
While maintaining the connection, reset or power-cycle the ESP32-C3.
Release the connection after the chip has entered download mode.
Connect to the ESP32-C3 using a compatible USB flashing tool.
Upload your firmware.
Why Does This Work?

The ESP32-C3 uses GPIO9 as a boot-strapping pin.

When GPIO9 is held LOW during the appropriate reset sequence, the chip enters its serial download boot mode instead of starting the normal application firmware.

In this PCB design, R3 provides an accessible connection to the GPIO9 programming signal.

This eliminates the need to touch the ESP32-C3 module's tightly spaced pins.

In short:

R3 GPIO9 Point + GND → GPIO9 LOW → Reset/Power Cycle → Download Mode → Flash Firmware

⚠️ Important: GPIO9 must be LOW when the ESP32-C3 samples its boot configuration during reset. Simply grounding GPIO9 after the chip has already started running will not necessarily activate download mode. Release the connection after entering download mode.
🛠️ Programming Point Summary
Microcontroller	Programming Point	Required Connection	Purpose
CH552G	C1 capacitor pads	Temporarily bridge both pads	Activate USB bootloader
ESP32-C3	Lower terminal of R3	GPIO9 to GND	Enter download mode
Quick Reference

CH552G:

Component: C1
Action: Short both capacitor terminals.
Timing: During USB connection.
Result: CH552G bootloader activation.

ESP32-C3:

Component: R3
Action: Connect the accessible GPIO9 point to GND.
Timing: During reset or power-up.
Result: ESP32-C3 download mode.
💻 Firmware and Software

The project is designed around two independently programmable microcontrollers.

Each controller requires its own compatible firmware.

CH552G

The CH552G firmware is responsible for the USB-side communication and PC interface.

A compatible CH552G development and flashing environment is required.

ESP32-C3

The ESP32-C3 firmware is responsible for wireless communication and related control functions.

Possible development environments include:

ESP-IDF.
Arduino IDE with ESP32 support.
Other compatible ESP32-C3 development tools.

Note: Firmware source code, flashing configurations and software installation instructions should be added as they become available.

📦 Manufacturing

The repository includes the necessary PCB manufacturing and assembly resources.

Gerber Files

The Gerber_PCB1_2026-10-30.zip archive contains the PCB manufacturing outputs.

These files can be submitted to a PCB fabrication service.

Bill of Materials

The BOM spreadsheet contains the component list required for PCB assembly.

Pick and Place

The Pick and Place spreadsheet provides component positioning information for automated PCB assembly.

⚠️ Safety and Hardware Precautions

Please read these instructions before programming or handling the PCB.

Always verify the programming points against the schematic before applying power.
Use fine-tipped tweezers to avoid accidentally shorting neighboring components.
Do not bridge C1 pads continuously.
Do not accidentally short V33 directly to GND.
Avoid touching multiple component terminals simultaneously.
Ensure the USB connector is correctly aligned before connecting it.
Do not apply external voltage to USB data lines.
Make sure the correct firmware is selected for each microcontroller.
Avoid programming the CH552G and ESP32-C3 simultaneously unless the firmware and hardware configuration explicitly support it.

Programming should be performed carefully. Incorrect connections can permanently damage the microcontrollers or other PCB components.

📂 Repository Structure
text
CONTBTIR/
│
├── 3d1.png
├── 3d2.png
│
├── schem.png
│
├── BOM_Board1_PCB1_2026-10-30.xlsx
│
├── PickAndPlace_PCB1_2026-10-30.xlsx
│
├── Gerber_PCB1_2026-10-30.zip
│
├── LICENSE
│
└── README.md

🎯 Project Goal

The main objective of CONTBTIR is simple:

Control your PC using a single compact board instead of relying on multiple different remote-control adapters.

By combining USB connectivity, infrared control and Bluetooth wireless communication, CONTBTIR aims to provide a convenient and flexible PC control solution.

The accessible programming points also make firmware development, testing and updating easier.

📜 License

This project is licensed under the MIT License.

See the LICENSE file for more information.

👨‍💻 Author

ALP GIRAY ALTUNSOY

Project: CONTBTIR

GitHub: https://github.com/alpgirayaltunsoy-alp/CONTBTIR

<p align="center"> <b>CONTBTIR — One Board, Multiple Ways to Control Your PC.</b> </p>
