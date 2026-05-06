# Dynamic-Violin-Fingerboard-Attachment

## Important Note: My original group was not allowed to continue so I had to join another one

## Week 4: 2026-02-09 - 2026-02-13
Discussing ideas in our proposal. I suggested that latency must be a consideration and that it must be consistent to within 7ms of each note. Otherwise, humans will notice those subtle changes and it will throw them off when playing the violin. I also suggested that the weight of the attachment should be less than half the weight of the violin itself which is 500g. Otherwise, it will greatly effect the playing.



## Week 5: 2026-02-16 - 2026-02-20
Worked on requirements and verifications needed for success of the project. There are three subsystems: power, input, and output, and each have three requirements.

**Power Subsystem**

| Requirements      | Verification |
| ----------- | ----------- |
| Must supply a consistent voltage to the ESP32 microcontroller that is within 3.3V +- 5% and a current of 250mA      | <ul> <li>Connect voltage supply to another subsystem</li> <li>Connect positive lead of multimeter to the microcontroller and the negative lead of the multimeter to ground</li> <li>Check the multimeter to make sure that the voltage that goes through the microcontroller deviates at most 5% from 3.3V</li> Increase the voltage supply to the other subsystem and make sure that the voltage that goes through the microcontroller still says within 5% of 3.3V</li> </ul>      |
| Must supply a consistent voltage to the LEDs and LCD display that is within 5V +- 5% and a current of 500mA   | <ul> <li>Connect voltage supply to another subsystem</li> <li>Connect positive lead of multimeter to the LCD display and LEDs and the negative lead of the multimeter to ground</li> <li>Check the multimeter to make sure that the voltage that goes through the the LCD display and LEDs deviates at most 5% from 5V</li> <li>Increase the voltage supply to the other subsystem and make sure that the voltage that goes through the LCD display and LEDs still says within 5% of 5V</li> </ul>        |


**Input Subsystem**

| Requirements      | Verification |
| ----------- | ----------- |
| The input subsystem should be able to detect finger placement of the user under 2mm +- 0.1mm     | <ul> <li>Use a ruler to measure the distance between a finger and a certain location on the membrane potentiometer</li> <li>Slowly inch finger towards desired location and stop moving finger once the potentiometer detects you’re in the “correct” place</li> <li>Measure distance between finger and ideal location and see if it is within 2mm. Detecting correct finger placement when the finger is up to 2.1mm away from the desired location is tolerable</li> </ul>      |
| ESP32 should take input via a USB port and download desired MIDI file within 5 seconds +- 0.5 seconds   | <ul> <li>Use a stopwatch and start it when the file is transmitted and stop it once the programmer receives the file and provides the notification that the file has been downloaded. This should take under 5 seconds, but taking as long as 5.5 seconds is acceptable</li> </ul>        |


**Output Subsystem**

| Requirements      | Verification |
| ----------- | ----------- |
| Latency between microcontroller and LEDs must be self-consistent within a variation tolerance of 7ms +- 1ms     | <ul> <li>This will be very difficult to measure with a stopwatch so we cannot use completely quantitative measurements</li> <li>Humans recognize latency between notes when it is above 7ms so as long as there is a consistent tempo between successive notes and we cannot notice any tempo changes then it meets the requirement</li> </ul>    |
| Sheet music to addressable LED mapping should be 98% +- 1% accurate   | <ul> <li>We provide the ESP32 sheet music for five different violin pieces, so 5 tests</li> <li>Between all those tests, at most 1-2 incorrect LEDs should light up.</li></ul>        |
| Changes to user-input settings should be reflected on the LCD display within 1 second +- 0.5 seconds | <ul> <li>Use a stopwatch, start it when user confirms a change in input settings and stop it when the changes are reflected on the LCD display</li> <li>The changes should be reflected within 1 second although a delay of up to 1.5 seconds is acceptable</li> </ul>  |




## Week 6: 2026-02-23 - 2026-02-27
Brainstorming ideas for distributing voltage throughout the circuits. A voltage regulator was suggested and so was a voltage divider. The circuit design for a voltage divider will add some complexity but is more cost-efficient. We wanted to use common enough resistance values where the ratio of the two resistances is 2:1. We intend on using a 9V battery for testing. Given this information, we can calculate the voltages going through the LED and the Microcontroller. Let’s assume R1 is twice the resistance of R2. <br>
**VLED = VBattery * (R1 / (R1+R2)) = 9V * (2/(2+1)) = 9V * (2/3) = 6V** <br>
**VMicrocontroller = VBattery * (R2 / (R1+R2)) = 9V * (1/(2+1)) = 9V * (1/3) = 3V** <br>
These are not the typical voltage levels of each of the two components. According to the datasheets however, the addressable LEDs can handle voltages ranging from 3V to 7V so 6V falls within this range. Likewise, the ESP32 microcontroller can handle voltages between 3V and 3.6V so the voltage divider implementation can work despite the voltages not being quite the desired values.



