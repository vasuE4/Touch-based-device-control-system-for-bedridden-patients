# Touch-based-device-control-system-for-bedridden-patients
This project is an assistive embedded system built on the LPC2148 microcontroller. It allows bedridden patients with limited mobility to operate household appliances safely and with minimal effort. It uses a secure keypad password check , SPI EEPROM , and a touch screen to easily toggle hardware devices.
## Features
1. Secure Access  
2. User Friendly Control  
3. Non-Volatile Memory  
## Required Components
1. LPC2148 Board  
2. LCD(20x4)  
3. Keypad Matrix  
4. Resistive Touch Screen(1255)  
5. EEPROM(AT25LC512)  
6. Buzzer  
7. 2 LED's  
8. Switch
## Tools   
Compiler: Keil uVision4   
Flashing Tools: Flash Magic   
## System Architecture and Component roles  
The system is built around the LPC2148, which acts as the intelligent hub for all peripheral interactions. The architecture is designed for reliability, ensuring that user commands are processed securely and executed predictably.   
1. The Central Controller: LPC2148  
The LPC2148 manages the entire execution flow. It handles:  
   * Peripheral Management: Managing the communication protocols (UART, SPI, GPIO) required to interface with external hardware.
   * Logic Processing: Running the main control loop for device state management and executing the password-based security check.
   * Interrupt Handling: Managing the EINT1 (External Interrupt) to allow for secure, real-time user-initiated password changes without interrupting standard device operation.
2. Security & Data Management:
   * Keypad Matrix: Acts as the primary Security Gateway. It provides a tactile interface for the user to authenticate before gaining access to the control interface, preventing accidental or unauthorized operation of home appliances.
   * SPI EEPROM (AT25LC512): Serves as the Non-Volatile Storage. It holds the user's password securely. By utilizing the SPI (Serial Peripheral Interface) protocol, the microcontroller can quickly read or update this password, ensuring credentials persist even after a complete power loss.
3. User Interface & Feedback:
   * Resistive Touch Screen: Communicates with the controller via UART (Universal Asynchronous Receiver-Transmitter). This allows the system to receive precise (x, y, z) coordinate data, which the controller maps to specific appliance "buttons." This provides an intuitive way for patients with limited mobility to interact with their environment.
   * LCD Module: Acts as the Visual Dashboard, providing immediate feedback to the patient. It displays system status (e.g., "Locked" vs. "Unlocked"), confirms password entries, and shows the current operational status of the fans and lights.
   * Buzzer & LEDs: Provide Multisensory Feedback.
         * Buzzer: Offers an audible alert to notify the Doctor in case of Emergency.
         * LEDs: Offer a visual confirmation of device states (e.g., an LED glowing when the fan is active), which is essential for patients who may have hearing or visual impairments.  
## Set-up Instructions
1. Before using the Peripherals you must initialize the peripherals by calling InitLCD(),InitKPM(),InitUART0(),Init_SPI().  
2. Notes that you must include all the required headers like "LPC21xx.h","string.h" and user defined hearders like "lcd_defines.h","lcd.h","delay.h","types.h","SPI.h","spi_defines.h","defines.h","KPM.h","UART_INT.h","cfgportpinfunc.h","KPM_defines.h".
   * "lcd_defines" contains the command values and the pin connections of LCD.  
   * "lcd.h" contains the function declarations of LCD.
   * "delay.h" contains the function declarations of delay functions according to the time of delay required.
   * "types.h" contains the type casted details of the existing data types.  
   * "SPI.h" contains the function declarations of the SPI related operations.
   * "spi_defines.h" contains the commands and the pin connections of SPI.
   * "defines.h" contains the macro expansions of Bit/Byte manipulation.
   * "KPM.h" contains the function declarations of the Keypad related operations.
   * "KPM_defines.h" contains the Pin connections of the Keypad.
   * "UART_INT.h" contains the function declarations of the UART related operations.
   * "cfgportpinfunc.h" contains the function declarations of the Pin configuration.
## Code Execution Flow
START   
    Initialize LCD, Keypad, UART, SPI (for EEPROM), and External Interrupts  
    Read stored password from EEPROM 
        
    LOOP (Main System):  
        IF (m == 1): // Security Check  
            Wait for valid password from Keypad   
            IF (password is correct):  
                Set m = 0, Display "Unlocked"
                   
        Wait for input from Touch Screen (UART)   
        Convert touch coordinates (x, y, z) to integers  
        IF (Touch is in "Enable/Disable" region):  
            Toggle 'enable' state  
  
        IF (enable == 1):  
            IF (Touch in "Buzzer" region):  
                Toggle buzzer, Update hardware  
            IF (Touch in "Fan" region):  
                Set fan state (ON/OFF), Update hardware  
            IF (Touch in "Light" region):  
                Set light state (ON/OFF), Update hardware  
     
        Update LCD with current states (Touch, Buzzer, Fan, Light)  
    END LOOP  
       
