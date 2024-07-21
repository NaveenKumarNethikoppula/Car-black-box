# Car-black-box
Car Black Box Using PIC18F4580
Overview
A car black box system, implemented using the PIC18F4580 microcontroller, records critical data related to vehicle operation and accidents. This data helps in post-accident analysis and can be used for improving vehicle safety, understanding crash dynamics, and providing evidence in legal cases.

Key Features
Data Logging: Records vehicle speed, engine RPM, braking status, and other parameters.
Accident Detection: Detects and records data during crash events.
Storage: Saves the recorded data in non-volatile memory for later retrieval.
Communication: Allows data retrieval via USB or CAN bus interface.
System Components
PIC18F4580 Microcontroller:

40-pin microcontroller from Microchip.
Built-in CAN (Controller Area Network) module for vehicle network communication.
32 KB Flash Program Memory, 1536 Bytes Data Memory.
Sensors:

Accelerometer: Detects sudden deceleration indicative of a crash.
Speed Sensor: Measures vehicle speed.
Gyroscope: Records angular velocity for understanding vehicle orientation during the crash.
Brake Sensor: Detects when brakes are applied.
Memory:

EEPROM: For non-volatile storage of logged data.
Communication Interface:

CAN Bus: For integration with the vehicle’s network.
USB Interface: For data retrieval and configuration.
Circuit Diagram
Below is a simplified block diagram of the system:

css
Copy code
[ Accelerometer ]    [ Speed Sensor ]    [ Gyroscope ]    [ Brake Sensor ]
        |                     |                   |                 |
        |                     |                   |                 |
        -------------------------------------------------------------
                                |
                          [ PIC18F4580 ]
                                |
                  [ CAN Bus ] - [ EEPROM ] - [ USB Interface ]
Working Principle
Data Collection: Sensors continuously monitor vehicle parameters and send data to the PIC18F4580 microcontroller.
Processing and Logging: The microcontroller processes the data and stores it in EEPROM. When a significant event, such as a crash, is detected (using the accelerometer data), the relevant data is locked and saved.
Data Retrieval: The stored data can be retrieved via the CAN bus or USB interface for analysis.
Example Code
Here is a simple example of how the PIC18F4580 can be programmed to read from an accelerometer and log data to EEPROM:

c
Copy code
#include <xc.h>
#include "i2c.h"
#include "eeprom.h"

#define ACCEL_ADDR 0x1D  // I2C address for accelerometer

void init_system() {
    // Initialize I2C for accelerometer communication
    i2c_init();
    // Initialize EEPROM
    eeprom_init();
}

void log_accelerometer_data() {
    uint8_t accel_data[6];
    // Read accelerometer data
    i2c_read(ACCEL_ADDR, 0x32, accel_data, 6);
    // Log data to EEPROM
    eeprom_write_block(0x00, accel_data, 6);
}

void main() {
    init_system();
    while (1) {
        log_accelerometer_data();
        __delay_ms(1000);  // Log data every second
    }
}
Benefits and Applications
Accident Analysis: Helps in understanding the cause of accidents.
Insurance Claims: Provides evidence for resolving insurance disputes.
Vehicle Maintenance: Monitors vehicle health and performance.
Safety Improvements: Data can be used to improve vehicle design and safety features.
The PIC18F4580-based car black box system is a robust solution for recording and analyzing vehicle data, significantly contributing to vehicle safety and post-accident investigations.
