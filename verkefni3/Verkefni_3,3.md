- [Myndbandið fyrir verkefni 3, hluta 3](https://youtube.com/shorts/xhZYerKSQKs)

- [Myndbandið fyrir verkefni 3, hluta 3 með Neyðarrofi](https://youtube.com/shorts/9MJTgItG4-o)


```cpp
#include "vex.h"
using namespace vex;

// Motors
motor LeftMotor(PORT1, ratio18_1, false);
motor RightMotor(PORT10, ratio18_1, true);

// Controller
controller Controller1;

bool STOP = false;

void EmergencyStop() {
  STOP = true;
  LeftMotor.stop(brake);
  RightMotor.stop(brake);
}

const double TRACK_WIDTH = 27.0;
const double WHEEL_DIAMETER = 10.0;

double wheelCircumference() {
  return 3.14159 * WHEEL_DIAMETER;
}

void driveCircle(double radius_cm) {

  double innerRadius = radius_cm - (TRACK_WIDTH / 2.0);
  double outerRadius = radius_cm + (TRACK_WIDTH / 2.0);

  double innerCirc = 2 * 3.14159 * innerRadius;
  double outerCirc = 2 * 3.14159 * outerRadius;

  double wheelCirc = wheelCircumference();
  double innerRot = innerCirc / wheelCirc;
  double outerRot = outerCirc / wheelCirc;

  double speedRatio = innerRot / outerRot;

  RightMotor.setVelocity(50, percent);
  LeftMotor.setVelocity(50 * speedRatio, percent);

  RightMotor.spinFor(forward, outerRot, rev, false);
  LeftMotor.spinFor(forward, innerRot, rev, false);

  while ((RightMotor.isSpinning() || LeftMotor.isSpinning()) && !STOP) {
    wait(10, msec);
  }

  LeftMotor.stop(brake);
  RightMotor.stop(brake);
}

int main() {

  vexcodeInit();

  // Emergency stop  Button X
  Controller1.ButtonX.pressed(EmergencyStop);

  double radius = 50.0;

  for (int i = 0; i < 3 && !STOP; i++) {
    driveCircle(radius);
    radius += 5.0;
    wait(1, seconds);
  }

  return 0;
}
```
