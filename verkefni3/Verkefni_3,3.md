- [Myndbandið fyrir verkefni 3, hluta 3](https://youtube.com/shorts/xhZYerKSQKs)


```cpp
#include "vex.h"

using namespace vex;
bumper BumperA = bumper(Brain.ThreeWirePort.A);

// Motors
motor LeftMotor(PORT1, ratio18_1, false);
motor RightMotor(PORT10, ratio18_1, true);

// Breidd milli hjóla (cm)
const double TRACK_WIDTH = 27.0;

// Þvermál hjóls (cm)
const double WHEEL_DIAMETER = 10.0;

void BumperPressed() {
  Brain.programStop();
}
// Reiknar ummál hjóls
double wheelCircumference() {
  return 3.14159 * WHEEL_DIAMETER;
}

// Fall sem lætur vélmennið keyra í hring með gefnum radíus
void driveCircle(double radius_cm) {

  // --- Reiknum innri og ytri radíus ---
  double innerRadius = radius_cm - (TRACK_WIDTH / 2.0);
  double outerRadius = radius_cm + (TRACK_WIDTH / 2.0);

  // --- Ummál hringja ---
  double innerCirc = 2 * 3.14159 * innerRadius;
  double outerCirc = 2 * 3.14159 * outerRadius;

  // --- Snúningar hjóla ---
  double wheelCirc = wheelCircumference();
  double innerRot = innerCirc / wheelCirc;
  double outerRot = outerCirc / wheelCirc;

  // --- Hraðahlutfall milli hjóla ---
  double speedRatio = innerRot / outerRot;

  // --- Setjum hraða ---
  RightMotor.setVelocity(50, percent);          // Ytra hjól
  LeftMotor.setVelocity(50 * speedRatio, percent); // Innra hjól

  // --- Keyrum hring ---
  RightMotor.spinFor(forward, outerRot, rev, false);
  LeftMotor.spinFor(forward, innerRot, rev, true);
}
double radius = 50.0;
int main() {
  // Keyrum 3 hringi með vaxandi radíus
  BumperA.pressed(BumperPressed);

    for (int i = 0; i < 3; i++) {
      driveCircle(radius);
      radius += 5.0;   // Stækkum radíus um 5 cm
      wait(1, seconds);
    }
  double radius = 50.0;

  for (int i = 0; i < 3; i++) {
    driveCircle(radius);
    radius += 5.0;   // Stækkum radíus um 5 cm
    wait(1, seconds);
  }
}
```
