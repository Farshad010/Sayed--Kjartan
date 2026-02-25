# main.cpp
```cpp
#include "vex.h"

using namespace vex;

// Distance tracking variables
double totalDistance = 0.0;
double wheelCircumference = 0.319; // meters for 4” wheel
double lastRotation = 0;

// Background task for distance + speed
int distanceTask() {
  Brain.Timer.reset();

  while (true) {
    double currentRotation = LeftDriveSmart.position(rev);
    double delta = (currentRotation - lastRotation) * wheelCircumference;

    totalDistance += delta;
    lastRotation = currentRotation;

    double timeSec = Brain.Timer.time(sec);
    double avgSpeed = (timeSec > 0) ? totalDistance / timeSec : 0;

    Brain.Screen.clearScreen();
    Brain.Screen.setCursor(1, 1);
    Brain.Screen.print("Distance: %.2f m", totalDistance);
    Brain.Screen.setCursor(2, 1);
    Brain.Screen.print("Avg Speed: %.2f m/s", avgSpeed);

    wait(100, msec);
  }
  return 0;
}

int main() {
  vexcodeInit();

  // Start distance tracking
  task t1(distanceTask);

  while (true) {

    // Emergency stop (Button B) 
    if (Controller1.ButtonB.pressing()) {
        LeftDriveSmart.stop();
        RightDriveSmart.stop();
        break;   // <-- instantly ends the program
    }

    // Light sensor control
    int lightValue = LightSensor.value(percentUnits::pct);

    Brain.Screen.setCursor(3, 1);
    Brain.Screen.print("Light: %d   ", lightValue);

    bool lightIsOn = (lightValue < 5);

    if (!lightIsOn) {
        LeftDriveSmart.stop();
        RightDriveSmart.stop();
        continue;
    }

    // Ultrasonic
    int dist = Ultrasonic.distance(distanceUnits::cm);

    Brain.Screen.setCursor(4, 1);
    Brain.Screen.print("Dist: %d   ", dist);

    if (dist > 0 && dist < 60) {
      LeftDriveSmart.stop();
      RightDriveSmart.stop();
      wait(500, msec);

      // Turn away from obstacle
      LeftDriveSmart.spin(forward, 50, pct);
      RightDriveSmart.spin(reverse, 50, pct);
      wait(500, msec);
    } 
    else {
      LeftDriveSmart.spin(forward, 50, pct);
      RightDriveSmart.spin(forward, 50, pct);
    }

    wait(20, msec);
  }
}

```
