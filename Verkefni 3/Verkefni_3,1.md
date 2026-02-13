- [Myndbandið fyrir verkefni 3, hluta 1](https://youtube.com/shorts/pI9PbSwMQXo)

# Fjarstýring – Takkaútskýring

## Akstur
- **Axis 3 (vinstri pinninn upp/niður):** Keyrir fram og aftur
- **Axis 1 (hægri pinninn vinstri/hægri):** Beygir

## Armur
- **R1:** Lyftir armi
- **R2:** Lætur arm síga

## Kló
- **L1:** Lokar kló
- **L2:** Opnar kló



## kóði fyrir verkefni 3 part 1

```cpp
#include "vex.h"

using namespace vex;

// Robot configuration code.

// Brain should be defined by default
brain Brain;

motor ClawMotor = motor(PORT3, ratio18_1, false);
motor ArmMotor = motor(PORT8, ratio18_1, false);
motor LeftMotor = motor(PORT1, ratio18_1, false);
motor RightMotor = motor(PORT10, ratio18_1, true);

controller Controller1 = controller(primary);

// Begin project code

void controller_L1_Pressed(){
  ArmMotor.spin(forward);  
  // Snýr arminum upp á meðan L1 er haldið inni

  while (Controller1.ButtonL1.pressing()) {
    wait(5, msec);  
    // Bíður smá stund svo lykkjan sé ekki of hröð
  }

  ArmMotor.stop();  
  // Stoppar arminn þegar takkanum er sleppt
}

void controller_L2_Pressed(){
  ArmMotor.spin(reverse);
  // Snýr arminum niður á meðan L2 er haldið inni

  while (Controller1.ButtonL2.pressing()) {
    wait(5, msec);
  }

  ArmMotor.stop();
}

void controller_R1_Pressed(){
  ClawMotor.spin(reverse);
  // Opnar klærnar á meðan R1 er haldið inni

  while (Controller1.ButtonR1.pressing()) {
    wait(5, msec);
  }

  ClawMotor.stop();
}

void controller_R2_Pressed(){
  ClawMotor.spin(forward);
  // Lokar klærnum á meðan R2 er haldið inni

  while (Controller1.ButtonR2.pressing()) {
    wait(5, msec);
  }

  ClawMotor.stop();
}

int main() {

  // Býr til "event" fyrir takkana — keyrir fall þegar takki er ýtt
  Controller1.ButtonL1.pressed(controller_L1_Pressed);
  Controller1.ButtonL2.pressed(controller_L2_Pressed);
  Controller1.ButtonR1.pressed(controller_R1_Pressed);
  Controller1.ButtonR2.pressed(controller_R2_Pressed);
  wait(15,msec);  
  // Smá töf svo events skráist rétt

  // Stillir mótora þannig að þeir haldi stöðu þegar þeir stoppa
  ArmMotor.setStopping(hold);
  ClawMotor.setStopping(hold);

  // Hraðastillingar fyrir arm og kló
  ArmMotor.setVelocity(60, percent);
  ClawMotor.setVelocity(30, percent);

  // Aðal lykkjan — stjórnar hjólunum með stýripinnum
  while(true){
    LeftMotor.setVelocity(Controller1.Axis3.position(), percent);  
    // Vinstri hjól fær gildi frá Axis3 (vinstri pinninn upp/niður)

    RightMotor.setVelocity(Controller1.Axis2.position(), percent); 
    // Hægri hjól fær gildi frá Axis2 (hægri pinninn upp/niður)

    LeftMotor.spin(forward);
    RightMotor.spin(forward);

    wait(5, msec);  
    // Smá töf til að minnka álag á örgjörvann
  }
}
```
