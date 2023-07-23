<h2 align="center">Demonstration video</h2>
<p align="center">
<a href="https://www.youtube.com/watch?v=T9QwjRZ5eqI&t=00s"><img src="images/thumbnail.png" height="70%" width="70%"></a>
</p>

## Contents:
<ol>
   <li><a href="https://s-nithish-kumar.github.io/Self-Balancing_Motorcycle/#1-objectives">Objectives</a></li>
   <li><a href="https://s-nithish-kumar.github.io/Self-Balancing_Motorcycle/#2-sensors">Sensors</a></li>
   <li><a href="https://s-nithish-kumar.github.io/Self-Balancing_Motorcycle/#3-actuators">Actuators</a></li>
   <li><a href="https://s-nithish-kumar.github.io/Self-Balancing_Motorcycle/#4-self-balancing-motorcycle-kinetics">Self-balancing Motorcycle Kinetics</a></li>
   <li><a href="https://s-nithish-kumar.github.io/Self-Balancing_Motorcycle/#5-implementation">Implementation</a></li>
   <li><a href="https://s-nithish-kumar.github.io/Self-Balancing_Motorcycle/#6-problems-and-troubleshooting">Problems and Troubleshooting</a></li>
   <li><a href="https://s-nithish-kumar.github.io/Self-Balancing_Motorcycle/#7-results-and-conclusion">Results and Conclusion</a></li>
</ol>

### 1. Objectives:
- To build and program a motorcycle that self balances and maneuvers by itself using a flywheel.
- Understand the kinetics of the motorcycle and design a controller for balancing the motorcycle.
- Tune the PID controller to self balance the motorcycle.
+ Balance and move the motorcycle in a straight line and in a circular path.

### 2. Sensors:
- Inertial Measurement Unit for measuring the motorcycle lean angle.
- Rotary encoder, mounted on the inertia wheel motor, to measure velocity of the inertia wheel.
- Rotary encoder, mounted on the rear wheel motor, to measure the velocity of the rear wheel. This sensor is not needed for balance.
+ Battery voltage sensor to measure the battery voltage level and turn the controller off before running out of charge.

### 3. Actuators:
- Inertia wheel motor to drive the inertia wheel.
- Rear wheel motor to drive the rear wheel.
+ Servo motor for steering control.

### 4. Self-balancing Motorcycle Kinetics:

<p align="center">
<img src="images/SpeechVive.jpg" height="60%" width="60%">
</p>
<p align="center">Figure 3 SpeechVive</p>

- The problem of balancing the motorcycle can be considered a problem of controlling an inverted pendulum. As shown in figure 1, when viewed from the rear, it may look like a pendulum rod with an inertia wheel attached to it.
- A is the rotation axis of the revolute joint between the pendulum rod and the ground. In this context, rotation axis A, means the axis going through A and perpendicular to the paper plane. lAD is the length of the pendulum rod, i.e., the length of the motorcycle frame.
- B is the center of mass of the pendulum rod. The pendulum rod is assumed to have a uniform density, which means point B will be in the middle between A and D.
+ C is the rotation axis of the revolute joint between the pendulum rod and the inertia wheel. 
+ After a few mathematical derivations, the following state vector derivative is obtained, which can be used for modeling in Simulink.

<p align="center">
<img src="images/SpeechVive.jpg" height="60%" width="60%">
</p>
<p align="center">Figure 3 SpeechVive</p>

'''

ẋ(t) - Derivative of state vector
θ - Lean angle of the pendulum (rads)
Theta dot - Angular velocity (rad/s)
Theta dot dot -  Angular acceleration (rad/s2)
ω - Rotational speed of the inertia wheel (rad/s)
Omega dot - Rotational acceleration of the inertia wheel (rad/s<sup>2</sup>)
m<sub>r</sub> - Mass of pendulum rod (kg)
m<sub>w</sub> - Mass of inertia wheel (kg)
l<sub>AB</sub> - Distance from ground to the center of mass of the pendulum rod (m)
l<sub>AC</sub> - Distance from ground to the center of mass of the inertia wheel (m)
l<sub>AD</sub> - Length of the pendulum rod (m)
𝜏<sub>m</sub> - Motor torque (Nm)
'''

### 5. Implementation:
#### Steps:
1. Using the state vector derivative, the inverted pendulum model is developed with Simulink, as shown below in Figure . Initially, when no motor torque is applied, the pendulum oscillates freely, as seen in Figure . When motor torque is applied, which is provided as feedback of the pendulum angle, the pendulum oscillates in the upright position, as depicted in Figures x and y, respectively.
2. A PID controller is added to further increase the stability of the inverted pendulum and maintain a zero degree lean angle. The simulink model with PID controller and the output of the model is shown in figures and, respectively.
3. The motorcycle is assembled, and each of the above-mentioned sensors is tested by developing models with Simulink for each sensor, and the models are run in External mode.
4. First, the IMU is tested by running the model in External mode. Simulink has a prebuilt function block for the BNO055 IMU sensor that shows the angular rate, euler angles, and calibration status of the sensor, as seen in Figure . The sensor has to be calibrated every time the controller is powered on.
5. The inertia wheel motor rotation is calculated using the Encoder block. A filtered derivative is used to obtain rotations per second and also to filter the signal. The image below shows the Simulink model.
6. The DC motors can be controlled with the DC Motors block, as shown below. A constant value between 0 and 1 is passed as input. A gain block with a value of 255 is added before sending the input to the DC Motors block.
7. The battery voltage can be measured with the Battery Read block, as shown below.
8. To control the servo motor for steering, the Servo Write block is used as shown in Figure
9. Once all the sensors and actuators are tested, all the models are combined into a subsystem.
10. The sensor data from the subsystem is fed into the PID controller, which gives a signal to rotate the inertia wheel motor for balancing the motorcycle.
11. The PID controller is tuned to balance the motorcycle even when it moves.
12. The Digital Controller block has safety logic that checks the IMU calibration status, battery level, and standing or falling state of the motorcycle. If any of the above conditions fail, then the controller turns off.
13. The rear motor speed is increased slowly using a slider to move the motorcycle.
14. Then the steering angle of the motorcycle is slightly changed, and the rear motor speed is slowly increased to make the motorcycle move in a circular path.

### 6. Problems and Troubleshooting:
#### Circuit Diagram:
- Tuning the PID was hard and time consuming. To make the tuning easier, the controller is tuned in the following sequence: P, D, and I. 
- Even a fairly large value of I leads to integral winding and makes the inertia motor speed increase continuously, making the motorcycle unstable. To overcome this problem, the I value is set to the minimum possible value.
- For the same bias value used for straight line motion, the motorcycle was unstable for circular motion as it leaned slightly after changing the steering angle. The bias value has been tuned again for circular motion.
+ A 3.7 V battery is used to power the motorcycle. Even with a small drop in voltage, the tuned PID values cannot stabilize the robot. The Battery voltage has to be checked and should be above 3.4 V for the motorcycle to stabilize.
+ Higher rear motor speeds provide vibrations that make the motorcycle fall. The rear motor is increased only up to a level where the vibrations are well contained

### 7. Results and Conclusion:
- The kinetics of the motorcycle were studied and modeled successfully with Simulink.
- The motorcycle can move in a straight path and circular path on smooth surfaces.
+ To move the robot in a circular path, the bias value in the controller is tuned, but this can be avoided by dynamic biasing, which uses steering angle to compute the bias value.












