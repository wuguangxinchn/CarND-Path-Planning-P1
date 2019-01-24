# CarND-Path-Planning-P1 
Self-Driving Car Engineer Nanodegree Program - Path Planning Project
   
## Goals
In this project your goal is to safely navigate around a virtual highway with other traffic that is driving +-10 MPH of the 50 MPH speed limit. You will be provided the car's localization and sensor fusion data, there is also a sparse map list of waypoints around the highway. The car should try to go as close as possible to the 50 MPH speed limit, which means passing slower traffic when possible, note that other cars will try to change lanes too. The car should avoid hitting other cars at all cost as well as driving inside of the marked road lanes at all times, unless going from one lane to another. The car should be able to make one complete loop around the 6946m highway. Since the car is trying to go 50 MPH, it should take a little over 5 minutes to complete 1 loop. Also the car should not experience total acceleration over 10 m/s^2 and jerk that is greater than 10 m/s^3.


## Basic Build Instructions
1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./path_planning`.

## Implementation and Reflection 
Firstly, I refer the approach & code introduced in the course (Q&A session), and make the car be able to run in the middle lane smoothly. The code of the tests are also in `main.cpp`. 

To make the car smoothly change lanes, I am following the steps:   

### Detect cars in nearby area 
Using the sensor data, we can detect the cars in nearby area (line 300~365): 
* Which lane is the car on? 
* Is the car on our lane, and ahead of us in less than a safety distance? (30 meters in this project) 
* Is the car on the left/right lane? does it impact us for lane changing? 

### Take actions if there is car ahead 
The logic of lane changing is simple (line 409~441):
* If there is a left lane and no car on it, then change lane left; 
* If there is a right lane and no car on it, then change lane right;
* If impossible to change lanes, then we have to brake; 
* We will try to go back to the center lane if we are not there. 

### Make the car run & change line smoothly  
* Create anchor points - I define 5 points, two from previous trajectory, and three from the near future trajectory (evenly 30m spaced)
* Create spline based on the anchor points  
* Fill up the rest of points   
The code for this part is located in lines 445~544. 

## Conclusion 
This solution works well, it's simple but not perfect. I didn't use the FSM approach, there is also no cost functions here. It means I didn't calculate and evaluate the trajectories, the logic of when/how to change lanes is pre-configured. 

PS: When reading the "Behavior Planning" chapter in the course, I have big confusion to understand "intended lane", "final lane" and "goal line"... until I found the link below: 
https://knowledge.udacity.com/questions/3662 
