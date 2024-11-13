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

```
#include "ch32v00x.h" // Include the CH32 header file for your microcontroller
#define OLED_ADDRESS 0x3C  // I2C address for the SSD1306 OLED display
// SSD1306 command bytes
#define SSD1306_COMMAND 0x00
#define SSD1306_DATA 0x40
// Function prototypes
void OLED_Init(void);
void OLED_Command(uint8_t command);
void OLED_Data(uint8_t data);
void OLED_Clear(void);
void OLED_DisplayText(const char *text);
// Main function
int I2C_Init(void)
{
    I2C_InitTypeDef I2C_InitStructure;
    I2C_InitStructure.I2C_ClockSpeed = 400000;  // 400kHz for fast-mode
    I2C_InitStructure.I2C_Mode = I2C_Mode_I2C;
    I2C_InitStructure.I2C_DutyCycle = I2C_DutyCycle_2;
    I2C_InitStructure.I2C_OwnAddress1 = 0x00;
    I2C_InitStructure.I2C_Ack = I2C_Ack_Enable;
    I2C_InitStructure.I2C_AcknowledgedAddress = I2C_AcknowledgedAddress_7bit;
    I2C_Init(I2C1, &I2C_InitStructure);
    I2C_Cmd(I2C1, ENABLE);
    while (1) {
        // Main loop
    }
}
void Delay_Ms(uint32_t n);
void GPIO_Config(void)
{
    GPIO_InitTypeDef GPIO_InitStructure = {0}; // structure variable used for GPIO configuration
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOD, ENABLE); // to enable the clock for port D
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOC, ENABLE); // to enable the clock for port C
    OLED_DisplayText("Hello, OLED!");  // Display text on the OLED
    RCC_APB1PeriphClockCmd(RCC_APB1Periph_I2C1, ENABLE);
    //RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOC, ENABLE);
    GPIO_InitTypeDef GPIO_InitStructure;
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_1 | GPIO_Pin_2;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_OD;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOC, &GPIO_InitStructure);
    // Configure GPIO pin 1 as input for the button
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_5 | GPIO_Pin_6;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU; // Input with pull-up
     GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOC, &GPIO_InitStructure);
    //LED_CHECK
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_6;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP; // Defined Output Type
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz; // Defined Speed
    GPIO_Init(GPIOD, &GPIO_InitStructure);
}
void USART_Config(void)
{
    USART_InitTypeDef USART_InitStructure = {0};
    // Initialize USART for USB serial communication
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_USART1, ENABLE);
    USART_InitStructure.USART_BaudRate = 9600;
    USART_InitStructure.USART_WordLength = USART_WordLength_8b;
    USART_InitStructure.USART_StopBits = USART_StopBits_1;
    USART_InitStructure.USART_Parity = USART_Parity_No;
    USART_InitStructure.USART_Mode = USART_Mode_Tx;
    USART_Init(USART1, &USART_InitStructure);
    USART_Cmd(USART1, ENABLE);
}
void ADC_Init(void)
{
    ADC_InitTypeDef ADC_InitStructure = {0};
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_ADC1, ENABLE); // Enable ADC clock
    ADC_InitStructure.ADC_Mode = ADC_Mode_Independent;
    ADC_InitStructure.ADC_ScanConvMode = DISABLE;
    ADC_InitStructure.ADC_ContinuousConvMode = ENABLE;
    ADC_InitStructure.ADC_ExternalTrigConv = ADC_ExternalTrigConv_None;
    ADC_InitStructure.ADC_DataAlign = ADC_DataAlign_Right;
    ADC_InitStructure.ADC_NbrOfChannel = 1;
    ADC_Init(&ADC_InitStructure);//ADC_Init(ADC1, &ADC_InitStructure);
    ADC_RegularChannelConfig(ADC1, ADC_Channel_0, 1, ADC_SampleTime_57Cycles); // Adjust for your channel
    ADC_Cmd(ADC1, ENABLE);
    // Start ADC calibration
    ADC_ResetCalibration(ADC1);
    while (ADC_GetResetCalibrationStatus(ADC1));
    ADC_StartCalibration(ADC1);
    while (ADC_GetCalibrationStatus(ADC1));
    // Start the ADC
    ADC_SoftwareStartConvCmd(ADC1, ENABLE);
}
uint16_t ADC_Read(void)
{
    // Wait until conversion is complete
    while (ADC_GetFlagStatus(ADC1, ADC_FLAG_EOC) == RESET);
    return ADC_GetConversionValue(ADC1);
}
int main(void)
{
    SystemCoreClockUpdate();
    Delay_Init();
    ADC_Init();
    GPIO_Config();
    USART_Config();
    I2C_Init();   // Initialize I2C
    OLED_Init();  // Initialize OLED
    OLED_Clear(); // Clear the screen
    while (1)
    {
        // Read ADC value from potentiometer
        uint16_t potValue = ADC_Read();
        // Scale to 8 bits (0-255) for volume control
        uint8_t volume = (uint8_t)(potValue / 16);  // Assuming a 12-bit ADC
        // Send the volume over serial
        USART_SendData(USART1, volume);
        while (USART_GetFlagStatus(USART1, USART_FLAG_TC) == RESET);
        Delay_Ms(100); // Delay to avoid excessive serial traffic
        // Check if the button is pressed (assuming active-low)
        if (GPIO_ReadInputDataBit(GPIOC, GPIO_Pin_5) == 0)
        {
            GPIO_WriteBit(GPIOD, GPIO_Pin_6, SET);
            // Send a character (e.g., 'S') over serial when the button is pressed
            USART_SendData(USART1, 'S');
            while (USART_GetFlagStatus(USART1, USART_FLAG_TC) == RESET);
            // Debounce delay
            Delay_Ms(300);
        }
        else
        {
            GPIO_WriteBit(GPIOA, GPIO_Pin_6, RESET);
        }
    }
}
// Display text (simple example, doesn't handle font)
void OLED_DisplayText(const char *text)
{
    while (*text) {
        OLED_Data(*text++);
    }
}
// Clear the OLED display
void OLED_Clear(void)
{
    for (uint16_t i = 0; i < 1024; i++) {
        OLED_Data(0x00);
    }
}
// Send data to OLED
void OLED_Data(uint8_t data)
{
    I2C_GenerateSTART(I2C1, ENABLE);
    while (!I2C_CheckEvent(I2C1, I2C_EVENT_MASTER_MODE_SELECT));
    I2C_Send7bitAddress(I2C1, OLED_ADDRESS << 1, I2C_Direction_Transmitter);
    while (!I2C_CheckEvent(I2C1, I2C_EVENT_MASTER_TRANSMITTER_MODE_SELECTED));
    I2C_SendData(I2C1, SSD1306_DATA); // Data mode
    while (!I2C_CheckEvent(I2C1, I2C_EVENT_MASTER_BYTE_TRANSMITTED));
    I2C_SendData(I2C1, data);
    while (!I2C_CheckEvent(I2C1, I2C_EVENT_MASTER_BYTE_TRANSMITTED));
    I2C_GenerateSTOP(I2C1, ENABLE);
}
// OLED Initialization
void OLED_Init(void)
{
    OLED_Command(0xAE); // Display OFF
    OLED_Command(0xD5); // Set display clock divide ratio/oscillator frequency
    OLED_Command(0x80); // Set divide ratio
    OLED_Command(0xA8); // Set multiplex ratio
    OLED_Command(0x3F); // 1/64 duty
    OLED_Command(0xD3); // Set display offset
    OLED_Command(0x00); // No offset
    OLED_Command(0x40); // Set start line address
    OLED_Command(0x8D); // Enable charge pump
    OLED_Command(0x14);
    OLED_Command(0x20); // Set memory addressing mode
    OLED_Command(0x00); // Horizontal addressing mode
    OLED_Command(0xA1); // Set segment re-map
    OLED_Command(0xC8); // Set COM output scan direction
    OLED_Command(0xDA); // Set COM pins hardware configuration
    OLED_Command(0x12);
    OLED_Command(0x81); // Set contrast control
    OLED_Command(0xCF);
    OLED_Command(0xD9); // Set pre-charge period
    OLED_Command(0xF1);
    OLED_Command(0xDB); // Set VCOMH deselect level
    OLED_Command(0x40);
    OLED_Command(0xA4); // Entire display ON
    OLED_Command(0xA6); // Set normal display
    OLED_Command(0xAF); // Display ON
}
// Send command to OLED
void OLED_Command(uint8_t command)
{
    I2C_GenerateSTART(I2C1, ENABLE);
    while (!I2C_CheckEvent(I2C1, I2C_EVENT_MASTER_MODE_SELECT));
    I2C_Send7bitAddress(I2C1, OLED_ADDRESS << 1, I2C_Direction_Transmitter);
    while (!I2C_CheckEvent(I2C1, I2C_EVENT_MASTER_TRANSMITTER_MODE_SELECTED));
    I2C_SendData(I2C1, SSD1306_COMMAND); // Command mode
    while (!I2C_CheckEvent(I2C1, I2C_EVENT_MASTER_BYTE_TRANSMITTED));
    I2C_SendData(I2C1, command);
    while (!I2C_CheckEvent(I2C1, I2C_EVENT_MASTER_BYTE_TRANSMITTED));
    I2C_GenerateSTOP(I2C1, ENABLE);
}
```
