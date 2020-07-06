# Self Driving Car Nanodegree PID Controller Project
Self-Driving Car Engineer Nanodegree Program

### By: Jayant Sachdev
### Date: July 6th, 2020 

## Reflection

#### Introduction to PID

A PID controller is a control loop mechanism that employs feedback to control a wide variety of control processes, or in the case of this project the throttle and road-wheel angle actuators.
The feedback loop consists of three distinct portions that sum up to the final control signal:

 - Proportional term: The proportional term looks at the current error and applies a gain on this error to scale the control signal as a function of the error. In terms of the steering control, this results in oscillatory behaviour around the center of the lane.
 - Intergral term: The integral term looks at the history of all the previous errors and applies a gain to the cumilative error. In terms of steering control this results in adjusting for steady state offsets (such as a wheel misalignment) which result in offcenter tracking.  
 - Derivative term: The derivative term looks at the rate of change of the error and applies a gain on it to account for how quicly the error is changing. This results in damping the control signal such that it both diverges slower and converges slower. In terms of the steering system, this reduces overshoot and oscillations around the center line.  
 
 #### Tuning of the PID Controller
 
For this project, the PID controller was tuned manually. I started tuning the steering control first with only P Gain = 0.005 and increased it until it was able to follow the curves loosely. There was a large amplitude oscillation during this point and increasing the PGain too much resulted in instability which meant that the oscillation amplitude kept growing unboundedly. I then added D-Gain to help reduce the oscillations. Adding too much d-gain results in a very high frequency oscilation as it tries to keep the vehicle at the current error point. Finally, i added some I-Gain to eliminate the systematic offsets which may be present. The same approach was applied to the throttle control but since the throttle control had a much more linear error dynamic it was much simpler to tune. The final PID gains for the two controllers are shown below:
 
Steering Control: [P<sub>Gain</sub>, I<sub>Gain</sub>, D<sub>Gain</sub>] = [0.11, 0.0015, 0.85])
Throttle Control: [P<sub>Gain</sub>, I<sub>Gain</sub>, D<sub>Gain</sub>] = [1.1, 0.0, 0.5])
 
 #### PID for Autonomous vehicle applications and Simulation Results
 
The steering problem itself is a highly non-linear problem, due to the fact that the error dynamics are vasty different on straights vs curves and can even change based on the crown of the road or changing weather patterns. As a result use of a PID alone results in sub-optimal performance across all regimes. The reason for this is that the PID gains are constant and as such it is structured to resolve a linear system with bounded error dynamics. This can be seen in the simulation, when in curves, the vehicle gets much closer to the lane lines. Tuning the PID to track the curves resulted in unstable performance in the straights as the entire system was overgained. As a result, the PID gains were chosen such that performance was sub-optimal for both regimes. This could be seen in the simulation, where the system has some overshoot in the straights and in the curves it gets very close to the outside of the curve. There are many ways to get around this: First of all, we can add feedforward terms based on the bicycle model and curvature/heading of the vehicle. This works by applying a control signal to account for the non-linearities such that the PID just has to deal with error dynamics which are much more consistant across all regimes. The issue with feedforward control is that errors in the feedforward model can result in inaccuracies in the control signal, which again adds non-linearity to the error dynamics the PID controller needs to deal with. One other solution is too use look up tables to allow some flexibility in the PID gains as we enter different control regimes. For example, we could use curvature based look up tables to select PID gains and then we can properly tune the PID for the error dynamics related to each curvature value. This allows more flexibility to have better tuned PIDs for all the expected error dynamics. We could also add additional PID's specifically for the different kind of errors, such as heading and curvature, this would also allow the control architecture to better deal with the non-linearities of the problem. 

The throttle problem was much more linear, as such a PD control architecture was sufficient to get really good performance from this controller. In the simulator, we adjusted for the requested steering angle, to slow the vehicle when we are in curves. Overall this was easy to tune and provided good results. 

Overall the performance of the PID controller was as expected. It struggled to provide a smooth steering control in straights and curves, but provided a very smooth throttle response. Adding logic to deal with the non-linearities of the steering problem would allow us to apply a smooth control signal for the steering problem as well.