ISR (EINT1 - Password Change):  
    Prompt user for current password  
    IF (current password matches EEPROM):  
        Loop:  
            Prompt for new password  
            Prompt to confirm new password    
            IF (confirm matches new):  
                Save new password to EEPROM  
                Restart system logic  
                BREAK Loop  
    RETURN from ISR     
## Pin Connections  
1. Connect P0.1 to the Resistive TOuch Screen(1255) to receive the xyz co-ordinates.  
2. Connect P0.3 to the switch to raise an External interrupt because P0.3 supports the EINT1 for functionality 4 in LPC2148.
3. Connect P0.4 to the SCLK pin of the EEPROM(AT25LC512).  
4. Connect P0.5 to the MISO(Master in-Slave out) of the EEPROM(AT25LC512).
5. Connect P0.6 to the MOSI(Master out-Slave in) of the EEPROM(AT25LC512).
6. Connect P0.7 to the CS(Chip Select) of the EEPROM(AT25LC512).
7. Connect the pin P0.8 to P0.15 to the LCD.
8. Connect the pin P0.17 to RS(Register Select) of LCD and P0.18 to EN(Enable) pin of LCD.
9. Connect the pin P0.22 to the LED2 and consider as Light.
10. Connect the pin P0.23 to the LED1 and consider as Fan.
11. Connect the pin P0.25 to the Buzzer.
12. Connect the VCC and GND pin of EEPROM(AT25LC512) to the 3.3V and GND respectively.
13. Connect the VCC and GND pin of Resistive Touch Screen(1255) to the 3.3V and GND respectively.
14. Connect the Keypad from the pins P1.16 to P1.23.  
## How to Use
1. System Power-Up:  
Upon connecting the power supply after Loading the code into the hardware, the system will initialize all peripherals.   
2. Authentication (Unlocking):   
The system requires authentication to prevent accidental usage.   
Enter your designated password using the Keypad Matrix.   
If the password is correct, the LCD will display "Unlocked," and you will gain access to the control interface.  
3. Operating Appliances:  
Once the system is unlocked, the Resistive Touch Screen becomes active.  
  Enable: First, touch the "Enable" region to activate the control interface.  
  Control: Select the desired region on the touch screen for the "Fan" or "Light".   
The system will toggle the state of the selected appliance (ON/OFF) and update the hardware. The corresponding LED will light up to provide visual confirmation of the device's current status.  
4. Changing Your Password:  
        * If you need to update your security credentials, press the physical button connected to the External Interrupt (EINT1) pin p0.3.  
        * The system will prompt you on the LCD to enter your current password for verification.  
        * Once verified, you will be prompted to enter and confirm your new password.  
        * The system will automatically save the new password to the EEPROM and return to the main control loop.  
Note: Always ensure the system is in the "Enabled" state before attempting to toggle appliances via the touch screen. If the system is "Disabled," touch inputs for appliances will be ignored for safety.  
## Future Improvements  
1. Remote Monitoring & Connectivity:
   Integrate a Wi-Fi or Bluetooth module to allow caretakers to monitor appliance status or toggle devices remotely via a smartphone app.
2. Intelligent Emergency Alerts:
   Present we are having an Emergency Buzzer,but we can improve that particular feature when pressed, triggers an audible alarm and sends a notification to a caregiver’s phone, ensuring rapid assistance when needed.
3. Implementation of RTC-based Automation:
   The system is architected to support a real-time clock, which can be expanded to include automated daily routines, customizable appointment reminders, and time-stamped activity logging to enhance the patient's independence like Medication etc.  
## Block Diagram   
<img width="858" height="616" alt="Screenshot (3)" src="https://github.com/user-attachments/assets/be0b9a5b-e843-4119-8640-ccadd23fdaec" />   
## Reference  
Output Video: https://github.com/user-attachments/assets/441dc235-6f77-425f-bc0e-de0daa45938b   
Output pics:   
<img width="1200" height="1600" alt="WhatsApp Image 2026-06-07 at 20 26 50" src="https://github.com/user-attachments/assets/de11b9dd-a0ab-4a2d-9bab-802ecf0293f8" />
<img width="1200" height="1600" alt="WhatsApp Image 2026-06-07 at 20 26 50 (1)" src="https://github.com/user-attachments/assets/fdf4dbc3-3356-4a7a-b92c-4473fd8d5878" />
<img width="1200" height="1600" alt="WhatsApp Image 2026-06-07 at 20 26 51" src="https://github.com/user-attachments/assets/a6be8774-3a2e-401d-a64f-0bfab0c9a995" />
<img width="1200" height="1600" alt="WhatsApp Image 2026-06-07 at 20 26 51 (1)" src="https://github.com/user-attachments/assets/0e5ff707-2b44-48f7-977c-7e7fa3034d8f" />

  
