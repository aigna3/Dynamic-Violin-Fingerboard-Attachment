# Dynamic-Violin-Fingerboard-Attachment

## 2026-02-22
Brainstorming ideas for distributing voltage throughout the circuits. A voltage regulator was suggested and so was a voltage divider. The circuit design for a voltage divider will add some complexity but is more cost-efficient. We wanted to use common enough resistance values where the ratio of the two resistances is 2:1. We intend on using a 9V battery for testing. Given this information, we can calculate the voltages going through the LED and the Microcontroller. Let’s assume R1 is twice the resistance of R2.
**VLED = VBattery * (R1 / (R1+R2)) = 9V * (2/(2+1)) = 9V * (2/3) = 6V**
**VMicrocontroller = VBattery * (R2 / (R1+R2)) = 9V * (1/(2+1)) = 9V * (1/3) = 3V**
These are not the typical voltage levels of each of the two components. According to the datasheets however, the addressable LEDs can handle voltages ranging from 3V to 7V so 6V falls within this range. Likewise, the ESP32 microcontroller can handle voltages between 3V and 3.6V so the voltage divider implementation can work despite the voltages not being quite the desired values.

