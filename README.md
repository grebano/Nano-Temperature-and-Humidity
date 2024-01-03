# Nano-Temperature-and-Humidity

This project is about creating a temperature and humidity sensor using Arduino Nano, a 0.96" OLED display, and an AHT10 (or AHT20) sensor. The project is developed using PlatformIO.

## Hardware Requirements

- Arduino Nano
- 0.96" OLED display
- AHT10 sensor
- Breadboard
- Jumper wires
- USB cable
- 5V power supply (optional)

## Software Requirements

- [PlatformIO](https://platformio.org/)

## Setup

1. Connect the Arduino Nano to the AHT10 sensor and the OLED display.
2. Open the project in PlatformIO.
3. Build and upload the code to the Arduino Nano.

## Images

Here are some images of the project:

![Image 1](./images/Temperature_and_Humidity_v2.png)
![Image 2](./images/RealPic.jpg)

Notice that the first image shows the schematic of the project realized on a breadboard. The second image shows the actual project.

## Code

The main code for the project is in `src/main.cpp`. The code is well commented and should be easy to understand. It is divided into two parts: the first part is the setup and the second part is the loop. The setup part is used to initialize the OLED display and the AHT10 sensor. The loop part is used to read the temperature and humidity from the sensor and display them on the OLED display.

## Low Power 
The LowPower library is used to put the Arduino Nano to sleep for 120 seconds between each reading. This is done to save power. In order to do so the Arduino will be put to sleep using the `LowPower.powerDown(SLEEP_8S, ADC_OFF, BOD_OFF);` command. The Arduino will wake up after 8 seconds and then go back to sleep. This will be repeated 15 times. After that it will wake up and read the temperature and humidity from the sensor. This will be repeated forever.

In order to lower the power consumption even more the clock speed of the Arduino is lowered from 16 MHz to 8 MHz. This is done by adding the following lines in setup:
```
CLKPR = 0x80;
CLKPR = 0x01;
```
The first line sets the CLKPCE bit in CLKPR to enable a change in the clock prescaler. The second line sets the CLKPS0 bit in CLKPR to divide the clock by 2.

The last thing that is done to lower the power consumption is to reduce the brightness of the OLED display. This is relevant, since the display that is always on. 

## Power draw
| Sleep Current | Active Current | Display Brightness | Voltage | Clock Frequency | LED Power |
|---------------|----------------|--------------------|---------|-----------------|-----------|
|      11mA     |       26mA     |            1       |   5V    |       16MHz     |     Yes   |
|      14mA     |       23mA     |          255       |   5V    |        8MHz     |     Yes   |
|      11mA     |       20mA     |            1       |   5V    |        8MHz     |     Yes   |
|       8mA     |       17mA     |            1       |   5V    |        8MHz     |     No    |

The best scenario is to have the display brightness at 1, the clock frequency at 8MHz, and the LED power off. This will result in a power draw of 8mA for most of the time. The power draw will increase to 17mA for a short period of time when the temperature and humidity is read from the sensor.
The clock has been set to 8MHz in order to be able to use a 3.7V lipo battery as a power supply. The Arduino Nano can in fact be powered by a 3.7V lipo battery, but the clock frequency has to be lowered to 8MHz in order to remain in safe operating area.


## License

This project is licensed under the terms of the `LICENSE` file.