I am currently trying to build my own drone from scratch, and slowly replace every component with my own, from creating fully custom PCBs for the flight controller or ESCs, to making a 3D printed drone chassis etc etc. And this is the fully fledged, multi functional, cool-looking flight controller. I present to you:

# *SABRE!*

For one of my biggest projects yet I designed a jack of all trades custom flight controller board. It also comes with custom firmware, or if possible compatibility with betaflight or other FC firmware out there. At it's core are two PCBs arranged in a stackup with the "top" and "bottom" sections. The two PCBs connect through the common 2.54mm pitch headers and stack on top of eachother and each serve to provide enough space for all the components and separate the sensitive digital components from the electromagnetic potential of the motor power rails. The bottom board houses E-Fuse protection and an INA226 current monitor to protect the boards, including powered FPV feed with the AT7456E and TX5813, accompanied by four MOSFET powered 12V LED strip channels. The top booard houses a powerful STM32F765VGT CPU which will, *fingers crossed,* be more than enough to run automatic flight levelling software quickly enough to not crash. Followed by a micro SD card slot for storing flight telemetry, programmable DIP switched for easy settings, BNO08x 9-DOF sensor module, segment display and pushbuttons for easy configuration without needing to reupload code and a BMP580 barometic sensor. More than I (or you) will ever need to fly a 4 blade, 1KG piece of PETG plastic :)

### Bottom Board:

Half of the bottom PCB houses the power delivery circuitry. This includes a dedicated TPS259850 chip, or an E-Fuse, required protection option for a drone that operates at around 20A sustained and 40A current peaks. Thanks to it's extremely low RDS on, basically no power is wasted, and it trips the circuit in a matter of microseconds when too much current is detected, which makes it extremely useful for a drone full of sensitive circuitry, compared to a traditional wire fuse. Followed by that is an INA226 chip, this aims to precisely monitor the current consumption of the entire drone, helping to predict flight times and efficiently manage battery health, through the use of a shunt resistor sized to the expected current draw. The current planes are optimized to make proper use of the different outer and inner layer thicknesses and properly distribute current with minimal heat up.

The other part of the bottom PCB houses the RF circuitry for the FPV system LED drivers togethor with power regulators that power the 12V rail for the LED drivers and the 5V rail for the FPV system and other 5V chips. The 12V power regulator (TPS55289) and the 5V regulator (TPS63070) both use the safest and most reliable layouts possible and are positioned to spatially divide the high ~32A currents on the bottom power distribution circuitry from the rest of the bottom board, a slide switch also controls the power supply to the TX5813 module, as this consumes the most out of the other board circuitry, at about ~200mA, this should help to increase flight time by cutting wasted power when the FPV function isn't being used. CSD16340 MOSFETS also serve to drive 4 seperate 12V LED strip channels, these I plan to connect to LED strips and create cool illuminations off of my drone.

The RF circuitry benefits from an impedance accurate JLC-3313 stackup and use of coplanar waveguide routing with via fencing, tearsrops and udnerpad via router to insure the least impedance mismatch possible. The actual RF system is composed of an external camera connector, or simple pin header (capable of reading almost any FPV camera such as the Caddx Ant Lite). The video signal is then passed through a meticulously routed 75ohm impedance matched trace into a MAX7456EUI (Or a newer drop in AT7456E replacement, pin to pin compatible), this module serves to inject an OSD (On screen display) into the video feed through communication with the STM32 MCU for easy sending of telemtry data like battery voltage, speed, altitude etc. The output from the OSD injection chip is then routed through another impedance matched trace with SAG correction into the video input of the TX5813. This modules handles the complicated circuitry of converting the video feed (~6MHz) into a radio signal of ~5.8GHz. That RF is then routed even more carefuly for the short length (~8mm) of the SMA launch into the SMA connector then into any respective 5.8GHz FPV antenna. (I also have a complemintary project for a 5.8GHz FPV reciever in another repository)

### Top Board:

