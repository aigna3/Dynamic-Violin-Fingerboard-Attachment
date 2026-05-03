# Dynamic-Violin-Fingerboard-Attachment

## 2026-02-09
Discussing ideas in our proposal. I suggested that latency must be a consideration and that it must be consistent to within 7ms of each note. Otherwise, humans will notice those subtle changes and it will throw them off when playing the violin. I also suggested that the weight of the attachment should be less than half the weight of the violin itself which is 500g. Otherwise, it will greatly effect the playing.



## 2026-02-16
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




## 2026-02-22
Brainstorming ideas for distributing voltage throughout the circuits. A voltage regulator was suggested and so was a voltage divider. The circuit design for a voltage divider will add some complexity but is more cost-efficient. We wanted to use common enough resistance values where the ratio of the two resistances is 2:1. We intend on using a 9V battery for testing. Given this information, we can calculate the voltages going through the LED and the Microcontroller. Let’s assume R1 is twice the resistance of R2. <br>
**VLED = VBattery * (R1 / (R1+R2)) = 9V * (2/(2+1)) = 9V * (2/3) = 6V** <br>
**VMicrocontroller = VBattery * (R2 / (R1+R2)) = 9V * (1/(2+1)) = 9V * (1/3) = 3V** <br>
These are not the typical voltage levels of each of the two components. According to the datasheets however, the addressable LEDs can handle voltages ranging from 3V to 7V so 6V falls within this range. Likewise, the ESP32 microcontroller can handle voltages between 3V and 3.6V so the voltage divider implementation can work despite the voltages not being quite the desired values.



## 2026-02-27
Discussed the idea of using flexible PCBs to fit on the violin fretboard. The only issue would be fitting it with the fretboard correctly since the frets aren't evenly spaced on a violin.



## 2026-03-06
Designing sketches for the enclosure. There would need to be holes for potentiometers, buttons, LED's, the LCD screen, Vin, Switch, Voltage Regulators.

![Sketch](https://annual-gold-gutqnh4c4m.edgeone.app/MachineShop.jpeg)




## 2026-03-10
Figuring out how to upload MIDI files onto an ESP32 programmer. Our plan is to upload the MIDI files onto the ESP32's file system called LittleFS, and then the files are parsed or serialized and is played via USB MIDI. Libraries considered include `ESP32MidiPlayer` and `Arduino-ESP32FS-plugin`.

