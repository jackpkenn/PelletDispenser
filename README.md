# Pellet Dispenser

This food pellet dispenser was developed by Dylan Guenther and Jack Kennedy at Dr. Andrew Maurer's Lab in the McKnight Brain Institute at the University of Florida. The dispenser consists of four separate 3D printed components and uses a bipolar stepper motor to dispense individual 45mg food pellets. An infrared break-beam attached to one of the external interrupt pins is used to keep track of how many pellets have been dispensed. The dispenser is designed to be operated with an Arduino using an Adafruit motor driver shield and can be adapted to any other micro-controller and H-bridge.

## Parts List
* __4x - M3x0.5 hex nut__ (McMaster-Carr part number: [90695A033](http://www.mcmaster.com/#90695A033))
* __2x - M3 35mm socket cap screw__ (McMaster-Carr part number: [92095A201](http://www.mcmaster.com/#92095A201))
* __2x - M3 50mm socket cap screw__ (McMaster-Carr part number: [92095A475](http://www.mcmaster.com/#92095A475))
* __1x - 3mm IR break-beam__ (Adafruit part number: [2167](https://www.adafruit.com/product/2167))
* __1x - Adafruit Motor Shield v2__ (Adafruit part number: [1438](https://www.adafruit.com/product/1438))
* __1x - Nema 14 bipolar stepper motor__ (Pololu part number: [1208](https://www.pololu.com/product/1208))
* __1x - Arduino Uno R3__ (Adafruit part number: [50](https://www.adafruit.com/products/50))

## Code Examples
Break-beams are interrupt driven to keep track of the number of pellets dispensed. The Adafruit motor driver library was used to handle the motor control.
```cpp
void beamBroken_isr(){
  noInterrupts();
  pells--;
  disp++;
  broken = true;
  Serial.println("Beam Broken");
  if(disp == 5){
    disp = 0;
    rot = !rot;
  }
  interrupts();
}
```
The dispenser is set to switch the direction of the rotor after every five steps. This was found to reduce jamming and crushed pellets. A delay of 100ms was added after each step to help the break-beams to detect individual pellets.
```cpp
if (f == "disp" || f == "Disp") {
  pellsR = param;
  Serial.print("Dispensing ");
  Serial.print(param);
  Serial.println(" pellets");
  while(pells>0){
    if(rot == false){
      myMotor->step(25, FORWARD, SINGLE);
      delay(100);
    }
    else if(rot == true){
      myMotor->step(25, BACKWARD, SINGLE);
      delay(100);
    }
  }
}
```
## License and Disclaimer
This software is open-sourced under the MIT Licence (see ```license.txt``` for the full license).

The software is provided as is. It might work as expected - or not. Just don't blame us.
