# TrinTheremin (Version 1)
***
##### About the Circuit
TrinTheremin, like the Sparkfun <a href="https://www.youtube.com/watch?v=2zZkeeQr49g" target="_blank">Digital Sandboxes</a> used in CS1/CS2, is an all in one programmable <a href="https://learn.sparkfun.com/tutorials/what-is-a-circuit" target="_blank">circuit</a> that allows the user to process/control different inputs/outputs on the board.   Its <a href="http://www.atmel.com/Images/Atmel-7766-8-bit-AVR-ATmega16U4-32U4_Datasheet.pdf" target="_blank">ATMEGA microcontroller chip</a> has its own CPU (central processing unit; the brain of the device), flash memory (where your program is stored), SRAM (where the sketch creates and manipulates variables while running), and <a href="http://www.atmel.com/Images/Atmel-7766-8-bit-AVR-ATmega16U4-32U4_Datasheet.pdf" target="_blank">EEPROM</a> (NON-VOLATILE storage space programmers can access/use; it will keep its stored data even without electrical power (when the unit is off), as opposed to volatile memory (like SRAM) that would lose its data without electrical power).   While you probably won't need to use any EEPROM for this project (although feel free to do so if you have a use for it), you will constantly be using the CPU and SRAM (as well as the flash memory, where your program lives).   See this chart for how much memory the microcontroller on the TrinTheremin (the ATMEGA 32u4) has of each main type (there are other memory types onboard that I won't go into):

| Memory Type:  | Amount of storage (bytes): |
| ------------- | ------------- |
| Flash  | 32000  |
| SRAM  | 2500  |
| EEPROM  | 1000  |

The flash is not all available, however as the board's "bootloader" (the software that allows you to upload Arduino code) takes up about 4kB, or 4000 bytes, leaving about 28kB usable for storing code.   The bootloader is already installed on all the boards.