## Week 7: 2026-03-02 - 2026-03-06
Discussed the idea of using flexible PCBs to fit on the violin fretboard. The only issue would be fitting it with the fretboard correctly since the frets aren't evenly spaced on a violin. Our TA also suggested using bluetooth or wireless transfer of files to the ESP32 instead of a USB-C. We have considered it, but we will move forward with the USB-C since it is much easier to implement. I also designed sketches for the enclosure. There would need to be holes for potentiometers, buttons, LED's, the LCD screen, Vin, Switch, Voltage Regulators.

![Sketch](https://annual-gold-gutqnh4c4m.edgeone.app/MachineShop.jpeg)




## Week 8: 2026-03-09 - 2026-03-13
Figuring out how to upload MIDI files onto an ESP32 programmer. Our plan is to upload the MIDI files onto the ESP32's file system called LittleFS, and then the files are parsed or serialized and is played via USB MIDI. Libraries considered include `ESP32MidiPlayer` and `Arduino-ESP32FS-plugin`.



## Week 9: 2026-03-23 - 2026-03-27
We abandoned working with the machine shop since they suggested drilling a hole in the violin which would not work for us. We are instead considering building our own enclosure which would go under the violin.





## Week 10: 2026-03-30 - 2026-04-03
Developing accuracy code at the moment. I am using Code2Flow which is a website to generate flowcharts using pseudocode. The accuracy code is the bulk of the software in this project. The idea involves calculating the correct notes against the total notes played and figuring out the accuracy percentage.

![Chart](https://extra-violet-k6b4etsybo.edgeone.app/Pseudocode&Flowchart.png)

There are two variables involved. The number of correct notes and total notes in the piece. Only total notes gets incremented, if the player plays a note incorrectly. Both variables get incremented if the player plays correctly. At the end accuracy percentage is calculated by dividing the correct notes by total notes and multiplying by 100.




## Week 11: 2026-04-06 - 2026-04-10
Developing code to determine which note is correct. Since it is touch-based, it would be difficult to determine whether a user is playing correctly first time so it is time-based. I utilized the millis() function to record the time that the note is loaded onto LEDs and give the user within 2 seconds to play the correct note. If the user finds the correct potentiometer placement for each note within that time interval, then it will be marked as correct. I am also looking at optical recognition software applications on GitHub we could possibly use to convert sheet music into MIDI or binary files. This is a reach goal, so it can possibly be attempted once the required goals have been met.




## Week 12: 2026-04-13 - 2026-04-17
We ran into a problem during the soldering process. We realized that the USB-C connector on the PCB was not designed properly so reconsidered the idea of using wireless file transfer during the TA meeting. Through discussion, we did learn that that we could upload the files from the dev board onto the ESP32 on the PCB instead, avoiding the need for wireless transfer. Finally, there was an issue regarding the arrival of some parts. We had a 5V regulator but we did not have a 3.3V regulator which powered the ESP32 chip. Waiting for the parts prohibited us from beginning testing.



## Week 13: 2026-04-20 - 2026-04-24
Tested throughout the week. We ran into multiple issues. These were all documented by all of us when measuring each component with the multimeter. <br>

**Power issues:**
<ul>
<li>The power bank is outputting over 6.4 volts → the 3.3 voltage regulator is outputting 4.2. As a result, when the microcontroller was probed, the voltage was too high</li>
<li>The 5 volt voltage regulator is getting 6.4 in, 0 out</li>
<li>When red to button pin and black to 3.3gnd regulator, Vin got 1.36 and Vout got 0</li>
<li>It seems that the voltage regulators are acting more like resistors than regulators</li>
</ul>

**Flexible PCB issues:**
<ul>
<li>pwr/gnd pads ripped off (WEAK)</li>
<li>LED spacing sideways (between strings) was very tight. The inner LEDs aren’t getting the right power, we suspect some amount of bridging
Couldn’t get ahold of enough capacitors–needed a specific value of *surface mount* and a lot of them</li>
</ul>

**Misc issues:**
<ul>
<li>USB port never got mounted b/c the double row of pads was deemed too difficult to solder and we can do a certain amount wirelessly</li>
<li>No zener diodes → decided it was simpler to piggyback off the dev board (they were just for diagnostics)</li>
</ul>


