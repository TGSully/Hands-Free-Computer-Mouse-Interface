# Hands Free Computer Mouse Interface

## 1	Introduction
Many of us take interfacing with a computer for granted.  What if you had limited to no movement of your hands and/or arms?  How would you manipulate the computer mouse efficiently?  This research project aims at solving this problem of a hands free computer mouse.  

## 1.1	Previous Works
There are a few products currently on the market that attempt to this problem, but are not completely hands free.  There is a product that costs a few hundred of dollars called the Computer Air Mouse that was designed with a head mounted laser for mouse movement, but still requires the use of a hand to click the mouse. My hands free mouse research project is different because it will operate completely hands free.  There are not many wearable HID mouse control devices on the market.  Various dwell-based mouse clicking software exists with the use of joysticks, however it has not been widely embedded in hardware.

## 1.2	General Overview
In order to achieve a completely hands free device, the movement of the head will be utilized.  Tilting the head in certain directions will trigger appropriate HID mouse commands and cursor movement.  

The orientation of the head is sensed by the MPU6050 9-degrees-of-freedom accelerometer and gyroscope.  A Texas Instruments MSP430G2553 microcontroller interprets the accelerometers data.  Finally the appropriate HID mouse command and movement data is transmitted to the computer via Bluetooth.  
Essential mouse commands include clicking, Upward/Downward cursor movements, and Left/Right cursor movement.  The MPU6050 will control the up, down, left, and right movements of the cursor based on  the pitch and roll measurements calculated system.  The clicking will be implemented using a method called dwell clicking.  This is executed by moving the mouse over a desired object and hovering for a predetermined amount of time.  After the clicking timer threshold is exceeded, the mouse will execute a double Left Click.  This method ensures that the device is completely hands free and does not require external buttons. 

## 1.3	MPU6050
The MPU6050 is a 9 degree-of-freedom accelerometer and gyroscope made by InvenSense.  The pitch and roll values are calculated from the data sent via I2c to the MSP430.  The pitch pertains to the head tilting back or looking downward, this is the range of a person nodding “Yes”.  This pitch variation will control the up and down movement of the mouse cursor.  Roll pertains to the head tilting left to right.  This roll variation will control the left to right movement of the mouse.  

## 1.4	MSP430G2553
The Texas Instrument MSP430G2 series microcontrollers are ready to program right out of the box.  However if a project requires a hardware serial connection, then a few alterations to the board must be made.  First, the TX and RX jumpers in the J3 quadrant must be connected parallel to the dotted line printed on the PCB.  This ensures that hardware serial is activated on the MSP430G2553 Launchpad.
               
	Tx and Rx jumper orientation in J3
Next the jumper for LED2 in junction J3 for port P1.6 should be removed.  If the jumper is not removed, the port P1.6 will be grounded and not function properly. This port is necessary for implementing I2c protocol with the MSP430G2553 launchpad.  Port P1.6 is the clock pulse data and port P1.7 is the data pin for I2c communication.
                   
               LED2 jumper removed from P1.6 
This process of setting up the MSP420G2553 to activate hardware serial and I2c protocol is not largely documented, so I felt this procedure was worth noting.  
	After the data from the MPU6050’s registers are read into the MSP430, the roll and pitch are calculated.  The code allows for a dead zone of up to ten degrees from level.  This lets the user have a region where the mouse movement will stop and the user can read and/or wait for a dwell click execution.  As the ten degree threshold is exceeded, the mouse will move in the according direction.  This threshold range can be easily altered by changing the trig variable in the MSP430’s code.  The larger the value, the bigger the dead zone range.  This is helpful for users who experience bodily tremors and require a larger amount of idle space.
There are four output pins on the MSP430.  Each pin is mapped to a specific direction +roll, -roll, +pitch and -pitch.  This will later translate into mouse cursor movement left, right, up, and down  The pins are activated when the threshold is exceeded in that direction or directions.  A maximum of two pins can be active at a time.   These pins are connected to the bases of four NPN transistors.  The emitters are connected to ground and the collectors are connected to the Feather 32U4.

## 1.5   Bluetooth/HID Communication
The Adafruit Feather 32U4 is a module that can handle many tasks.  This device was assigned the tasks of HID communication and Bluefruit, a Bluetooth LE transmission.  The HID commands utilized were mapped to four digital pins of the Feather.  The command mapping is as follows; D2 triggers cursor up, D3 triggers cursor down, D5 triggers cursor left and lastly D6 triggers cursor right.  These digital pins are active low and require an internal pull-up resistor initialized in the source code.  Each pin is connected to the collector pin of the NPN transistor.  When the MSP430 sends a signal to one of the base pins of the transistors, the transistor is put into active mode and acts like a closed switch.  The result is grounding the activated digital pin of the Feather which triggers a particular mouse cursor movement.
	Finally the HID command must be then sent to the HID compatible computer or device.  This is handled by the onboard Bluetooth LE communication called Bluefruit.  This low energy bluetooth connection runs on 3.3v and is great for close range data communication.  Another fantastic feature is that Bluefruit also handles wireless bluetooth communication with mobile phones.  
	

Breadboard for Hands Free Mouse:

## 2	Schematic
![Alt text](/path/to/img.jpg "Optional title")
## 3	Hands Free Mouse Flowchart

## 5	Value of the Invention
This project can provide a world of value to the right user.  A target audience for this hands free mouse would be someone who has limited mobility of their hands or arms.  However, many types of people may find this device useful. 
Degenerative diseases such as ALS and Parkinson’s affect fine motor skills therefore a device such as this can aide in the the user’s navigation of their computer, phone, and any other Bluetooth LE HID compliant device.  Phone navigation is another technological aspect that can be difficult with limited hand motion.  
Another implementation of this device would be to extend the mode of operation.  The device could also be attached to the user’s hand or finger.  Any object which can perform pitch and roll can be interfaced with the hands free mouse interface.
Further advancements on this topic might include movement-based communication software, game console implementation, or music production and performance.  Home automation has become increasingly popular over the years.  Determining a user’s body orientation in a home could lead to advancements in the modern house technology.  
Medical alert systems could utilize this device for monitoring if a patient falls abruptly.  Monitoring the amount of movement during a patients sleep can also be easily accomplished.

Hands Free Mouse Demo Link: <br />
https://www.youtube.com/watch?v=s068qIjzAFE


# 6	References
[1] Gorodnichy, D. and Roth, G. (2004). Nouse ‘use your nose as a mouse’ perceptual vision technology for hands-free games and interfaces. Image and Vision Computing, 22(12), pp.931-942.

[2] Toyama, K. (2018). [online] Available at: https://www.researchgate.net/profile/Kentaro_Toyama/publication/2646708_Look_Ma_-_No_Hands_-_Hands-Free_Cursor_Control_with_Real-Time_3D_Face_Tracking/links/5411e0b40cf2788c4b354ebb.pdf [Accessed 28 Jul. 2018].

[3] Castillo, A., Cortez, G., Diaz, D., Espiritu, R., IIisastigui, K., O'Bard, B. and George, K. (2016). Hands Free Mouse. [online] Available at: https://ieeexplore.ieee.org/abstract/document/7516242/.

[4] Simpson, T., Broughton, C., Gauthier, M. and Prochazka, A. (2008). Tooth-Click Control of a Hands-Free Computer Interface. IEEE Transactions on Biomedical Engineering.
