```cpp

Hluti_2

/*----------------------------------------------------------------------------*/
/*    Braut eins og á mynd (50 cm hvert strik)                               */
/*----------------------------------------------------------------------------*/

// ---- START VEXCODE CONFIGURED DEVICES ----
// Robot Configuration:
// [Name]               [Type]        [Port(s)]
// Drivetrain           drivetrain    1, 10, D
// ---- END VEXCODE CONFIGURED DEVICES ----

#include "vex.h"

using namespace vex;

int main() {
  // DO NOT REMOVE
  vexcodeInit();

  // Mæli með að hafa ekki of mikinn hraða
  Drivetrain.setDriveVelocity(40, percent);
  Drivetrain.setTurnVelocity(30, percent);

  const int SEG = 500; // 50 cm í mm

  // Byrjar við Start og snýr til hægri

  // 1: beint til hægri
  Drivetrain.driveFor(forward, SEG, mm);

  // 2: niður
  Drivetrain.turnFor(right, 90, degrees);
  Drivetrain.driveFor(forward, SEG, mm);

  // 3: til hægri
  Drivetrain.turnFor(left, 90, degrees);
  Drivetrain.driveFor(forward, SEG, mm);

  // 4: upp
  Drivetrain.turnFor(left, 90, degrees);
  Drivetrain.driveFor(forward, SEG, mm);

  // 5: til hægri (efri “armur” krossins)
  Drivetrain.turnFor(right, 90, degrees);
  Drivetrain.driveFor(forward, SEG, mm);

  // 6: niður
  Drivetrain.turnFor(right, 90, degrees);
  Drivetrain.driveFor(forward, SEG, mm);

  // 7: til hægri (miðja hægra megin)
  Drivetrain.turnFor(left, 90, degrees);
  Drivetrain.driveFor(forward, SEG, mm);

  // 8: niður (langt niður hægra megin)
  Drivetrain.turnFor(right, 90, degrees);
  Drivetrain.driveFor(forward, SEG, mm);

  // 9: til vinstri (neðri hluti)
  Drivetrain.turnFor(right, 90, degrees);
  Drivetrain.driveFor(forward, SEG, mm);

  // 10: niður
  Drivetrain.turnFor(left, 90, degrees);
  Drivetrain.driveFor(forward, SEG, mm);

  // 11: til vinstri
  Drivetrain.turnFor(right, 90, degrees);
  Drivetrain.driveFor(forward, SEG, mm);

  // 12: upp
  Drivetrain.turnFor(right, 90, degrees);
  Drivetrain.driveFor(forward, SEG, mm);

  // 13: til vinstri
  Drivetrain.turnFor(left, 90, degrees);
  Drivetrain.driveFor(forward, SEG, mm);

  // 14: niður
  Drivetrain.turnFor(left, 90, degrees);
  Drivetrain.driveFor(forward, SEG, mm);

  // 15: til vinstri inn í “End”
  Drivetrain.turnFor(right, 90, degrees);
  Drivetrain.driveFor(forward, SEG, mm);

  return 0;
}
