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



## 2026-02-22
Brainstorming ideas for distributing voltage throughout the circuits. A voltage regulator was suggested and so was a voltage divider. The circuit design for a voltage divider will add some complexity but is more cost-efficient. We wanted to use common enough resistance values where the ratio of the two resistances is 2:1. We intend on using a 9V battery for testing. Given this information, we can calculate the voltages going through the LED and the Microcontroller. Let’s assume R1 is twice the resistance of R2. <br>
**VLED = VBattery * (R1 / (R1+R2)) = 9V * (2/(2+1)) = 9V * (2/3) = 6V** <br>
**VMicrocontroller = VBattery * (R2 / (R1+R2)) = 9V * (1/(2+1)) = 9V * (1/3) = 3V** <br>
These are not the typical voltage levels of each of the two components. According to the datasheets however, the addressable LEDs can handle voltages ranging from 3V to 7V so 6V falls within this range. Likewise, the ESP32 microcontroller can handle voltages between 3V and 3.6V so the voltage divider implementation can work despite the voltages not being quite the desired values.




## 2026-03-10
Figuring out how to upload MIDI files onto an ESP32 programmer. Our plan is to upload the MIDI files onto the ESP32's file system called LittleFS, and then the files are parsed or serialized and is played via USB MIDI. Libraries considered include `ESP32MidiPlayer` and `Arduino-ESP32FS-plugin`.

