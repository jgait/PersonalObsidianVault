- Looking @ [[Model Predictive Control]] applied to human robot collaboration
	- **Minimize:** a (com reference position tracking error) + b (distance between foot and COM on projection plane). This is $u$ and $\ddot{r}$ 
	- **Constraints:** LIP Dynamics, Kinematic Constraints, State Bounds
- Simplifies the digit robot into a Linear Inverted Pendulum Model (LIP) in order to simplify model
- **Look Up:** Diverging and converging components ($r_{convergent}$ $r_{ic}$ --> Divergent)
- Motion moves along the divergent direction 
- Capture point --> $u_{k,x} = r_{ic}$ when this is achieved the system is captured and the system is able to come to a complete stop
	- 0 step capture satisfies the equation in one step
	- 1 step capture satisfies the equation in two steps (Need to ensure that this is possible)
	- All modes have a capture region about the divergent point that the foot must be placed in
	- when a lower order capture cannot be achieved due to non intersection of robot operation and the capture region, the order of the capture process increases
	- The progression can be back calculated to N steps
	- Trending N to infinity creates the bounding box that brings LIP to stop in finite steps
	- If the capture point falls within the box, we are stable
- By replacing state bounds with a constraint that forces the solution to fall within the infinite capturable region, we ensure feasibility of the controller 
- Adding external forces yields a modified capturability range, as well as constraints on the magnitude of the force for stability

# Goal


# Improvements
- I think an animation/video to explain the capture regions would be hugely useful
