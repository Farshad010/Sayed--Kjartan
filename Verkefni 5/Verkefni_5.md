## main.cpp
```cpp
#include "vex.h"

using namespace vex;

float threshold = 30;

// Task that prints robot position relative to the line
int displayStatus() {
  while (true) {
    int L = LineLeft.reflectivity();
    int M = LineMid.reflectivity();
    int R = LineRight.reflectivity();

    Brain.Screen.clearScreen();
    Brain.Screen.setCursor(1, 1);

    if (M > threshold) {
      Brain.Screen.print("On line");
    }
    else if (L > threshold) {
      Brain.Screen.print("Right of line");
    }
    else if (R > threshold) {
      Brain.Screen.print("Left of line");
    }
    else {
      Brain.Screen.print("Searching for line");
    }

    wait(100, msec);
  }
  return 0;
}


int main() {
  vexcodeInit();

  // display
  task statusTask(displayStatus);

  while (true) {
    int L = LineLeft.reflectivity();
    int M = LineMid.reflectivity();
    int R = LineRight.reflectivity();

    if (M > threshold) {
      // Go straight
      LeftMotor.spin(forward, 10, pct);
      RightMotor.spin(forward, 10, pct);
    }
    else if (L > threshold) {
      // Turn left
      LeftMotor.spin(reverse, 10, pct);
      RightMotor.spin(forward, 10, pct);
    }
    else if (R > threshold) {
      // Turn right
      LeftMotor.spin(forward, 10, pct);
      RightMotor.spin(reverse, 10, pct);
    }
    else {
      // Search for line
      LeftMotor.spin(reverse, 5, pct);
      RightMotor.spin(forward, 5, pct);
    }

    wait(20, msec);
  }
}
```


## Robot-config.cpp
```cpp
// robot-config.cpp
#include "vex.h"

using namespace vex;
using signature = vision::signature;
using code = vision::code;

brain Brain;

// Line sensors
line LineRight = line(Brain.ThreeWirePort.F);
line LineMid   = line(Brain.ThreeWirePort.G);
line LineLeft  = line(Brain.ThreeWirePort.H);

// Ultrasonic sensor
sonar Ultrasonic = sonar(Brain.ThreeWirePort.A);

// Motors
motor LeftMotor = motor(PORT1, ratio18_1, false);
motor RightMotor = motor(PORT10, ratio18_1, true);

void vexcodeInit(void) {
  // nothing to initialize
}
```

## Robot-config.h
```cpp
// robot-config.h
using namespace vex;

extern brain Brain;

// Line sensors
extern line LineRight;
extern line LineMid;
extern line LineLeft;

// Ultrasonic
extern sonar Ultrasonic;

// Motors
extern motor LeftMotor;
extern motor RightMotor;

void vexcodeInit(void);
```