Flying the drone is an STM32F765VGT CPU with up to 216MHz clock speed, hopefully resulting in almost perfect drone levelling and flight, on the other side of the board is also a USB C receptacle for programming the STM CPU, and powering the board while doing so (everything except the motors, of course, I also included diodes to prevent power backfeed). Close by is a BNO08x 9-DOF accelerometer, gyroscope and magnetometer, complete with an integrated 32-bit ARM Cortex-M0+ for quick sensor fusion with pre built firmware (meaning I have to code waaaay less :D). Motor PWM outputs for the ESCs also connected directly to PWM channels on the STM32 and two solder bridgeable connections to allow me to use the 5V supply from the ESCs in case my 5V regulator doesn't function properly under load (I've had bad experience with voltage regulators). Several voltage indicator LEDs are also placed near the USB C port at the back. Then there is also an UART FPC port designed to connect to a separate GPS board (large board for good GPS recieving power, check my other repositories) on the left side of the board. A 7 segment, 4 digit bitbanged display and several tactile pushbottoms also occupy the centre of the board, quite useless but they might be useful in changing more accute settings without having to reupload firmware every time. A micro SD card also connects through 4 channel SPI to STM32, I plan to use this to store settings or flight telemetry information that I could analyze while debugging. Powering all that is a 3.3V regulator (LMR51430XF), simplest (and therefore most reliable) I could find. And several other breakout headers for power/functionality. PWM inputs from the radio remote is recieved through a 3x8 header pinout designed for my specific radio reciever module.

## Features:
- Fully integrated design:
	- Custom firmware uploadable
	- Power distribution and control in one place
	- Dual stacked PCB design
 	- No screw, no wire inter board connection

- Power
	- Extremely low RDS protection e-Fuse (us trip time)
 	- INA226 current and voltage monitoring
  	- Backup/calibration voltage divider monitoring
  	- Fully optimized current planes, 40A+ capability
  	- Firmware built in battery management and flight warning system
  	- Meticulously designed 5V, 12V and 3.3V switching voltage regulators
  	- Backup ESC 5V supply availible
  	- LED indicators for power rail status
  	- 4 channel driver for 12V LED strips
  	- Power switch for TX5813 module for longer flight times

- Fully integrated FPV support
	- MAX7456/AT7456E OSD injection into FPV feed
	- Channel switch ability
	- TX5813 RF launch module
	- Accurate and reliable RF design
 	- Designed for impedance matched JLC 3313 stackup
 
- Flight Control
	- USB C programming connection 
	- Powerful STM32F765VGT CPU
 	- MicroSD card slot (For inflight telemtry storage)
  	- Programmable DIP switches
  	- Display/pushbuttons for setting adjustment without code reupload
  	- BNO08x 9-DOF sensor module with built in sensor fusion
  	- BMP580 Barometric sensor
  	- UART FPC connection for external GPS board
  	- Extra power and I2C debug headers
  	- 7 input PWM channels
  	- 4 ESC output PWM channels


## PCB
This is the PCB, with L1 to L4 Layers, silkscreen for both layers and an all layers view

<img src=Pictures/PCB1.png alt="PCB" width="300"/> <img src=Pictures/PCB2.png alt="PCB 2" width="300"/> <img src=Pictures/PCB3.png alt="PCB 3" width="300"/> 
<img src=Pictures/PCB4.png alt="PCB 4" width="300"/> <img src=Pictures/PCB5.png alt="PCB 5" width="300"/> <img src=Pictures/PCB6.png alt="PCB 6" width="300"/>
<img src=Pictures/PCB7.png alt="PCB 7" width="300"/> <img src=Pictures/PCB8.png alt="PCB 8" width="300"/> <img src=Pictures/PCB9.png alt="PCB 9" width="300"/> 


And here is the schematic

<img src=Pictures/SCHEMATIC1.png alt="Schematic1" width="800"/>
<img src=Pictures/SCHEMATIC2.png alt="Schematic2" width="800"/>
<img src=Pictures/SCHEMATIC3.png alt="Schematic3" width="800"/>


## 3D render
Here is also a 3D render to show off the PCB :)

<img src=Pictures/RENDER1.png alt="Render1" width="300"/>

## Firmware Overview
I've included some basic firmware (**untested**) that should get the TFT display working out of the box with a quick upload from arduino IDE

## BOM:
The BOM list for the actual PCB components is in the BOM folder section and here is also a BOM list for all the other components I plan to buy for this:


- PCB             $378.08		
- Antenna         $7.77      https://www.aliexpress.com/item/1005006153683653.html		
- 90 degree SMA   $4.51      https://www.aliexpress.com/item/1005005665998117.html						
- TX5813 module   $15.82     https://www.aliexpress.com/item/1005011698073364.html
											
Total			      $406.18	