Beyond the chip (and its dedicated hardware such as its crystal and resistors and capacitors), all the circuits on this board should be pretty familiar (possibly with the exception of the speaker's circuit).   See the <a href="https://github.com/TrinitySchoolComputerScience/TrinTheremin/tree/master/Circuit%20Design/Design%20Pictures" target="_blank">circuit design pictures</a> folder.   On the <a href="https://github.com/TrinitySchoolComputerScience/TrinTheremin/tree/master/Circuit%20Design/Design%20Pictures/Schematic/Close%20Ups/Left%20Side%20(Arduino)" target="_blank">left side</a> of the schematic is all the dedicated Arduino hardware, however the <a href="https://github.com/TrinitySchoolComputerScience/TrinTheremin/tree/master/Circuit%20Design/Design%20Pictures/Schematic/Close%20Ups/Right%20Side%20(Peripherals)" target="_blank">right side</a> shows the circuits for all the peripherals (the buttons, switch, light sensor, LEDs, etc).   Take a look at the right side to see how simple (and common) the circuits behind the peripherals are.

---

##### Usage
The TrinTheremin was purpose built for Trinity's CS2 and Advanced CS1 programs (specifically the physical computing and digital logic units, respectively), although it could be used in a wide variety of applications that use inputs and outputs involving sound and light.

###### Battery
The TrinTheremin's battery should be connected to the 9v battery connector on the bottom of the board and stored in the clip under the slider (also on the bottom of the board).  

(Picture)

Like any device, the TrinTheremin will eventually run out of battery.   Trinity's collection of Apex batteries have a 595mAh capacity.   Milliamp hours (mAh) are a unit of electrical charge (one milliamp hour is about 3.6 coulombs, a unit commonly taught in Trinity physics that is generally used when working with static electricity rather than circuits).   This means that the batteries can supply 595 mA of current for one hour. The TrinTheremin draws about 400 mA when the speaker is on at full volume so at minimum the TrinTheremin will run for about 1.5 hours (with everything running and its speaker on at full volume) continuously.   If you do not have the speaker on the device draws only about 25 mA, which will yield about a day of continuous use (so do not feel the need to turn off the device while programming it as having it on does not use up much battery life) To more accurately determine how much battery life the device has left (and without keeping track of how much time you have used it for), you can measure the remaining battery life with a multimeter (located in the top drawer in the drawers by the door in the computer science lab -- ask Mr. Rose or Mr. Gohde if you are unsure where this is) and a few quick calculations.


![Alt text](https://www.powerstream.com/z/9v-100ma-discharge-tests.png)

First set the multimeter to measure DC voltage and make sure the leads are connected to the right ports (red to positive (+) and black to negative (-, sometimes referred to as COM) -- see this tutorial on multimeters.   Then measure the voltage of the battery (touching the red lead of the multimeter to the positive terminal of the battery (marked by the + symbol on the battery) and the black lead to the negative terminal (marked by the -).   Here is a chart that shows the battery life estimates for an alkaline cell:

| Alkaline Voltage Level and Range (V):  | Usability: |
| ------------- | ------------- |
| 1.50  | New and unused  |
| 1.35 – 1.49  | Good  |
| 1.20 – 1.34  | Average  |
| 1.10 – 1.19  | Low  |
| 0.90 – 1.09  | Very Low  |
| < 0.90  | Dead – Discard!  |

Given that alkaline 9v batteries (Trinity's Apex 9v batteries are alkaline) are 6 alkaline cells, we can multiply through these ranges to find voltages that apply to the reading from the 9v battery:

| 9v Battery Voltage Level and Range (V):  | Usability: |
| ------------- | ------------- |
| 9  | New and unused  |
| 8.1 – 8.94  | Good  |
| 7.2 – 8.04  | Average  |
| 6.6 – 7.14  | Low (Unusuable below about 6.75v)|
| 5.4 – 6.54  | Very Low (Unusable)|
| < 5.4  | Dead – Discard! (Unusable)|

There is one more factor to consider, though, and that is that the TrinTheremin needs about 6.9v to run.   This is because the voltage regulator (a chip on the board that takes the voltage being supplied by the battery and regulates it at a constant 5v for the circuit to use) needs at least a 6.75v to be able to output 5v as its supposed to.   This extra 1.75 volts it needs is called the "dropout voltage" -- this chart from the chip's <a href="http://www.st.com/content/ccc/resource/technical/document/datasheet/41/4f/b3/b0/12/d4/47/88/CD00000444.pdf/files/CD00000444.pdf/jcr:content/translations/en.CD00000444.pdf" target="_blank">datasheet</a> was how I found that the dropout voltage is 1.75 volts:

![](README%20Figures%20(ignore)/dropout.png?raw=true)



###### Programming
The TrinTheremin has the same chip as an <a href="https://store.arduino.cc/usa/arduino-micro">Arduino Micro</a>, so it will behave almost identically to that board.   Important caveats to note:
* When using Serial (rx/tx) on the "COM breakout" you must use Serial1 instead of Serial.   Since this chip has its own embedded usb circuit, Serial.print will print to the Arduino console as usual, however Serial1 will reference the hardware serial ports (rx/tx). So you would use Serial1.begin(9600), Serial1.print("text"), etc to use the HARDWARE serial.   On an Arduino Uno, for example, which has an external USB circuit that is connected inline with the hardware serial ports, the Serial command references both the hardware and the usb serial (so Serial.print will output serial on the tx port but also print to the Arduino developer's console)

* Because the chip has built in USB, when you plug it into your computer it will be recognized as a mouse or keyboard (and you can actually use it as such -- more on that <a href="https://www.arduino.cc/en/Reference/MouseKeyboard">here</a>).

* When you upload your code, select "Arduino Micro" under boards, and the appropriate port under ports.



-----
##### Plans for future versions
If any future versions of the TrinTheremin are produced these are some of the goals that did not get included in this initial version:

* _A more efficient amplifier_ -- the amplifier circuit on the TrinTheremin right now is very simple, works with the ToneAC library, and has pretty good quality sound, however in future versions a more sophisticated amplifier circuit might be combined with a possible TrinTheremin Arduino library to keep the programming simplicity of ToneAC while improving quality and GREATLY improving efficiency (battery life would be improved immensely).

* _A voltage divider circuit_ -- this is one of the easiest (and also most helpful) changes that could be introduced in future versions.   Essentially this would take the voltage of the battery, divide it in two (9v becomes 4.5, 7 becomes 3.5, etc), and feed it to an analog input (0v-5v yields a 0-1023 reading) on the microcontroller.   This would allow the programmer to measure the voltage of the battery.   With this they could create an integrated battery life check that would alert the user if the battery were low.

* _Reduced cost_ -- right now the TrinTheremin costs about 25$ per board to produce, however with hardware changes and different production methods, this could potentially be significantly reduced.   For reference an Arduino Micro costs about 22$ (with no inputs or outputs built in)

* _Expanded documentation and a more seamless user interface -- maybe a website?_ -- Right now trintherem.in redirects to this github repository, however in the future a website dedicated to the TrinTheremin could be produced.

----




