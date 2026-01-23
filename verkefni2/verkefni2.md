/*----------------------------------------------------------------------------*/
/*                                                                            */
/*    Module:       main.cpp                                                  */
/*    Description:  Drivetrain distance assignment                            */
/*                                                                            */
/*----------------------------------------------------------------------------*/

// ---- START VEXCODE CONFIGURED DEVICES ----
// Robot Configuration:
// Drivetrain drivetrain 1, 10, D
// ---- END VEXCODE CONFIGURED DEVICES ----

#include "vex.h"

using namespace vex;

// Constants (values that do not change)
const double START_DISTANCE = 0.5;   // meters
const double MAX_DISTANCE = 2.5;     // meters
const double DISTANCE_STEP = 0.5;    // meters

// Function to drive forward and then backward
void driveForwardAndBack(double distanceMeters) {
  Drivetrain.driveFor(forward, distanceMeters, meters);
  Drivetrain.driveFor(reverse, distanceMeters, meters);
}

int main() {
  // Initializing Robot Configuration. DO NOT REMOVE!
  vexcodeInit();

  // Loop through distances from 0.5m to 2.5m
  for (double distance = START_DISTANCE; distance <= MAX_DISTANCE; distance += DISTANCE_STEP) {
    driveForwardAndBack(distance);
  }

  // Stop drivetrain at the end
  Drivetrain.stop();
}
