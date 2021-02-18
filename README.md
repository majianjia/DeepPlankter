# DeepPlankter
An autonomous wave propelled drone boat.

# Hardware
This repo contains the main controller board of DeepPlankter. 

**The controller:**

- Dimension: 80 x 100 mm
- MCU: STM32H743ZI or STM32H7A3ZI (pin compatible)
- NorFlash
- RTC


**Sensors:**
- IMU: BMI088
- e-compass: HMC5883L
- Eviroment sensor: BME280 (humidity, atmospheric pressure, temperature)

**Power supplies:**
- 5.1V, 4A (max) x 3 controlled channels.
- 3.3V, 4A (max) x 2 controlled channels.
- 2 battery connectors 
- 2 charging connectors. 

**Power connector functions**

| Connector | Ctrl. Switched | Voltage | Current  | Comments|
| --- | ---- | ---- | ---- | ---- |
| 3V (CH1, CH2) | yes |  |  |  |
| 5V (CH1, CH2, CH3) | yes |  |  |  |
| BAT (CH1, CH2) | *yes | yes | yes |  |
| Charge (CH1, CH2) | |  | yes |  |
| ESC power ) | yes | | yes| |
| Servo (CH1, CH2)| yes| | yes| |
| Servo (CH3)| | | | |

**Expension IOs:**
- 2 x 2P NTC (for each battery)
- 1 x 2P ADC
- 1 x 2P External LED (Flash light)
- 1 x 2P External Buzzer
- 3 x 6P UART (marked TELE1, TELE2, main GPS)
- 2 x 4P UART
- 2 x 8P SPI (MISO, MOSI, SCK, CS1, CS2, IO)
- 2 x 4P I2C (same PHY)
- 1 x 4P FDCAN
- 1 x USBD
- 1 x 24P FPC DCMI camera interface (for OV2640)
- 1 x 8P FPC and 2x4 rows debugging port (SWD + UART)
- 1 x SDCard slot (SDIO)



# Contact
Jianjia Ma 
`majianjia(-at-)live.com`