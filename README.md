I am currently trying to build my own drone from scratch, and slowly replace every component with my own, from creating fully custom PCBs for the flight controller or ESCs, to making a 3D printed drone chassis etc etc. And this is the fully fledged, multi functional, cool-looking flight controller. I present to you:

# **Sabre!**

For one of my biggest projects yet I designed a custom flight controller board. It also comes with custom firmware, or if possible compatibility with betaflight or other FC firmware out there. At it's core are two PCBs arranged in a stackup with the "top" and "bottom" sections. The two PCBs connect through the common 2.54mm pitch headers and stack on top of eachother and each serve to provide enough space for all the components and separate the sensitive digital components from the electromagnetic potential of the motor power rails.

### Bottom Board

Half of the bottom PCB houses the power delivery circuitry. This includes a dedicated TPS259850 chip, or an E-Fuse, a very good protection option for a drone that operates at around 20A sustained and 40A current peaks. Thanks to it's extremely low RDS on, basically no power is wasted, and it trips the circuit in a matter of microseconds when too much current is detected, which makes it extremely useful for a drone full of sensitive circuitry, compared to a traditional wire fuse. Followed by that is an INA226 chip, this aims to precisely monitor the current consumption of the entire drone, helping to predict flight times and efficiently manage battery health, through the use of a shunt resistor sized to the expected current draw. The current planes are optimized to make proper use of the different outer and inner layer thicknesses and properly distribute current with minimal heat up.

The other part of the bottom PCB houses the RF circuitry for the FPV system LED drivers togethor with power regulators that power the 12V rail for the LED drivers and the 5V rail for the FPV system and other 5V chips. The 12V power regulator (TPS55289) and the 5V regulator (TPS63070) both use the safest and most reliable layouts possible and are positioned to spatially divide the high ~32A currents on the bottom power distribution circuitry from the rest of the bottom board.

but also integrate sensors and control directly into the PCB and have PWM inputs/outputs for remote control input and ESC output. All with as small a footprint as possible to save space efficiently.

## Features:
- USB C power supply programming port for the RP2040
- 1s LiPo integration with built in charging
- 4.3" 40 pin TFT display support
- 5.8GHz video band
- Channel switch ability
- RSSI support
- No screw assembly

## PCB
This is the PCB, with L1 to L4 Layers, silkscreen for both layers and an all layers view

<img src=Pictures/Finish/PCB_L1.png alt="PCB Layer 1" width="300"/> <img src=Pictures/Finish/PCB_L2.png alt="PCB Layer 2" width="300"/> <img src=Pictures/Finish/PCB_L3.png alt="PCB Layer 3" width="300"/> 
<img src=Pictures/Finish/PCB_L4.png alt="PCB Layer 4" width="300"/> <img src=Pictures/Finish/PCB_Silkscreen_Top.png alt="PCB Slikscreen Top" width="300"/> <img src=Pictures/Finish/PCB_Silkscreen_Bottom.png alt="PCB Silkscreen Bottom" width="300"/> 


And here is the schematic

<img src=Pictures/Finish/top_schem.png alt="Schematic" width="800"/>
<img src=Pictures/Finish/bottom_schem.png alt="Schematic" width="800"/>

## Case and 3D renders
Here are also some 3D renders done in fusion to show off the case and it's functionality :)

<img src=Pictures/Renders/render1.png alt="Case render 1" width="300"/> <img src=Pictures/Renders/render2.png alt="Case render 2" width="300"/> <img src=Pictures/Renders/render3.png alt="Case render 3" width="300"/> 
<img src=Pictures/Renders/render4.png alt="Case render 4" width="300"/> <img src=Pictures/Renders/render5.png alt="Case render 5" width="300"/> <img src=Pictures/Renders/render6.png alt="Case render 6" width="300"/>
<img src=Pictures/Renders/render7.png alt="Case render 7" width="300"/> <img src=Pictures/Renders/render8.png alt="Case render 8" width="300"/> <img src=Pictures/Renders/render9.png alt="Case render 9" width="300"/> 

And last but not least this is a render of the PCB itself:

<img src=Pictures/Renders/render10.png alt="PCB render" width="300"/>

## Firmware Overview
I've included some basic firmware (**untested**) that should get the TFT display working out of the box with a quick upload from arduino IDE

## BOM:
The BOM list for the actual PCB components is in the BOM folder section and here is also a BOM list for all the other components I plan to buy for this:


- PCB             $198.94		
- Antenna         $7.77      https://www.aliexpress.com/item/1005006153683653.html		
- 90 degree SMA   $4.51      https://www.aliexpress.com/item/1005005665998117.html						
- RX5808 module   $33.54     https://www.ebay.co.uk/itm/264574341923						                (This module is not cheaper on AliExpress)
- Battery         $9.39      https://www.ebay.co.uk/itm/396305473532?var=665862385939						(I don't feel safe buying batteries off Ali)
											
Total			      $246.38		

<img width="1083" height="141" alt="image" src="https://github.com/user-attachments/assets/e6ed5a2c-47a3-496e-8e86-db1041965be2" />


