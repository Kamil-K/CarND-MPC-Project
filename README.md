Kamil Kaczmarczyk
2017-10-13

# Model Predictive Controller Project
Self-Driving Car Engineer Nanodegree Program

![alt tex](https://github.com/Kamil-K/CarND-MPC-Project/blob/master/pics/pic1.PNG "Successful Run")

## Introduction

This directory contains an implementation of a Model Predictive Controller for a Self-Driving Car NanoDegree from Udacity. The goal of the project is to make the car drive along waypoints on a race track in a [simulator](https://github.com/udacity/self-driving-car-sim/releases). 

## Discussion

### The Model

The model used in this project is a kinematic model which includes 6 state variables such as x- and y- coordinates, psi- orientation angle, v- velocity, cte- cross-track-error, epsi- orientation error as well as 2 actuator variables such as a- acceleration and delta- steering angle.

The model uses previous step state variables and actuations to estimate the current step state variables according to the equation set below:

![equation](http://latex.codecogs.com/gif.latex?x_%7Bt&plus;1%7D%3Dx_%7Bt%7D&plus;v_%7Bt%7D*%5Ccos%28%5Cpsi_%7Bt%7D%29*dt)

![equation](http://latex.codecogs.com/gif.latex?y_%7Bt&plus;1%7D%3Dy_%7Bt%7D&plus;v_%7Bt%7D*%5Csin%28%5Cpsi_%7Bt%7D%29*dt)

![equation](http://latex.codecogs.com/gif.latex?%5Cpsi_%7Bt&plus;1%7D%3D%5Cpsi_%7Bt%7D&plus;v_%7Bt%7D/L_%7Bf%7D*%5Cdelta_%7Bt%7D*dt)

![equation](http://latex.codecogs.com/gif.latex?v_%7Bt&plus;1%7D%3Dv_%7Bt%7D&plus;a_%7Bt%7D*dt)

![equation](http://latex.codecogs.com/gif.latex?cte_%7Bt&plus;1%7D%3Df%28x_%7Bt%7D%29-y_%7Bt%7D&plus;%28v_%7Bt%7D*sin%28e%5Cpsi_%7Bt%7D%29*dt%29)

![equation](http://latex.codecogs.com/gif.latex?e%5Cpsi_%7Bt&plus;1%7D%3D%5Cpsi_%7Bt&plus;1%7D-%5Cpsides_%7Bt%7D&plus;%28v_%7Bt%7D/L_%7Bf%7D*%5Cdelta_%7Bt%7D*dt%29)

### Timestep Length and Elapsed Duration (N & dt)

The final values are (N = 10; dt = 0.1) and other values tried are (N = 25; dt = 0.05), (N = 15; dt = 0.1) and (N = 5; dt = 0.1). The other values tried produced a behaviour with diverging trajectories around the optimal driving path. The method used was simple a trial and error to find the optimal values. The explanation could be that the final values give a horizon of 1 sec which seems pretty reasonable considering the variability of the track and also give 10 points to optmize for which also seems not too many to not be computationally too expensive. Additionally as it happened the value of dt is in this case corresponding to the latency of the system of 100ms which was used in the later part of the project.

### Polynomial Fitting and MPC Preprocessing

To simplify the calculations the waypoints are transformed into the reference frame of the vehicle. This means that the x and y coordinates as well as the orientation angle psi become now zero and hence it is computationally cheaper to fit the polynomial.

### Model Predictive Control with Latency

In this work the latency of 100ms is imposed. Accidentaly the step value dt is also chosen to be equal to 0.1 sec. This simplifies the implementation step to account for this latency by taking the actuation values shifted by one step dt (of course if time step is greater than 1). The code for this implementation can be found in the MPC.cpp file in lines 116-119.

## YouTube video of the final ride
Click [here](https://youtu.be/g3HmccIpYwk) or on the image below to go to the YouTube video<p>
[![MPC Track Ride](https://img.youtube.com/vi/g3HmccIpYwk/0.jpg)](https://youtu.be/g3HmccIpYwk)

---

## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1(mac, linux), 3.81(Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `install-mac.sh` or `install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets
    cd uWebSockets
    git checkout e94b6e1
    ```
    Some function signatures have changed in v0.14.x. See [this PR](https://github.com/udacity/CarND-MPC-Project/pull/3) for more details.

* **Ipopt and CppAD:** Please refer to [this document](https://github.com/udacity/CarND-MPC-Project/blob/master/install_Ipopt_CppAD.md) for installation instructions.
* [Eigen](http://eigen.tuxfamily.org/index.php?title=Main_Page). This is already part of the repo so you shouldn't have to worry about it.
* Simulator. You can download these from the [releases tab](https://github.com/udacity/self-driving-car-sim/releases).
* Not a dependency but read the [DATA.md](./DATA.md) for a description of the data sent back from the simulator.


## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./mpc`.

## Tips

1. It's recommended to test the MPC on basic examples to see if your implementation behaves as desired. One possible example
is the vehicle starting offset of a straight line (reference). If the MPC implementation is correct, after some number of timesteps
(not too many) it should find and track the reference line.
2. The `lake_track_waypoints.csv` file has the waypoints of the lake track. You could use this to fit polynomials and points and see of how well your model tracks curve. NOTE: This file might be not completely in sync with the simulator so your solution should NOT depend on it.
3. For visualization this C++ [matplotlib wrapper](https://github.com/lava/matplotlib-cpp) could be helpful.)
4.  Tips for setting up your environment are available [here](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/0949fca6-b379-42af-a919-ee50aa304e6a/lessons/f758c44c-5e40-4e01-93b5-1a82aa4e044f/concepts/23d376c7-0195-4276-bdf0-e02f1f3c665d)
5. **VM Latency:** Some students have reported differences in behavior using VM's ostensibly a result of latency.  Please let us know if issues arise as a result of a VM environment.

## Editor Settings

We've purposefully kept editor configuration files out of this repo in order to
keep it as simple and environment agnostic as possible. However, we recommend
using the following settings:

* indent using spaces
* set tab width to 2 spaces (keeps the matrices in source code aligned)

## Code Style

Please (do your best to) stick to [Google's C++ style guide](https://google.github.io/styleguide/cppguide.html).

## Project Instructions and Rubric

Note: regardless of the changes you make, your project must be buildable using
cmake and make!

More information is only accessible by people who are already enrolled in Term 2
of CarND. If you are enrolled, see [the project page](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/f1820894-8322-4bb3-81aa-b26b3c6dcbaf/lessons/b1ff3be0-c904-438e-aad3-2b5379f0e0c3/concepts/1a2255a0-e23c-44cf-8d41-39b8a3c8264a)
for instructions and the project rubric.

## Hints!

* You don't have to follow this directory structure, but if you do, your work
  will span all of the .cpp files here. Keep an eye out for TODOs.

## Call for IDE Profiles Pull Requests

Help your fellow students!

We decided to create Makefiles with cmake to keep this project as platform
agnostic as possible. Similarly, we omitted IDE profiles in order to we ensure
that students don't feel pressured to use one IDE or another.

However! I'd love to help people get up and running with their IDEs of choice.
If you've created a profile for an IDE that you think other students would
appreciate, we'd love to have you add the requisite profile files and
instructions to ide_profiles/. For example if you wanted to add a VS Code
profile, you'd add:

* /ide_profiles/vscode/.vscode
* /ide_profiles/vscode/README.md

The README should explain what the profile does, how to take advantage of it,
and how to install it.

Frankly, I've never been involved in a project with multiple IDE profiles
before. I believe the best way to handle this would be to keep them out of the
repo root to avoid clutter. My expectation is that most profiles will include
instructions to copy files to a new location to get picked up by the IDE, but
that's just a guess.

One last note here: regardless of the IDE used, every submitted project must
still be compilable with cmake and make./

## How to write a README
A well written README file can enhance your project and portfolio.  Develop your abilities to create professional README files by completing [this free course](https://www.udacity.com/course/writing-readmes--ud777).
