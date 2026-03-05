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
