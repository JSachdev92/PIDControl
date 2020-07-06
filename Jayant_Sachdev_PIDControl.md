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
 
 #### PID for Autonomous vehicle applications
 
 The steering problem itself is a highly non-linear problem, due to the fact that the error dynamics are vasty different on straights vs curves and can even change based on the crown of the road or changing weather patterns. As a result use of a PID alone results in sub-optimal performance across all regimes. The reason for this is that the PID gains are constant and as such it is structured to resolve a linear system with bounded error dynamics. 
