# Touch-based-device-control-system-for-bedridden-patients
This project is an assistive embedded system built on the LPC2148 microcontroller. It allows bedridden patients with limited mobility to operate household appliances safely and with minimal effort. It uses a secure keypad password check , SPI EEPROM , and a touch screen to easily toggle hardware devices.
## Required Components
1. LPC2148 Board  
2. LCD  
3. Keypad Matrix  
4. Resistive Touch Screen(1255)  
5. EEPROM(AT25LC512)  
6. Buzzer  
7. 2 LED's  
8. Switch  
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
