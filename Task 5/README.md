# Macro Pad with Volume Control using VSD Squadron Mini

## Overview
This project aims to act as a proof of concept and somewhat of a testing stage for a bigger Project that I have in Mind - A 9 Key MacroPad with Each Key Display and Control Knobs. The Project for this Internship involves utilizing the VSDSquadron Mini Board, and the CH32 framework to program 2 Macro Keys, a Volume Knob and a Display for Macro Functions. The Project utilizes I2C communication to display images on the OLED Display, and USART communication alongside Pyhton Scripts to Perform Operatons on a Windows 11 machine, in accordance with the input received from the Keys and Volume Knob. This project demonstartes the capabilities of the VSDSquadronMini Board, by showcasing it's compatibility with Serial Communication Standards like USART, and I2C.
## Components Required
- VSD Squadron Mini
- 2 Mechanical Key Switches for Macro Input
- OLED Screen for Button Mapping Display
- 10K Potentiometer Knob for Volume Control
- BreadBoard
- Jumper Wires
- VS Code
- PlatformIO Framework

## Hardware Connections
- **Input:** 3 Input GPIO pinis are required, 2 are in Digital Mode (Mechanical Keys) and 1 in Analog (10K Potentiometer).
- **Output:** The 0.96 inch display is connected to PC1(SDA), PC2(SCL), 5V(VDD) and GND for I2C communcation with the OLED Display
## Source Code