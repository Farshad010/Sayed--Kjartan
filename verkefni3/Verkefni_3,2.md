```cpp
kóði fyrir verkefni-3 part -2
/*----------------------------------------------------------------------------*/
/*    Braut eins og á mynd (50 cm hvert strik) – með while lykkju           */
/*----------------------------------------------------------------------------*/

// ---- START VEXCODE CONFIGURED DEVICES ----
// Robot Configuration:
// [Name]               [Type]        [Port(s)]
// Drivetrain           drivetrain    1, 10, D
// ---- END VEXCODE CONFIGURED DEVICES ----

#include "vex.h"

using namespace vex;

// --- NÝTT: breyta og fall fyrir bumper stop ------------------
bool STOP = false;                     // merkir hvort stoppa eigi

void BumperPressed() {                 // keyrir þegar ýtt er á bumper
  STOP = true;                         // segjum “nú á að stoppa”
  Drivetrain.stop();                   // stöðvum bílinn strax
}
// --------------------------------------------------------------

int main() {
  // DO NOT REMOVE
  vexcodeInit();

  // NÝTT: tengjum bumper event við fallið okkar
  BumperA.pressed(BumperPressed);

  // Smá bið svo allt sé tilbúið (sérstaklega ef þið notið gyro/inertial)
  wait(2, seconds);

  // Stillum hraða
  Drivetrain.setDriveVelocity(40, percent);
  Drivetrain.setTurnVelocity(30, percent);

  const int SEG = 500; // 50 cm í mm

  // Beygjur: R = right, L = left
  // Þessi röð fylgir brautinni á myndinni
  char turns[] = {
    'R','L','L','R','R','L','R','R','L','R','R','L','L','R'
  };

  int i = 0;
  int steps = sizeof(turns) / sizeof(turns[0]);

  // Fyrsti kaflinn: frá "Start" beint til hægri
  Drivetrain.driveFor(forward, SEG, mm);

  // while lykkja: tekur eina beygju + keyrir beint, aftur og aftur
  // NÝTT: bætum !STOP við skilyrðið
  while (i < steps && !STOP) {

    // Veljum hægri/vinstri út frá turns[i]
    if (turns[i] == 'R') {
      Drivetrain.turnFor(right, 90, degrees);
    } else {
      Drivetrain.turnFor(left, 90, degrees);
    }

    // NÝTT: ef bumper var ýtt á meðan við vorum að snúa
    if (STOP) {
      break;
    }

    // Keyrum næsta 50 cm kafla
    Drivetrain.driveFor(forward, SEG, mm);

    // hækkum teljarann
    i++;
  }

  // búið – róbotinn endar í “End” eða stoppar þegar bumper er ýtt
  Drivetrain.stop();   // gott að hafa í lokin

  return 0;
}
```
